---
title: "ORM per .NET: un'interessante comparativa"
url: "43/orm-per--net--uninteressante-comparativa"
date: 2010-09-23T18:00:00.0000000Z
lastmod: 2010-09-23T18:00:00.0000000Z
tags: ["Imported"]
draft: false
---
<p>
	Nel corso dello sviluppo di un progetto utilizzando l'Entity Framework per il layer di accesso ai dati e LINQ to Entities come formalismo per l'interrogazione del database, mi sono imbattuto in alcuni problemi di performance che mi hanno spinto a fare qualche approfondimento su questa tecnologia per l'accesso ai dati.</p>
<p>
	Premetto subito che sto usando il .NET Framework 4 insieme ai POCO come modello, in maniera da slegare la materializzazione degli oggetti dalla specifica tecnologia di accesso ai dati scelta.</p>
<p>
	Navigando sul sito di Devart (il database server usato dal progetto è Oracle 11g) ho trovato un link a <a href="http://ormbattle.net" target="_blank">ORMBattle.NET</a>, un'interessantissimo sito che effettua una comparazione tra molteplici ORM su .NET valutandole l'implementazione di LINQ e le performance.</p>
<p style="text-align: center; ">
	<img alt="" src="/public/image/Queries.png" style="width: 450px; height: 360px; " /></p>
<p>
	I risultati non sono per nulla scontati. Innanzitutto l'Entity Framework non è l'ORM più efficiente del lotto. Si può dire che fornisce un supporto a LINQ nella media, mentre dal punto di vista delle performance è complessivamente il terzultimo. Peggio di lui fanno solo NHibernate e Subsonic.</p>
<p>
	Molto interessanti, invece, sono le soluzioni DataObjects.NET e BLToolkit. Quest'ultimo, tuttavia, non può essere considerato un vero e proprio ORM in quanto, da quello che ho potuto vedere, effettua più che altro una materializzazione degli oggetti risultanti da una query. L'effort in termini di configurazione è sicuramente più elevato rispetto alle altre soluzioni e la valutazione di questo aspetto è a mio parere il punto debole della comparativa: non si considera quanto tempo fa risparmiare l'uso di un ORM rispetto ad altro o rispetto a non usarne per niente. E' certamente una valutazione difficile da stimare, ma secondo me non impossibile.</p>
<p>
	In fin dei conti che le performance degli ORM siano inferiori rispetto a fare query a botta di DataReader è certamente scontato, ma del resto non è per questo motivo che si utilizzano tali strumenti.</p>
<p>
	Sempre nell'ambito del lavoro svolto in questo progetto ho avuto modo di valutare il comportamento del provider di Devart con l'Entity Framework e analizzando le query generate posso serenamente affermare che, a parte cambiare le parentesi quadre con le virgolette e sostituire il token parameter @ con i due punti, altre differenze non ci sono nelle query generate rispetto al provider Microsoft per SQL Server,</p>
<p>
	A questo punto mi chiedo: si tratta di una limitazione dell'Entity Framework che non consente ad ogni singolo provider di generare completamente le query oppure i signori di Devart hanno "preso spunto" dal provider Microsoft per realizzare il loro per Oracle?</p>
