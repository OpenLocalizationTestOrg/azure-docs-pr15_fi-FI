<properties 
    pageTitle="Vianmääritys Analytics - sovelluksen tiedot tehokkaita hakutyökalun | Microsoft Azure" 
    description="Hakemuksen tiedot analytics ongelmia? Aloita tästä. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/11/2016" 
    ms.author="awills"/>


# <a name="troubleshoot-analytics-in-application-insights"></a>Hakemuksen tiedot Analytics vianmääritys


[Hakemuksen tiedot Analytics](app-insights-analytics.md)ongelmia? Aloita tästä. Analyysin on Visual Studio hakemuksen tiedot tehokas haku-työkalu.



## <a name="limits"></a>Rajoitukset

* Tällä hetkellä kyselytulokset on rajoitettu juuri viimeisten tietojen viikon ajan.
* Olemme testaamista selaimet: uusin Chrome reuna ja Internet Explorer-versioissa.


## <a name="known-incompatible-browser-extensions"></a>Tunnetut ei ole yhteensopiva selaimen tunnisteet

* Ghostery

Tunnisteen poistaminen käytöstä tai käytä toista selainta.


##<a name="e-a"></a>"Odottamaton virhe"

![Odottamaton virhe-näyttö](./media/app-insights-analytics-troubleshooting/010.png)

Sisäinen virhe aikana portaalin runtime – käsittelemättömän poikkeuksen vuoksi.

* Tyhjennä selaimen välimuisti. 

## <a name="e-b"></a>403... Yritä ladata uudelleen

![403... Yritä ladata uudelleen](./media/app-insights-analytics-troubleshooting/020.png)

Todennus, jotka liittyvät virhe (todennuksen aikana tai access-tunnuksen luonnin aikana). Portaalissa on ehkä ei voi palauttaa muuttamatta selaimen asetukset.

* Tarkista selaimessa [kolmannen osapuolen evästeet valintaruudut ovat käytössä](#cookies) . 


## <a name="authentication"></a>403... Varmista vyöhykkeellä

![403.. .verify vyöhykkeellä](./media/app-insights-analytics-troubleshooting/030.png)

Todennus, jotka liittyvät virhe (todennuksen aikana tai access-tunnuksen luonnin aikana). Portaalissa on ehkä ei voi palauttaa muuttamatta selaimen asetukset.

1. Tarkista selaimessa [kolmannen osapuolen evästeet valintaruudut ovat käytössä](#cookies) . 

2. Olet käyttänyt suosikki, kirjanmerkki tai tallennettu linkki avaa Analytics-portaali? Olet kirjautunut sisään eri tunnistetietoja kuin voit käyttää, kun olet tallentanut linkin?

2. Kokeile--yksityinen tai incognito-selainikkunassa (kuten windows Sulje). Sinun on annettava tunnistetiedot. 

2. Avaa toisen (tavallisen) selainikkuna ja siirry [Azure](https://portal.azure.com). Kirjaudu ulos. Avaa linkkiä ja kirjaudu sisään tarvittavat tunnistetiedot.

2. Reunan ja Internet Explorerin käyttäjät pääsevät myös tämän virheen, kun Luotetut vyöhykkeen asetukset eivät ole tuettuja.

    Tarkista [Analytics portal](https://analytics.applicationinsights.io) - ja [Azure Active Directory-portaalin](https://portal.azure.com) ovat samassa vyöhykkeellä:

 * Avaa Internet Explorerin **Internet-asetukset**, **Suojaus**, **Luotetut sivustot**, **sivustot**:

    ![Sivuston lisääminen Luotetut sivustot Internet-asetukset-valintaikkunassa](./media/app-insights-analytics-troubleshooting/033.png)

    Valitse sivustot-luettelosta Jos jokin seuraavista URL-osoitteista on mukana, varmista, että muut sisältyvät myös:

    https://Analytics.applicationinsights.IO<br/>
   https://Login.microsoftonline.com<br/>
   https://Login.Windows.NET


## <a name="e-d"></a>404... Resurssia ei löydy

![404... resurssia ei löydy](./media/app-insights-analytics-troubleshooting/040.png)

Sovelluksen resurssi on poistettu sovelluksen tiedot, ja se ei enää. Näin voi käydä, jos olet tallentanut Analytics-sivun URL-osoite.


## <a name="e-e"></a>403... Lupaa ei

![403... ei saa](./media/app-insights-analytics-troubleshooting/050.png)

Ei ole oikeutta tämän sovelluksen avaaminen Analytics.

* Hankit linkin joltakulta? Pyydä heitä varmistaaksesi, että olet [lukijat ja osallistujien resurssin ryhmän](app-insights-resources-roles-access-control.md).
* Tallensit käyttämällä eri tunnistetietoja linkin? Avaa [Azure portal](https://portal.azure.com), kirjaudu ulos ja yritä sitten tätä linkkiä uudelleen oikeat käyttöoikeudet.

## <a name="html-storage"></a>403... HTML5-tallennustilan

Tutustu portal käyttää HTML5 localStorage ja sessionStorage.

* Chrome: Asetukset-tietosuoja sisältöasetukset.
* Internet Explorer: Internet-asetukset, Lisäasetukset-välilehdessä suojaus, ota selaimen Datatallennus


![403... yrittää HTML5 tallennustilan ottaminen käyttöön](./media/app-insights-analytics-troubleshooting/060.png)

## <a name="e-g"></a>404... Tilaus ei löydy


![404... Tilaus ei löydy](./media/app-insights-analytics-troubleshooting/070.png)

URL-osoite on virheellinen. 

* Avaa app resurssi [Sovelluksen tiedot](https://portal.azure.com)-portaalissa. Käytä Analytics-painike.

## <a name="e-h"></a>404... sivua ei ole

![404... Sivua ei ole](./media/app-insights-analytics-troubleshooting/080.png)

URL-osoite on virheellinen.

* Avaa app resurssi [Sovelluksen tiedot](https://portal.azure.com)-portaalissa. Käytä Analytics-painike.

## <a name="cookies"></a>Kolmannen osapuolen evästeet käyttöön

  Katso, [miten voit poistaa käytöstä kolmannen osapuolen evästeet](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), mutta huomaat annettava **käyttöön** niitä.

## <a name="e-x"></a>Jos mikään muu ei onnistu    

[Ota meihin yhteyttä](app-insights-get-dev-support.md).
 
[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

