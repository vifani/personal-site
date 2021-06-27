---
title: "InstanceStore e WorkflowApplication in WF4"
url: "46/instancestore-e-workflowapplication-in-wf4"
date: 2010-11-02T19:51:00.0000000Z
lastmod: 2010-11-02T19:51:00.0000000Z
tags: ["Imported"]
draft: false
---
<p>
	Quando si utilizza Workflow Foundation 4 si ha a disposizione più di un'opzione per l'hosting di un'istanza di workflow, cioè essenzialmente per avviare ed eseguire un workflow.</p>
<p>
	Una delle grandi funzionalità introdotte da Workflow Foundation 4 è la classe <strong>WorkflowServiceHost </strong>grazie alla quale è possibile esporre un workflow come servizio e, attraverso un meccanismo di correlation, legare l'input delle chiamate al servizio in modo da identificare l'istanza di workflow al quale la chiamata fa riferimento.</p>
<p>
	Tutto il sistema legato al WorkflowServiceHost è molto interessante e direi piuttosto facile da utilizzare considerando che gestisce in autonomia attività come il resuming di un'istanza di workflow, la gestione di timer che scadono per le istanze di idle, ecc...</p>
<p>
	Cosa accade, però, se si ha la necessità di ottenere maggior controllo sul runtime di Workflow Foundation 4? Supponiamo di dover realizzare un'infrastruttura che, dato uno standard di comunicazione tra i workflow e il mondo esterno, debba consentire di gestire più tipologie di un workflow? E' evidente che questo tipo di requisito è incompatibile con l'hosting via WorkflowServiceHost semplicemente perché tale hosting si basa sul presupposto che ogni tipologia di workflow ha i suoi specifici servizi e tale presupposto non consente una standardizzazione della comunicazione tra il runtime del workflow e il mondo esterno. </p>
<p>
	A che serve, vi chiederete voi, una cosa del genere? Beh, se dovete disaccoppiare la logica del workflow dall'interfaccia grafica, è buona norma evitare di legare le chiamate al workflow ai web service esposti dal workflow stesso. In caso contrario una minima modifica a questi contratti, comporterà la ricompilazione di tutta l'applicazione.</p>
<p>
	La classe <strong>WorkflowApplication </strong>è la scelta prediletta nel caso in cui vogliate gestire manualmente l'hosting e la comunicazione tra le istanze di workflow e le applicazioni che devono richiamarle.</p>
<p>
	Fondamentale a tal proposito è la gestione della persistenza. Se siete così fortunati da non avere la necessità di persistere le vostre istanze di workflow, allora, oltre ad invidiarvi molto, vi comunico che potete anche non proseguire la lettura di questo post :-)</p>
<p>
	La persistenza passa dalle classi che ereditano dalla classe base InstanceStore. Ovviamente Workflow Foundation 4 integra quella necessaria per utilizzare SqlServer come repository e pertanto utilizzeremo la classe SqlWorkflowInstanceStore.</p>
<pre><span style="font-size:100%;">var workflow = new Workflow1();
var workflowApplication = new WorkflowApplication(workflow);
var instanceStore = new SqlWorkflowInstanceStore(connectionString);
workflowApplication.InstanceStore = instanceStore;
</span></pre>
<p>
	Il codice riportato funziona benissimo, ma ha un piccolo problema di efficienza: vi costringe a creare una nuova istanza di <strong>SqlWorkflowInstanceStore </strong>ogni volta che dovete gestire un'istanza di workflow. Se provate ad assegnare la stessa istanza di InstanceStore a più WorkflowApplication otterrete un bel <strong>InstancePersistenceCommandException </strong>con il seguente messaggio:</p>
<pre>SqlWorkflowInstanceStore does not support creating more than one lock owner 
concurrently. Consider setting InstanceStore.DefaultInstanceOwner to share the 
store among many applications.</pre>
<p>
	Il problema in sostanza è che un'istanza di SqlWorkflowInstanceStore funge anche da lock owner, cioè essenzialmente blocca un'istanza di workflow nel corso del suo ciclo di vita (dal caricamento alla persistenza) per impedire che parallelamente più host possano accedere alla medesima istanza.</p>
<p>
	Per fare in modo che un'istanza di SqlWorkflowInstanceStore possa caricare più istanze contemporanemente è necessario definire il ciclo di vita del lock owner e a tal proposito è necessario impostare la proprietà <strong>DefaultInstanceOwner </strong>con il risultato dell'esecuzione di un comando denominato <strong>CreateWorkflowOwnerCommand </strong>come segue:</p>
<pre><span style="font-size:100%;">var instanceHandle = instanceStore.CreateInstanceHandle();
var createOwnerCmd = new CreateWorkflowOwnerCommand();
var view = instanceStore.Execute(instanceHandle, createOwnerCmd, 
TimeSpan.FromSeconds(30));
instanceStore.DefaultInstanceOwner = view.InstanceOwner;</span></pre>
<p>
	Particolare importanza risiede nel <strong>InstanceHandle</strong>. Non sottovalutate la gestione di questo handle perché è da lui che dipende l'uso delle risorse del SqlWorkflowInstanceStore.</p>
<p>
	Una volta impostato il DefaultInstanceOwner potete far utilizzare questo InstanceStore a più istanze della classe WorkflowApplication. Non dovete, tuttavia, dimenticare di richiamare il comando <strong>DeleteWorkflowOwnerCommand </strong>quando avete completato le operazioni da eseguire sulle istanze di WorkflowApplication:</p>
<pre><span class="Apple-style-span" style="font-family: monospace; white-space: pre; ">var deleteOwnerCmd = new DeleteWorkflowOwnerCommand();
</span><span class="Apple-style-span" style="font-family: monospace; white-space: pre; ">instanceStore.Execute(instanceHandle, deleteOwnerCmd, TimeSpan.FromSeconds(30));</span></pre>
<p>
	L'esecuzione di questo comando garantisce che l'owner non sia più attivo e, pertanto, consente ad altri lock owner di accedere alle istanze di workflow. Da notare che sia il comando di Create che quello di Delete abbiano in comune l'instanceHandle che, a seguito del comando di Delete, viene correttamente rilasciato insieme a tutte le risorse che identifica. Nel caso in cui vogliate rilasciare le risorse prima di richiamare il comando Delete, potete utilizzare il metodo Free.</p>
