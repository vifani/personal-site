---
title: "Microsoft .NET Framework 4 Platform Update 1"
url: "66/microsoft--net-framework-4-platform-update-1"
date: 2011-07-15T23:46:22.0000000Z
lastmod: 2011-07-15T23:46:22.0000000Z
tags: ["Imported"]
draft: false
---
<p>
	Forse non tutti sanno che nel mese di Aprile di quest'anno Microsoft ha rilasciato un aggiornamento del .NET Framework 4 particolarmente importante e che a mio avviso avrebbe dovuto ricevere maggiore attenzione dalla community online di sviluppatori .NET, ma che per ragioni poco comprensibili è passato quasi inosservato.</p>
<p>
	Sto parlando del <a href="http://blogs.msdn.com/b/endpoint/archive/2011/04/18/microsoft-net-framework-4-platform-update-1.aspx" target="_blank"><strong>Platform Update 1</strong></a>, un aggiornamento che si compone di tre diversi pacchetti:</p>
<ul>
	<li>
		Microsoft .NET Framework 4 Platform Update 1 (KB2478063)</li>
	<li>
		Multi-Targeting Pack for Microsoft .NET Framework 4 Platform Update 1 (KB2495638)</li>
	<li>
		Microsoft .NET Framework 4 Platform Update 1 – Design-time Package for Visual Studio 2010 SP1 (KB2495593)</li>
</ul>
<p>
	Il Platform Update 1 porta una interessantissima novità in <strong>Windows Workflow Foundation 4</strong>: <strong>le macchine a stati</strong>. Chi di voi ha seguito questa API, sa bene che una delle sue deficienze, anzi probabilmente l'unica sua pecca, è rappresentata dall'impossibità di realizzare workflow a stati in maniera esplicita.</p>
<p>
	Il Platform Update 1 colma questa lacuna e l'importanza di questo aggiornamento è tale da farlo quasi sembrare una sorta di Service Pack 1 del .NET Framework 4 in quanto per utilizzare le nuove funzionalità è necessario installare il runtime (il primo dei tre pacchetti che vi ho citato), aggiungere il supporto al multi-targeting (secondo pacchetto) ed aggiungere un pacchetto dedicato al designer dei workflow di Visual Studio 2010 SP1.</p>
<p style="text-align: center;">
	 <img alt="" src="http://www.vifani.com/public/image/NET-Framework-4-Platform-Update-1.jpg" style="width: 550px; height: 302px;" /></p>
<p>
	Installati tutti e tre i pacchetti è necessario cambiare il target del progetto .NET impostandolo esplicitamente a ".NET Framework 4 Platform Update 1 (KB2478063)". Senza effettuare questa operazione non è possibile utilizzare i nuovi workflow a stati.</p>
<p>
	Ho già testato questi nuovi workflow e non ho riscontrato particolari problemi, anzi consentono di modellare in maniera abbastanza esauriente un flusso a stati. Consiglio a chi fosse interessato ad approfondirli, di dare un'occhiata ad un <a href="http://blogs.msdn.com/b/endpoint/archive/2011/04/20/wf4-state-machine-user-experience.aspx" target="_blank">articolo pubblicato nei blog MSDN</a> che effettua una panoramica sulle novità introdotte.</p>
<p>
	L'unico elemento negativo che mi sento di sottoporre alla vostra attenzione è la modalità con cui Microsoft ha rilasciato tale aggiornamento. Chi credeva che con il .NET Framework 4 fossero finite le incongruenze nelle modalità con il quale il framework è stato versionato in passato (3.0, 3.5, 3.5 SP1, ecc...), dovrà ricredersi visto che, invece di chiamare questo aggiornamento 4.1 (come saggiamente fatto dal team dell'Entity Framework), hanno deciso di adottare il termine Platform Update 1 che vi ritrovare addirittura nel menu a tendina del target completo di numero della knowledge base. Una scelta che certamente non va nella direzione della massima chiarezza.</p>
