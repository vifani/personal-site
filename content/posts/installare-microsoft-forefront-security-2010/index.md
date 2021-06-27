---
title: "Installare Microsoft Forefront Security 2010"
url: "52/installare-microsoft-forefront-security-2010"
date: 2011-01-25T20:04:00.0000000Z
lastmod: 2011-01-25T20:04:00.0000000Z
tags: ["Imported"]
draft: false
---
<p>
	Anche se da sempre dal punto di vista professionale ho lavorato nel mondo dello sviluppo software, non ho mai disprezzato la controparte sistemistica. Anzi, come ogni buon informatico che si rispetti, ho la mia cerchia di amici e parenti che sovente mi chiedono aiuto e consigli. Qualche volta mi capita anche di assemblare qualche PC, un'attività che trovo sempre interessante perché mi consente di mantenermi al passo con i tempi anche dal punto di vista hardware.</p>
<p>
	E' capitato, tuttavia, che nella mia azienda, in questi giorni si è deciso di affidare la sicurezza al nuovo prodotto Microsoft Forefront Security 2010. Molti potrebbero pensare che quest'attività sia semplice e banale, magari basata su un classico setup del tipo "Avanti, Avanti, Avanti.... Fine". Beh ho potuto constatare con mano che non sempre è così e che il lavoro del sistemista sia un lavoro che meriti moltissimo rispetto, se non altro per l'enorme pazienza di cui bisogna armarsi quando ci si agginge a fare un'operazione come quella che ho fatto io.</p>
<p style="text-align: center">
	<img alt="" src="/public/image/forefront.jpg" style="border-bottom: 1px solid; border-left: 1px solid; width: 400px; height: 345px; border-top: 1px solid; border-right: 1px solid" /></p>
<p>
	Per installare Forefront Security 2010 è necessario, infatti, installare prima Windows Update Services e System Center Configuration Manager, due strumenti che consentono di centralizzare gli aggiornamenti di Windows all'interno di un dominio e di gestire tutta una serie di operazioni tra le quali, la più importante per chi, come me, voleva installare Forefront Security 2010, deployare sulle macchine appartenenti al dominio un'applicazione.</p>
<p>
	Inutile dirvi che c'è da perdersi nell'elenco di prerequisiti dei prerequisiti che sono necessari, oltre a tutta una serie di operazioni più o meno ufficiali, ma di fondamentale importanza al fine di raggiungere il risultato atteso.</p>
<p>
	Morale della favola: dopo tre giorni di duro lavoro tra Group Policy, aggiornamenti, impostazioni varie, regole del firewall e quant'altro, e dopo che un mio carissimo collega, in un attimo di smarrimento, mi ha ricordato con una domanda retorica ("ma non doveve installare solo un antivirus?") quale era il mio obiettivo, sono riuscito a raggiungerlo :-)</p>
<p>
	La più grande soddisfazione l'ho ottenuta a seguito della prima scansione schedulata sui client: due virus trovati, uno dei quali pensavamo di essere riusciti a debellare completamente nel corso dell'estate :-D</p>
