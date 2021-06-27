---
title: "AutomationFactory in Silverlight 5"
url: "automationfactory-in-silverlight-5"
date: 2012-04-23T07:32:18.0000000Z
lastmod: 2012-04-19T08:37:00.0000000Z
tags: ["Imported"]
draft: false
---
<p style="text-align: center;"><img src="/Media/Default/BlogPost//silverlight_logo.png" alt="" width="450" height="146" /></p>
<p>E' da diverse versioni che Silverlight supporta la possibilità di richiamare <strong>Automation Server</strong> mediante l'API <a href="http://msdn.microsoft.com/en-us/library/ff457794(VS.95).aspx" title="AutomationFactory" target="_blank">AutomationFactory</a>. In <strong>Silverlight 5</strong>, tuttavia, la possibilità di avere applicazioni in browser di tipo Trusted, consente un più ampio ventaglio di utilizzi di questa tecnologia. Fino a Silverlight 4, infatti, qualsiasi operazione che richiedesse un aumento dei privilegi poteva essere eseguita solo ed esclusivamente se l'applicazione era di tipo out of browser il che, soprattutto dal punto di vista commerciale/marketing, potrebbe rappresentare un problema. Diciamolo chiaramente: gli addetti al settore sanno benissimo che Silverlight, così come Flash, non possono essere definite tecnologie puramente web. Se si vuole fare un sistema web puro, si deve usare HTML, Javascript e qualche linguaggio server side. Tuttavia, benché con HTML 5, CSS 3 e tutto ciò che gira attorno, tale tendenza sembra essere quella che inevitabilmente da qui a qualche anno si diffonderà, utilizzare ambienti di sviluppo come Silverlight rappresenta a mio parere, almeno fino ad ora, un notevole vantaggio nei seguenti aspetti:</p>
<ul>
<li>linguaggio di programmazione - con tutto il rispetto per javascript, usare C# garantisce una netta superiorità dal punto di vista della produttività, degli strumenti di sviluppo/debug, degli strumenti per test automatici e dell'implementazione di design pattern</li>
<li>accesso a risorse locali - non tutte le applicazioni possono astrarsi dalla macchina su cui lavorano, alcune necessitano si accedere a dispositivi hardware e molto spesso non possibile accedervi usando javascript</li>
</ul>
<p>In Silverlight 5 è possibile, quindi, lavorare con applicazioni Trusted in browser e questa peculiarità ci permette di eseguire operazioni come, per esempio, il deploy in locale di un pacchetto Windows Installer che, una volta installato, potrebbe esporci mediante una libreria COM visible, funzionalità normalmente non accessibili. Di seguito un esempio di codice che recupera il pacchetto di installazione da risorse dello XAP stesso (ma potreste scaricarlo da un web server o in qualunque altro modo), lo scrive sul disco (la funzione WriteStreamIntoStream non fa altro che travasare gli stream) e poi lo esegue utilizzando l'oggetto COM <strong>WScript.Shell</strong> e, in particolare, il metodo Run:</p>
<pre class="brush: csharp;">StreamResourceInfo srMSI = Application.GetResourceStream(new Uri("Test;component/Packages/Test.msi", UriKind.Relative));
var filePath = System.IO.Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "Test.msi");
using (var setupStream = File.Open(filePath, FileMode.OpenOrCreate))
{
	WriteStreamIntoStream(srMSI.Stream, setupStream);
}

var shell = AutomationFactory.CreateObject("WScript.Shell");
var result = shell.Run(filePath, 1, true);
return result == 0;</pre>
<p>Chiaramente se il pacchetto di installazione richiede privilegi di amministratore, in ambienti come Windows Vista o Windows 7 con UAC attivo, verrà richiesta l'elevazione dei privilegi e, a tal proposito, vi consiglio caldamente di firmare con un certificato digitale il pacchetto MSI in modo che il cliente abbia ben chiara l'origine dell'operazione.</p>