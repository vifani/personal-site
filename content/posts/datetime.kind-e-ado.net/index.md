---
title: "DateTime.Kind e ADO.NET"
url: "datetime.kind-e-ado.net"
date: 2012-01-10T12:08:17.0000000Z
lastmod: 2012-01-10T12:13:39.0000000Z
tags: ["Imported"]
draft: false
---
<p>Nell'ambito della realizzazione di un sistema cloud based è necessario prestare particolare attenzione alla gestione dei DateTime. Se, infatti, nel deployment di soluzioni on premise è possibile controllare (non sempre, ma spesso si) le impostazioni relative al timezone del server e del client, quando un'applicazione gira dei datacenter di Microsoft, tale controllo non c'è. Potete chiaramente ben intuire quale problema si ponga nel momento in cui il timezone del client sia diverso da quello del server: poiché ADO.NET, quando recupera un DateTime dal database, di default gli imposta la <a href="http://msdn.microsoft.com/en-us/library/system.datetime.kind.aspx" target="_blank" title="MSDN DateTime.Kind">proprietà Kind</a> su Unspecified che, in sostanza, viene trattato lato client come se si trattasse di un valore locale, supponendo che il valore sia stato generato lato server con la chiamata DateTime.Now, capite bene che il rischio di ritrovarsi sul client date locali del server è molto elevato. Per risolvere questo problema esistono vari approcci, ma quello che preferisco è il seguente:</p>
<ul>
<li>Lato server è necessario bandire la chiamata DateTime.Now ed utilizzare piuttosto <a href="http://msdn.microsoft.com/en-us/library/system.datetime.utcnow.aspx" title="MSDN: DateTime.UtcNow property" target="_blank">DateTime.UtcNow</a></li>
<li style="text-align: left;">Lato server, nel momento in cui si recuperano i dati, è necessario assicurarsi che le date siano DateTime con la proprietà Kind impostata su DateTimeKind.Utc. Se si utilizzano le care vecchie DataTable, vi renderete conto che queste vi restituiscono sempre DateTime con Kind impostato un Unspecified. Per risolvere questo problema è necessario impostare la proprietà <a href="http://msdn.microsoft.com/en-us/library/system.data.datacolumn.datetimemode.aspx" target="_blank">DataColumn.DateTimeMode</a> su DataSetDataTime.Utc</li>
<li style="text-align: left;">Lato client, prima di visualizzare o inserire i dati, sarà necessario convertirli da e verso il formato Utc nel formato locale. Il tutto può essere fatto mediante converter in WPF/Silverlight e con apposite formattazioni in ASP.NET</li>
</ul>
<p></p>