---
title: "Visual Studio 2010 SP1 loading toolbox..."
url: "visual-studio-2010-sp1-loading-toolbox"
date: 2012-04-04T18:45:00.0000000Z
lastmod: 2012-04-10T07:38:18.0000000Z
tags: ["Imported"]
draft: false
---
<p style="text-align: center;"><img src="/Media/Default/BlogPost/VisualStudio2010Ultimate.png" alt="" width="450" height="322" /></p>
<p>Se avete installato i <strong>Silverlight 5 Tools</strong> per Visual Studio 2010 SP1 è molto probabile che al primo accesso alla toolbox, Visual Studio si blocchi per 50-60 secondi con il seguente messaggio nella status bar:</p>
<p style="text-align: center;"><em>Loading toolbox content from package Microsoft.VisualStudio.IDE.Toolbox.ControlInstaller.ToolboxInstallerPackage '{2C98B35-07DA-45F1-96A3-BE55D91C8D7A}'</em></p>
<p>Il problema sembra essere causato dall'installazione dei Silverlight 5 Tools e, in particolare, dal toolkit dei <strong>WCF RIA Services</strong> e di Silverlight 5 stesso. Per risolvere questo problema potete trovare online una serie di workaround, ma quello che a mio parere è il più valido è il seguente</p>
<ol>
<li>Disinstallate WCF RIA Services V1.0 SP2</li>
<li>Disinstallate WCF RIA Services Toolkit</li>
<li>Disinstallate Silverlight 5 Toolkit</li>
<li>Avviate Visual Studio 2010 -> Pulsante Destro sulla Toolbox -> Reset Toolbox</li>
<li>Chiudete Visual Studio 2010</li>
<li>Andate in %USERPROFILE%\Local Settings\Application Data\Microsoft\VisualStudio\10.0 ed eliminate tutti i file .TBD</li>
</ol>
<p>Dopo aver eseguito questa procedura, il primo avvio di Visual Studio sicuramente sarà più lento del solito, in particolare per caricare la toolbox, ma tutti i successivi avvii non vi riproporranno più l'antipatico problema. Se avete necessità di utilizzare i vari toolkit disinstallati, vi consiglio caldamente di abbracciare l'approccio NuGet e di smetterla di installarvi sull'ambiente di sviluppo e in GAC tutte le librerie che usate.</p>