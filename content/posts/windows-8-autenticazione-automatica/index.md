---
title: "Windows 8: autenticazione automatica"
url: "windows-8-autenticazione-automatica"
date: 2012-12-02T10:19:00.0000000Z
lastmod: 2012-12-02T10:20:27.0000000Z
tags: ["Imported"]
draft: false
---
<p>Lo so che in questo post potrei passare per uno che è legato al "vecchio", ma aggiornando tutti i miei computer a Windows 8 ho sentito la necessità di risolvere un problema che magari alcuni di voi hanno riscontrato. Io sono uno di quelli che ha ancora il caro vecchio desktop o pc fisso che trovo ancora utile perché posso avere la mia bella postazione fissa con doppio monitor ed un sistema aggiornabile e potente senza dover ogni volta cambiare tutto il computer (come accadrebbe per esempio con un notebook). Come ogni buon utilizzatore di un PC fisso, non trovo utile ad ogni avvio dover eseguire l'autenticazione di Windows.</p>
<p>Fino ad oggi, tutti i sistemi operativi da Windows 95 in poi, si avviavano senza chiedere nulla, anche perché il più delle volte il PC era impostato con un solo utente amministratore senza password. Con Windows 8, tuttavia, è stata introdotta una straordinaria possibilità: utilizzare il proprio account live per autenticarsi. Questa possibilità consente ad un utente di installare applicazioni, configurarle, etc.... ritrovandosi magicamente tali impostazioni su tutti i computer con Windows 8 che utilizzano il medesimo account. In realtà questo meccanismo non è automatico per tutte le app, ma dipende da come queste sono fatte: impostazioni come gli account email e dei social network funzionano in questo modo e lo trovo molto comodo. Il problema principale di questo approccio è che Windows 8 richiede l'immissione della password ad ogni avvio e, se tale pratica la trovo utile e consigliabile per un notebook o un tablet, francamente sul mio caro vecchio PC fisso voglio evitarla.</p>
<p style="text-align: center;"><img src="/Media/Default/BlogPost/netplwiz.png" alt="" width="477" height="517" /></p>
<p>Per fare in modo che Windows 8 si avvii automaticamente autenticandosi senza richiedere password, dovete seguire la seguente procedura:</p>
<ol>
<li>Avviate un programma chiamato <strong>netplwiz</strong> (consiglio WIN+R e poi netplwiz + INVIO)</li>
<li>Si aprirà una schermata che mostra tutti gli utenti del vostro sistema</li>
<li>Eliminate la spunta "<em>Users must enter a user name and password to use this computer</em>" (o l'equivalente in italiano)</li>
<li>Vi verrà chiesto nome utente e password da usare per l'autenticazione automatica all'avvio del computer</li>
<li>Cliccate su Ok</li>
</ol>
<p>Al successivo riavvio potrete verificare che il sistema operativo parte senza richiedere nulla :)</p>