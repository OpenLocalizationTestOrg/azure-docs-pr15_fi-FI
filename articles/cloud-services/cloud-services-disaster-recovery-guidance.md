<properties
    pageTitle="Mitä tehdä, jos Azure palvelun häiriöitä, jotka vaikuttaa Azure pilvipalveluihin | Microsoft Azure"
    description="Katso, mitä tehdään, jos Azure keskeytetty, jotka vaikuttaa Azure pilvipalveluihin."
    services="cloud-services"
    documentationCenter=""
    authors="kmouss"
    manager="drewm"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="cloud-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Mitä tehdä, jos Azure palvelun häiriöitä, jotka vaikuttaa Azure pilvipalveluihin

Microsoftilla on toimivat kiintolevyn ja varmista, että palveluiden ovat aina käytettävissäsi, kun tarvitset niitä. Pakottaa lisäksi ohjausobjektille joskus vaikuttaa us määrittäminen tavalla, joka aiheuttaa suunnittelematon palvelu keskeytyksiä.

Microsoft toimittaa palvelussa palvelutasosopimusta (SLA) sen palveluita kuin sitoumusta käytettävyyttä ja yhteys. Yksittäisten Azure palveluiden SLA tukikäytännöistä [Azure palvelun tason sopimuksia](https://azure.microsoft.com/support/legal/sla/).

Azure on jo useita valmiin ympäristö-ominaisuuksia, jotka tukevat erittäin käytettävissä olevat sovellukset. Lisätietoja näiden palvelujen lukea [palauttaminen ja suuren käytettävyyden Azure sovellusten](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Tässä artikkelissa käsitellään tilanne tosi tietojen palauttaminen, kun koko alueelta ilmenee käyttökatkosta pää luonnollinen tietojen tai juuri palvelukatkos vuoksi. Nämä ovat harvinaisissa esiintymät, mutta sinun täytyy laatia olevan käyttökatkosta koko alueen mahdollisuudesta. Jos koko aluetta ilmenee keskeytetty, tietojen paikallisesti tarpeettomat kopioita tilapäisesti olisi ei käytettävissä. Jos olet ottanut geo replikoinnin, kolme kopioita Azuren tallennustilaan BLOB-objektit ja taulukot on tallennettu toisella alueella. Valmis alueellisen käyttökatkosta tai tietojen, jossa ensisijaisen alue ei voi palauttaa Azure määrittää tehdyt uudelleen kaikki DNS-arvojen geo replikoida-alueelle.

>[AZURE.NOTE]Huomaa, että sinulla ei ole mitään päättää, tämä toimenpide ja se tapahtuu vain palvelinkeskuksen laajuinen palveluiden keskeytyksiä. Tästä syystä luottaa myös muita sovelluksen kielikohtaiset varmuuskopion strategioita saavuttamiseksi parhaan mahdollisen käytettävyys. Lisätietoja on kohdassa Lisätietoja [tietojen palauttaminen strategioita](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md#DSDR). Jos haluat voivat vaikuttaa oman automaattisesti, haluat ehkä harkita [lukuoikeudet geo tarpeettomat storage (RA GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage), joka luo toisen alueen tietojen vain luku-kopion.

Avulla voit käsitellä nämä harvinaisissa esiintymien annamme Azure-virtuaalikoneissa (VMs) seuraavat ohjeet kyseessä keskeytetty koko alueen, jossa Azure AM-sovellus otetaan käyttöön.

##<a name="option-1-wait-for-recovery"></a>Vaihtoehto 1: Odota palauttaminen
Tässä tapauksessa osaltasi mitään toimia ei tarvita. Tiedät, että Azure ryhmiä toimivat apuväline luotaessa joukkopostituksia palauttaa palvelun saatavuus. Näet palvelun tilan Microsoftin [Azure palvelun kunnon koontinäyttö](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Tämä on paras vaihtoehto, jos asiakas ei ole määrittänyt Azure palauttaminen tai on toissijainen käyttöönoton toisella alueella.

Asiakkaat, jotka haluat niiden käyttöön pilvipalveluihin heti käyttöösi seuraavat asetukset ovat käytettävissä.

>[AZURE.NOTE]Huomaa, että nämä asetukset on vahinkojen kadota tietoja.     

##<a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>Vaihtoehto 2: Uudelleen käyttöön cloud palvelun määrityksestä, uusi alue

Jos sinulla on alkuperäinen koodi, voit jättää vain käyttöön sovelluksen, liittyvät määritykset ja niihin liittyvät resurssit uusi pilvipalveluun uuden alueen.  

Tarkempia tietoja luominen ja käyttöönotto cloud palvelusovellus, katso, [miten voit luoda ja ottaa käyttöön pilvipalveluun](./cloud-services-how-to-create-deploy-portal.md).

Sovelluksen tietolähteistä, mukaan joudut ehkä etsimään sovelluksen tietolähteen palautustoiminnot.
  * Azuren tallennustilaan tietolähteiden artikkelissa [Azure-tallennustilan replikoinnin](../storage/storage-redundancy.md#read-access-geo-redundant-storage) tarkistaa käytettävissä olevat asetukset, valitse replikoinnin mallin sovelluksen.
  * SQL-tietokantaan lähteistä, lue [yleiskatsaus: Cloud liiketoiminnan jatkuvuuden ja tietokannan tietojen palauttaminen SQL-tietokannan](../sql-database/sql-database-business-continuity.md) tarkistaa käytettävissä olevat vaihtoehdot perusteella sovelluksen valitsemasi replikoinnin-malli.

##<a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>Vaihtoehto 3: Käytä varmuuskopion käyttöönoton kautta Azure liikenteen hallinta
Tämä vaihtoehto olettaa on jo suunnitellut sovelluksen ratkaisu alueellisen palauttaminen mielessä kanssa. Voit käyttää tämä vaihtoehto, jos sinulla on jo toissijainen cloud services sovellusten käyttöönoton, joka toimii toisella alueella ja yhdistetty liikenteen hallinta kanavan kautta. Tässä tapauksessa toissijainen käyttöönoton kuntotietojen tarkistus. Jos se on kunnossa, voit ohjata liikenne sitä kautta Azure liikenteen hallinta. Tämä strategia kanssa voit hyödyntää liikenne reititys menetelmästä ja automaattisesti tilauksen käyttömahdollisuudet Azure liikenteen hallinta. Lisätietoja on artikkelissa [liikenteen hallinta-asetusten määrittäminen](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings).

![Tasaamisen Azure pilvipalveluihin eri alueilla Azure liikenteen hallinta](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##<a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja palauttaminen ja suuren käytettävyyden strategiakartta ottamisesta käyttöön on artikkelissa [tietojen palauttaminen ja suuren käytettävyyden Azure sovellusten](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Voit tehdä yksityiskohtaisia teknisiä tietoja cloud-ympäristössä ominaisuuksia, katso [Azure vikasietoisuudelle teknisiä ohjeita](../resiliency/resiliency-technical-guidance.md).

Jos ohjeet ovat ei poista tai jos haluat suorittaa puolestasi toimenpiteet Microsoftille, ota yhteyttä [Asiakastukeen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
