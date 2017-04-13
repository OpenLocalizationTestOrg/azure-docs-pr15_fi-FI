<properties
    pageTitle="Hallinta ja yhdistimien ja sovelluksen Service API-sovellusten valvonta | Microsoft Azure"
    description="Näytä suorituskykyä yhdistimien ja API sovellusten logiikan-sovelluksissa microservices arkkitehtuuri"
    services="app-service\logic"
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger"
    manager="anneta"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="mandia"/>

# <a name="manage-and-monitor-your-built-in-api-apps-and-connectors"></a>Hallinta ja valvonta valmiin API-sovellusten ja yhdistimet

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2014 – 12-01 – esikatselu rakenteen versio.

Voit luoda API valmiin sovelluksen. Mitä seuraavaksi?

Azure-tietokannassa joka API-sovellus on erillinen sivuston isännöimät Azure. Tällöin näet helposti montako pyynnöt tehdään ja katso, kuinka paljon tietoja käytetään yhdistin. Voit myös varmuuskopioida API-sovelluksen, Luo ilmoituksia, Tinapaperi suojauksen ottaminen käyttöön ja Lisää käyttäjät ja roolit.

Tässä ohjeaiheessa kuvataan hallittavan API sovelluksen eri vaihtoehtoja.

Saat näitä valmiita ominaisuuksia, Avaa API sovelluksen [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Jos API-sovellus on kohdassa startboard, valitse Ominaisuudet. Voit myös valita **Selaa**, **API**-sovellukset ja valitse sitten Ohjelmointirajapinta sovelluksen:

![][browse]

## <a name="see-the-properties-you-entered"></a>Katso kirjoittamasi ominaisuudet

Avattaessa API-sovellus on useita ominaisuuksia ja tehtävien käytettävissä:

![][settings]

Voit:

- **Asetukset** näkyvät tiettyjen tietojen API-sovellukseen, mukaan lukien Tilaustiedot, ja siinä käyttäjillä on pääsy API-sovelluksen. Voit myös suurentaa tai pienentää asteikko-ominaisuutta, API sovelluksen esiintymien määrän kesken muita ominaisuuksia.
- **Aloittaa** ja **lopettaa** -painikkeiden avulla voit hallita API-sovellus.
- Kun tuotepäivityksiä tehdään Ohjelmointirajapinta sovelluksen käyttämät pohjana olevat tiedostot, voit valita **päivitys** , jotta saat uusimmat versiot. Esimerkiksi jos on korjaus tai Microsoftin julkaisemat suojauspäivitys, valitsemalla **Päivitä** automaattisesti päivittää API sovelluksen sisältämään korjaa.
- Valitse **Muuta suunnitteleminen** päivitys tai artikkelissa tietoliikenteestä API-sovelluksen perusteella. Voit käyttää tätä ominaisuutta myös Nähdäksesi tietojen käyttö.
- Kun luot yhdistin, joka on taulukoita, kuten SQL-yhdistin, voit halutessasi kirjoittaa taulukkonimi, johon yhteys muodostetaan. Rakenne-taulukkoon perustuva on automaattisesti luotu ja käytettävissä, kun napsautat **Lataa rakenteet**. Voit sitten muunto tai kartan luominen tätä ladatut rakennetta.

## <a name="change-your-connector-or-api-configuration-values-you-entered"></a>Yhdistimen tai määrittämäsi API määritysten arvot muuttaminen

Kun määritetty tai että rakennettu yhdistin luotu, voit muuttaa kirjoittamasi arvot. Esimerkiksi jos olet määrittänyt SQL-yhdistin ja haluat muuttaa SQL Server tai taulukkonimi, mahdollisuuksista Tämä API App sivu sitten yhdistimen.

Seuraavat vaiheet:

1. Avaa yhdistimen tai API-sovelluksessa. Kun teet, API sovellus-sivu avautuu.
2. Valitse **Essentials**-kohdassa Host (isäntä)-ominaisuuden hyperlinkin. Hyperlinkin nimeltä jotain, kuten *slackconnector* tai *microsoftsqlconnector123*:

    ![][apiapphost]

3. Valitse Ohjelmointirajapinta App Host (isäntä)-sivu **-asetukset**. Valitse **Sovellusasetukset**asetukset-sivu. Määritysten arvot näkyvät **Sovelluksen asetukset**-kohdassa:

    ![][hostsettings]

4. Valitsemalla haluat muuttaa, anna uusi arvo ja **Tallenna** muutokset.


## <a name="install-the-hybrid-connection-manager---optional"></a>Asenna Hybrid Yhteyksienhallinnan - valinnainen

![][hcsetup]

Hybrid Yhteyksienhallinnan antaa yhteyden paikalliseen järjestelmään, kuten SQL Server tai SAP. Tämä hybrid yhdistämispalvelu Azure palvelun Bus Muodosta yhteys ja suojattuina Azure resurssit ja paikallisen resurssien välillä.

Katso [Hybrid Yhteyksienhallinnan Azure sovelluksen-palvelussa](app-service-logic-hybrid-connection-manager.md).

> [AZURE.NOTE] Hybrid Yhteyksienhallinnan vaaditaan vain, jos olet muodostamassa yhteyttä paikalliseen resurssin palomuurin takana. Jos olet muodostamassa ei paikalliseen järjestelmään, Hybrid Yhteyksienhallinnan ei on lueteltu connector-sivu.

## <a name="monitor-the-performance"></a>Valvoa suorituskykyä
Suorituskyvyn mittarit ovat valmiita ominaisuuksia ja jokaisen API-sovelluksen, voit luoda mukana. Nämä arvot ovat API-sijaitseva Azure-sovellus. Esimerkki arvot:

![][monitoring]

Voit:

- Valitse **pyynnöt ja virheet** lisätä eri suorituskyvyn mittarit, mukaan lukien yleisesti tunnetut HTTP-virhekoodit, kuten 200, 400 tai 500 HTTP-tilakoodin. Voit myös tarkastella vastauksen aikoja, katso, kuinka monta pyynnöt tehdään API-sovellukseen ja katso, kuinka paljon tietoja on ja miten paljon tietoja käy. Perusteella suorituskyvyn mittarit, voit luoda sähköpostin ilmoitukset Jos mittarin kynnysarvo valitsemaasi sähköpostikansioon.
- Näet **käyttö**, kuinka paljon **Suoritinta** käytetään API-sovelluksessa, tarkista nykyisen **Käyttökiintiö** megatavuina ja katso suurin tietojen käytön kustannukset-tason perusteella. **Arvioitu käyttävät** auttavat selvittämään mahdolliset kustannukset API-sovellusta.
- Valitse **prosessit** Avaa Process Explorerista. Tämä näyttää web-esiintymät ja niiden ominaisuudet, mukaan lukien viestiketjun Laske- ja muistin käyttö.

Näillä työkaluilla voit määrittää Jos App palvelun suunnitteleminen pitäisi olla skaalata tai skaalata alas ja yrityksesi tarpeiden mukaan. Nämä ominaisuudet ovat valmiita portaaliin ei ole muita työkaluja, pakollinen.

## <a name="control-the-security"></a>Suojattuina

API-sovellusten käyttäminen Roolipohjainen suojaus. Nämä roolit koskevat koko Azure ratkaisun, mukaan lukien API-sovellusten ja Azure resursseihin. Roolit ovat seuraavat:

Rooli | Kuvaus
--- | ---
Omistaja | On täydet oikeudet hallintatoimintojen ja antaa muille käyttäjille tai ryhmille.
Avustaja | On täydet oikeudet hallintatoimintojen. Access ei voi antaa muille käyttäjille tai ryhmille.
Lukija | Voit tarkastella kaikkien resurssien tietoja lukuun ottamatta.
Käyttäjän Access-järjestelmänvalvoja | Voit tarkastella kaikkien resurssien, luoda ja hallita roolit ja luoda ja hallita tuki liput.

Katso [Roolipohjainen käyttöoikeuksien valvonta Microsoft Azure-portaalissa](../active-directory/role-based-access-control-configure.md).

Voit lisätä käyttäjiä ja määrittää niitä tiettyjen roolit API-sovellukseen. Portaalin Näyttää käyttäjät, joilla on käyttöoikeudet ja niiden määritetty roolia:

![][access]  

- Valitse Lisää käyttäjä, Määritä rooli ja poistaa käyttäjän **käyttäjät** .
- Valitse rooli, kaikkien käyttäjien näkevän **roolit** käyttäjän lisääminen rooli ja poistaa käyttäjän rooli.


## <a name="more-good-stuff"></a>Lisää hyvä tiedostot
- Valitse Avaa Swagger tiedoston automaattisesti luotu tietyn Ohjelmointirajapinta sovelluksen **Ohjelmointirajapinta määritys** .
- Valitse **riippuvuuksien** vaatimat API-sovelluksen tiedostot. Jos käytät SAP-yhdistin, olet asentanut esimerkiksi joitakin muita tiedostoja, valitse paikallisen Hybrid yhteyden hallinta. Riippuvuussuhteita näkyvät API-sovelluksen sivu.

>[AZURE.IMPORTANT] Kun avaat API-sovelluksen ominaisuudet ja Etsi **Essentials**-liittyy **Host** ja **yhdyskäytävän** linkeistä, jotka avaavat uuden lavat:
>
> ![][host]
>
>Nämä ominaisuudet ovat tiettyyn sivustoon, joka isännöi API-sovellus. Valmiin API-sovelluksen tai yhdistin käytettäessä useimmat ominaisuuksia ei käytetä todella ja suosittelemme, että et Päivitä nämä ominaisuudet. Jos olet luonut API-sovelluksen Visual Studiossa ja ottaa käyttöön Azure tilauksen, voit käyttää isännän ja yhdyskäytävän näiden. <br/><br/>


>[AZURE.NOTE] Aloita logiikan sovellukset ennen rekisteröimässä Azure-tili, siirry [Yritä logiikan-sovellukseen](https://tryappservice.azure.com/?appservice=logic). Voit luoda lyhytkestoinen starter logiikan-sovelluksen. Ei ole pakollinen luottokortit ja ei sitoumukset.

## <a name="read-more"></a>Lisätietoja on artikkelissa

[Logiikan sovellusten valvonta](app-service-logic-monitor-your-logic-apps.md)<br/>
[Yhdistimien ja sovelluksen Service API sovellusten luettelo](app-service-logic-connectors-list.md)<br/>
[Roolipohjainen käyttöoikeuksien valvonta Microsoft Azure-portaalissa](../active-directory/role-based-access-control-configure.md)<br/>
[Hybrid Yhteyksienhallinnan käyttäminen Azure sovelluksen-palvelu](app-service-logic-hybrid-connection-manager.md)


<!--Image references-->
[browse]: ./media/app-service-logic-monitor-your-connectors/browse.png
[settings]: ./media/app-service-logic-monitor-your-connectors/settings.png
[hcsetup]: ./media/app-service-logic-monitor-your-connectors/hcsetup.png
[monitoring]: ./media/app-service-logic-monitor-your-connectors/monitoring.png
[access]: ./media/app-service-logic-monitor-your-connectors/access.png
[host]: ./media/app-service-logic-monitor-your-connectors/host.png
[hostsettings]: ./media/app-service-logic-monitor-your-connectors/hostsettings.png
[apiapphost]: ./media/app-service-logic-monitor-your-connectors/apiapphost.png
