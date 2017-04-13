<properties 
   pageTitle="Korvaa StorSimple laitteen ohjaimen | Microsoft Azure"
   description="Kerrotaan, miten voit poistaa ja korvata toisen tai molemmat ohjauskoneen moduulit StorSimple laitteen."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Korvaa StorSimple laitteen ohjain moduuli

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kerrotaan, miten voit poistaa ja korvata toisen tai molemmat ohjauskoneen moduulit StorSimple-laitteessa. Raportissa käsitellään myös yksittäisten ja kahden ohjauskoneen korvaavan käyttötavoista pohjana logiikan.

>[AZURE.NOTE] Ennen suorittamiseen ohjauskoneen korvaa, suosittelemme, että päivität ohjauskoneen laitteisto aina uusimpaan versioon.
>
>Jotta vioittuneet StorSimple laitteeseen, ei poista ohjaimen, kunnes merkkivalot näytetään jompikumpi seuraavista:
>
>- Kaikki valot on ei käytössä.
>- LED 3 ![vihreä valintamerkki-kuvake](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), ja ![punainen rajat kuvake](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) ovat vilkkuvat ja LED 0 ja LED 7 ovat **käytössä**.

Seuraavassa taulukossa on tuettua ohjainta korvaavan skenaarioita.

|Kirjainkoko|Korvaava-skenaario|Sovellettava toimintosarja|
|:---|:-------------------|:-------------------|
|1|Yhden ohjauskoneen on epäonnistunut tilassa, muut ohjauskoneen on kunnossa ja aktiivinen.|[Yksittäisen ohjauskoneen korvaavan](#replace-a-single-controller), jossa kuvataan [logiikan takana yhden ohjauskoneen korvaa](#single-controller-replacement-logic)sekä [korvaavan vaiheet](#single-controller-replacement-steps).|
|2|Molemmat ohjaimet epäonnistui ja vaatii korvaavan. Alustan, levyjen and.disk kehyksen on kunnossa.|[Kahden ohjauskoneen korvaavan](#replace-both-controllers), jossa kuvataan [logiikan takana kahden ohjauskoneen korvaa](#dual-controller-replacement-logic)sekä [korvaavan vaiheet](#dual-controller-replacement-steps). |
|3|Ohjaimet samaan laitteeseen tai eri laitteissa on vaihtaa paikkaa. Alustan, levyjen ja levyn liitteet ovat kunnossa.|Paikan ristiriita ilmoitussanoma tulee näkyviin.|
|4|Yhden ohjauskoneen puuttuu ja muut ohjauskoneen epäonnistuu.|[Kahden ohjauskoneen korvaavan](#replace-both-controllers), jossa kuvataan [logiikan takana kahden ohjauskoneen korvaa](#dual-controller-replacement-logic)sekä [korvaavan vaiheet](#dual-controller-replacement-steps).|
|5|Toinen tai molemmat ohjaimet epäonnistui. Et voi käyttää laitteen serial konsolin tai etäpalvelujen Windows PowerShellin kautta.|[Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md) manuaalinen ohjauskoneen korvaavan Ohje.|
|6|Ohjaimet on eri muodosta-versiota, joka syy voi olla:<ul><li>Ohjaimet on eri ohjelmistoversio.</li><li>Ohjaimet on eri laitteisto-versio.</li></ul>|Jos ohjauskoneen ohjelmistoversiot ovat erilaiset, korvaava-logiikkaa havaitsee, että ja päivittää korvaava-ohjaimen versio.<br><br>Jos ohjauskoneen laitteisto-versiot ovat eri ja laiteohjelmiston vanha versio ei **ole** näkyvät automaattisesti laajennettavat ilmoitussanoma Azure perinteinen portaalin. Kannattaa tarkistaa päivitykset ja asenna laitteisto-päivitykset.</br></br>Jos ohjauskoneen laitteisto-versiot ovat eri ja laiteohjelmiston vanha versio on automaattisesti laajennettavat, ohjauskoneen korvaavan logiikan havaitsee tämän ja ohjaimen käynnistyttyä laitteisto päivitetään automaattisesti.|

Sinun täytyy poistaa ohjauskoneen moduuli, jos se ei onnistunut. Toinen tai molemmat ohjauskoneen moduulit saattaa epäonnistua, joka voi johtaa yhden ohjauskoneen korvaaminen tai kahden ohjauskoneen korvaavan. Korvaava-ohjeita ja taakse logiikkaa on seuraavissa artikkeleissa:

- [Korvaa yhden ohjain](#replace-a-single-controller)
- [Korvaa molemmat ohjaimet](#replace-both-controllers)
- [Poista ohjain](#remove-a-controller)
- [Lisää ohjain](#insert-a-controller)
- [Määritä aktiivinen ohjauskoneen laitteessasi](#identify-the-active-controller-on-your-device)

>[AZURE.IMPORTANT] Poistaminen ja korvaaminen valvonta on ennen [StorSimple laitteiston osan korvaava](storsimple-hardware-component-replacement.md)-arviointitietoja turvallisuus.

## <a name="replace-a-single-controller"></a>Korvaa yhden ohjain

Kun jokin Microsoft Azure StorSimple laitteeseen kahden ohjauskoneen epäonnistuu, on virheellisesti tai puuttuu, haluat korvata yhden ohjauskoneen. 

### <a name="single-controller-replacement-logic"></a>Yksittäisen ohjauskoneen korvaavan logiikka

Yksittäisen ohjauskoneen, korvaa poistettava ensin epäonnistui ohjauskoneen. (Jäljellä oleva ohjauskoneen laitteessa on aktiivinen ohjauskoneen.) Kun lisäät korvaava-ohjain, seuraavat toiminnot tapahtuvat:

1. Korvaava-ohjain käynnistyy heti yhteydessä StorSimple-laitteen kanssa.

2. Tilannevedoksen virtual kiintolevyn (Näennäiskiintolevyn) active ohjaimen kopioidaan korvaavan ohjaimen.

3. Näyttökuvan on muutettu niin, että korvaava-ohjain käynnistyessä-tämän Näennäiskiintolevyn se tunnistaa valmiustilassa ohjauskoneen.

4. Kun muutokset ovat valmiit, korvaava-ohjain alkaa valmiustilassa ohjauskoneen nimellä.

5. Kun molemmat ohjaimet ovat käynnissä, klusterin kirjautuu.

### <a name="single-controller-replacement-steps"></a>Yksittäisen ohjauskoneen korvaavan vaiheet

Tehtävä seuraavat toimet, jos jokin Microsoft Azure StorSimple laitteen ohjaimet epäonnistuu. (Muut ohjauskoneen on oltava aktiivinen ja käytössä. Jos molemmat ohjaimet epäonnistua tai virheellisesti, siirry [kahden ohjauskoneen korvaavan ohjeita](#dual-controller-replacement-steps).)

>[AZURE.NOTE] Saattaa kestää 30 – 45 minuuttia ohjaimen uudelleen ja palauttaminen kokonaan yhden ohjauskoneen korvaavan kuvatulla tavalla. Koko menettely kokonaisaika liittäminen kaapeli, mukaan lukien on noin 2 tuntia.

#### <a name="to-remove-a-single-failed-controller-module"></a>Jos haluat poistaa yhden epäonnistui ohjauskoneen moduuli

1. StorSimple hallintapalvelu Siirry Azure perinteinen portal, **laitteet** -välilehti ja valitse laite, jota haluat seurata nimi.

2. Siirry **ylläpito > laitteiston tila**. Controller 0 tai 1 ohjauskoneen tilan on oltava punainen, joka ilmaisee epäonnistui.

    >[AZURE.NOTE] Korvaa yhden ohjauskoneen epäonnistui ohjauskoneen on aina valmiustilassa ohjauskoneen.

3. Kuva 1 ja seuraavan taulukon avulla voit paikantaa epäonnistui ohjauskoneen moduuli.  

    ![Backplane-liitäntä laitteen ensisijainen kehyksen moduulit](./media/storsimple-controller-replacement/IC740994.png)

    **Kuva 1** Taustapuolta StorSimple laite

  	|Otsikko|Kuvaus|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Controller 0|
  	|4|Ohjaimen 1|

4. Epäonnistuneiden ohjaimen Poista kaikki yhdistetyn verkkokaapelit tietoliikenteen portit. Jos käytössäsi on 8600: aa mallin, myös poistaa SAS kaapeli, jotka muodostavat ohjaimen EBOD valvojalle.

5. Voit poistaa vioittuneen ohjauskoneen [poistaminen valvonta on](#remove-a-controller) noudattamalla. 

6. Asenna factory korvaaminen saman paikka, josta epäonnistui ohjauskoneen on poistettu. Tämä käynnistää yhden ohjauskoneen korvaava-logiikkaa. Lisätietoja on artikkelissa [yhden ohjauskoneen korvaavan logiikan](#single-controller-replacement-logic).

7. Kun yksi ohjauskoneen korvaavan logiikan etenee taustalla, Yhdistä kaapeli. Huolehtia muodostaa kaikki kaapeli täsmälleen samalla tavalla, että ne on yhdistetty ennen korvaa.

8. Ohjaimen uudelleenkäynnistyksen jälkeen valitse **ohjauskoneen tila** ja **klusterin tila** Azure perinteinen portaalissa varmistaa, että ohjain on takaisin kunnossa tilaan ja on valmiustilassa.

>[AZURE.NOTE] Jos ovat seuranta sarja-konsolin kautta laitteen, näkyviin voi tulla useita käynnistyy, kun ohjaimen palautuu korvaava-toimintosarjan. Kun esitetään serial konsoli-valikko, sitten tiedät, että korvaa on valmis. Jos valikossa ei näy aloittamisesta ohjauskoneen korvaaminen kahden tunnin kuluessa, ota yhteyttä [Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md).

## <a name="replace-both-controllers"></a>Korvaa molemmat ohjaimet

Kun molemmat ohjaimet Microsoft Azure StorSimple laitteeseen epäonnistui, on virheellisesti tai puuttuu, sinun täytyy korvaa molemmat ohjaimet. 

### <a name="dual-controller-replacement-logic"></a>Kahden ohjauskoneen korvaavan logiikka

Kahden ohjauskoneen korvaa Poista ensin molemmat epäonnistui ohjaimet ja lisää korvauksia. Kun kaksi korvaava-ohjaimet lisätään, seuraavat toiminnot tapahtuvat:

1. Paikan 0 korvaava-ohjain tarkistaa seuraavasti:
 
   1. Se käyttää laitteisto ja ohjelmiston nykyiset versiot?

   2. Onko se klusterin osa?

   3. On käynnissä peer ohjauskoneen ja on liitetty?
                            
    Jos mikään ehdot ovat tosia, ohjaimen etsii uusimman päivittäinen varmuuskopiointi (sijaitsevat **nonDOMstorage** asemassa S). Ohjaimen kopioi Näennäiskiintolevyn uusimman tilannevedoksen varmuuskopion.

2. Paikan 0 ohjain käyttää tilannevedoksen kuvan itse.

3. Tänä aikana paikan 1 ohjaimen odottaa controller 0 imaging ja Käynnistä-painiketta.

4. Controller 0 käynnistyttyä ohjauskoneen 1 tunnistaa klusterin luoma controller 0, joka tuottaa yhden ohjauskoneen korvaava-logiikkaa. Lisätietoja on artikkelissa [yhden ohjauskoneen korvaavan logiikan](#single-controller-replacement-logic).

5. Jälkeenpäin sekä ohjaimet oltava käynnissä ja klusterin toimitetaan online.

>[AZURE.IMPORTANT] Seuraavat kaksi ohjauskoneen, korvaa jälkeen StorSimple laite on määritetty, on tärkeää tehdä manuaalinen varmuuskopiointi laitteen. Päivittäinen laitteen määritysten varmuuskopiot ei käynnistää vasta sen jälkeen, kun on kulunut 24 tuntia. [Microsoft-tuen](storsimple-contact-microsoft-support.md) ja varmuuskopioi laitteen manuaalinen käsitteleminen.

### <a name="dual-controller-replacement-steps"></a>Kahden ohjauskoneen korvaavan vaiheet

Tämä työnkulku tarvitaan, kun toinen Microsoft Azure StorSimple laitteen ohjaimet epäonnistuu. Tämä voi johtua palvelinkeskukseen, jossa jäähdytysjärjestelmä lakkaa toimimasta ja tuloksena molemmat ohjaimet hylätty lyhyen ajan kuluessa. Sen mukaan, onko StorSimple laite on otettu käyttöön tai poistaminen käytöstä ja 8600 käytätkö tai 8100 mallin samat vaiheet on pakollinen.

>[AZURE.IMPORTANT] 45 minuuttia voi kestää, Käynnistä uudelleen ja palauttaminen kokonaan kahden ohjauskoneen korvaavan ohjeen ohjaimen 1 tunti. Kokonaisaika koko ohje on myös liittämistä kaapeli on noin 2,5 tuntia.

#### <a name="to-replace-both-controller-modules"></a>Jos haluat korvata sekä ohjain moduulit

1. Jos laite on poistettu käytöstä, Ohita tämä vaihe ja siirry seuraavaan vaiheeseen. Jos laite on otettu käyttöön, poistaminen käytöstä laite.
                                        
    1. Jos käytössäsi on 8600-mallin, ensimmäinen ensisijainen kehyksen poistaminen käytöstä ja poista käytöstä EBOD kehys.

    2. Odota, kunnes laite on Sammuta kokonaan. Kaikki laitteen taustalla merkkivalot ovat käytöstä.

2. Poista kaikki, jotka ovat yhteydessä tietoliikenteen portit verkkokaapelit. Jos käytössäsi on 8600: aa mallin, myös poistaa SAS kaapeli, joka muodostaa EBOD kehyksen ensisijainen kehys.

3. Poista molemmat ohjaimet StorSimple laitteesta. Lisätietoja on artikkelissa [valvonta on poistettava](#remove-a-controller).

4. Lisää ensin factory korvaa Controller 0 ja lisää ohjauskoneen 1. Lisätietoja on artikkelissa [valvonta on Lisää](#insert-a-controller). Tämä käynnistää kahden ohjauskoneen korvaava-logiikkaa. Lisätietoja on artikkelissa [kahden ohjauskoneen korvaavan logiikan](#dual-controller-replacement-logic).

5. Kun ohjauskoneen korvaavan logiikan etenee taustalla, Yhdistä kaapeli. Huolehtia muodostaa kaikki kaapeli täsmälleen samalla tavalla, että ne on yhdistetty ennen korvaa. Katso tarkat ohjeet mallin kaapelin laite-osassa [Asenna StorSimple 8100 laitteen](storsimple-8100-hardware-installation.md) tai [asentaa StorSimple 8600 laitteen](storsimple-8600-hardware-installation.md).

6. Ota StorSimple laitteeseen. Jos käytössäsi on 8600-malliin:

    1. Varmista, että EBOD kehyksen on otettu käyttöön ensimmäisen.

    2. Odota, kunnes EBOD kehys on käynnissä.

    3. Ota perus kehys.

    4. Kun ensimmäisen ohjauskoneen käynnistyy ja kunnossa tilassa, järjestelmä toimii.

    >[AZURE.NOTE] Jos ovat seuranta sarja-konsolin kautta laitteen, näkyviin voi tulla useita käynnistyy, kun ohjaimen palautuu korvaava-toimintosarjan. Kun serial console-valikko avautuu, valitse tiedät korvaaminen on valmis. Jos valikossa ei näy aloittamisesta ohjauskoneen korvaaminen 2,5 tunnin kuluessa, ota yhteyttä [Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md).

## <a name="remove-a-controller"></a>Poista ohjain

Seuraavien ohjeiden avulla voit poistaa StorSimple laitteen viallinen ohjain moduuli.

>[AZURE.NOTE] Seuraavat kuvat ovat controller 0. Ohjaimen 1 ne voi kumota.

#### <a name="to-remove-a-controller-module"></a>Jos haluat poistaa ohjauskoneen moduuli

1. Vaikealta moduulin salvan napin ja etusormen välillä.

2. Hyödyntää varovasti napin ja etusormen yhteen, kun haluat vapauttaa ohjauskoneen salvan.

    ![Ohjaimen salvan vapauttaminen](./media/storsimple-controller-replacement/IC741047.png)

    **Kuva 2** Ohjaimen salvan vapauttaminen

2. Dian loitontaminen alustan ohjauskoneen hoitamaan salvan avulla.

    ![Liukuva ohjauskoneen alustan ulos](./media/storsimple-controller-replacement/IC741048.png)

    **Kuva 3** Liukuva ulos alustan ohjain

## <a name="insert-a-controller"></a>Lisää ohjain

Asenna factory toimittaman ohjauskoneen moduuli poistettuasi viallinen moduuli StorSimple laitteesta seuraavien ohjeiden avulla.

#### <a name="to-install-a-controller-module"></a>Valvonta-moduulin asentaminen

1. Tarkista, onko vahinkojen käyttöliittymän yhdysviivat. Älä asenna moduuli, jos jokin yhdistimen PIN on vahingoittunut tai taipuneet.

2. Siirrä ohjauskoneen moduulin alustan samalla, kun salvan täysin julkaistu. 

    ![Liukuva ohjauskoneen alustan tuominen](./media/storsimple-controller-replacement/IC741053.png)

    **Kuva 4** Liukuva ohjauskoneen alustan tuominen

3. Lisätty ohjauskoneen moduuliin alkaa sulkeminen salvan samalla, kun push ohjauskoneen moduulin alustan kyselyjä. Salvan osallistuminen apuviivaan ohjaimen haluamaasi kohtaan.

    ![Ohjaimen salvan sulkeminen](./media/storsimple-controller-replacement/IC741054.png)

    **Kuva 5** Ohjaimen salvan sulkeminen

4. Olet valmis, kun salvan kohdistuu haluamaasi kohtaan. **OK** -LED pitäisi nyt olla.  

    >[AZURE.NOTE] Voi kestää jopa 5 minuuttia ohjaimen ja aktivoi LED.

5. Vahvista korvaaminen on tarkistettu, Azure perinteinen-portaalissa, siirry **laitteet** > **ylläpito** > **Laitteiston tila**ja varmista, että controller 0 ja 1 ohjauskoneen ovat kunnossa (tila on vihreä).

## <a name="identify-the-active-controller-on-your-device"></a>Määritä aktiivinen ohjauskoneen laitteessasi

Monia tilanteita, kuten uusille laitteen rekisteröinti tai controller korvaava-, jotka edellyttävät Etsi aktiivinen ohjauskoneen StorSimple laitteessa. Aktiivinen ohjauskoneen käsittelee kaikki levyn laitteisto ja tuomasta toiminnot. Aktiivinen ohjauskoneen tunnistavan jollakin seuraavista tavoista:

- [Azure perinteinen portaalin avulla voit selvittää aktiivinen ohjain](#use-the-azure-classic-portal-to-identify-the-active-controller)

- [Windows PowerShell StorSimple avulla voit määrittää aktiivisen ohjain](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)

- [Tarkista fyysinen laite tunnistaa active ohjain](#check-the-physical-device-to-identify-the-active-controller)

Kaikki seuraavat toimenpiteet on kuvattu Seuraava.

### <a name="use-the-azure-classic-portal-to-identify-the-active-controller"></a>Azure perinteinen portaalin avulla voit selvittää aktiivinen ohjain

Siirry Azure perinteinen portal **laitteet** > **ylläpito**ja vieritä **ohjaimet** -osassa. Tässä voit tarkistaa, mitkä ohjauskoneen on aktiivinen.

![Määritä aktiivinen ohjauskoneen Azure perinteinen-portaalissa](./media/storsimple-controller-replacement/IC752072.png)

**Kuva 6** Azure active ohjauskoneen näyttäminen perinteinen portal

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Windows PowerShell StorSimple avulla voit määrittää aktiivisen ohjain

Kun käytät laitteesi sarja-konsolin kautta, nauha viestin esitetään. Ilmoituspalkin viesti sisältää laitteen tiedot, kuten mallin nimi, asennettujen ohjelmistoversio ja ohjauskoneen, kun käytät tila. Seuraavassa kuvassa on esimerkki nauha viestin:

![Sarja nauha-viesti](./media/storsimple-controller-replacement/IC741098.png)

**Kuva 7** Sanoman näyttäminen controller 0 aktiiviseksi juliste

Voit selvittää, onko yhteys on muodostettu ohjauskoneen aktiivisen tai passiivisen nauha-viesti.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Tarkista fyysinen laite tunnistaa active ohjain

Laji laitteen aktiivinen ohjauskoneen, Etsi sininen LED yläpuolella ensisijainen kehyksen taustalle tietojen 5-porttiin.

Jos tämä LED vilkkuvaa-ohjain on aktiivinen ja muiden ohjauskoneen on valmiustilassa. Käytä seuraavaa kaavio ja taulukon apuna.

![Laitteen ensisijainen kehyksen backplane-liitäntä dataporteista kanssa](./media/storsimple-controller-replacement/IC741055.png)

**Kuva 8** Ensisijainen kehyksen tietoliikenteen portit ja seurantaa merkkivalot taustapuolta

|Otsikko|Kuvaus|
|:----|:----------|
|1-6|TIETOJA 0 – 5 verkkoportit|
|7|Sininen LED|


## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [StorSimple laitteiston osan korvaaminen](storsimple-hardware-component-replacement.md).
