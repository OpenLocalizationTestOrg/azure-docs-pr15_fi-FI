<properties 
   pageTitle="Testaus pilvipalveluun suorituskyvyn | Microsoft Azure"
   description="Testaa pilvipalveluun käyttämällä Visual Studio Profilerin suorituskyky"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="testing-the-performance-of-a-cloud-service"></a>Pilvipalveluun suorituskyvyn testaaminen 

##<a name="overview"></a>Yleiskatsaus

Voit testata pilvipalveluun suorituskyvyn seuraavilla tavoilla:

- Käytä Azure diagnostiikka kerää tietoja pyyntöjä ja yhteydet ja tarkistamaan sivuston tilastotiedot, jotka osoittavat, miten palvelun suorittaa asiakkaan näkökulmasta. Käytön aloittaminen-artikkelissa [Azure pilvipalveluihin ja näennäiskoneiden diagnostiikan asetusten määrittäminen]( http://go.microsoft.com/fwlink/p/?LinkId=623009).

- Saat tarkempia analyysi laskennallinen näkökohdat palvelu-ohjelman suorittamistavan Visual Studio Profilerin avulla. Kun tässä ohjeaiheessa kerrotaan, voit käyttää profiloija mitataan suorituskyvyn, kun palvelua käytetään Azure. Tietoja mitata suorituskykyä, kun palvelua käytetään paikallisesti Laske emulaattorin profiloija avulla on artikkelissa [testaus Azure Cloud palvelun paikallisesti Laske emulaattorin käyttämällä suorituskyvyn Visual Studio Profilerin](http://go.microsoft.com/fwlink/p/?LinkId=262845).



## <a name="choosing-a-performance-testing-method"></a>Suorituskyvyn, testaaminen menetelmän valitseminen

###<a name="use-azure-diagnostics-to-collect"></a>Azure Diagnostiikan avulla voit kerätä:###

- Verkkosivujen tai palvelut, kuten pyynnöt ja yhteydet tilastotiedot.

- Roolit, esimerkiksi kuinka usein rooli käynnistetään tilastoja.

- Yleinen käynnissä roolin määrittäminen muistinkäytön, kuten aika, joka kestää muistista tai muistin prosenttiosuus tietoja.

###<a name="use-the-visual-studio-profiler-to"></a>Käytä Visual Studio profiloija avulla:###

- Selvitä, joka toimii kestää eniten aikaa.

- Mittaa, kuinka paljon aikaa laskennallisesti tehostettu ohjelman jokaisen osan.

- Vertaa kahden version palvelu yksityiskohtaiset suorituskykyraporttien.

- Analysoi muistin varaamista tarkemmin kuin yksittäisten muistin kohdistukset taso.

- Analysoi samanaikainen ongelmia monisäikeisten koodi.

Kun käytät profiloija, voit kerätä tietoja suoritettaessa pilvipalveluun paikallisesti tai Azure-tietokannassa.

###<a name="collect-profiling-data-locally-to"></a>Kerää profiloinnin tietoja paikallisesti avulla:###

- Testaa pilvipalveluun, kuten tietyn työntekijän rooli, joka ei edellytä realistisia Simuloitu kuormituksen suorittamisen osan suorituskykyä.

- Testaa pilvipalveluun suorituskyvyn yksinään valvottu olosuhteissa.

- Testaa pilvipalveluun suorituskyvyn, ennen kuin otat sen Azure.

- Testaa pilvipalveluun suorituskyvyn yksityisesti liikuttamatta olemassa olevissa käyttöympäristöissä.

- Testaa palvelun suorituskyvyn ilman lisäkustannuksia Azure suorittamista varten.

###<a name="collect-profiling-data-in-azure-to"></a>Profiloinnin Azure-tietokannassa tietojen kerääminen:###

- Testaa pilvipalvelussa simuloidusta tai todellisiin kuormitus suorituskykyä.

- Käytä profiloinnin tietojen keräämisen instrumentation menetelmää kuin tässä ohjeaiheessa kerrotaan myöhemmin.

- Testaa palvelun suorituskyvyn siinä muodossa, kun palvelua käytetään tuotannon-ympäristössä.

Tavalliseen tapaan kuormituksen testi pilvipalveluihin normaalilla tai kuormitus ehtoja.

## <a name="profiling-a-cloud-service-in-azure"></a>Profilointi pilvipalvelussa Azure-tietokannassa

Kun julkaiset Visual Studio cloud-palvelussa, voit profiilin palvelu ja profiloinnin asetuksia, jotka antavat tietoja, jotka haluat määrittää. Profiloinnin istunto on käynnistetty jokaisen esiintymän rooli. Lisätietoja siitä, miten voit julkaista palvelun Visual Studio on artikkelissa [Azure-pilvipalvelussa Visual Studio julkaisun](https://msdn.microsoft.com/library/azure/ee460772.aspx).

Lisätietoja suorituskyvyn profilointi Visual Studiossa on artikkelissa [Aloittelijoille opas suorituskyvyn profilointi](https://msdn.microsoft.com/library/azure/ms182372.aspx) ja [Sovelluksen suorituskyvyn analysoiminen käyttämällä profilointi työkalujen](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

>[AZURE.NOTE] Voit ottaa IntelliTrace tai kun julkaiset pilvipalvelussa sijaitsevaan profilointi. Et voi ottaa käyttöön molemmat.

###<a name="profiler-collection-methods"></a>Profilerin sivustokokoelman menetelmät

Voit käyttää sivustokokoelman eri tavoista, profilointi, suorituskykyongelmia perusteella:

- **Suorittimen esimerkkejä** – Tämä menetelmä kerää tilastoja, joka sopii ensimmäisen analyysin suorittimen käyttö ongelmista. Suorittimen esimerkkejä on ehdotettu aloittamisen useimmat suorituskyvyn tutkimuksia. Valitse sovellus, jossa on profilointi kun suorittimen esimerkkejä tietojen kerääminen on pieni vaikutus.

- **Instrumentation** – Tämä menetelmä kerää yksityiskohtaisia Aikatietojen, joka on hyödyllinen aktiivisen analyysia varten ja analysoiminen i/suorituskykyongelmia. Instrumentation menetelmä tietueiden moduulissa jokaisen merkinnän, Lopeta ja funktiokutsun on aikana profilointi Suorita. Tätä menetelmää kannattaa käyttää yksityiskohtaiset ajoitus keräämisessä koodin osan tietoja ja tietoja syöttö- ja toimintojen sovelluksen suorituskykyyn vaikuttavat. Tämä menetelmä on poistettu käytöstä tietokoneessa, jossa on 32-bittinen käyttöjärjestelmä. Tämä asetus on käytettävissä vain, kun ohjaamiseen pilvipalvelussa Azure-tietokannassa, ei paikallisesti Laske emulaattori.

- **.NET muistin varaamista** : Tämä menetelmä kerää .NET Framework muistin kohdistus tietoja käyttämällä profilointi menetelmä esimerkkejä. Kerättyjen tietojen sisältää määrän ja kohdistettu objektien koon.

- **Samanaikainen** – Tämä menetelmä kerää ristiriita resurssitietojen ja prosessi ja viestiketjun suorittamisen tiedot, jotka ovat käteviä analysoiminen Salli muut samanaikaiset ja usean Käsittele sovellukset. Samanaikainen menetelmä kerää tietoja kuhunkin tapahtumaan estää koodin, esimerkiksi kun säikeen odottaa suorittaminen lukittu access-sovelluksen resurssin vapautumista varten. Tätä menetelmää kannattaa käyttää analysointiin Salli muut samanaikaiset sovellukset.

- Voit myös ottaa **Taso vuorovaikutus profilointi**, joka sisältää lisätietoja synkronisen ADO.NET suorittamisen ajat tallenteita usean tasoisen sovelluksia, jotka yhteydenpito yksi tai useampi tietokanta-funktiot. Voit kerätä taso vuorovaikutus tietojen kanssa profiloinnin tavoilla. Lisätietoja taso vuorovaikutus profilointi artikkelissa [Taso vuorovaikutukset tarkasteleminen](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Profiloinnin asetusten määrittäminen

Seuraavassa kuvassa on julkaista Azure-sovellus-valintaikkunassa profiloinnin asetusten määrittämisestä.

![Profilointi asetusten määrittäminen](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

>[AZURE.NOTE] Voit ottaa käyttöön **profiloinnin** -valintaruudun, sinulla on oltava paikallisessa tietokoneessa, jonka avulla voit julkaista pilvipalvelussa sijaitsevaan Profilerin. Oletusarvon mukaan profiloija on asennettu, kun asennat Visual Studio.

### <a name="to-configure-profiling-settings"></a>Profiloinnin asetusten määrittäminen

1. Napsauta ratkaisunhallinnassa Avaa pikavalikko Azure projektin ja valitse sitten **Julkaise**. Yksityiskohtaisia ohjeita siitä, miten voit julkaista pilvipalveluun on artikkelissa [julkaiseminen pilvestä palvelun Azure työkaluilla](http://go.microsoft.com/fwlink/p?LinkId=623012).

1. Valitse **Julkaise Azure-sovellus** -valintaikkunassa on **Lisäasetukset** -välilehti.

1. Ota profilointi käyttöön, valitse **profiloinnin** -valintaruutu.

1. Voit määrittää profiloinnin asetukset, valitsemalla **asetukset** hyperlinkin. Profilointi asetukset-valintaikkuna tulee näkyviin.

1. **Mitä menetelmää profilointi haluat käyttää** valintanappia Valitse tyypin profilointi, sinun on.

1. Taso vuorovaikutus profiloinnin tietojen keräämiseen, valitse **Taso-vuorovaikutus profilointi käyttöön** -valintaruutu.

1. Tallenna asetukset valitsemalla **OK** -painiketta.

    Kun julkaiset tämän sovelluksen, nämä asetukset käytetään kunkin roolin profiloinnin istunnon luominen.

## <a name="viewing-profiling-reports"></a>Profilointi raporttien tarkasteleminen

Profiloinnin istunnon luodaan esiintymissä oman pilvipalvelussa rooli. Profiloinnin raporttisi istunnossa Visual Studio katselemista voit tarkastella palvelimen ikkunan ja valitse sitten Valitse rooli esiintymä Azure Laske-solmu. Voit tarkastella profiloinnin raportin sitten seuraavassa kuvassa esitetyllä tavalla.

![Azuren profiloinnin raportin tarkasteleminen](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Profiloinnin raporttien tarkasteleminen

1. Voit tarkastella palvelimen ikkunan Visual Studiossa valikkorivillä valitsemalla Näytä-palvelimen Explorer.

1. Valitse Laske Azure-solmu ja valitse pilvipalvelussa, jonka valitsit profiili, kun olet julkaissut Visual Studio Azure käyttöönoton solmu.

1. Voit tarkastella esiintymän profiloinnin raportteja, valitse rooli-palvelussa, Avaa pikavalikko tietyn kohteen ja valitse sitten **Näytä profilointi raportti**.

    Raportin .vsp, tiedosto ladataan nyt azuren ja Azure tehtävän Log näkyy latauksen tila. Kun lataus on valmis, profiloinnin raportti näkyy Visual Studio nimeltä editori-välilehden <Role name> _<Instance Number>_ <identifier>.vsp. Raportin yhteenveto tiedot tulevat näkyviin.

1. Näytetään raportin eri näkymiin nykyinen näkymä-luettelossa, valitse haluamasi näkymä tyyppi. Lisätietoja on artikkelissa [Profilointi Työkalut raporttinäkymiä](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Seuraavat vaiheet

[Virheenkorjaus pilvipalveluihin](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Visual Studio Azure pilvipalvelussa julkaiseminen](https://msdn.microsoft.com/library/azure/ee460772.aspx)

