---
title: "Silverlight e la sua asincronicità"
url: "51/silverlight-e-la-sua-asincronicit-"
date: 2011-01-07T19:35:12.0000000Z
lastmod: 2011-01-07T19:35:12.0000000Z
tags: ["Imported"]
draft: false
---
<p>
	Avete mai lavorato ad un progetto Silverlight con un backend fatto con il .NET Framework 4? Se lo avete fatto, vi renderete conto che una volta abituati a tutta una serie di comodità come, ad esempio, Parallel FX, ma anche semplici classi per la compressione degli stream, vorrete ben presto convincere il responsabile del progetto ad abbandonare l'idea di un sistema client fatto in Silverlight per ripiegare su WPF.</p>
<p>
	La realtà è che noi siamo informatici e che il nostro lavoro è per buona parte quello di risolvere problemi. Possiamo scegliere di farlo ad alto livello, risolvendo un problema ad un cliente, o a basso livello, risolvendo un problema di programmazione, ma in fin dei conti sempre di ideare/trovare/scopiazzare soluzioni stiamo parlando.</p>
<p>
	Nella fattispecie sto trovando molto stimolante l'ultimo progetto a cui sto lavorando, perché lavorare con Silverlight mi ha posto di fronte sfide nuove e la ricerca delle soluzioni non è mai stata noiosa e banale. Vi faccio qualche esempio di una serie di situazioni che mi hanno coinvolto.</p>
<p>
	Se sviluppate un'applicazione business in Silverlight, prima o poi vi scontrerete con l'accesso ai dati e vi renderete ben conto che ovviamente non esiste uno straccio di ADO.NET. Questo può essere un grande problema per chi è abituato a lavorare a botte di DataSet e DataTable, ma oggi esistono gli strumenti per andare oltre ADO.NET grazie ad ORM come l'Entity Framework 4 e al supporto dei POCO.</p>
<p>
	Chiaramente l'accesso ai dati da parte di Silverlight non viene fatto direttamente mediante connessione a database, ma solo tramite WCF. Poco male diranno i molti che conoscono WCF e che sanno bene quanto è semplice esporre servizi (e quindi dati) con questa potente API.</p>
<p>
	Ciò che però molti ignorano è che utilizzare WCF da Silverlight non è esattamente uguale ad usarlo da altre applicazioni .NET per una semplicissima ragione: <strong><u>Silverlight richiede necessariamente l'uso di chiamate asincrone</u></strong>.</p>
<p>
	I pattern asincroni sono ben noti fin dalle prime versioni del .NET Framework, ma diciamoci la verità: ragionare e progettare un sistema in modo asincrono non viene naturale quanto lo è farlo in modo sincrono e, pertanto, la realtà è che pochissimi hanno utilizzato questo tipo di pattern nella progettazione di un'applicazione standard.</p>
<p>
	Inizialmente ho provato a forzare il sistema tentando di de-asincronizzare le chiamate utilizzando un AutoResetEvent, un trucco meschino devo ammetterlo, che tra l'altro è fallito miseramente perché se mettete in attesa il thread della UI, bloccate automaticamente la vostra applicazione e non riceverete neanche la vostra response dal servizio WCF.</p>
<p>
	A questo punto ho preso la decisione di fermarmi, di sedermi a tavolino e di riprogettare le componenti client utilizzando pattern asincroni a go go. Bene, sapete che vi dico? Che sono molto soddisfatto del risultato che sto raggiungendo insieme al mio team di sviluppo. Non è semplice effettuare questo cambio di prospettiva, ma è bastato fare di necessità virtù e lavorare sodo per ottenere risultati più che ragguardevoli.</p>
<p>
	Il consiglio che posso dare a tutti coloro che hanno riscontrato difficoltà di questo tipo con Silverlight, è di non arrendervi, ma anzi di cogliere l'occasione per approfondire approcci meno canonici ai nostri problemi.</p>
