 (varmuuskopion vaults<properties
   pageTitle = "Azure Backup limits table"
   kuvaus = "kuvataan järjestelmän rajoitukset Azure Backup."
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />
<tags
   ms.service="backup"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/05/2015"
   ms.author="trinadhk";"jimpark"; "aashishr" />


Seuraavat rajoitukset koskevat Azure varmuuskopion.

| Raja-tunniste | Oletusarvoinen enimmäismäärä |
|---|---|
|Palvelinten/koneet, joita voi rekisteröidä vastaan kunkin säilö määrä|Windows Server/Client/SCDPM 50 <br/> IaaS VMs 200|
|Azure säilö tallennustilaan tallennettuja tietoja tietolähteen muuttaminen|54400 gt enintään<sup>1</sup>|
|Varmuuskopion vaults, joka voidaan luoda jokaisen Azure tilauksen määrä|25 (varmuuskopiointi vaults) <br/> 25 palautus-palveluiden säilö alueittain.|
|Kuinka monta kertaa päivässä voidaan ajoittaa varmuuskopiointi|Windows Server/Client päivässä 3 <br/> 2 SCDPM päivässä <br/> Kerran päivässä IaaS VMs|
|Yhdistetty Azure virtual-konetta varmuuskopion tietojen levyjä|16|

- <sup>1</sup> 54400 Gigatavun rajoitus ei koske IaaS AM varmuuskopion.

