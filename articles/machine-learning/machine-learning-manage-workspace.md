<properties
    pageTitle="Hallitse koneen Learning työtilan | Microsoft Azure"
    description="Hallita niiden käyttöä Azure koneen Learning työtilojen ja käyttöön ja hallita ML API-verkkopalvelut"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="manage-an-azure-machine-learning-workspace"></a>Hallitse Azure koneen Learning-työtila

>[AZURE.NOTE] Tässä artikkelissa kuvattujen liittyvät Azure kone Learning perinteinen Web services. Lisätietoja hallinta verkkopalvelut koneen Learning Web Services-portaalissa on kohdassa [Manage Web-palvelu Azure koneen Learning Web Services-portaalissa](machine-learning-manage-new-webservice.md).

Azure perinteinen portaalissa voit hallita tietokoneen Learning työtilat:

- Miten työtilan käytetään valvonta
- Salli tai estä työtilan määrittäminen
- Työtilan luotu Web-palveluiden hallinta
- Työtilan poistaminen

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Koontinäyttö-välilehti on lisäksi yleiskatsaus työtilan käyttö ja silmäyksellä työtilan tietojen.  

> [AZURE.TIP] Azure koneen Learning Studiossa, **WWW-palvelut** -välilehden voit lisätä, päivittää tai poistaa Machine Learning verkkopalvelun.

Voit hallita työtilan seuraavasti:

1.  Kirjaudu sisään käyttämällä Microsoft Azure-tiliä [Azure perinteinen portal](https://manage.windowsazure.com/) - tiliä, joka on liitetty Azure-tilaukseesi.
2.  Napsauta Microsoft Azure palvelut-paneelin **Koneen LEARNING**.
3.  Napsauta työtilaa haluat hallita.

Työtila-sivulla on kolme välilehteä:

- **DASHBOARD** - mahdollistaa työtilan käytön tarkasteleminen ja tiedot
- **Määritä** - avulla voit hallita niiden käyttöä työtilaan
- **VERKKOPALVELUT** - avulla voit hallita verkkopalvelut, jotka on julkaistu tästä työtilasta

## <a name="to-monitor-how-the-workspace-is-being-used"></a>Voit valvoa sitä, kuinka työtilan käytetään

Valitse **KOONTINÄYTTÖ** -välilehti.

Koontinäytöstä voit tarkastella työtilan yleinen käyttö ja saada silmäyksellä työtilan tietojen.

- **Laske** kaavio näyttää käytössä työtilan Laske resurssit. Voit muuttaa näkymään suhteellinen tai absoluuttinen arvot, ja voit muuttaa kaaviossa näkyviä ajankohta.
- **Käyttö yleiskatsaus** näyttää Azure tallennustilan työtilan käytössä.
- **Quick glance** yhteenvedon työtilan tiedot ja hyödyllisiä linkkejä.

> [AZURE.NOTE] **Kirjautuminen ML Studio** -linkki avaa tietokoneen Learning Studio olet kirjautunut sisään Microsoft-Account. Kirjautuminen Azure perinteinen portaaliin työtilan luonnin yhteydessä käyttämäsi Microsoft-Account ei ole oikeutta avata työtilan automaattisesti. Työtilan avaaminen sinun on kirjauduttava Microsoft-Account, joka on määritetty työtilan omistaja tai haluat vastaanottaa kutsun liittymään työtilan omistaja.


## <a name="to-grant-or-suspend-access-for-users"></a>Voit myöntää tai keskeyttää käyttäjien ##

Valitse **määritys** -välilehti.

Valitse määritys-välilehti, tee näin:

- Voit keskeyttää tietokoneen Learning työtilan käyttöoikeus valitsemalla HYLKÄÄ. Käyttäjät eivät enää voi avata työtilan koneen Learning Studiossa. Voit palauttaa access valitsemalla Salli.

Voit hallita tilejä, joka käyttää tietokoneen Learning Studiossa työtilaan valitsemalla **Kirjautuminen ML Studio** **DASHBOARD** -välilehdessä (Katso koskeva **Kirjautuminen ML Studio**edeltävien-Huomautus). Tämä avaa työtilan koneen Learning Studiossa. Tässä näkymässä napsauttamalla **asetukset** -välilehti ja sitten **käyttäjät**. Voit valita portaalisivuston käyttöoikeuksien myöntäminen työtilassa tai valitse käyttäjä ja valitse **Poista** **Kutsu Lisää käyttäjiä** .


## <a name="to-manage-web-services-in-this-workspace"></a>Voit hallita tämän työtilan verkkopalvelut

Valitse **VERKKOPALVELUT** -välilehti.

Tämä näyttää luettelon verkkopalvelut julkaistu tästä työtilasta.
Voit hallita verkkopalvelun valitsemalla Avaa palvelun sivu-luettelosta nimi.

Web-palvelu on ehkä määritetty vähintään yksi päätepisteet.

- Voit määrittää lisää päätepisteet lisäksi "Oletus" päätepiste. Voit lisätä päätepiste valitsemalla **Hallitse päätepisteet** Raporttinäkymät-ikkunan alalaidassa Avaa Azure koneen Learning Web Services-portaali.

- Voit poistaa päätepisteen (ei voi poistaa "Oletus" päätepisteen), päätepisteen rivin alussa oleva valintaruutu ja valitse **Poista**. Tämä poistaa päätepisteen WWW-palvelusta.

    > [AZURE.NOTE] Jos sovellus on käytössä WWW-Palvelupäätepisteen, kun päätepisteen poistetaan, sovellus tulla virhesanoma seuraavan kerran, se yrittää käyttää palvelua.

Napsauta WWW-Palvelupäätepisteen, avaa se nimeä. 

Raporttinäkymät-ikkunan voit tarkastella WWW-palvelun yleinen käyttö ajan kuluessa. Voit valita, jolta haluat nähdä kauden pudotusvalikosta käyttö kaavion oikeassa yläkulmassa. Koontinäytön näkyvät seuraavat tiedot:

- **Pyynnöt tietyllä ajanjaksolla** näkyy pyyntöjen määrä vaiheen päälle valitulle aikajaksolle. Se tunnistaa Jos kohtaat käyttö piikkarit.
- **Pyynnön vastaus pyynnöt** näyttää palvelu on vastaanottanut valitulle aikajaksolle ja kuinka monta ne epäonnistui-vastaus kutsujen määrä.
- **Keskiarvo-vastaus Laske aika** näkyy keskiarvo vastaanotettu pyynnöt suorittamiseen tarvittava aika.
- **Erän pyynnöt** näyttää erä pyyntöjen palvelu on vastaanottanut valitulle aikajaksolle ja kuinka monta ne epäonnistui kokonaismäärä.
- **Keskimääräinen työn odotusaika** näyttää keskimäärin vastaanotettujen pyyntöjen suorittamiseen tarvittava aika.
- **Virheet** näkyvät asennuksen aikana tapahtuneista virheistä kokonaismäärä kutsuja web-palveluun.
- **Palvelujen kustannukset** näyttää kulujen-palveluun liitetyn Laskutus palvelupaketin.

Määrittäminen-sivulla voit päivittää seuraavat ominaisuudet:

* **Kuvaus** antaa WWW-palvelun kuvaus. Kuvaus on muuttaminen pakolliseksi kentäksi.
* **Lokiin kirjaaminen** avulla voit ottaa käyttöön tai poistaa käytöstä päätepisteen virheloki. Lisätietoja kirjaaminen on artikkelissa Ota [lokiin kirjaaminen koneen Learning web Services](machine-learning-web-services-logging.md).
* **Ota käyttöön mallitiedot** ansiosta voit lähettää mallitiedot, jonka avulla voit testata vastaus palvelua. Jos olet luonut WWW-palvelun koneen Learning Studiossa, esimerkkitiedot haetaan tiedot kouluttaminen mallin oman käytetään. Jos olet luonut palvelun ohjelmallisesti, tiedot haetaan antamasi JSON-ohjelmistopaketin osana esimerkkitietoja.

[consume]: machine-learning-consume-web-services.md
[marketplace]: machine-learning-publish-web-service-to-azure-marketplace.md
