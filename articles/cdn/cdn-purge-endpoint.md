<properties
    pageTitle="Tyhjennä Azure CDN päätepisteen | Microsoft Azure"
    description="Opettele Tyhjennä kaikki välimuistiin tallennetut sisältö-CDN päätepiste."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="purge-an-azure-cdn-endpoint"></a>Tyhjennä Azure CDN päätepisteen

## <a name="overview"></a>Yleiskatsaus

Azure CDN reunan solmujen välimuistiin varat, kunnes resurssi--elinaika (TTL) on kulunut.  Kohteen TTL päätyttyä, kun asiakastietokone pyytää kohteen reuna-solmun, reuna-solmu hakee päivitetyn uuden kohteen tukemaan pyyntö ja kaupan välimuistin päivittäminen.

Voi joskus haluat tyhjentää välimuistin sisällön suojatusta kaikki reuna-solmut ja pakottaa niiden noutaa uusia päivitetyt kalusto.  Tämä voi johtua päivitykset web-sovelluksen tai nopeasti päivitys-omaisuus, joka sisältää virheellisiä tietoja.

> [AZURE.TIP] Huomaa, että vain poistetaan tyhjentää välimuistin sisällön CDN reunapalvelimet.  Edeltävät tallentaa, kuten välityspalvelimet ja tallentaa paikallisesta selaimesta edelleen voi pitää tiedoston kopiota.  On tärkeää muistaa, kun määrität tiedoston aika-to-live.  Voit pakottaa edeltävät asiakkaan pyytää tiedoston uusimman version antamalla yksilöllinen nimi aina, kun päivität sen tai hyödyntämistä [kyselymerkkijonon välimuistiin](cdn-query-string.md).  

Tässä opetusohjelmassa esitellään poistetaan kalusto-kaikki reuna-solmut päätepisteen.

## <a name="walkthrough"></a>Vaiheittainen kuvaus

1. Siirry [Azure Portal](https://portal.azure.com)sisältävä haluat tyhjentää päätepisteen CDN profiilin.

2. Valitse Tyhjennä-painike CDN profiili-sivu.

    ![CDN profiili-sivu](./media/cdn-purge-endpoint/cdn-profile-blade.png)

    Tyhjennä-sivu avautuu.

    ![CDN Tyhjennä sivu](./media/cdn-purge-endpoint/cdn-purge-blade.png)

3. Valitse palvelun osoitetta haluat tyhjentää URL avattavasta valikosta Tyhjennä-sivu.

    ![Tyhjennä lomake](./media/cdn-purge-endpoint/cdn-purge-form.png)

    > [AZURE.NOTE] Voit myös siirtyä poisto-sivu napsauttamalla CDN päätepisteen sivu **Tyhjennä** -painiketta.  Tässä tapauksessa **URL** -kenttä on valmiiksi, tietyn päätepisteen service-osoite.

4. Valitse mitä haluat poistaa reunan solmut-omaisuutta.  Jos haluat poistaa kaikkia kohteita, valitse **Tyhjennä kaikki** -valintaruutu.  Kirjoita muutoin kunkin resurssi haluat tyhjentää koko polku (esimerkiksi `/pictures/kitten.png`) **polku** -tekstiruutuun.

    > [AZURE.TIP] Lisää **polku** tekstiruutujen tulee näkyviin, kun kirjoitat tekstiä, jotta voit luoda useita kohteita.  Voit poistaa kohteita luettelosta napsauttamalla kolmea pistettä (…)-painiketta.
    >
    > Polkujen on oltava suhteellinen URL-osoite, jotka sopivat seuraava [säännöllinen lauseke](https://msdn.microsoft.com/library/az24scfc.aspx): `^\/(?:[a-zA-Z0-9-_.\u0020]+\/)*\*$";`.  **Azure-Verizon CDN** (vakio ja Premium), tähti (\*) voidaan käyttää yleismerkkinä (esimerkiksi `/music/*`).  **Azure CDN-Akamai**ei sallita yleismerkkejä ja **Tyhjennä kaikki** .
    
5. Napsauta **Poista** -painiketta.

    ![Tyhjennä-painike](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [AZURE.IMPORTANT] Tyhjennä pyynnöt kestää noin 2 – 3 minuuttia käsittelemään **Azure CDN-Verizon** (vakio ja Premium) ja noin 7 minuuttia **Azure CDN-Akamai**kanssa.  Azure CDN on rajoitukset 50 samanaikaiset pyynnöt Tyhjennä annetun milloin tahansa. 

## <a name="see-also"></a>Katso myös
- [Lataa valmiiksi Azure CDN päätepisteen resurssit](cdn-preload-endpoint.md)
- [Azure CDN REST API-viittaus - Tyhjennä tai lataat valmiiksi päätepisteen](https://msdn.microsoft.com/library/mt634451.aspx)
