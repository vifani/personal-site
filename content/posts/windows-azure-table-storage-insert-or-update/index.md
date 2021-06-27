---
title: "Windows Azure Table Storage InsertOrUpdate"
url: "windows-azure-table-storage-insert-or-update"
date: 2011-12-14T15:02:57.0000000Z
lastmod: 2011-12-14T21:17:47.0000000Z
tags: ["Imported"]
draft: false
---
<p style="text-align: justify;">E' ormai da diverse settimane che mi cimento nello sviluppo di un sistema destinato a girare nell'infrastruttura Cloud di Microsoft e, in particolare, le tecnologie che sto impiegando sono Windows Azure, SQL Azure, Windows Azure Table Storage, Windows Azure Blob Storage e AppFabric Cache. Una delle più grandi sfide quando si utilizza lo storage di Windows Azure è l'abbattimento del numero di accessi ad esso. Come probabilmente saprete, accedere agli storage di Windows Azure ha un costo calcolato sia sulla base della quantità di spazio occupata, sia sul numero di transazioni, cioè di operazioni di lettura/scrittura.</p>
<p style="text-align: center;"><img alt="" src="http://upload.wikimedia.org/wikipedia/it/thumb/3/3a/Windows_azure.png/320px-Windows_azure.png" width="320" height="60" /></p>
<p style="text-align: justify;">Supponiamo di dover eseguire la classica Insert Or Update di un dato all'interno del Table Storage. Avendo sempre ben in mente che stiamo parlando di un database NoSQL nel quale, quindi, il concetto di transazione non esiste, per implementare questo tipo di operazione è necessario scrivere qualcosa del genere:</p>
<pre class="brush: csharp;">CloudTableClient tableClient = GetTableClient();
TableServiceContext context = tableClient.GetDataServiceContext();
context.IgnoreResourceNotFoundException = true;
var item = context.CreateQuery<cacheentity>(tableName)
           .Where(p => p.PartitionKey == partitionName && p.RowKey == storageKey)
           .FirstOrDefault();
if (item != null) 
{ 
   context.DeleteObject(item); 
} 
context.AddObject(tableName, newItem); 
context.SaveChangesWithRetries(); 
</pre>
<p style="text-align: justify;">Il problema di questo approccio è che il numero di operazioni effettuate è, nel caso peggiore, pari a tre (in realtà si potrebbe fare la delete e la insert in batch, ma non è importante), mentre nel caso migliore è pari a due.</p>
<p>Grazie all'Insert-Or-Update Entity introdotto da qualche mese è possibile ridurre il numero di accessi nel modo seguente:</p>
<pre class="brush: csharp;">CloudTableClient tableClient = GetTableClient();
TableServiceContext context = tableClient.GetDataServiceContext();
context.AttachTo(tableName, newItem);
context.UpdateObject(tableName, newItem);
context.SaveChangesWithRetries(SaveChangesOptions.ReplaceOnUpdate); 
</pre>
<p style="text-align: justify;">Questo codice esegue la Insert o, eventualmente, l'update dell'elemento in tabella grazie all'uso dell'opzione ReplaceOnUpdate. Naturalmente non poteva essere tutto così semplice: se provate quest'ultimo codice in locale vi renderete conto che non funziona. Il problema è che la Insert-Or-Update Entity non è disponibile per il local storage, compreso quindi il vostro storage emulato. Per risolvere questo problema potete affidavi alla propria booleana statica RoleEnvironment.IsEmulated ed implementare, quindi, nel caso in cui sia true la soluzione meno efficiente che però verrà eseguita solo in ambiente di sviluppo, il che è assolutamente accettabile.</p>