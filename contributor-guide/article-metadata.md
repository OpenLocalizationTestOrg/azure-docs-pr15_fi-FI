

#<a name="metadata-for-azure-technical-articles"></a>Azure artikkeleja metatiedot

Azure artikkeleja sisältää kaksi metatietojen osaa - ominaisuudet-osan ja tunnisteet-osassa. Ominaisuudet-osan avulla kaikki sivuston automaatio ja Hakukoneoptimoinnin tiedostot, kun tunnisteet-osan avulla paljon sisäinen sisällön raportointi. Molemmissa osissa on pakollinen.

- [Syntaksi]
- [Käyttö]
- [Määritteet ja ominaisuudet-osan arvoja]
- [Määritteet ja arvot tags-osa]

##<a name="syntax"></a>Syntaksi

Ominaisuudet-osan käyttää seuraavaa syntaksia:

    <properties
       pageTitle="Page title that displays in search results and the browser tab | Microsoft Azure"
       description="Article description that will be displayed on landing pages and in most search results"
       services="service-name"
       documentationCenter="dev-center-name"
       authors="GitHub-alias-of-only-one-author"
       manager="manager-alias"
       editor=""
       tags="optional"
       keywords="For use by SEO champs only. Separate terms with commas. Check with your SEO champ before you change content in this article containing these terms."/>

Tunnisteet-osassa käyttää seuraavaa syntaksia:

    <tags
       ms.service="required"
       ms.devlang="may be required"
       ms.topic="article"
       ms.tgt_pltfrm="may be required"
       ms.workload="na"
       ms.date="mm/dd/yyyy"
       ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

##<a name="usage"></a>Käyttö

- Rakenteen nimeä ja määritteiden nimet ovat kirjainkoko on merkitsevä.
- - <properties> osa on oltava tiedoston ensimmäisen rivin.
- Jätä tyhjän rivin metatietojen jokaisen osan jälkeen ja ennen sivun otsikon varmistaa, että sivun otsikon oikein muunnetaan HTML julkaisuprosessin aikana.

## <a name="attributes-and-values-for-the-properties-section"></a>Määritteet ja ominaisuudet-osan arvoja

![](./media/article-metadata/checkmark-small.png)**pageTitle**: pakollinen; tärkeää Hakukoneoptimoinnin. Tämän määritteen teksti näkyy selaimen-välilehti ja hakutulosten otsikkona. Käytä 55 60 merkkiä välilyönnit mukaan lukien ja myös sivuston tunnus *| Microsoft Azure* (kuin kirjoitit: pystyviivaa tilaa Microsoft Azure välilyönti).  PageTitle saa olla sama kuin H1.

![](./media/article-metadata/checkmark-small.png)**kuvaus**: pakollinen; tärkeää Hakukoneoptimoinnin (asiayhteyden) ja sivuston toimintoja. Kuvaus on oltava vähintään 125 merkkiä pitkä, 155 merkkiä välilyönnit mukaan lukien. Kuvaus aihe sisältöä, jotta asiakkaat tietävät, valitse hakutulosten luettelosta. Arvo on:

- Tämä teksti saattaa näkyä kuvaus tai abstraktit hakutulokset Google-kohdassa.
- Tämä teksti näkyy [artikkelissa indeksi tulokset](https://azure.microsoft.com/documentation/articles/).

![](./media/article-metadata/checkmark-small.png)**palvelujen**: artikkeleihin, joissa käsitellä palvelua varten tarvittavat. Tämän arvon kutsutaan ofter "palvl dynaamisen tietokentän". Luettele kaikki sovellettavan palvelut, erota ne toisistaan puolipisteillä. Ensimmäinen palvelu, luettelo vaikuttavat siirtymis navigointipolun sivun ja vasemmanpuoleisesta siirtymisruudusta, joka on diplayed sivun kanssa.

Artikkeleissa, jotka määrittävät palvelut-arvon ja documentationCenter arvon palvelut-arvo vaikuttavat linkkipolussa. Muita arvoja, jotka luettelo tulee näkyviin tunnisteina julkaistun artikkelin. Arvot:

- Active directory
- Active directory-b2c
- Active directory-ds
- sovelluksen service\api
- API hallinta
- sovelluksen-palvelu
- sovelluksen servic\mobile
- sovelluksen service\web
- sovelluksen service\logic
- sovelluksen yhdyskäytävään
- hakemuksen tiedot
- automaatio
- Azure-portaalissa
- Azure--Resurssienhallinta
- Azure-pino
- Varmuuskopiointi
- erän
- parhaiden käytäntöjen
- BizTalk-palvelut
- Välimuistin
- CDN
- cloud services
- tiedot-luettelo
- tietoja factory
- tiedot-järvi – analysoinnin
- tiedot-järvi-säilö
- devtest testiympäristössä
- DNS
- documentdb
- expressroute
- tapahtuman keskittimet
- hdinsightiin
- IOT-toiminnossa
- avaimen säilö
- kuormituksen
- koneen opiskelun
- Marketplace
- Media-palvelut
- Mobiili-välitys
- Mobiili-palvelut
- Avainhankkeiden monivaiheisen-factor todentaminen
- ilmoitus keskittimet
- toiminnalliset tiedot
- Toimintojen hallinta-ohjelmistopaketti
- powerapps
- palautus hallinta
- Redis.txt välimuisti
- RemoteApp
- Sisältöoikeuksien hallinta
- ajoitus
- haku
- Tietoturvakeskus
- palvelun bus
- palvelun kangasta
- sivuston palauttaminen
- SQL-tietokanta
- SQL--tietovarasto
- SQL-raportointi
- tallennustilan
- tallentaminen
- storsimple
- virta analytics
- liikenteen hallinta
- virtuaalinen koneet
- virtuaalinen verkossa
- Visual-studio online
- VPN-yhdyskäytävän
- sivustot

![](./media/article-metadata/checkmark-small.png)**documentationCenter**: pakollinen keskihajonta keskitettyä artikkeleita parhaat tarjottu keskihajonta center kautta. Määritä yksi keskihajonta keskelle tai kielen, jota käytetään on artikkelissa. Luettelo arvo vaikuttavat siirtymis navigointipolun sivun. Artikkeleissa, jotka määrittävät palvelut-arvon ja documentationCenter arvon palvelut-arvo vaikuttavat linkkipolussa. Arvot:

- **.NET**
- **nodejs**
- **Java**
- **php**
- **Python**
- **Ruby**
- **mobiili**: vanhentunut. Korvaa tietyn mobiilisovellusten ympäristön.
- **iOS**: Verifing tämä uusi arvo
- **Android**: Tarkistetaan tämän uuden arvon
- **Windows**: Tarkistetaan tämän uuden arvon
- **xamarin**: Tarkistetaan tämän uuden arvon

![](./media/article-metadata/checkmark-small.png)**tekijät**: vaaditaan vain yhden arvon. Luettele ensisijainen tekijän tai artikkelissa pk GitHub-tili. Tämän määritteen asemista tekijän nimi julkaistun artikkelin. Luettele huolimatta määrite monikkonimi vain yksi.

![](./media/article-metadata/checkmark-small.png)**hallinta**: vaaditaan, jos olet Microsoft osallistujan. Luettelon sisällön julkaiseminen esimiehen tekniikka alueen sähköpostialias. Jos olet yhteisön osallistujan, Sisällytä määrite mutta jätä se tyhjäksi niin että voit täyttää tietoja.

![](./media/article-metadata/checkmark-small.png)**Editorin**: ei käytetä. Älä käytä sen muuhun tarkoitukseen.

![](./media/article-metadata/checkmark-small.png)**tunnisteet**: valinnainen. Sisällytä hyväksytyn arvoja vastaavia artikkeleita prefiltered luetteloon vain, jos haluat ottaa käyttöön artikkelissa navigointipolkujen artikkelissa indeksi-sivulle (http://azure.microsoft.com/documentation/articles/)-linkkiä. Näitä arvoja on tarkoitus ryhmitellä sisällön yhteen, kun sisällön ryhmittelyä ei ole palvelukohtaisia lisäämistapaa. Tunnisteet voit myös lisätä otsikoita, joka ilmaisee artikkeli koskee tekniikat. Tämä arvo **ei** tue vapaamuotoiset tunnisteita tai hashtags; tunnisteet on otettava käyttöön sivustossa. Voit määrittää useita tunnisteita arvoja yhden artikkeliin, erota ne toisistaan puolipisteillä. Hyväksytyt arvot ovat seuraavat:

  - arkkitehtuuri
  - Azure--Resurssienhallinta
  - Azure hallinta
  - Laskutus
  - MySQL

![](./media/article-metadata/checkmark-small.png)**avainsanojen**: valinnainen. Käytettäväksi vain Hakukoneoptimoinnin champs. Pilkuilla erillisiä ehtoja. **Hakukoneoptimoinnin champ Kysy ennen kuin muutat tai poistat tämän artikkelin, joka sisältää näitä ehtoja sisältö.** Tämän määritteen tietueiden avainsanojen Hakukoneoptimoinnin champ on kohdistettu ja Etsi järjestys parantamiseksi seuranta. Avainsanat eivät toimi julkaistun HTML-muodossa. Vahvistaminen ei edellytä tämän määritteen.

## <a name="attributes-and-values-for-the-tags-section"></a>Määritteet ja arvot tags-osa

![](./media/article-metadata/checkmark-small.png)**MS.Service**: pakollinen. Määrittää Azure-palvelu, työkalua tai ominaisuutta, on artikkelissa koskee. Yhden arvon yhdelle sivulle.

 Jos sivun koskee palveluihin, valitse palvelu, johon se useimmat koskee suoraan; artikkelissa, joka käyttää sovelluksen pitäminen verkkosivustojen osoittamaan palvelun Bus toiminnot on esimerkiksi- **palvelun bus** arvon sijaan **sivustot**. Jos sivun koskee myös palveluihin, valitse **useita**. Jos sivulla ei koske palveluiden (tämä on harvinaisissa), valitse **puuttuu**.

 - **Active directory**
 - **Active directory-b2c**
 - **Active directory-ds**
 - **API hallinta**
 - **sovelluksen-palvelu**: koskee vain Yleinen käsitteellinen materiaali sovelluksen-palvelusta
 - **sovelluksen-palvelu-ohjelmointirajapinta**
 - **sovelluksen-palvelu-logiikka**
 - **sovelluksen-palvelu-mobile**
 - **sovelluksen-palvelu-web**
 - **hakemuksen tiedot**
 - **sovelluksen yhdyskäytävään**
 - **automaatio**
 - **Azure--Resurssienhallinta**
 - **Azure-suojaus**
 - **Azure-pino**
 - **Varmuuskopiointi**
 - **erän**
 - **parhaiden käytäntöjen**
 - **BizTalk-palvelut**
 - **Laskutus**
 - **Välimuistin**
 - **CDN**
 - **cloud services**
 - **tiedot-luettelo**
 - **tiedot-järvi-säilö**
 - **tiedot-järvi – analysoinnin**
 - **devtest testiympäristössä**
 - **expressroute**
 - **hdinsightiin**
 - **asioita Internet**
 - **IOT-toiminnossa**
 - **avaimen säilö**
 - **koneen opiskelun**
 - **Marketplace**: koskevien Azure Marketplacesta
 - **Media-palvelut**
 - **Mobiili-välitys**
 - **Mobiili-palvelut**
 - **Avainhankkeiden monivaiheisen-factor todentaminen**
 - **useiden**: sivun koskee myös palveluihin
 - **puuttuu**: sivua ei koske palveluiden (harvinaisissa)
 - **ilmoitus keskittimet**
 - **toiminnalliset tiedot**
 - **powerapps**
 - **palautus hallinta**
 - **Redis.txt välimuisti**
 - **RemoteApp**
 - **Sisältöoikeuksien hallinta**
 - **ajoitus**
 - **Tietoturvakeskus**
 - **palvelun bus**
 - **palvelun kangasta**
 - **sivuston palautus**: aiemmin palautus-palvelut
 - **SQL-tietokanta**
 - **SQL--tietovarasto**
 - **SQL-raportointi**
 - **tallennustilan**
 - **tallentaa**: käytössä Azure-kaupan kautta käsitteleviä artikkeleita
 - **storsimple**
 - **liikenteen hallinta**
 - **virtuaalinen koneet**
 - **virtuaalinen verkossa**
 - **Visual-studio online**
 - **VPN-yhdyskäytävän**
 - **sivustot**

![](./media/article-metadata/checkmark-small.png)**MS.devlang**: pakollinen. Määrittää ohjelmointikieli, joka on artikkelissa koskee. Yksittäinen arvo, yhdelle sivulle.

 Jos sivun koskee myös ohjelmoinnin kielet, valitse **useita**. Jos sivu on ensisijaisesti käsitteellisiä ja sen sisältö on yleensä käytettävissä ohjelmoinnin monikielisiä, valitse **useita**. Jos sivulla ei ole kohdistettu osoitteessa kehittäjät ja ohjelmoinnin kielet ei ole merkitystä, valitse **puuttuu**. **Rest api** avulla voit tunnistaa REST API-ohjeaiheista.

 - **cpp**
 - **DotNet**
 - **Java**
 - **JavaScript**
 - **useiden**: sivun koskee ohjelmoinnin monikielisiä tasaisesti.
 - **puuttuu**: sivun ei kohde kehittäjät ja ei ole mitään ohjelmoinnin kielten.
 - **nodejs**
 - **Tavoite-c**
 - **php**
 - **Python**
 - **REST-ohjelmointirajapinta**
 - **Ruby**


![](./media/article-metadata/checkmark-small.png)**MS.topic**: pakollinen. Tekniset tiedot aiheen Kirjoita. Useimmat osallistujat luoma uusia sivuja on artikkelissa tai viittaus.

 - **artikkelissa**: käsitteellinen aiheen, opetusohjelma, toiminto-opas tai muiden viittauksen artikkelissa

 - **Markkinointikampanja-sivulla**: vain Azure.com.  Sivu, joka on suunniteltu erityisesti ulkoisen Kampanjat siirtymissivu ja ei ole ensisijaisessa mainittuihin mukana  Ei voi käyttää dokumentaatio artikkeleita tai tavallisen asiakirjan sivujen purkamisen.  Esimerkkejä: azure.microsoft.com/develop/net/aspnet/; Azure.microsoft.com/develop/Mobile/iOS/

 - **keskihajonta-hallintakeskus-aloitus-sivulla**: vain Azure.com.  Keskihajonta keskittää kotisivu, kuten/kehittää/verkon /

 - **Get-aloittaminen-artikkelissa**: määrittää artikkeleihin, joissa on esitelty vasemmanpuoleisesta siirtymisruudusta palvelun aloittaminen tai yhteenveto-osassa.

 - **pääkuva-artikkeli**: "pääkuva"-opetusohjelma, joka on suunniteltu johdanto palveluun tai ominaisuus, joka saa vierailijat-palvelun avulla nopeasti alkuun ja asemat-kokeiluversio ilmoittautumisiin ja MSDN-aktivointia. Tämä arvo määrittää vain artikkeleihin, joissa asiakirjat-aloitussivu, jossa palvelun yläreunassa ajankohtainen.

 - **Kotisivu**: ylimmän tason asiakirjat aloitussivulle. Vain on kaksi: azure.microsoft.com/documentation/ ja msdn.microsoft.com/library/azure/

 - **indeksi-sivulla**: toisen tason purkamisen sivut ohjelmointi kielet, palvelut tai ominaisuuksia. Nämä leviävät Azure.com ja kirjasto ja käytetään pikakuvakkeet kohdistetuissa tarkempia tietoja. Esimerkkejä: http://azure.microsoft.com/develop/mobile/resources-wp8/, http://msdn.microsoft.com/library/azure/jj673460.aspx, http://msdn.microsoft.com/library/azure/hh689864.aspx

 - **infographic-sivulla**: vain Azure.com. Sivu, jossa on muuttaa infographic tai julisteen, esimerkiksi http://azure.microsoft.com/documentation/infographics/windows-azure/

 - **Viite**: API viittaus sivu (mukaan lukien REST API) tai PowerShell cmdlet-viittaus sivun

 - **Service-kotisivu**: vain Azure.com.  Asiakirjan palvelun kotisivu, kuten /documentation/services/virtual-machines /

 - **sivusto-osa-kotisivu**: vain Azure.com. "Kotisivu" azure.com esimerkkejä sisällön tietyn tyyppisiä: http://azure.microsoft.com/documentation/infographics/ http://azure.microsoft.com/documentation/scripts/, http://azure.microsoft.com/documentation/videos/home/, http://azure.microsoft.com/downloads/

 - **Video-sivulla**: vain Azure.com.  Sivu, joka sisältää videon, esimerkiksi http://azure.microsoft.com/documentation/videos/azure-webjobs-hosting-testing-net/

![](./media/article-metadata/checkmark-small.png)**MS.tgt_pltfrm**: pakollinen. Määrittää kohdeympäristö, esimerkiksi Windows, Linux, Windows Phone-, iOS, Android tai erityinen välimuistin ympäristöissä. Yhden arvon yhdelle sivulle. Tämä arvo on useimmissa lukuun ottamatta mobile aiheet ja näennäiskoneiden **puuttuu** .

 - **Välimuistin rooli**
 - **Välimuistin useita**
 - **Välimuistin Redis.txt**
 - **Välimuistin-palvelu**
 - **Välimuistin jaettu**
 - **Command-line-käyttöliittymä**
 - **Ibiza**: Ibiza portaalin käyttävä sisältö. Käytä tätä vain tapauksia, joissa on käsitellyt ominaisuus on käytettävissä Ibiza-portaalin ja nykyisen portaalin.
 - **Mobile android**: vain tällä hetkellä Azure.com
 - **Mobile html**: vain tällä hetkellä Azure.com
 - **Mobile ios**: vain tällä hetkellä Azure.com
 - **Mobile kindle**: vain tällä hetkellä Azure.com
 - **Mobiili-useita**
 - **mobiili-nokia-x**: vain tällä hetkellä Azure.com
 - **Mobile phonegap**: vain tällä hetkellä Azure.com
 - **Mobile sencha**: vain tällä hetkellä Azure.com
 - **Mobile-windows**: Azure.com vain tällä hetkellä; Windowsin Universal
 - **Mobile windows-puhelin**
 - **Mobile windows-kaupan**
 - **Mobile xamarin**: Azure.com vain tällä hetkellä; Xamarin kaikissa ympäristöissä
 - **mobiili-xamarin-android**: vain tällä hetkellä Azure.com
 - **mobiili-xamarin-ios**: vain tällä hetkellä Azure.com
 - **useiden**: sivun koskee myös useiden ympäristöjen
 - **puuttuu**: ympäristö-määrite ei ole käytettävissä tällä sivulla
 - **PowerShellin**
 - **AM linux**
 - **AM useita**
 - **AM windows**
 - **AM-windows-sharepoint**
 - **AM-windows-sql-palvelin**
 - **ja-– aloittaminen**: tunnistaa ja käytön aloittaminen-sivu-ryhmä. Tunnisteen lisääminen 12/1/14.
 - **ja-Entä-on tapahtunut**: tunnistaa ja käytön aloittaminen mitä on tapahtunut-sivu. Tunnisteen lisääminen 12/1/14.

![](./media/article-metadata/checkmark-small.png)**MS.Workload**: pakollinen. Määrittää Azure työmäärää, joka koskee sivun. Yhden arvon vain artikkelissa kohden.

**Päivitä 8/6/15** Ms.workload arvo yhdistetään xls ei .md tiedoston arvon mukaan. Ms.workload-arvo on edelleen pakollinen vahvistusta varten, ennen kuin toiminto voidaan päivittää. Tukevat ajoitetaan nyt.  Käytä **"puuttuu"** arvona nyt.

![](./media/article-metadata/checkmark-small.png)**MS.Date**: pakollinen. Määrittää päivämäärän, on artikkelissa viimeksi tarkistanut asiayhteyden, tarkkuus, oikea näyttökuvien ja toimimasta linkkejä. Kirjoita päivämäärä kk/pp/vvvv-muodossa. Tämä päivämäärä näkyy myös viimeksi päivitetty päivämääränä julkaistun artikkelin.

![](./media/article-metadata/checkmark-small.png)**MS.AUTHOR**: pakollinen. Määrittää teosten liittyvä aihe. Sisäisten raporttien (kuten tuoreus) oikean teosten liittäminen artikkelin tätä arvoa käytetään. Voit määrittää useita arvoja olisi erota ne toisistaan puolipisteillä. Microsoft aliases tai valmis sähköpostiosoitteet ovat oikein. Pituus voi olla enintään 200 merkkiä.


###<a name="contributors-guide-links"></a>Osallistujat-opas linkit

- [Yleistä artikkelissa](./../README.md)
- [Indeksi ohjeet artikkeleista](./contributor-guide-index.md)


<!--Anchors-->
[Syntaksi]: #syntax
[Käyttö]: #usage
[Määritteet ja ominaisuudet-osan arvoja]: #attributes-and-values-for-the-properties-section
[Määritteet ja arvot tags-osa]: #attributes-and-values-for-the-tags-section
