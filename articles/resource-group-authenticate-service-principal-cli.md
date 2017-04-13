<properties
   pageTitle="Luo palvelun pääasiallista Azure CLI | Microsoft Azure"
   description="Tässä artikkelissa käsitellään Azure CLI avulla voit luoda Active Directory-sovelluksen ja palvelun lyhennys ja myöntää resurssien Roolipohjainen käyttöoikeuksien valvonta kautta. Se näyttää, miten sovellus, jonka salasanan tai varmenteen todentamiseen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/30/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Azure CLI avulla voit luoda service-lyhennys access-resurssit

> [AZURE.SELECTOR]
- [PowerShellin](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Kun sovellus tai komentosarjan, joka on resursseja, sinulla ei todennäköisesti et halua suorittaa tämä prosessi tunnistetiedoillasi. Joudut ehkä, jota haluat käyttää sovelluksen käyttöoikeuksia ja haluat jatkaa käyttämällä tunnistetietoja, jos tehtäviisi muuttaa sovelluksen. Sen sijaan voit luoda sovelluksen, joka sisältää todennuksen käyttäjätiedot ja roolimäärityksiä jäsenyyden. Aina, kun sovellus käynnistyy, se todentaa itse nämä tunnuksilla. Tässä ohjeaiheessa esitellään, miten [Mac, Linux ja Windows Azure CLI](xplat-cli-install.md) avulla voit määrittää sovelluksen suoritetaan omassa tunnistetiedot ja käyttäjätiedot.

Azure CLI, jossa on kaksi vaihtoehtoa: todennustapa AD-sovelluksen:

 - salasana
 - varmenne

Tässä ohjeaiheessa esitellään käyttämisestä Azure CLI kumpaankin vaihtoehtoon. Jos haluat kirjautua Azure ohjelmoinnin framework (esimerkiksi Python, Ruby tai Node.js), salasanan todennusta voi olla paras vaihtoehto. Ennen kuin päätät käyttää salasanan tai varmenteen on esimerkkejä eri kehysten todennustapa [otoksen sovellukset](#sample-applications) -osassa.

## <a name="active-directory-concepts"></a>Active Directory-käsitteestä

Tässä artikkelissa Luo kaksi objektia - Active Directory (AD)-sovelluksen ja lainan palvelu. AD-sovellus on yleinen esitys sovelluksesta. Se sisältää tunnistetiedot (tunnus ja joko salasanan tai varmenteen). Lainan palvelu on paikallisen Active Directory-sovelluksen-esitys. Siinä on roolimääritys. Tässä artikkelissa keskitytään yhden vuokraajan-sovellusta, jos sovellus on tarkoitettu toimimaan vain yhden organisaatiosi. Käytetään tavallisesti yhden vuokraajan sovellusten liiketoiminta-sovellukset, jotka suoritetaan organisaatiossa. Yhden vuokraajan-sovelluksessa on yhteen AD-sovellukseen ja yksi palvelu lyhennyksen.

Ihmettelet ehkä - mihin tarvitsen molemmat objektit? Tämän menetelmän tekee kannattaa, kun pidät usean vuokraajan sovellukset. Käytetään yleensä usean vuokraajan sovellusten ohjelmiston nimellä--palvelun (SaaS)-sovelluksia, jossa sovellus suoritetaan useita eri tilaukset. Usean vuokraajan sovellusten on yksi AD-sovellus ja useita palvelun ansaitun (yksi kullekin Active Directoryssa, joka antaa access-sovellukseen). Määrittämään monen vuokraajan sovelluksen [kehittäjän](resource-manager-api-authentication.md)oppaassa luvan Azure Resurssienhallinta-Ohjelmointirajapinnan kanssa.

## <a name="required-permissions"></a>Tarvittavat käyttöoikeudet

Tämän ohjeaiheen suorittamiseen tarvitset riittäviä käyttöoikeuksia Azure Active Directory-ja Azure tilauksen. Tarkemmin sanottuna sinun on voitava Active Directory-sovelluksen luominen ja määritä palvelun lyhennys rooli. 

Active Directory-tilin on oltava järjestelmänvalvoja (esimerkiksi **Yleisen järjestelmänvalvojan** tai **Käyttäjän järjestelmänvalvoja**). Jos tiliisi on määritetty **roolia** , joudut käyttöoikeuksien laajentamista järjestelmänvalvojaa.

Tilauksen, tilisi on oltava `Microsoft.Authorization/*/Write` access, jotka myönnetään [omistajan](./active-directory/role-based-access-built-in-roles.md#owner) rooli tai [käyttäjän Access](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) -järjestelmänvalvojaksi. Jos tiliisi on määritetty **osallistujan** rooli, näyttöön tulee virhe yritettäessä palvelun lyhennys Määritä rooli. Uudelleen-tilauksen järjestelmänvalvoja on myönnettävä sinulle tarvittavat käyttöoikeudet.

Siirry nyt osan [salasanan](#create-service-principal-with-password) tai [varmenteen](#create-service-principal-with-certificate) todentamiseen.

## <a name="create-service-principal-with-password"></a>Luo palvelu pääasiallista salasanalla

Tässä osassa toimien AD-sovelluksen luominen salasanalla ja määrittää lukija pääasiallista palvelu.

Seuraavassa kerrotaan seuraavasti.

1. Kirjaudu tiliisi.

        azure login

1. Käytettävissä on kaksi vaihtoehtoista AD-sovelluksen luomiseen. Voit luoda AD-sovelluksen ja palvelun pääasiallista yhdessä vaiheessa tai luo ne erikseen. Luoda niitä yhdessä vaiheessa, jos sinun ei tarvitse määrittää Aloitussivu ja sovelluksen URI tunnus. Luo ne erikseen Jos haluat määrittää nämä arvot web-sovelluksen. Molemmat vaihtoehdot näkyvät tässä vaiheessa.

     - Voit luoda AD-sovelluksen ja palvelun pääasiallista yhdessä vaiheessa nimetä sovellus ja salasana, kuten seuraava komento:
     
            azure ad sp create -n exampleapp -p {your-password}     
     
     - Voit luoda AD-sovelluksen erikseen, nimetä sovellus, kotisivun URI, URI-tunnus ja salasana, kuten seuraava komento:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>

         Edeltävä komento palauttaa sovelluksen tunnus-arvon. Palvelun lyhennys luomiseen on kyseisen arvon parametrina seuraava komento:
     
            azure ad sp create -a <AppId>
     
     Jos tiliä ei ole [tarvittavia käyttöoikeuksia](#required-permissions) Active Directory, näyttöön tulee virhesanoma, jossa sanotaan "Authentication_Unauthorized" tai "tilausta ei löydy kontekstissa".
    
     Molemmat vaihtoehdot uusi palvelu lyhennys palautetaan. **Objektitunnus** tarvita, kun oikeuksien myöntämistä. **Palvelun lyhennys nimien** luettelossa GUID-tunnus tarvitaan kirjauduttaessa. Tämä GUID-tunnus on saman arvon kuin sovelluksen tunnus. Esimerkki-sovelluksissa tätä arvoa käytetään nimitystä **Asiakastunnus**. 
    
        info:    Executing command ad sp create
        + Creating application exampleapp
        / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
        data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
        data:    Display Name:            exampleapp
        data:    Service Principal Names:
        data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
        data:                             https://www.contoso.org/example
        info:    ad sp create command OK

2. Tilauksen service principal-käyttöoikeuksien myöntäminen Tässä esimerkissä palvelun lyhennys lisääminen **lukija** -rooli, joka antaa lukuoikeudet tilauksen kaikki resurssit. Lisätietoja muiden roolien [RBAC: valmiin roolien](./active-directory/role-based-access-built-in-roles.md). **ServicePrincipalName** -parametrin on **objektitunnus** , jota käytit luodessasi sovellus. 

        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

     Jos tiliä ei ole riittäviä käyttöoikeuksia, Määritä rooli, näyttöön tulee virhesanoma. Viesti, joka ilmaisee oman tilillä **ei ole lupaa suorittavan toiminnon Microsoft.Authorization/roleAssignments/write laajuus/tilaukset / {guid} päälle**. 

Joka on tämä! AD-sovelluksen ja palvelun lyhennys on määritetty. Seuraavassa osassa näytetään, miten kirjautuaksesi sisään tunnistetiedon Azure CLI kautta. Jos haluat käyttää tunnistetieto koodi-sovelluksessa, sinun ei tarvitse jatkaa tämän ohjeaiheen. Voit siirtyä [otoksen sovellusten](#sample-applications) esimerkkejä sisäänkirjautumisessa tunnus ja salasana. 

### <a name="provide-credentials-through-azure-cli"></a>Anna tunnistetiedot Azure CLI kautta

Nyt sinun täytyy suorittaa sovelluksen Kirjaudu sisään.

1. Aina, kun kirjautuminen palvelun lyhennys, sinun täytyy antaa hakemiston vuokraajan tunnus AD-sovellus. Palvelutili on Active Directory-esiintymä. Voit noutaa vuokraajan todennettuna tilauksen tunnus, käyttämällä:

        azure account show

     Joka palauttaa:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Jos sinun on hankittava toiseen tilaukseen vuokraajan tunnus, kirjoita seuraava komento:

        azure account show -s {subscription-id}

2. Jos haluat hakea Asiakastunnus käytettävät kirjautumisesta, käytä:

        azure ad sp show -c exampleapp --json

     Arvon, jota käytetään kirjauduttaessa on guid-palvelun pääasiallista nimet-luettelossa.

        [
          {
            "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "7132aca4-1bdb-4238-ad81-996ff91d8db4"
            ]
          }
        ]

3. Kirjaudu sisään palvelun lyhennys.

        azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}

    Pyydettäessä salasana. On määritetty luotaessa AD-sovelluksen salasana.

        info:    Executing command login
        Password: ********

Voit nyt todennetaan kuin palvelun lyhennys varten, jonka loit palvelun lyhennys.

## <a name="create-service-principal-with-certificate"></a>Luo palvelu pääasiallista sertifikaatilla

Tässä osassa suorittaa seuraavien ohjeiden avulla:

- itse allekirjoitetun varmenteen luominen
- Varmenteen ja palvelun lyhennys AD-sovelluksen luominen
- Lukija-roolin määrittäminen palvelun lyhennyksen.

Voit suorittaa nämä vaiheet, sinulla on oltava asennettuna [OpenSSL](http://www.openssl.org/) .

1. Itse allekirjoitetun varmenteen luominen

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. Voit yhdistää julkiset ja yksityiset avaimet.

        cat privkey.pem cert.pem > examplecert.pem

3. Avaa **examplecert.pem** -tiedosto ja Etsi pitkä merkkijono **---Aloita VARMENTEEN---** ja **---lopettaa VARMENTEEN---**välillä. Kopioi varmenteen tiedot. Nämä tiedot välittää parametrina luomiseen palvelun pääasiallista.

1. Kirjaudu tiliisi.

        azure login

1. Käytettävissä on kaksi vaihtoehtoista AD-sovelluksen luomiseen. Voit luoda AD-sovelluksen ja palvelun pääasiallista yhdessä vaiheessa tai luo ne erikseen. Luoda niitä yhdessä vaiheessa, jos sinun ei tarvitse määrittää Aloitussivu ja sovelluksen URI tunnus. Luo ne erikseen Jos haluat määrittää nämä arvot web-sovelluksen. Molemmat vaihtoehdot näkyvät tässä vaiheessa.

     - Jos haluat luoda AD-sovelluksen ja palvelun pääasiallista yhdessä vaiheessa, Anna nimi varmenteen tiedot, ja sovelluksen mukaisesti seuraava komento:
     
            azure ad sp create -n exampleapp --cert-value <certificate data>
     
     - AD-sovelluksen luominen erikseen, Anna nimi sovellus, kotisivun URI, URI-tunnus ja varmenteen tiedot, kuten seuraava komento:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example --cert-value <certificate data>

         Edeltävä komento palauttaa sovelluksen tunnus-arvon. Palvelun lyhennys luomiseen on kyseisen arvon parametrina seuraava komento:
     
            azure ad sp create -a <AppId>
  
     Jos tiliä ei ole [tarvittavia käyttöoikeuksia](#required-permissions) Active Directory, näyttöön tulee virhesanoma, jossa sanotaan "Authentication_Unauthorized" tai "tilausta ei löydy kontekstissa".
    
     Molemmat vaihtoehdot uusi palvelu lyhennys palautetaan. Objektitunnus tarvita, kun oikeuksien myöntämistä. **Palvelun lyhennys nimien** luettelossa GUID-tunnus tarvitaan kirjauduttaessa. Tämä GUID-tunnus on saman arvon kuin sovelluksen tunnus. Esimerkki-sovelluksissa tätä arvoa käytetään nimitystä **Asiakastunnus**. 
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. Tilauksen service principal-käyttöoikeuksien myöntäminen Tässä esimerkissä palvelun lyhennys lisääminen **lukija** -rooli, joka antaa lukuoikeudet tilauksen kaikki resurssit. Lisätietoja muiden roolien [RBAC: valmiin roolien](./active-directory/role-based-access-built-in-roles.md). **ServicePrincipalName** -parametrin on **objektitunnus** , jota käytit luodessasi sovellus. 

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

     Jos tiliä ei ole riittäviä käyttöoikeuksia, Määritä rooli, näyttöön tulee virhesanoma. Viesti, joka ilmaisee oman tilillä **ei ole lupaa suorittavan toiminnon Microsoft.Authorization/roleAssignments/write laajuus/tilaukset / {guid} päälle**. 

### <a name="provide-certificate-through-automated-azure-cli-script"></a>Antaa Varmenteen automaattinen Azure CLI komentosarjan avulla

Nyt sinun täytyy suorittaa sovelluksen Kirjaudu sisään.

1. Aina, kun kirjautuminen palvelun lyhennys, sinun täytyy antaa hakemiston vuokraajan tunnus AD-sovellus. Palvelutili on Active Directory-esiintymä. Voit noutaa vuokraajan todennettuna tilauksen tunnus, käyttämällä:

        azure account show

     Joka palauttaa:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Jos sinun on hankittava toiseen tilaukseen vuokraajan tunnus, kirjoita seuraava komento:

        azure account show -s {subscription-id}

1. Voit noutaa varmenteen allekirjoitus ja poista tarpeettomat merkit avulla:

        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
     Joka palauttaa arvon allekirjoitus samalla tavalla kuin:

        30996D9CE48A0B6E0CD49DBB9A48059BF9355851

2. Jos haluat hakea Asiakastunnus käytettävät kirjautumisesta, käytä:

        azure ad sp show -c exampleapp

     Arvon, jota käytetään kirjauduttaessa on guid-palvelun pääasiallista nimet-luettelossa.

        [
          {
            "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "4fd39843-c338-417d-b549-a545f584a745",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "4fd39843-c338-417d-b549-a545f584a745"
            ]
          }
        ]

1. Kirjaudu sisään palvelun lyhennys.

        azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

Voit nyt todennetaan Active Directory-sovellus, jonka loit pääasiallisena palveluna.

## <a name="sample-applications"></a>Esimerkki sovellukset

Esimerkki seuraavien sovellusten näyttää, miten Kirjaudu palvelun lyhennys.

**.NET**

- [Ottaa käyttöön SSH käytössä AM .NET-malli](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Azure resurssit ja .NET resurssiryhmien hallinta](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Java-resurssit - käyttöönotto Azure Resurssienhallinta mallilla - aloittaminen](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Java-resurssit - hallita resurssiryhmä - aloittaminen](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Ottaa käyttöön SSH käytössä AM Python käyttämällä mallia](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Azure resurssi ja Python resurssiryhmien hallinta](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Ottaa käyttöön SSH käytössä AM Node.js käyttämällä mallia](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure resursseja ja Node.js resurssiryhmien hallinta](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Ottaa käyttöön SSH käytössä AM Ruby-malli](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Azure resurssi ja Ruby resurssiryhmien hallinta](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Seuraavat vaiheet
  
- Yksityiskohtaiset ohjeet sovelluksen integroiminen Azure resurssien hallintaa varten katso [luvan Azure Resurssienhallinta API Kehittäjän ohje](resource-manager-api-authentication.md).
- Lisätietoja varmenteet ja Azure CLI käyttämisestä on artikkelissa [Azure palvelun ansaitun Linux komentoriviltä Varmenteen todentaminen](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
