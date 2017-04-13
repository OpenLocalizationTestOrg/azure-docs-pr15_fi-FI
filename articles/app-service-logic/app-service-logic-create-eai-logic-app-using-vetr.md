<properties
   pageTitle="Luo EAI logiikan sovellukseen käyttämällä VETR logiikan: n sovelluksen Azure-palvelu | Microsoft Azure"
   description="Vahvista, koodata ja muuntaminen BizTalk XML Servicesin ominaisuudet"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="create-eai-logic-app-using-vetr"></a>Luo EAI logiikan sovellusta VETR

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Useimmat yrityksen sovelluksen integrointi (EAI) skenaarioita mediate lähteen ja kohteen tietojen. Tällaisia tilanteita, joissa on usein yhteiset vaatimukset:

- Varmista, että tiedot eri järjestelmistä on muotoiltu oikein.
- Suorita "hakuarvo" saapuvien tietojen päätösten tekemiseen.
- Muuntaminen tietoja muodosta toiseen. Muuntaa esimerkiksi tietojen CRM-järjestelmän tietomuoto ERP-järjestelmän tietojen muoto.
- Reitittää haluamasi sovellus tai järjestelmän tiedot.

Tässä artikkelissa kerrotaan yleisiä integrointi kuviossa: "yksisuuntainen viestin sitä" tai VETR (Vahvista Enrich, muunto, reitin). VETR kuvio mediates tietojen lähdekohteen ja kohde-kohteen. Lähde- ja ovat yleensä tietolähteet.

Harkitse sivustoa, joka hyväksyy tilaukset. Käyttäjien viestejä tilaukset http:n järjestelmään. Taustalla järjestelmä vahvistaa oikeellisuuden saapuvat tiedot, Normalisoi sen ja jatkuu palvelun Bus jonossa lisäkäsittelyä varten. Järjestelmä kestää tilaukset käytöstä jonossa odotetaan tietyssä muodossa. Lopusta loppuun-työnkulku on näin:

**HTTP** → **Vahvista** → **Transform** → **palvelun Bus**

![Tavallinen VETR työnkulku][1]

Tätä mallia apuna BizTalk API-sovellukset:

* **HTTP-käynnistimen** - käynnistimen viestin tapahtuman lähde
* **Vahvista** - tarkistaa oikeellisuuden saapuvat tiedot
* **Muunna** - muunnoksia tietojen edeltävät järjestelmän vaatii saapuvan format-muoto
* **Palvelun Bus Connector** - kohde kohde, johon tiedot lähetetään


## <a name="constructing-the-basic-vetr-pattern"></a>Tavallinen VETR kuvion luominen
### <a name="the-basics"></a>Perusteet

Azure-portaalissa Valitse **+ Uusi**, valitse **Web + Mobile**ja valitse sitten **Logiikan sovelluksen**. Valitse nimi, sijainti, tilauksen, resurssiryhmä ja sijainnin, joka toimii. Resurssiryhmät edustajana säilöjä sovelluksia; kaikki resurssit, kun sovellus Siirry saman resurssiryhmä.

Seuraavaksi lisääminen Käynnistimet ja toiminnot.


## <a name="add-http-trigger"></a>Lisää HTTP käynnistin
1. Valitse **Logiikan sovellusmallit** **luominen alusta alkaen**.
1. Valitse **HTTP-kuuntelutoiminnon** voit luoda uuden kuuntelijan valikoimasta. Soita **HTTP1**.
2. Määritä **Lähetä vastaus automaattisesti?** arvoksi false. Määrittää käynnistimen toiminnon määrittämällä _HTTP-menetelmä_ _viestiin_ ja _/OneWayPipeline_ _Suhteellinen URL-osoite_ -asetukseksi:  
    ![HTTP-käynnistin][2]
3. Valitse Viimeistele käynnistin vihreä valintamerkki.

## <a name="add-validate-action"></a>Lisää Vahvista toiminto

Nyt Kirjoita japanin toiminnot, jotka suoritetaan aina, kun käynnistin käynnistyy, eli aina, kun puhelu on vastaanotettu HTTP-päätepisteen.

1. Lisää **BizTalk XML tarkistajan** valikoimasta ja anna sille nimi _(Validate1)_ luominen.
2. Määritä XSD-rakenne XML saapuvien viestien vahvistamiseen. Valitse _Vahvista_ -toiminto ja valitse _triggers('httplistener').outputs. Sisällön_ _inputXml_ -parametrin arvon.

Vahvista-toiminto on nyt ensimmäisen toiminnon HTTP-kuuntelutoiminnon jälkeen: 

![BizTalk XML tarkistajan][3]

Vastaavasti lisääminen muiden toimintojen. 

## <a name="add-transform-action"></a>Muunna-toiminnon lisääminen
Määritä japanin muunnokset normitettava saapuvat tiedot.

1. Lisää **BizTalk Transform palvelun** valikoimasta.
2. Voit määrittää muunto muuntamiseen XML saapuvat viestit, valitsemalla kuin toiminto suoritetaan, kun tämän API kutsutaan **Transform** -toiminto. Valitse ```triggers(‘httplistener’).outputs.Content``` _inputXml_summana. *Määritys* on valinnaisten parametrien, koska kaikki määritetyn muunnokset vastaa saapuvat tiedot, ja niitä käytetään, vain ne arvot, jotka vastaavat rakenne.
3. Lopuksi muunnoksen suorittaa vain silloin, kun Validate onnistuu. Määritä tämä ehto, valitse oikeassa ylälaidassa olevaa hammaspyöräkuvaketta ja valitse _Lisää ehdon on täytyttävä_. Määritä ehto ```equals(actions('xmlvalidator').status,'Succeeded')```:  

![BizTalk muunnokset][4]


## <a name="add-service-bus-connector"></a>Lisää palvelu Bus yhdysviiva
Seuraavaksi lisääminen kohde – palvelun Bus jonon – voit kirjoittaa tiedot.

1. Lisää **Palvelu Bus yhdistimen** valikoimasta. Määritetty **nimi** _Servicebus1_, määrittää **Yhteysmerkkijono** yhteysmerkkijono palvelun bus-esiintymässä, Määritä **Kohteen nimi** _jonossa_ja ohittaa **tilauksen nimi**.
2. Valitse **Lähetä viesti** -toiminto ja määritä _actions('transformservice').outputs toiminnon **sisältö** -kentässä. OutputXml_.
3. **Sisältötyypin** -kentän arvoksi *sovellus-xml*:  

![Palvelun Bus][5]


## <a name="send-http-response"></a>Lähetä HTTP-vastaus
Myyntijakso käsittelyn jälkeen Lähetä takaisin HTTP-vastauksen sekä onnistumisesta tai epäonnistumisesta seuraavasti:

1. Lisää **HTTP-kuuntelutoiminnon** valikoimasta ja valitse **Lähetä HTTP-vastaus** -toiminto.
2. Määritä voit lähettää *viestin* **vastauksen tunnus** .
2. Määritä **Vastauksen sisällön** *myyntijakso käsittely valmis*.
3. *200* **Vastauksen tilakoodi** osoittamaan HTTP 200 OK.
4. Valitse avattavassa valikossa oikeassa ylälaidassa, ja valitse **Lisää ehdon on täytyttävä**.  Määritä ehto seuraava lauseke:  
    ```@equals(actions('azureservicebusconnector').status,'Succeeded')```  <br/>
5. Toista nämä vaiheet lähettämään HTTP vastauksen sekä virheen. Muuta **ehto** seuraava lauseke:  
```@not(equals(actions('azureservicebusconnector').status,'Succeeded'))``` <br/>
6. Valitse **OK** ja valitse sitten **Luo**.



## <a name="completion"></a>Täyttäminen
Aina, kun joku lähettää viestin HTTP-päätepisteen, se tuottaa sovellus ja suorittaa juuri luomasi toiminnot. Voit hallita näiden funktioiden sovellusten luominen, valitse **Selaa** Azure-portaalissa ja **Logiikka**sovellukset. Saat lisätietoja sovelluksen valitseminen

Jotkin hyödyllisiä aiheita:

[Hallinta ja valvonta API-sovellusten ja yhdistimet](app-service-logic-monitor-your-connectors.md)  <br/>
[Logiikan sovellusten valvonta](app-service-logic-monitor-your-logic-apps.md)

<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG
