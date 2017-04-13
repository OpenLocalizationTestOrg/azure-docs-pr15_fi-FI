<properties
   pageTitle="Vikasietoisuudelle tekniset ohjeet indeksi | Microsoft Azure"
   description="Teknisiä tietoja ja suunnittelemisesta joustavat, helposti saatavilla, vikasietoinen sovellusten sekä tietojen palauttaminen ja liiketoiminnan jatkuvuuden suunnitteleminen artikkeleita indeksointi"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance"></a>Azure vikasietoisuudelle tekniset ohjeet

##<a name="introduction"></a>Johdanto

Suuri käytettävyys ja tietojen palauttaminen vaatimukset täyttävät on kahdentyyppisiä knowledge:

- Yksityiskohtainen teknistä tietoa cloud-ympäristössä ominaisuuksia
- Tiedot siitä, miten arkkitehti oikein hajautettu palvelu

Tämä artikkelisarja kattaa ensiksi: ominaisuudet ja rajoitukset Azure ympäristön, jotka koskevat vikasietoisuudelle (kutsutaan joskus Liiketoiminnan jatkuvuus). Jos olet kiinnostunut viimeksi, katso kohdistettu [palauttaminen ja suuren käytettävyyden Azure sovellusten](https://aka.ms/drtechguide)artikkelissa sarjan. Vaikka tämän artikkelin sarjan koskettaa-arkkitehtuuri-ja rakenne-, joka ei ole kohdistuksen sarjan. Rakenne-ohjeita Ota yhteyttä materiaalista [lisäresursseja](#additional-resources) -osassa.

Tiedot on järjestetty on seuraavissa artikkeleissa:

- [Paikalliset virheet palauttamisesta](resiliency-technical-guidance-recovery-local-failures.md).
Fyysinen laitteisto (esimerkiksi asemista, palvelinten ja verkkolaitteiden) voi epäonnistua. Resursseja on täyttynyt Kun kuormituksen piikkejä. Tässä artikkelissa kuvataan ominaisuuksista, joita Azure tarjoaa pitämään suuren käytettävyyden näissä tilanteissa.

- [Azure alueen laajuinen keskeytetty palauttamisesta](resiliency-technical-guidance-recovery-loss-azure-region.md).
Juuri virheet eivät harvinaisissa, mutta ne ovat teoriassa mahdollista. Koko alueet voivat muuttua erillään verkon virheiden vuoksi tai ne voivat olla fyysisesti vioittunut luonnonmullistuksilta. Tässä artikkelissa kerrotaan, miten Azure avulla voit luoda sovelluksia, jotka ulottuvat maantieteellisesti monipuolisen alueiden.

- [Palauttamisesta paikalliseen Azure avulla](resiliency-technical-guidance-recovery-on-premises-azure.md).
Pilveen merkittävästi muuttaa rakennusmenetelmän palauttaminen ottaminen käyttöön organisaatiot voivat käyttää Azure muodostaa toisen sivuston palauttamista varten. Voit tehdä tämän, luominen ja ylläpito toissijainen palvelinkeskuksen kustannusten murtoluku. Tässä artikkelissa kerrotaan ominaisuuksista, joita Azure tarjoaa laajentaminen paikallisen-palvelinkeskuksen pilveen.

- [Palautus tietovirheitä tai vahingossa](resiliency-technical-guidance-recovery-data-corruption.md).
Sovellusten voi olla virheet viallinen tiedot. Operaattorien virheellisesti poistaa tärkeitä tietoja. Tässä artikkelissa kerrotaan, mitä Azure tarjoaa tietojen varmuuskopiointia ja palauttamista aiempaan sitä kerran.

##<a name="additional-resources"></a>Lisäresursseja

- [Tietojen palauttaminen ja suuren käytettävyyden sovellusten Microsoft Azure rakennettu](resiliency-disaster-recovery-high-availability-azure-applications.md).
Tässä artikkelissa on yksityiskohtaiset yleiskatsaus käytettävyys ja palauttaminen. Se peittää manuaalinen replikoinnin viittaus ja tapahtumatietojen todennus. Lopullinen osissa yhteenvetoja erityyppisiä tietojen palauttaminen topologioissa, jotka ulottuvat Azure alueiden käytettävyys ylimmällä tasolla.

- [Suuren käytettävyyden tarkistusluettelon](resiliency-high-availability-checklist.md).
Tässä artikkelissa on luettelo ominaisuuksista, palvelujen ja rakenteita, joiden avulla voit parantaa vikasietoisuudesta ja sovelluksen käytettävyyttä.

- [Microsoft Azure palvelun vikasietoisuudelle ohjeita](resiliency-service-guidance-index.md).
Tässä artikkelissa on indeksi Azure services ja linkit tietojen palauttaminen ohjeet ja rakenne-ohjeet.

- [Yleiskatsaus: Cloud liiketoiminnan jatkuvuuden ja tietokannan tietojen palauttaminen SQL-tietokannan](../sql-database/sql-database-business-continuity.md).
Tässä artikkelissa Azure SQL-tietokanta-tekniikoiden käytettävyyden. Se ensisijaisesti keskittää varmuuskopiointi ja palauttaminen. Jos käytät omaa pilvipalvelussa Azure SQL-tietokanta, tarkista tässä asiakirjassa ja sen liittyvät resurssit.

- [Suuri käytettävyys ja SQL Server Azuren näennäiskoneiden palauttaminen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).
Tässä artikkelissa käsitellään käytettävyys-asetuksia, jotka voit tutkia, kun käytät infrastruktuurin serviceksi (IaaS) isännöimiseen tietokannan palvelujen. Tässä artikkelissa kerrotaan, AlwaysOn käytettävyys ryhmät, tietokantapeilausta, toimitus log ja varmuuskopiointi ja palauttaminen. Useita opetusohjelmat näyttää, miten voit käyttää seuraavia tekniikoita.

- [Parhaita käytäntöjä suunnitteluun suurissa Azure Cloud Services-palvelut](https://azure.microsoft.com//blog/best-practices-for-designing-large-scale-services-on-windows-azure/).
Tässä artikkelissa keskitytään kehittäminen erittäin skaalattava cloud arkkitehtuureihin. Monia tekniikoita, joiden avulla voit parantaa skaalattavuus käyttää myös parantaa käytettävyyttä. Myös, jos sovellus et voi skaalata parantavat kuormitus, skaalattavuus tulee käytettävyys ongelma.

- [Varmuuskopiointi- ja SQL Server Azuren näennäiskoneiden](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
Tässä artikkelissa on teknisiä ohjeita siitä, miten voit varmuuskopioida ja palauttaa Microsoft SQL Server-Azuren näennäiskoneiden käynnissä.

- [Failsafe: joustavat cloud arkkitehtuureihin ohjeet](https://channel9.msdn.com/Series/FailSafe).
Tässä artikkelissa on ohjeita etsimisen joustavat cloud arkkitehtuureihin täytäntöön näiden arkkitehtuureihin Microsoft-tekniikoiden ja reseptit toteuttamisesta, tietyissä skenaarioissa nämä arkkitehtuureihin ohjeita.

- [Tekninen tapaustutkimus: cloud technologies avulla voit parantaa tietojen palauttaminen](https://www.microsoft.com/itshowcase/Article/Content/737/Using-cloud-technologies-to-improve-disaster-recovery).
Tämä tapaustutkimus näyttää, miten Microsoftin IT käyttää Azure palauttaminen parantamiseksi.

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on kohdistettu tekniset ohjeet Azure vikasietoisuudelle sarjaan kuuluvan. Jos haluat muuttaa muita artikkeleita samasta lukunäkymässä, voit aloittaa [Paikalliset virheet palauttamisesta](resiliency-technical-guidance-recovery-local-failures.md)kanssa.
