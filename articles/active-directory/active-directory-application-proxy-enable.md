<properties
    pageTitle="Ota käyttöön Azure AD-sovelluksen välityspalvelimen | Microsoft Azure"
    description="Ottaa käyttöön sovelluksen välityspalvelimen Azure perinteinen portaalissa ja asenna yhdistimet käänteisen välityspalvelimen."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>

# <a name="enable-application-proxy-in-the-azure-portal"></a>Ottaa käyttöön sovelluksen välityspalvelimen Azure-portaalissa

Tässä artikkelissa käydään läpi vaiheet ja ota käyttöön Microsoft Azure AD sovelluksen välityspalvelimen cloud-kansion Azure AD.

Jos et tiedä, mitä välityspalvelinta auttavat toimi, Lue lisää [siitä, miten voit etäkäyttöä paikallisen sovellukset](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Sovelluksen välityspalvelimen edellytykset
Ennen kuin voit ottaa käyttöön ja käyttää välityspalvelinta palveluja, sinun on on:

- [Microsoft Azure AD basic tai premium-tilaus](active-directory-editions.md) ja, jonka olet Yleinen järjestelmänvalvoja Azure AD-kansio.
- Palvelimen Windows Server 2012 R2 tai Windows 8.1 tai uudempi versio, jolla voit asentaa sovelluksen välityspalvelimen yhdistin. Palvelin lähettää pyynnöt pilveen sovelluksen välityspalvelin-palveluihin ja tarvitsemiaan sovellukset, jotka haluat julkaista HTTP tai HTTPS-yhteyttä.

    - Kertakirjautumisen julkaistun ohjelmat, saat tämän tietokoneen olisi voidaan toimialueeseen liittyneet-sovellukset, jotka haluat julkaista AD toimialueelle.

- Jos polku on palomuuri, varmista, että se avataan niin, että yhdistimen voi tehdä HTTPS (TCP) pyynnöt sovelluksen välityspalvelin. Yhdistimen käyttää porttien alitoimialueita, jotka ovat osa ylätason toimialueiden msappproxy.net ja servicebus.windows.net. Varmista, että Avaa **Lähtevän** tietoliikenteen seuraavat portit:

  	| Porttinumero | Kuvaus |
  	| --- | --- |
  	| 80 | Ota käyttöön kelpoisuus HTTP liikenteen. |
  	| 443 | Ota käyttöön käyttöoikeuksien vastaan Azure AD (vaaditaan vain yhdistimen rekisteröitymisen) |
  	| 10100 – 10120 | Ota käyttöön LOB HTTP vastaukset lähetetään takaisin välityspalvelin |
  	| 9352, 5671 | Ota käyttöön välisen yhdistimen pyynnöt Azure palvelu kohti. |
  	| 9350 | Valinnainen, jotta parempi suorituskyky koskevat pyynnöt |
  	| 8080 | Yhdistimen käynnistyksen järjestys ja yhdistimen automaattisten päivitysten ottaminen käyttöön |
  	| 9090 | Ota käyttöön yhdistimen rekisteröinti (vaaditaan vain yhdistimen rekisteröitymisen) |
  	| 9091 | Yhdistimen luota varmenteen automaattisen uusimisen ottaminen käyttöön |

    Jos palomuurin pakottaa liikenne mukaan peräisin käyttäjille, Avaa seuraavat portit tietoliikenteen tulevat verkko-palvelu Windows-palveluita. Varmista myös, että port 8080 käyttöön NT Authority\System.

- Jos organisaatiossa käytetään välityspalvelimet internet-yhteys, tutustu katsaus blogimerkinnän [käyttäminen aiemmin paikalliseen välityspalvelimet](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) lisätietoja siitä, miten voit määrittää niitä.

## <a name="step-1-enable-application-proxy-in-azure-ad"></a>Vaihe 1: Käyttöönotto Azure AD-sovelluksen välityspalvelin
1. Kirjaudu sisään järjestelmänvalvojana [Azure perinteinen portal](https://manage.windowsazure.com/).
2. Siirry Active Directory ja valitse kansio, johon haluat ottaa käyttöön sovelluksen välityspalvelin.

    ![Active Directory - kuvake](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Valitse **Määritä** hakemisto-sivulta ja vieritä **Välityspalvelinta**.
4. Ota **käyttöön välityspalvelimen sovelluspalveluja tähän kansioon** **käytössä**.

    ![Ottaa käyttöön sovelluksen välityspalvelin](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. Valitse **Lataa nyt**. Tämä avaa **Azure AD sovelluksen välityspalvelimen yhdistimen lataaminen**. Lue ja hyväksy käyttöoikeusehdot ja valitse Tallenna yhdistimen Windows Installer-tiedosto (.exe) **Lataa** .

## <a name="step-2-install-and-register-the-connector"></a>Vaihe 2: Asenna ja rekisteröi yhdistin
1. Valmiiden mukaan edellytykset palvelimessa suoritettavat **AADApplicationProxyConnectorInstaller.exe** .
2. Asenna ohjatun toiminnon ohjeiden mukaan.
3. Asennuksen aikana ohjelma pyytää rekisteröidä yhdistimen sovelluksen välityspalvelin Azure AD-vuokraajan.

  - Anna Azure AD-yleisen järjestelmänvalvojan tunnistetiedot. Yleinen järjestelmänvalvoja-vuokraajan voi olla eri kuin Microsoft Azure tunnistetiedot.
  - Varmista, että kuka Rekisteröi yhdistimen järjestelmänvalvoja on otit sovelluksen välityspalvelu samassa kansiossa. Esimerkiksi jos vuokraajan toimialueen contoso.com, järjestelmänvalvojan on oltava admin@contoso.com tai muiden, toimialueen alias.
  - Jos **Internet Explorerin parannetun suojauksen asetukset** on määritetty päivittymään **-** palvelimessa kohtaa, johon olet asentamassa yhdistimen, rekisteröintinäytön saattaa estää. Noudata käyttää virhesanoma. Varmista, että Internet Explorerin tehostettu suojaus ei ole käytössä.
  - Jos yhdistimen rekisteröinti ei onnistu, katso [Vianmääritys sovelluksen välityspalvelin](active-directory-application-proxy-troubleshoot.md).  

4. Kun asennus on valmis, kaksi uusia palveluita lisätään-palvelimeen:

    - **Microsoft AAD sovelluksen välityspalvelimen Connectorin** mahdollistaa yhteys
    - **Microsoft AAD sovelluksen välityspalvelimen yhdistimen Updater** on automaattinen päivitys-palvelun, joka säännöllisesti yhdistimen uusia versioita tarkistaa ja päivittää yhdistimen tarpeen mukaan.

    ![Sovelluksen välityspalvelimen yhdistimen palvelut - näyttökuva](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. Valitse **Viimeistele** asennus-ikkunassa.

Suuren käytettävyyden tarkoituksiin tulisi ottaa käyttöön vähintään kaksi yhdistimiä. Ottaa käyttöön useita yhdistimiä, toista vaiheet 2 ja 3. Kunkin yhdistimen on oltava rekisteröity erikseen.

Jos haluat poistaa yhdistimen asennuksen Connector-palvelun ja Updater-palvelun. Palvelun poistaminen kokonaan tietokone.


## <a name="next-steps"></a>Seuraavat vaiheet

Olet nyt valmiina [sovelluksen välityspalvelimen Julkaise](active-directory-application-proxy-publish.md)-sovellusten ominaisuuksia.

Jos sinulla on erillinen verkko- tai eri sijainneissa olevat sovellukset, yhdistimen ryhmien avulla voit järjestää eri yhdistimet looginen yksiköt. Katso lisätietoja [sovelluksen välityspalvelimen yhdistimien käyttämisestä](active-directory-application-proxy-connectors.md).
