---
title: "ADO.NET, DataTable e il metodo Load"
url: "45/ado-net--datatable-e-il-metodo-load"
date: 2010-10-17T14:50:00.0000000Z
lastmod: 2010-10-17T14:50:00.0000000Z
tags: ["Imported"]
draft: false
---
<p>
	Quando si studia .NET uno dei aspetti che più di altri spesso vengono rimarcati è la <strong>gestione delle risorse</strong> e, in particolare, di tutte le classi che implementano l'interfaccia <em>IDisposable</em>. Come molto di voi sapranno quasi sempre le classi che implementano queste interfacce utilizzano risorse unmanaged ed è per questo che il loro corretto trattamento  è fondamentale al fine di liberare tali risorse. Gli esempi più classici sono gli stream come FileStream e NetworkStream che gestiscono rispettivamente l'accesso al file sistem e alla rete via socket.</p>
<p>
	Memore di questa buona pratica (sono quasi un fanatico dell'uso del costrutto <em>using</em>), ho spesso strutturato l'accesso al database in maniera tale da renderlo il più rapido ed efficiente possibile e in maniera da liberare al più presto la <em>DbConnection </em>utilizzata.</p>
<p>
	Tuttavia mi sono reso conto che una delle pratiche che ho spesso utilizzato per caricare i risultati di una query SELECT non è la più efficiente. In particolare, dopo aver correttamente costruito il mio <em>DbCommand</em>, averlo associato alla <em>DbConnection</em>, aver inserito eventuali <em>DbParameter</em>, mia comune pratica a seguito dell'esecuzione del metodo <em>ExecuteReader</em>, era quella di dichiarare una <em>DataTable </em>e di caricare il contenuto del <em>DbDataReader </em>attraverso il <strong>metodo <em>Load </em></strong>della classe <em>DataTable</em>.</p>
<p>
	Come saprete la classe <em>DataTable </em>non è altro che una struttura dati disconnessa che rappresenta in memoria il risultato di una query che restituisce almeno un result set. Di per sé non ha molto senso utilizzarla al giorno d'oggi visto che spesso si usa un ORM per accedere al database, tuttavia esistono casi in cui è necessario affidarsi ancora al buon vecchio ADO.NET.</p>
<p>
	Il metodo <em>Load </em>della classe <em>DataTable </em>è molto comodo perché carica in memoria il risultato di una query strutturando correttamente la DataTable in maniera che ogni sua <em>DataColumn </em>contenga le informazioni (tipo, nome, ecc....) relative ad ogni colonna del result set.</p>
<p style="text-align: center; ">
	<img alt="" src="/public/image/oracle.jpg" style="width: 475px; height: 185px; " /></p>
<p>
	Ho riscontato tuttavia un problema. Quando si utilizzano i Guid in Oracle, indipendentemente dal provider che utilizzate, che questo sia il Devart, ODP.NET o l'OracleClient del .NET Framework (a proposito quest'ultimo è ormai deprecated nella versione 4.0, quindi non usatelo più) nella vostra <em>DataTable</em>,<strong> in corrispondenza della colonna che dovrebbe contenere i Guid vi ritroverete un bel array di byte</strong>. Oracle gestisce i Guid come RAW(16), quindi effettivamente un array di 16 byte e, pertanto, il metodo Load della <em>DataTable </em>convertirà la colonna in un bel byte[].</p>
<p>
	Come risolvere questo problema? Inizialmente avevo pensato di scorrermi successivamente alla <em>Load</em>, nuovamente la <em>DataTable </em>per convertire le <em>DataColumn </em>di byte array in Guid. Inutile dirvi che questa è una pessima pensata perché le prestazioni risultano essere dannatamente penalizzanti, soprattutto in confronto all'esecuzione della medesima operazione su SQL Server dove questo problema non c'è e dove, pertanto, il passaggio di conversione non serve.</p>
<p>
	La soluzione a questo problema è stata di abbandonare il metodo <em>Load </em>della <em>DataTable </em>e di costruirsela a mano scorrendosi il <em>DbDataReader </em>come un bel ciclo while. Il <em>DbDataReader </em>contiene un metodo di Get per ogni tipo di dato supportato tra i quali il Guid. Poiché quando io carico il risultato dal <em>DbDataReader </em>so perfettamente a priori quale tipo di dato mi aspetto da ogni colonna, è stato sufficiente fare un bel switch case e richiamare il metodo di Get corretto.</p>
<p>
	Mentre implementavo questa soluzione pensavo di ottenere prestazioni molto basse rispetto all'uso del metodo <em>Load </em>della <em>DataTable </em>e, invece, mi sono dovuto ricredere. Non solo con questo nuovo sistema sono riuscito ad ottenere in ogni colonna i dati del giusto tipo (dovete usare il provider di Devart per poter usare la <em>GetGuid </em>su Oracle in quanto non è supportata dall'OracleClient e dall'ODP.NET), ma <strong>le prestazioni complessivamente sono aumentate notevolmente sia su Oracle, che su SQL Server di oltre 5 volte</strong>. Se prima per caricare 10.000 record la <em>Load </em>ci impiegava circa 400 ms, con la nuova soluzione ottengo il medesimo risultato in 70-80 ms, notevole non credete?</p>
<p>
	Certo, la quantità di codice scritta è molto più corposa (passiamo da 1 riga di codice, appunto la <em>Load</em>, ad una ventina di righe), ma poiché l'accesso ai dati è spesso uno dei maggiori colli di bottiglia in un sistema, incrementarne le prestazioni di oltre 5 volte non può che far bene.</p>
