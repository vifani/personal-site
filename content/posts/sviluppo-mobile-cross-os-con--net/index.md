---
title: "Sviluppo mobile cross-OS con .NET"
url: "44/sviluppo-mobile-cross-os-con--net"
date: 2010-09-28T17:41:00.0000000Z
lastmod: 2010-09-28T17:41:00.0000000Z
tags: ["Imported"]
draft: false
---
<p>
	Ultimamente mi sto cimentando nella programmazione per sistemi operativi mobile. In particolare, insieme ad un mio carissimo amico, stiamo tentando di concretizzare qualche progettino destinato al mercato degli smartphone.</p>
<p>
	Nella progettazione di queste soluzioni (beh si, non guardatemi male, io di solito inizio dalla progettazione e non scrivendo direttamente codice) abbiamo identificato la seguente architettura:</p>
<ul>
	<li>
		Web Service esposti sul web contenti la logica di business</li>
	<li>
		Database server sul web server</li>
	<li>
		Applicazione mobile che richiama i servizi e si occupa essenzialmente della visualizzazione dei risultati e di gestire la UI</li>
</ul>
<p>
	A questa organizzazione del lavoro, siamo giunti dopo aver analizzato i seguenti aspetti:</p>
<ol>
	<li>
		Inserire la logica lato server ci permette di rendere il servizio disponibile su più tipologie diverse di client, cioè essenzialmente su più piattaforme</li>
	<li>
		Inserire la logica lato server ci permette di semplificare al massimo la logica lato client</li>
	<li>
		Inserire la logica lato server ci permette di programmare la logica in C# usando .NET, il che, fidatevi, è un enorme vantaggio</li>
</ol>
<p>
	Il terzo punto in particolare va sottolineato perché se, come abbiamo intenzione di fare noi, si vogliono realizzare applicazioni su iPhone e iPad, è necessario imparare l'objective-c e buona parte della libreria Cocoa. Considerando che gran parte delle applicazioni per smartphone che riportano informazioni, lo fanno parserizzando selvaggiamente pagine HTML, volete mettere quando è semplice farlo utilizzando le API di .NET rispetto al SDK di iOS?</p>
<p>
	In realtà stiamo valutando anche la possibilità di scrivere le applicazioni lato client in C# utilizzando <a href="http://monotouch.net/" target="_blank">Monotouch</a>. Questo ci permetterebbe di rendere l'applicazione facilmente "portabile" anche su Windows Phone 7 e, contestualmente, semplifica non poco effettuare operazioni come le chiamate ai web service che su iOS vanno praticamente costruite a manina. Pensate che nella roadmap di Mono è previsto perfino Monodroid per lo sviluppo in C# per Android :-)</p>
