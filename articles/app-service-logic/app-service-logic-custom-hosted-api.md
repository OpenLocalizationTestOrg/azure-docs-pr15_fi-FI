<properties
    pageTitle="Soita mukautetun API logiikan-sovelluksissa"
    description="Oman sovelluksen palvelun logiikan sovelluksilla isännöimät mukautetut Ohjelmointirajapinnan käyttäminen"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>

# <a name="using-your-custom-api-hosted-on-app-service-with-logic-apps"></a>Oman sovelluksen palvelun logiikan sovelluksilla isännöimät mukautetut Ohjelmointirajapinnan käyttäminen

Vaikka logiikan sovellukset on monenlaisia yhdistimet 40 + erilaisia palveluja, voit soittaa omia mukautettuja API, voit suorittaa oman koodin. Isännöimiseen omia mukautettuja verkko-Ohjelmointirajapinnan on eniten skaalattava ja helpoin tapa on sovelluksen-palvelun käyttöä varten. Tässä artikkelissa kerrotaan, miten voit soittaa minkä tahansa verkko-Ohjelmointirajapinnan ylläpidettävä App Service API sovelluksen, web App-sovelluksen tai mobiilisovelluksessa.

Lisätietoja rakentaminen ohjelmointirajapinnan käynnistimen tai toiminnon logiikan-sovelluksista on Katso [Tässä artikkelissa](app-service-logic-create-api-app.md).

## <a name="deploy-your-web-app"></a>Web-sovelluksen käyttöönotto

Sinun on ensin ottamaan yhteyttä API kuin Web App-sovelluksen-palvelussa. Nämä ohjeet kattaa basic käyttöönoton: [Luo ASP.NET web app-sovelluksessa](../app-service-web/web-sites-dotnet-get-started.md).  Vaikka voit soittaa minkä tahansa API logiikan-sovelluksesta, saat parhaan kokemuksen Suosittelemme Swagger metatietojen integroiminen helposti logiikan sovellusten toimintojen lisääminen.  Tietoja lisäämisestä [swagger](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui).

### <a name="api-settings"></a>API-asetukset

Logiikan sovellusten Designerin jäsentää oman Swagger järjestyksessä on tärkeää, että CORS ottaminen käyttöön ja koodiin APIDefinition-ominaisuuksien määrittäminen.  Tämä on hyvin helppoa Azure-portaalissa.  Vain Avaa Web App-asetukset-sivu ja API-kohdassa Aseta 'API määritys"URL-osoite (tämä on yleensä https://{name}.azurewebsites.net/swagger/docs/v1) swagger.json tiedoston ja lisää CORS käytäntöä" * "pyynnöt logiikan sovelluksista Designer varten.

## <a name="calling-into-the-api"></a>Soittavan Ohjelmointirajapinnan tuominen

Logiikan sovellusten portaalin sisällä, jos olet määrittänyt CORS ja API määritelmä-ominaisuudet pitäisi onnistua voit helposti lisätä oman työnkulun mukautetut API toimintoja.  Voit valita suunnittelija selaamalla tilaus-sivustojen luetteloon sivustot ja määritetty swagger URL-osoite.  Voit myös käyttää HTTP + Swagger-toiminto, joka swagger osoittamalla ja käytettävissä olevien toimintojen ja syötteiden luettelon.  Lopuksi voit luoda aina pyyntö soittaminen minkä tahansa API, myös ne, jotka eivät ole tai näyttää swagger doc-tiedoston HTTP-toiminnon avulla.

Jos haluat suojata oman API, valitse muutama eri tavoin tehdä:

1. Koodin muutoksia ei tarvita – Azure Active Directory voidaan suojata oman API tarvitsematta koodin muutokset tai lukea.
1. Pakota Basic Auth, AAD Auth tai varmenteen Auth oman API-koodiin.

## <a name="securing-calls-to-your-api-without-a-code-change"></a>Oman koodin muutos ilman API puhelut suojaaminen

Tässä osassa Luo kaksi Azure Active Directory-sovellusten – yksi logiikan sovelluksen ja yksi Web App-ohjelman.  Puhelut käyttäen palvelua lyhennys (OstajanTunnus ja salaisuus) logiikan-sovelluksen AAD-sovellukseen kytketty koodiin todennusta. Lopuksi sisältävät sovelluksen logiikan sovelluksen määrityksen tunnukset.

### <a name="part-1-setting-up-an-application-identity-for-your-logic-app"></a>Osa 1: Sovelluksen käyttäjätiedot logiikan-sovelluksen määrittäminen

Tämä on logiikan sovellus käyttää tarkistamiseen active Directorysta. Voit vain *on* toimi samoin kerran hakemistossa. Voit esimerkiksi valita käytettävän kaikki sovelluksesi logiikan tunnistetiedot vaikka voit luoda yksilölliset käyttäjätietojen kohti logiikan app myös halutessasi. Voit tehdä tämän Käyttöliittymän tai PowerShellin käyttäminen.

#### <a name="create-the-application-identity-using-the-azure-classic-portal"></a>Azure-perinteinen portaalissa sovelluksen käyttäjätietojen luominen

1. Siirry [Azure perinteinen portaalissa Active directory](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) ja valitse kansio, jota käytetään Web App
2. Valitse **sovellukset** -välilehti
3. Valitse **Lisää** sivun alareunassa olevia painikkeita
4. Anna henkilöllisyytesi nimi, valitse Seuraava-nuoli
5. Sijoittaa kaksi tekstiruuduissa toimialueen muotoiltuna yksilöllinen merkkijono ja valitse valintamerkki
6. Valitse tämän sovelluksen **määrittäminen** -välilehti
7. Kopioi **Asiakastunnus**, tämä käytetään logiikan-sovelluksessa
8. **Näppäimet** -kohdassa Valitse **keston** ja valitse 1 vuosi tai kahden vuoden
9. Napsauta **Tallenna** -painiketta (saatat joutua odottamaan hetkinen) näytön alareunassa
10. Nyt muista kopioida avain-ruutuun. Tätä myös käyttää logiikan-sovelluksessa

#### <a name="create-the-application-identity-using-powershell"></a>Luo sovelluksen käyttäjätiedot PowerShellin avulla
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. Varmista, että haluat kopioida **Vuokraajan tunnus**, **Tunnus** ja salasana, joita käytit

### <a name="part-2-protect-your-web-app-with-an-aad-app-identity"></a>Osa 2: Web-sovelluksen kanssa AAD sovelluksen jäsenyyden suojaaminen

Jos Web app on käytössä voit ottaa sen heti portaalissa. Muussa tapauksessa voit tehdä ottaminen käyttöön luvan Azure resurssien hallinta käyttöönoton osa.

#### <a name="enable-authorization-in-the-azure-portal"></a>Ota käyttöön luvan Azure-portaalissa

1. Siirry Web app ja sitten komentopalkin **asetukset** .
2. Valitse **Todennus ja todennusta**.
3. Ota **käyttöön**.

Tässä vaiheessa sovellus on luotu automaattisesti puolestasi. Tarvitset tämän sovelluksen Asiakastunnus osa 3, joten sinun on:

1. Siirry [Azure perinteinen portaalissa Active directory](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) ja valitse hakemistossa.
2. Etsi sovellus hakuruutuun
3. Valitse se luettelosta
4. Valitse **määritys** -välilehti
5. Raportissa pitäisi näkyä **Ostajantunnus**

#### <a name="deploying-your-web-app-using-an-arm-template"></a>ARM-mallin avulla koodiin käyttöönotto

Sinun on ensin Web app-sovelluksen luominen. Tämä saa olla sama kuin sovellus, jota käytetään, kun logiikan-sovellus. Aluksi seuraavat vaiheet osa 1, mutta nyt **Aloitussivu** ja **IdentifierUris** käytön koodiin todellinen https://**URL-osoite** .

>[AZURE.NOTE]Kun luot Web App sovelluksen, sinun on käytettävä [Azure perinteinen portaalin lähestymistapa](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory)kuin PowerShell-komentosovelmalla ole määrittänyt tarvittavat käyttöoikeudet käyttäjät kirjautumaan sivustoon.

Kun olet asiakkaan tunnus ja vuokraajan tunnus, Sisällytä seuraavat sub resurssiksi Web Appin käyttöönotto-malliin:

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

Voit suorittaa käyttöönoton automaattisesti, tyhjä Web app- ja logiikka app ryhmitteleminen, jotka käyttävät AAD ottaa käyttöön valitsemalla Seuraava painike:

[![Ottaa käyttöön Azure](./media/app-service-logic-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

Artikkelissa [logiikan app kutsuu mukautettu API isännöimät App palvelun ja suojattu AAD](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json)valmis malli.


### <a name="part-3-populate-the-authorization-section-in-the-logic-app"></a>Osa 3: Täytä luvan osan logiikan-sovelluksessa

**HTTP** -toiminnon **todennus** -osassa:`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Osa | Kuvaus |
|---------|-------------|
| tyyppi | Todennus tyyppi. Arvo on ActiveDirectoryOAuth ActiveDirectoryOAuth todennusta varten. |
| vuokraajan | AD-vuokraajan tunnistetaan vuokraajan tunnus. |
| käyttäjäryhmälle | Pakollinen. Resurssi, johon olet muodostamassa yhteyttä. |
| clientID | Azure AD-sovelluksen asiakkaan tunnus. |
| salainen | Pakollinen. Asiakas, joka pyytää tunnuksen salainen. |

Yllä mallissa on jo tämä määrittäminen, mutta jos muokkaat logiikan sovelluksen suoraan, sinun on koko todennus-osa.

## <a name="securing-your-api-in-code"></a>Oman koodin Ohjelmointirajapinnan suojaaminen

### <a name="certificate-auth"></a>Varmenteen auth

Voit tarkistaa saapuvat pyynnöt Web Appiin asiakkaan varmenteet. Katso [Siitä, miten voit määrittää TLS molemminpuoleista Web App](../app-service-web/app-service-web-configure-tls-mutual-auth.md) -koodisi määrittämisestä.

*Todennus* -kohdassa Anna: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`.

| Osa | Kuvaus |
|---------|-------------|
| tyyppi | Pakollinen. Todennus tyyppi. SSL-asiakasohjelman varmenteet-arvon on oltava ClientCertificate. |
| PFX | Pakollinen. Base64-koodattu PFX-tiedoston sisällön. |
| salasana | Pakollinen. Salasana PFX-tiedostoa. |

### <a name="basic-auth"></a>Basic auth

Voit tarkistaa saapuvat pyynnöt perustodentamista (kuten käyttäjänimi ja salasana). Voit tehdä sen millä tahansa kielellä, voit luoda-sovelluksen Basic auth on tavallisen kuvion.

*Todennus* -kohdassa Anna: `{"type": "basic","username": "test","password": "test"}`.

| Osa | Kuvaus |
|---------|-------------|
| tyyppi | Pakollinen. Todennus tyyppi. Arvon on oltava Basic Basic todennusta varten. |
| käyttäjänimi | Pakollinen. Käyttäjänimi tarkistamiseen. |
| salasana | Pakollinen. Salasanan todennusta. |

### <a name="handle-aad-auth-in-code"></a>Käsittele AAD auth koodissa

Oletusarvon mukaan Azure Active Directory-todennus, jotka mahdollistavat portaalissa tehdä tarkasti rajattuja luvan. Se ei esimerkiksi Lukitse oman API tietyn käyttäjän tai sovelluksen, mutta vain tietyn vuokraajalle.

Jos haluat rajoittaa vain logiikan sovellus, esimerkiksi Ohjelmointirajapinnan koodissa, voit purkaa otsikko, joka sisältää JWT ja tarkista, kuka soittajan, Hylkää, jotka eivät vastaa pyyntöihin.

Siirtyminen tarkemmin, jos haluat toteuta se kokonaan oman koodin ja hyödyntää ei Portal-ominaisuutta, voit lukea tämän artikkelin: [Käytä Active Directory Azure App palvelun todennusta varten](../app-service-web/web-sites-authentication-authorization.md).

Tarvitset edelleen sovelluksen käyttäjätiedot logiikan-sovelluksen luominen ja käyttäminen, Soita Ohjelmointirajapinnan edellä mainitut toimet.
