<properties
    pageTitle="Toimintaohjeet silloin, kun Azure keskeytetty vaikuttaa Azuren näennäiskoneiden | Microsoft Azure"
    description="Noudata näitä ohjeita silloin, kun Azure keskeytetty vaikuttaa Azure-virtuaalikoneissa."
    services="virtual-machines"
    documentationCenter=""
    authors="kmouss"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="virtual-machines"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-virtual-machines"></a>Toimintaohjeet silloin, kun Azure keskeytetty vaikuttaa Azure-virtuaalikoneissa

Microsoftilla on toimivat kiintolevyn ja varmista, että palveluiden ovat aina käytettävissäsi, kun tarvitset niitä. Pakottaa lisäksi ohjausobjektille joskus vaikuttaa us määrittäminen tavalla, joka aiheuttaa suunnittelematon palvelu keskeytyksiä.

Microsoft toimittaa palvelussa palvelutasosopimusta (SLA) sen palveluita kuin sitoumusta käytettävyyttä ja yhteys. Yksittäisten Azure palveluiden SLA tukikäytännöistä [Azure palvelun tason sopimuksia](https://azure.microsoft.com/support/legal/sla/).

Azure on jo useita valmiin ympäristö-ominaisuuksia, jotka tukevat erittäin käytettävissä olevat sovellukset. Lisätietoja näiden palvelujen lukea [palauttaminen ja suuren käytettävyyden Azure sovellusten](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Tässä artikkelissa käsitellään tilanne tosi tietojen palauttaminen, kun koko alueelta ilmenee käyttökatkosta pää luonnollinen tietojen tai juuri palvelukatkos vuoksi. Nämä ovat harvinaisissa esiintymät, mutta sinun täytyy laatia olevan käyttökatkosta koko alueen mahdollisuudesta. Jos koko aluetta ilmenee keskeytetty, tietojen paikallisesti tarpeettomat kopioita tilapäisesti olisi ei käytettävissä. Jos olet ottanut geo replikoinnin, kolme kopioita Azuren tallennustilaan BLOB-objektit ja taulukot on tallennettu toisella alueella. Valmis alueellisen käyttökatkosta tai tietojen, jossa ensisijaisen alue ei voi palauttaa Azure määrittää tehdyt uudelleen kaikki DNS-arvojen geo replikoida-alueelle.

>[AZURE.NOTE]Huomaa, että sinulla ei ole mitään päättää, tämä toimenpide ja se tapahtuu vain alueen laajuinen palveluiden keskeytyksiä. Tästä syystä luottaa myös muita sovelluksen kielikohtaiset varmuuskopion strategioita saavuttamiseksi parhaan mahdollisen käytettävyys. Lisätietoja on kohdassa [tietojen palauttaminen strategioita](../resiliency/resiliency-disaster-recovery-azure-applications.md#data-strategies-for-disaster-recovery)käyttöön.

Avulla voit käsitellä nämä harvinaisissa esiintymien annamme Azuren näennäiskoneiden kyseessä palvelun häiriöitä koko alue, jossa Azure virtuaalikoneen-sovellus otetaan käyttöön seuraavat ohjeet.

##<a name="option-1-wait-for-recovery"></a>Vaihtoehto 1: Odota palauttaminen
Tässä tapauksessa osaltasi mitään toimia ei tarvita. Tiedät, että Yritämme apuväline luotaessa joukkopostituksia palauttaa palvelun saatavuus. Näet palvelun tilan Microsoftin [Azure palvelun kunnon koontinäyttö](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Tämä on paras vaihtoehto, jos ei ole määritetty Azure palauttaminen, virtuaalikoneen varmuuskopion, lukuoikeus geo tarpeettomat tallennustilan tai geo tarpeettomat tallennustilan ennen häiriöt. Jos olet määrittänyt geo tarpeettomat tallennus- tai lukuoikeudet tallennustilan tilin geo tarpeettomat tallennustilan lisääminen AM virtual kiintolevyillä (näennäiskiintolevyjen) tallennuspaikkaa, voit palauttaa kuvan Näennäiskiintolevyn kohde ja yritä valmistella uuden AM siitä. Tämä ei ole haluamallasi tavalla, koska ei ole tietojen synkronoiminen oikeudet paitsi Azure AM varmuuskopiointi tai Azure palauttaminen käytetään. Näin ollen tämä asetus on ei ehkä toimi.

Asiakkaat, jotka haluat näennäiskoneiden heti käyttöösi seuraavat vaihtoehdot ovat käytettävissä.  

>[AZURE.NOTE]Huomaa, että molemmat seuraavista asetuksista on vahinkojen kadota tietoja.     

##<a name="option-2-restore-a-vm-from-a-backup"></a>Vaihtoehto 2: Palauttaminen varmuuskopiosta AM
Asiakkaat, jotka on määritetty AM varmuuskopion voit palauttaa AM sen varmuuskopiointi- ja pisteestä.

Palauttaa uuden AM Azure varmuuskopiosta, on artikkelissa [palauttaa näennäiskoneiden Azure-tietokannassa](../backup/backup-azure-restore-vms.md).

Auttaa Azuren näennäiskoneiden varmuuskopion infrastruktuurin suunnitteleminen, katso [Azure AM varmuuskopio-infrastruktuuria](../backup/backup-azure-vms-introduction.md).

##<a name="option-3-initiate-a-failover-by-using-azure-site-recovery"></a>Vaihtoehto 3: Aloittaa automaattisesti käyttämällä Azure sivuston palauttaminen
Jos olet määrittänyt Azure palauttaminen sellaisiin Azuren näennäiskoneiden käyttöä varten, voit palauttaa käyttäjän VMs niiden. Nämä replikat voivat sijaita Azure tai paikallinen. Tässä tapauksessa voit luoda uuden AM sen aiemmin replikasta. Palauttaa oman VMs Azure palauttaminen replikasta, on artikkelissa [Azure IaaS siirtää näennäiskoneiden Azure alueet, joiden Azure palauttaminen välillä](../site-recovery/site-recovery-migrate-azure-to-azure.md).

>[AZURE.NOTE]Vaikka Azure virtuaalikoneen käyttöjärjestelmä ja tietojen levyjen replikoida toissijainen SITEN, jos ne eivät geo ylimääräinen tai lukuoikeudet geo tarpeettomat tallennustilan tilin, kunkin Näennäiskiintolevyn replikoida erikseen. Tämä taso replikoinnin ei takaa yhdenmukaisuuden replikoitua näennäiskiintolevyjen yli. Jos sovellus ja/tai tietokannoissa, jotka käyttävät näitä tietoja levyjen riippuvuuksia toisiinsa, on ei välttämättä kaikissa näennäiskiintolevyjen, replikoida yhden tilannevedoksena. Se myös ei välttämättä, että se Näennäiskiintolevyn geo ylimääräinen tai lukuoikeudet geo tarpeettomat tallennustilan johtaa-sovelluksen yhdenmukaisia tilannevedoksen käynnistys AM.

##<a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja palauttaminen ja suuren käytettävyyden strategiakartta ottamisesta käyttöön on artikkelissa [tietojen palauttaminen ja suuren käytettävyyden Azure sovelluksille](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Voit tehdä yksityiskohtaisia teknisiä tietoja cloud-ympäristössä ominaisuuksia, katso [Azure vikasietoisuudelle teknisiä ohjeita](../resiliency/resiliency-technical-guidance.md).

Lisätietoja VMs varmuuskopioiminen on artikkelissa [Azure-virtuaalikoneissa varmuuskopioida](../backup/backup-azure-vms.md).

Opettele käyttämään Azure palauttaminen orchestrate ja automatisoida oman fyysinen (ja virtual) Windows- ja Linux koneet, joka suorittaa VMWare ja Hyper-V VMs on artikkelissa [Azure palauttaminen](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)suojaus.

Jos ohjeet ovat ei poista tai jos haluat suorittaa puolestasi toimenpiteet Microsoftille, ota yhteyttä [Asiakastukeen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
