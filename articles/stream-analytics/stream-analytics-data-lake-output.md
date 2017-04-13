<properties
    pageTitle="Virta Analytics järvi tietovaraston tulosteen | Microsoft Azure"
    description="Todennus- ja Azure tietojen järvi säilön Stream Analytics projektin määrittäminen"
    keywords=""
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="stream-analytics-data-lake-store-output"></a>Virta Analytics järvi tietovaraston tulostus

Virta Analytics työt tukevat eri tulostus-vaihtoehtoja toisaalta [Azure Lake Tietosäilölle](https://azure.microsoft.com/services/data-lake-store/). Azure järvi tietosäilö on yritystason hyper-akseli säilö big datasta analyyttisten toiminnoista. Tietosäilö järvi avulla voit tallentaa minkä tahansa kokoa, tyyppi ja nieltynä nopeus toiminnalliset ja kokeilevaa analysoinnissa tietojen.

## <a name="authorize-a-data-lake-store-account"></a>Määritä järvi tietosäilö-tili

1.  Kun järvi tietosäilö on valittu tulos Azure hallinta-portaalissa, sinua pyydetään sallia aiemmin tietojen järvi myymälä käyttö tai tietojen järvi säilön esikatselun kautta Azure perinteinen portaalin käyttöoikeuksien pyytäminen.

    ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  

2.  Jos on jo access järvi tietosäilö, valitse "Määritä nyt" ja lyhyt ajan sivun ponnahtaa ylös, joka ilmaisee "Uudelleenohjaus luvan..". Sivun sulkeutuu automaattisesti, ja voit valita jommankumman, joiden perusteella voit määrittää järvi tietovaraston tulosteen sivulta.

Jos ei ole rekisteröitynyt tietojen järvi säilön Preview-"Rekisteröidy nyt"-linkkiä, jossa pyynnön aloittaminen tai [Hae aloittaminen ohjeita](../data-lake-store/data-lake-store-get-started-portal.md)noudattamalla.

## <a name="configure-the-data-lake-store-output-properties"></a>Tietosäilö järvi tulostus-ominaisuuksien määrittäminen

Kun todennettu järvi tietosäilö-tili, voit määrittää ominaisuuksia järvi tietovaraston tulostusta varten. Seuraavassa taulukossa on luettelo ominaisuuksien nimiä ja niiden kuvaukset määrittäminen järvi tietovaraston tulos.

<table>
<tbody>
<tr>
<td><B>OMINAISUUDEN NIMI</B></td>
<td><B>KUVAUS</B></td>
</tr>
<tr>
<td>Tunnus</td>
<td>Tämä on kutsumanimi, jota käytetään kyselyissä voidaan ohjata tämän tietojen järvi Store kyselyn tuloksiin.</td>
</tr>
<tr>
<td>Tietosäilö järvi tili</td>
<td>Jos haluat lähettää jaettavaan tallennustilan tilin nimi. Voit valita jommankumman kanssa avattavasta luettelosta järvi tietovaraston tileistä, johon kirjautuneena portaaliin käyttäjällä on käyttöoikeus.</td>
</tr>
<tr>
<td>Polun etuliite kuvion [<I>Valinnainen</I>]</td>
<td>Tiedostopolku, Tallenna tiedostosi määritetyn tietojen järvi kaupan tilin avulla. <BR>{date}, {time}<BR>Esimerkki 1: folder1/lokit / {date} / {aika}<BR>Esimerkki 2: folder1/lokit / {date}</td>
</tr>
<tr>
<td>Päivämäärämuoto [<I>Valinnainen</I>]</td>
<td>Jos etuliite polku käytetään päivämäärä-tunnuksen, voit valita päivämäärämuodon jossa tiedostojen järjestetään. Esimerkki: VVVV/KK/PP</td>
</tr>
<tr>
<td>Aikamuoto [<I>Valinnainen</I>]</td>
<td>Jos aika-tunnuksen käytetään etuliite-polku, Määritä Aikamuoto, johon tiedostot on järjestetty. Ainoa tuettu arvo on tällä hetkellä HH.</td>
</tr>
<tr>
<td>Tapahtuman Sarjatoiminto muoto</td>
<td>Sarjatoiminto tulosteen tietojen muoto. JSON, CSV ja Avro tuetaan.</td>
</tr>
<tr>
<td>Koodaus</td>
<td>Jos CSV- tai JSON Muotoile koodauksen on määritetty. UTF-8 on ainoa tuettu koodauksen tiedostomuoto tällä hetkellä.</td>
</tr>
<tr>
<td>Erottimen</td>
<td>Käytettävissä vain CSV Sarjatoiminto. Virta Analytics tukee useita yleisiä erottimet sarjoitettaessa CSV-tiedot. Tuetut arvot ovat pilkku, puolipiste, tila, välilehden ja pystysuora palkki.</td>
</tr>
<tr>
<td>Muotoile</td>
<td>Käytettävissä vain JSON Sarjatoiminto. Erotettu viiva osoittaa, että tulos muotoiltu siten, että kunkin uuden rivin erotettu JSON-objektin. Matriisin määrittää tulosteen muotoillaan JSON objektien matriisina.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Tietosäilö järvi luvan uusiminen

Tällä hetkellä ei rajoitus joissa todennus-tunnuksen on päivitettävä manuaalisesti jokaisen kaikista projekteista, joiden järvi tietovaraston tulosteen 90 päivää. Sinun on myös tarkistamiseen järvi tietovaraston tilin uudelleen, jos olet muuttanut salasanan, koska työ on luotu tai viimeksi todennettu. Tämä ongelma päätyy yleensä on työn tuloksia ja virheestä, joka ilmaisee uudelleen luvan tarvetta toiminnon lokit.

Voit ratkaista ongelman pysäyttää käynnissä olevan projektin ja siirry järvi tietovaraston tulos. "Uusia luvan"-linkki ja lyhyt ajan sivun ponnahtaa ylös, joka ilmaisee "Uudelleenohjaus luvan..". Sivun sulkeutuu automaattisesti ja, jos onnistunut kertovat "Todennus on onnistuneesti uusittu". Voit sitten tarvitse valitsemalla sivun alareunassa "Tallenna" ja voit jatkaa käynnistämällä työtäsi edellisen pysäytetty kerran tietojen menetyksen välttämiseksi.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)
