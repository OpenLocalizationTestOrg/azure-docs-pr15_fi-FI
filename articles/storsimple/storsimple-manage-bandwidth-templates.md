<properties
   pageTitle="StorSimple kaistanleveyden-mallien hallinta | Microsoft Azure"
   description="Tässä artikkelissa käsitellään hallinta StorSimple kaistanleveyden mallit, joiden avulla voit hallita kaistanleveyden käyttöä."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storsimple-bandwidth-templates"></a>StorSimple hallinta-palvelun avulla voit hallita StorSimple kaistanleveyden mallit

## <a name="overview"></a>Yleiskatsaus

Kaistanleveyden mallien avulla voit määrittää verkon kaistanleveyden käytön useita päivän ajan aikataulut ja taso StorSimple laitteen tiedot pilvipalveluun yli.

Kaistanleveyden rajoittimen aikataulujen avulla voit:

- Määritä mukautettu kaistanleveyden aikatauluja työmäärää verkon käytön mukaan.

- Keskittää hallinta ja käyttää aikatauluja yli useilla eri laitteilla helposti ja saumattomasti tavalla.

> [AZURE.NOTE] Tämä ominaisuus on käytettävissä vain StorSimple fyysisiä laitteita, ei virtual laitteet.

Kaikki kaistanleveyden palvelun mallit näkyvät taulukkomuodossa ja sisältää seuraavat tiedot:

- **Nimi** – liitetään kaistanleveyden mallia, kun se on luotu yksilöllinen nimi.

- **Aikataulun** – aikatauluja annetun kaistanleveyden mallin sisältämien määrä.

- **Käytön mukaan** – tietomääristä käyttämällä kaistanleveyden malleja määrä.

Käytät StorSimple hallinta-palvelun **määrittäminen** -sivulla Azure perinteinen portaalissa kaistanleveyden mallien hallinta.

Löytyvät myös muita tietoja, jotka auttavat määrittäminen kaistanleveyden mallit:

- Kaistanleveyden malleja liittyviä kysymyksiä ja vastauksia
- Mallien kaistanleveyden parhaat käytännöt

## <a name="add-a-bandwidth-template"></a>Lisää kaistanleveyden malli

Seuraavien toimien kaistanleveyden uuden mallin luominen.

#### <a name="to-add-a-bandwidth-template"></a>Jos haluat lisätä kaistanleveyden mallia

1. Valitse StorSimple hallinta-palvelun **määrittäminen** -sivulla **Lisää/Muokkaa kaistanleveyden mallia**.

2. **Lisää/Muokkaa kaistanleveyden malli** -valintaikkunassa:

   1. **Malli** -avattavasta luettelosta valitsemalla **Luo uusi** Lisää kaistanleveyden mallina.
   2. Määritä kaistanleveyden mallin yksilöllinen nimi.

3. Määritä **kaistanleveyden aikataulu**. Aikataulun luominen

   1. Valitse avattavasta luettelosta päivän, viikon aikataulun on määritetty. Voit valita useita päiviä valitsemalla sijoittaa ennen luettelon vastaaviin päivien valintaruudut.
   2. Valitse **Koko päivä** -vaihtoehto, jos aikataulun pakotetaan koko päivän. Kun tämä asetus on valittuna, voit määrittää **Aloitusaika** tai **Päättymisaika**ei ole enää. Aikataulun suorittaa klo 24.00 23:59:00.
   3. Avattavasta luettelosta Valitse **Aloitusaika**. Tämä on kun aikatauluun alkaa.
   4. Valitse avattavasta luettelosta ei ole **Päättymisaika**. Tämä on kun aikatauluun lopetetaan.

         > [AZURE.NOTE] Päällekkäisten aikatauluja, ei sallita. Jos alkamis- ja päättymisajat johtaa päällekkäisiä aikataulu, näet virhesanoman asiasta.

   5. Määritä **kaistanleveyden korko**. Tämä on kaistanleveys megabitteinä sekunnissa (Mbps) käyttämä StorSimple laitteen toimia pilveen (sekä keskeytetyt lataukset). Anna numero väliltä 1 – 1 000 tälle kentälle.

   6. Valitse tarkistuksen kuvake ![tarkistuksen kuvake](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Malli, jonka olet luonut lisätään kaistanleveyden malliluettelon **määritys** -sivulle.

    ![Luo uusi kaistanleveyden malli](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)

4. Valitse sivun alalaidassa **Tallenna** ja valitse sitten **Kyllä** , kun ohjelma pyytää vahvistusta. Tallennetaan, jotka olet tehnyt määritysmuutoksia.

## <a name="edit-a-bandwidth-template"></a>Muokkaa kaistanleveyden mallia

Seuraavien toimien Muokkaa kaistanleveyden mallina.

### <a name="to-edit-a-bandwidth-template"></a>Jos haluat muokata kaistanleveyden-malli

1. Valitse **Lisää/Muokkaa kaistanleveyden malli**.

2. **Lisää/Muokkaa kaistanleveyden malli** -valintaikkunassa:

   1. Valitse **malli** -pudotusvalikosta kaistanleveyden mallia, jota haluat muokata.
   2. Tee haluamasi muutokset. (Voit muokata aiemmin luotuja asetuksia.)
   3. Valitse Tarkista-kuvake ![Tarkistuksen kuvake](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Näet kaistanleveyden malliluettelon muokatun mallin määrittäminen-sivulle.

3. Tallenna muutokset valitsemalla sivun alareunassa **Tallenna** . Valitse **Kyllä** , kun ohjelma pyytää vahvistusta.

> [AZURE.NOTE] Sinun ei sallita Tallenna tekemäsi muutokset, jos muokatun aikatauluun päällekkäinen aiemmin aikataulu, jota olet päivittämässä kaistanleveyden mallissa.

## <a name="delete-a-bandwidth-template"></a>Kaistanleveyden mallin poistaminen

Seuraavien toimien poistaminen kaistanleveyden mallia.

#### <a name="to-delete-a-bandwidth-template"></a>Jos haluat poistaa kaistanleveyden-malli

1. Valitse malli, jonka haluat poistaa taulukkomuotoinen palvelun kaistanleveyden mallit-luettelosta. Poista-kuvaketta (**x**) näkyy erittäin oikealla puolella valittu malli. Valitse poistettavan mallin **x** -kuvaketta.

2. Voit pyydetään vahvistamaan. Jatka valitsemalla **OK** .

Jos malli on käyttää mitä tahansa asema(t), sinun ei sallita poista se. Näyttöön tulee virhesanoma, joka ilmaisee, että malli on käytössä. Virhe viesti-valintaikkuna tulee näkyviin kehotus, että kaikki malliin viittaukset poistetaan.

Voit poistaa kaikki malliin viittaukset käyttäminen **Aseman säilöt** -sivulle ja muokkaamalla aseman säilöt, jotka käyttävät tätä mallia, niin, että ne käyttää toiseen malliin tai mukautetun tai rajoittamaton kaistanleveyden-asetuksen avulla. Kun kaikki viittaukset on poistettu, voit poistaa mallia.

## <a name="use-a-default-bandwidth-template"></a>Käyttää oletusmallia, kaistanleveyden

Oletusmallin kaistanleveys on annettu ja käyttävät aseman säilöjen oletusarvoisesti Pakota kaistanleveyden ohjausobjektit pilveen käytettäessä. Oletusmallin toimii myös käyttäjille, jotka omien mallien luominen valmis viittaukseksi. Tämä oletusmallin tiedot ovat seuraavat:

- **Nimi** – rajoittamaton Hotelliöiden ja viikonloput

- **Aikataulun** – yksi aikataulun maanantaista perjantaihin, joka koskee kaistanleveyden korvaus 1 Mbps 8 AM – 5 PM laitteen aika. Kaistanleveys on määritetty rajoittamaton viikon jäljellä.

Voit muokata oletusmallia. Tämä malli (mukaan lukien muokatun versiot) käyttö seurataan.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Koko päivän, joka käynnistää määritettynä ajankohtana kaistanleveyden-mallin luominen

Noudata näitä ohjeita voit luoda aikataulun, joka alkaa jakson määritetyllä aikavälillä ja suorittaa koko päivä. Esimerkissä aikatauluun alkaa kello aamulla ja suorittaa 9 AM seuraava aamu asti. On tärkeää muistaa, että annetun sarjoille alkamis- ja päättymisajat sekä sivujen on oltava sama 24 tunnin aikataulun ja et voi olla useita päiviä. Jos haluat määrittää kaistanleveyden malleja, jotka ulottuvat useita päiviä, sinun on käytettävä useita aikatauluja (kuten esimerkissä).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Koko päivän kestävä kaistanleveyden-mallin luominen

1. Luo aikataulun, joka alkaa kello aamulla ja suorittaa keskiyöhön saakka.

2. Lisää toinen aikataulu. Määritä toisen aikataulun pitäminen keskiyön 9 AM aamulla asti.

3. Tallenna kaistanleveyden malli.

Koosteen aikatauluun käynnistyy sitten sähköpostikansioon kerrallaan ja suorita koko päivän kestävä.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Kaistanleveyden malleja liittyviä kysymyksiä ja vastauksia

**Q**. Mitä tapahtuu kaistanleveyden ohjausobjekteihin, kun olet aikatauluja välissä? (Aikataulun on päättynyt, ja toinen ei ole vielä aloitettu.)

**A**. Tässä tapauksessa käytetty kaistanleveyden ilman vuorovaikutteisuutta. Tämä tarkoittaa, että laitteen käyttää rajoittamaton kaistanleveyden, kun tiering tiedot pilvipalveluun.

**Q**. Voit muokata kaistanleveyden malleja offline-laitteessa?

**A**. Sinulla voi muokata tietomääristä säilöjen kaistanleveyden mallit, jos vastaava laite on offline-tilassa.

**Q**. Voit muokata kaistanleveyden mallin liitetyn aseman säilön liittyvät asemat offline-tilassa?

**A**. Voit muokata kaistanleveyden mallin liittyvän aseman säilön jonka tietomääristä offline-tilassa. Huomaa, että asemat ollessa offline-tilassa ei ole tietoja voidaan tasoisen laitteesta pilveen.

**Q**. Voit poistaa oletusmallin?

**A**. Vaikket voi poistaa oletusmallin, se ei ole hyvä tekemään niin. Oletusmallin, mukaan lukien muokatun versiot käyttö seurataan. Seuranta-tiedot analysoidaan ja ajan myötä käytetään oletusmallin parantamiseen.

**Q**. Miten voit selvittää, mallisi kaistanleveys on muokattava?

**A**. Yksi merkkejä, sinun on muokattava kaistanleveyden mallit on näe verkon hidastaa tai useita kertoja-päivän kutistus käynnistyessä. Jos näin käy, seurata ja tallennustilaa käyttö-verkko katsomalla i/o suorituskyky ja verkon nopeus-kaavioita.

Verkon siirtonopeuden tiedoista Määritä päivän ja jossa verkon pullonkaula ilmenee aseman säilöjen. Jos näin tapahtuu, kun tiedot on parhaillaan tasoisen pilveen (saada tämän tiedon kaikki aseman säilöt laitteen cloud suorituskyvyn i/o), valitse tarvitset aseman säilöjen liittyvät kaistanleveyden-mallien muuttaminen.

Kun käytössä on muokattu malleja, sinun on seurannassa uudelleen merkittäviä viiveitä verkosta. Jos nämä edelleen olemassa, täytyy palata tähän määritykseen kaistanleveyden malleja.

**Q**. Mitä tapahtuu, jos useita aseman säilöjen laitteessa on ajoittaa, joissa Limittäisyys mutta erilaisia raja-arvot koskevat kunkin?

**A**. Oletetaan, että sinulla on 3 aseman säilöjen laitteella. Näiden säilöjen liittyvät kokonaan aikatauluja päällekkäin. Kunkin näiden säilöjen käytetään kaistanleveyden raja-arvot ovat 5, 10 ja 15 Mbps. Kaikkien näiden säilöjen toistuu i samanaikaisesti, kun 3 kaistanleveyden rajoituksia pienin voi suojata: Tässä tapauksessa 5 Mbps kuin nämä lähtevän i/o-pyyntöjen jakaa saman jonossa.

## <a name="best-practices-for-bandwidth-templates"></a>Mallien kaistanleveyden parhaat käytännöt

Toimi näiden parhaiden käytäntöjen StorSimple laitteen:

- Määritä kaistanleveyden malleja laitteen käyttöön muuttujan rajoittimen siitä, mitä verkon laitteen päivän eri aikoina. Kaistanleveyden mallien varmuuskopion aikatauluja käytettäessä voidaan hyödyntää muita kaistanleveys cloud toimintoja tehokkaasti myöhemmin.

- Laske todellinen kaistanleveyden pakollinen tietyn käyttöönottoa koko käyttöönotto ja tarvittavat palautus aika tavoitteen (RTO) perusteella.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun avulla](storsimple-manager-service-administration.md).
