<properties 
    pageTitle="Mitkä ovat logiikan sovelluksia?" 
    description="Lisätietoja palvelun logiikan sovellukset" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="what-are-logic-apps"></a>Mitkä ovat logiikan sovelluksia?

Logiikan sovellusten lisäämistapaa yksinkertaistamaan ja toteuttaa skaalattava integroinnit ja työnkulut pilveen. Se sisältää visual designer malli ja automatisoida nimeltään työnkulun vaiheet sarjana.  Yli cloud ja paikalliseen nopeasti integroiminen palvelut ja protokollat on [monta yhdistimiä](../connectors/apis-list.md) .  Logiikan sovelluksen alkaa käynnistin (like "kun tili on lisätty Dynamics CRM") ja sen jälkeen käynnistämiseen aloittaa monta kombinaatioiden toimintoja, muunnokset ja ehto logiikan.

Logiikan-sovellusten käyttämisestä on hyötyä seuraavilla tavoilla:  

- Säästää aikaa käyttämällä helppoa ymmärtää suunnittelutyökalujen monimutkaista suunnittelemalla
- Käyttöönoton kuvioiden ja työnkulut saumattomasti, jotka muuten voi olla vaikeaa toteuttavien koodi
- Käytön aloittaminen nopeasti malleista
- Logiikan sovelluksen omia mukautettuja API, koodi ja toiminnot mukauttaminen
- Yhdistä ja synchronise erillisiä Systemsin paikallisen ja pilveen
- Muodosta luettelosta BizTalk server, API hallinta, Azure-Funktiot ja Azure palvelun Bus pääosin integrointi tuki

Logiikan sovellukset on täysin hallitun iPaaS (integrointi ympäristö palveluna) salliminen kehittäjien ei tarvitse huolehtia isännöinnin, skaalattavuus, käytettävyys ja hallinta.  Logiikan sovellusten skaalautuvat automaattisesti tarvittaessa mukaiseksi.

![Työnkulku sovelluksen suunnittelu](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Kuten edellä, logiikan sovelluksilla voit automatisoida liiketoimintaprosesseja. Seuraavassa on muutamia esimerkkejä:  
 
* Siirrä tiedostoja ladataan Azure varastoon FTP-palvelin
* Prosessin ja reitin tilaukset yli paikallisen ja cloud järjestelmät
* Valvo kaikki tweets tiettyjen aiheiden, analysoida markkinatunnelma ja luo ilmoituksia ja tehtävien kohteita, jotka on seuranta.

Tilanteita, joissa esimerkiksi seuraavia voi määrittää kaikki visual designer ja yksi tekstirivi koodin kirjoittamatta. Hae aloittaminen [rakentaminen logiikan sovelluksen nyt][create].  Kun kirjoitettu - logiikkaa-sovellus voi olla [nopeasti käyttöön ja uudelleen](app-service-logic-create-deploy-template.md) eri ympäristöissä ja alueiden välillä.

## <a name="why-logic-apps"></a>Miksi logiikan sovelluksia?

Logiikan sovellusten yhdistää nopeuden ja skaalattavuus yrityksen integrointi tilaa.  Suunnittelun helppous erilaisia käytettävissä Käynnistimet ja toiminnot ja tehokas hallintatyökalut Varmista keskittäminen oman ohjelmointirajapinnan yksinkertaisempi kuin koskaan.  Yritysten toiseen digitalization kohti, logiikan sovellusten avulla voit muodostaa vanhojen ja kehittyneet yhdessä.

Lisäksi Microsoftin [Yrityksen integrointi tilin] kanssa[ biztalk] voit skaalata erääntyneiden integrointi skenaarioiden kanssa potenssiin [XML messaging][xml], [kaupankäyntipäivä kumppanin hallinta][tpm], ja paljon muuta.

- **Helppokäyttöinen suunnittelutyökalut** - logiikan sovellukset voivat olla suunniteltu-kattavan selaimessa tai Visual Studio työkaluilla. Aloita käynnistimen kanssa – yksinkertainen aikatauluun-GitHub ongelman luomisen yhteydessä. Valitse orchestrate haluamasi määrän toimintoja yhdistimet RTF-valikoimassa.

- **Yhteyden ohjelmointirajapinnan helposti** -jopa yhdistelmä tehtävät, jotka on helppo kuvaa on vaikea toteuttavien koodi. Logiikan sovellukset on helppo muodostaa erillisiä systems. Haluat muodostaa yhteyttä cloud markkinoinnin oman paikallisen ratkaisun Laskutusosoitteen järjestelmän? Jos haluat keskittää messaging API- ja Enterprise-palvelun Bus on välillä Logiikan sovellukset on nopein ja parhaiten silloin tapa pitää ratkaisut ongelmat.

- **Pääset alkuun nopeasti malleista** - avulla pääset alkuun on määritetty [valikoiman malleja] [ templates] , voit luoda nopeasti joitakin yleisiä ratkaisuja. -Advanced B2B ratkaisuja yksinkertainen SaaS connectivity ja jopa joitakin, jotka ovat vain "hauskaa' - valikoima on nopein tapa aloittaa logiikan sovellusten power.

- **Laajennettavuus baked-** - tarvitset yhdistin ei ole näkyvissä? Logiikan sovellukset on suunniteltu toimimaan oman ohjelmointirajapinnan ja koodi; Voit helposti luoda omia API-sovelluksen käyttää mukautetun yhdistimen tai soittaa [Azure-funktio](https://functions.azure.com) suorittaa koodin tarvittaessa katkelmat. 

- **Todellinen integrointi tehoa** - Käynnistä helposti ja kasvaa, kun tarvitset. Logiikan sovellusten helposti voidaan hyödyntää BizTalk Microsoftin alussa integrointi ratkaisu käyttöön integrointi ammattilaiset voivat luoda ratkaisuja, jotka he tarvitsevat potenssiin. Lisätietoja [Yrityksen Integration Pack](./app-service-logic-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Logiikan App käsitteitä

Seuraavassa on joitakin tärkeimmät osat, jotka muodostavat logiikan sovellukset-toiminto. 

- **Työnkulun** - logiikkaa sovellusten mahdollistaa graafinen malli liiketoimintaprosesseja vaiheet tai työnkulun sarjana.
- **Hallitun yhdysviivat** - logiikkaa sovelluksia on käyttää tietoja ja palveluja. Hallitut yhdistimet luodaan erityisesti tuki, kun yhteyden ja tietojen käsitteleminen. Näet luettelon yhdistimiä, jotka ovat nyt käytettävissä [hallitun yhdistimien][managedapis].
- **Käynnistimien** - jotkin hallitun yhdistimet voi myös toimia käynnistintä. Käynnistimen käynnistyy tietyn tapahtuman, kuten sähköpostiviestiin tai Azure-tallennustilan tilin muutoksen saapumisesta perustuvalle uuden esiintymän.
-  **Toiminnot** - jokaisen vaiheen jälkeen työnkulun käynnistäminen kutsutaan toiminnon. Kutakin yhdistää yleensä toiminnon hallitun yhdistimen tai mukautetun API-sovellukset.
- **Yrityksen Integration Pack** - monimutkaisemman integrointi tilanteessa, logiikan sovellukset sisältyy BizTalk mahdollisuudet. BizTalk on Microsoftin alan johtava integrointi ympäristö. Yrityksen Integration Pack yhdistimet mahdollistavat ovat helposti kelpoisuuden, muunnos ja lisää-logiikkaa App työnkulkuja.

## <a name="getting-started"></a>Käytön aloittaminen  

- Aloita logiikan sovelluksia, noudata [logiikan sovelluksen luominen] [ create] opetusohjelma.  
- [Näytä yleisiä esimerkkejä ja skenaariot](app-service-logic-examples-and-scenarios.md)
- [Voit automatisoida liiketoimintaprosesseja logiikan sovelluksilla](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Opettele järjestelmien integroida logiikan sovellukset](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: app-service-logic-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-enterprise-integration-accounts.md
[xml]: app-service-logic-enterprise-integration-b2b.md
[templates]: app-service-logic-use-logic-app-templates.md
