---
title: "Come soddisfare lo Zio Sam se avete fatto un'app per iPhone"
url: "come-soddisfare-lo-zio-sam-se-avete-fatto-un-app-per-iphone"
date: 2012-06-04T16:35:00.0000000Z
lastmod: 2012-06-04T16:38:12.0000000Z
tags: ["Imported"]
draft: false
---
<p>Negli ultimi giorni mi sono imbattuto in una problematica molto particolare. Pubblicando sull'Apple Store alcune mie nuove <strong>app che fanno uso di cifratura AES</strong>, ho scoperto che è necessario fare una dichiarazione al <strong>U.S. Bureau of Industry and Security</strong>. In particolare bisogna sottomettere a questo organismo governativo statunitense il modulo chiamato "Encryption Registration <a href="http://ecfr.gpoaccess.gov/cgi/t/text/text-idx?c=ecfr&sid=bb3991df3df60878ea26f43aaeedb186&rgn=div9&view=text&node=15:2.1.3.4.26.0.1.20.34&idno=15" target="_blank">Supplement No. 5 to Part 742</a>".</p>
<p style="text-align: center;"><img src="/Media/Default/BlogPost/zio_sam.jpg" alt="" width="300" height="380" /></p>
<p>In base a quello che ho sostanzialmente capito, se si esporta dall'USA ad altri paesi un prodotto che utilizza tecnologie di cifratura che vanno oltre determinati standard (per esempio se usate AES a più di 64 bit), bisogna dichiararlo con questo modulo al fine di ottenere un<strong> Encryption Registration Number (ERN)</strong>. Poiché quando si pubblica un'app sull'Apple Store si invia ai server di Apple in USA un software che poi viene venduto in tutto il mondo (quindi esportato), è necessario fare questo tipo di dichiarazione ed inviare l'ERN ad Apple nel momento in cui si pubblicano le applicazioni.</p>
<p>Da italiano quale sono, abituato agli standard della pubblica amministrazione del Bel Paese, mi sono immediatamente preoccupato dei <strong>tempi</strong> che tale procedura avrebbe comportato. In realtà dopo aver svolto la procedura posso rassicurare tutti coloro che, come me, potrebbero trovarsi nella stessa situazione: l'USA non è l'Italia e, anche se tale obbligo potrebbe risultare essere ai più eccessivo (i soliti discorsi dello Zio Sam che vuole sapere e controllare tutto), in realtà non se ne sono andate più di 4 ore per completare il tutto :)</p>
<p>La procedura da seguire è abbastanza semplice:</p>
<ol>
<li>Andate sul sito <a href="https://snapr.bis.doc.gov/registration/Register.do">https://snapr.bis.doc.gov/registration/Register.do</a> e registratevi</li>
<li>Vi invieranno un paio di email per confermare la registrazione e scegliere le vostre credenziali. Questo è essenzialmente tutto il tempo che perderete, perché avute le credenziali il resto della procedura sarà immediata</li>
<li>Autenticatevi sul sito <a href="https://snapr.bis.doc.gov/snapr/exp/UserLoginLoad">https://snapr.bis.doc.gov/snapr/exp/UserLoginLoad</a></li>
<li>Selezionate la voce "Create Work Item"</li>
<li>Come type selezionate "Encryption Registration" e mettete un Reference Number nel formato richiesto (è un numero di riferimento di vostra fantasia, ma con un formato da rispettare)</li>
<li>Cliccate su "Create"</li>
<li>Nella schermata che vi verrà presentata, potrete, in fondo, allegare documenti in formato PDF. Create un documento nel formato del <a href="http://ecfr.gpoaccess.gov/cgi/t/text/text-idx?c=ecfr&sid=bb3991df3df60878ea26f43aaeedb186&rgn=div9&view=text&node=15:2.1.3.4.26.0.1.20.34&idno=15" target="_blank">Supplement No. 5 to Part 742</a> e compilatelo (un esempio di compilazione potete trovarla in <a href="http://tigelane.blogspot.it/2011/01/apple-itunes-export-restrictions-on.html" target="_blank">questo blog</a>). Allegate il documento appena creato in formato PDF</li>
<li>Procedete nel "Submit" del work item</li>
<li>Completata questa procedura, tornate alla home del sito e, restando autenticati, andate nell'elenco dei messaggi ricevuti e troverete un messaggio come <a href="http://4.bp.blogspot.com/_6Z_cRdQXY5k/TTTYRtkVm9I/AAAAAAAAAdU/RdmMhMOrW_s/s1600/screen-capture-280.jpg" target="_blank">questo</a> contenente l'ERN di cui avete bisogno</li>
<li>Fate uno screenshot del messaggio, mettetelo in un documento ed allegate questo documento all'atto della pubblicazione dell'app sull'Apple Store</li>
</ol>
<p>Come anticipato, non lasciatevi intimorire dalla procedura: è semplice e veloce e potrete sottomettere ad Apple la vostra app che usa API di encryption senza alcun timore che in futuro ve la ritirino dall'Apple Store.</p>
<p>Chiaramente tale procedura è necessaria solo ed esclusivamente se la vostra app usa tecnologie di cifratura oltre determinati standard (sul sito del Bureau trovate le specifiche), mentre non è richiesta in altri casi. Ponete particolare attenzione, infine, nel caso in cui avete implementato un qualche metodo di cifratura custom: se l'uso di algoritmi standard richiede al più l'ERN, se ne avete inventato uno tutto vostro la documentazione da presentare sarà molto più complessa e i tempi risulteranno essere più lunghi.</p>