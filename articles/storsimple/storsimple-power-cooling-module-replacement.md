<properties 
   pageTitle="Korvaa PCM StorSimple-laitteessa | Microsoft Azure"
   description="Kerrotaan, miten voit poistaa ja korvata StorSimple laitteen Power ja jäähdytys moduuli (PCM)"
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Korvaa Power ja jäähdytys moduulin StorSimple laitteen

## <a name="overview"></a>Yleiskatsaus

Power ja jäähdytys moduuli (PCM) Microsoft Azure StorSimple laitteen koostuu power toimituksen ja jäähdytys tuulettimista, ohjataan perus- ja EBOD liitteet. On vain yksi malli, joka on sertifioitu kunkin kehyksen PCM. Ensisijainen kehyksen on sertifioitu 764 W-PCM varten ja EBOD kehyksen on sertifioitu 580 W-PCM varten. Vaikka ensisijainen kehys ja EBOD kehyksen PCMs ovat eri, korvaava-toiminto on sama.

Tässä opetusohjelmassa kerrotaan, miten voit:

- Poista PCM
- Asenna korvaa PCM

>[AZURE.IMPORTANT] Ennen poistaminen ja korvaamiseen PCM [StorSimple laitteiston osan korvaava](storsimple-hardware-component-replacement.md)-arviointitietoja turvallisuus.

## <a name="before-you-replace-a-pcm"></a>Ennen kuin voit korvata PCM

Otettava huomioon seuraavat tärkeät seikat, ennen kuin voit korvata oman PCM:

- Jos PCM power toimittaminen epäonnistuu, jätä viallinen moduuli asennettu, mutta poistaa virtajohto. Tuulettimen säilyvät power vastaanottaa kehys ja jatkaa ERISNIMI jäähdytys. Jos tuulettimen epäonnistuu, PCM on vaihdettava heti.

- Ennen kuin poistat PCM irrota potenssiin PCM poistamalla käytöstä tärkeimmät (jos sellainen on) tai poistamalla fyysisesti virtajohto. Tämä on varoitus, järjestelmä power sammuttamisen on päättymässä.

- Tarkista, että muut PCM on toiminnassa jatkuvan järjestelmän toimintaa ennen korvaamista viallinen PCM. Viallinen PCM korvattava perustoiminnot PCM mahdollisimman pian.

- PCM moduulin korvaavan kestää vain muutaman minuutin, mutta se on täytettävä poistaminen epäonnistui PCM estää ylikuumenemisesta 10 minuutin kuluessa.

- Huomaa, että korvaava 764 W PCM moduulit tehdas lähettäjä eivät sisällä varmuuskopion varauksen moduuli. Sinun on viallinen PCM varauksen poistaminen ja lisääminen ennen suorittamiseen korvaaminen korvaava-moduuli. Lisätietoja on artikkelissa [poistaminen](storsimple-battery-replacement.md)ja lisää varmuuskopion varauksen moduuli.


## <a name="remove-a-pcm"></a>Poista PCM

Noudata näitä ohjeita, kun olet valmis poistamaan Power ja jäähdytys moduuli (PCM) Microsoft Azure StorSimple laitteesta.

>[AZURE.NOTE] Ennen kuin voit poistaa käyttäjän PCM, varmista, että oikea korvaa (764 ensisijainen kehyksen vko) tai 580 W EBOD kehyksen varten.

#### <a name="to-remove-a-pcm"></a>Jos haluat poistaa PCM

1. Azure perinteinen-portaalissa, valitse **laitteet** > **ylläpito** > **Laitteiston tila**. **Jaetut osat** tunnistavan PCM epäonnistuneiden PCM-osien tilan tarkistaminen:

     - Jos power toimituksen PCM 0-päivitys epäonnistuu, **Sanasta PCM 0** tila on punainen.

     - Jos power toimituksen PCM 1 epäonnistuu, **Sanasta PCM 1** tila on punainen.

     - Jos tuulettimen PCM 1 epäonnistuu, **jäähdytys PCM 0 0** tai **1 jäähdytys PCM 0** tila on punainen.

2. Etsi epäonnistui PCM ensisijainen kehyksen takapuolella. Jos käytössäsi on 8600-mallin, Määritä ensisijainen kehyksen tarkistamalla järjestelmä yksikön tunnusnumero näytetään paneeli LED-näytössä. Yksikön tunnus näytössä ensisijainen kehyksen oletusarvo on **00**yksikön tunnus näytössä EBOD kehyksen oletusarvo on **01**. Seuraavassa kaaviossa ja taulukon kerrotaan paneeli esittää LED-näytössä.

    ![Järjestelmätunnus OPS paneeli](./media/storsimple-power-cooling-module-replacement/IC740991.png)

     **Kuva 1** Laitteen paneeli  

  	|Otsikko|Kuvaus|
  	|:---|:-----------|
  	|1|Vaimenna-painiketta|
  	|2|Järjestelmän virta|
  	|3|Moduulin vika|
  	|4|Loogisen vika|
  	|5|Yksikön tunnus-näyttö|

3. Seurannan ilmaisin merkkivalot taustalla ensisijainen kehyksen myös voidaan tunnistaa viallinen PCM. Katso seuraavat kaavio- ja Etsi viallinen PCM merkkivalot avulla, kuinka taulukko. Esimerkiksi jos vastaavat **Tuuletin epäonnistua** LED palaa, tuulettimen on epäonnistunut. Vastaavasti jos vastaavat **AC virheiden** LED palaa, power toimitus epäonnistui. 

    ![Backplane-liitäntä laitteen PCM seurantaa ilmaisimen merkkivalot](./media/storsimple-power-cooling-module-replacement/IC740992.png)

     **Kuva 2** Taustapuolta PCM ilmaisin merkkivalot kanssa

  	|Otsikko|Kuvaus|
  	|:---|:-----------|
  	|1|AC power virhe|
  	|2|Tuulettimen virhe|
  	|3|Varauksen virhe|
  	|4|PCM OK|
  	|5|Toimialueen Ohjauskoneen power virhe|
  	|6|Varauksen kunnossa|

4. Lisätietoja StorSimple laitteen Etsi PCM moduulin taustalle seuraavassa kaaviossa. PCM 0 on vasemmalla ja oikealla on PCM 1. Taulukko, joka seuraa kerrotaan moduulit.

     ![Backplane-liitäntä laitteen ensisijainen kehyksen moduulit](./media/storsimple-power-cooling-module-replacement/IC740994.png)

     **Kuva 3** Taustapuolta laitetta, jonka moduulit 

  	|Otsikko|Kuvaus|
  	|:---|:-----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Controller 0|
  	|4|Ohjaimen 1|

5. Viallinen PCM käytöstä ja irrota toimituksen virtajohto. Voit nyt poistaa PCM.

6. Vaikealta Salvan ja PCM kahvaa napin ja etusormen välillä reunan ja hyödyntää niitä yhteen, kun haluat avata kahvaa.

    ![Avaava PCM kahva](./media/storsimple-power-cooling-module-replacement/IC740995.png)

    **Kuva 4** Avaaminen PCM kahva

7. Grip kahvaa ja poista PCM.

    ![Laitteen PCM poistaminen](./media/storsimple-power-cooling-module-replacement/IC740996.png)

    **Kuva 5** Poistaminen PCM

## <a name="install-a-replacement-pcm"></a>Asenna korvaa PCM

Noudata seuraavia ohjeita PCM asentaa StorSimple laitteen. Varmista, että olet lisännyt varmuuskopion varauksen moduulin ennen asennuksen korvaavan PCM (koskee vain 764 W PCMs). Lisätietoja on artikkelissa [poistaminen](storsimple-battery-replacement.md)ja lisää varmuuskopion varauksen moduuli.

#### <a name="to-install-a-pcm"></a>Asenna PCM

1. Varmista, että sinulla on oikeat korvaa PCM tämän kehyksen. Ensisijainen kehyksen 764 W-PCM ja EBOD kehyksen on 580 W-PCM. Kun ei yrität käyttää 580 W PCM, valitse ensisijainen kehyksen tai 764 W PCM, valitse EBOD kehys. Seuraavassa kuvassa näkyy tunnistaa otsikkoa, joka on kiinnitetty PCM tiedot.

    ![Laitteen PCM selite](./media/storsimple-power-cooling-module-replacement/IC740973.png)

    **Kuva 6** PCM otsikko

2. Tarkista vahingoittuminen kehyksen maksaa yhdistimet erityistä huomiota. 
                                        
    >[AZURE.NOTE] **Älä asenna moduuli, jos minkä tahansa yhdistimen PIN ovat taivuttaa.**

3. Siirrä moduuli kehyksen auki PCM, kahvan avulla.

    ![Laitteen PCM asentaminen](./media/storsimple-power-cooling-module-replacement/IC740975.png)

    **Kuva 7** Asentaminen PCM

4. Sulje manuaalisesti PCM kahvaa. Kuulet olisi napsauttamalla kuin kahvaa salvan alkaa. 
                                        
    >[AZURE.NOTE] Voit varmistaa, että yhdistin PIN on kytketty, voit voit varovasti tug kahvaa ilman vapauttaminen salvan. Jos PCM dioja se tarkoittaa sitä salvan on suljettu, ennen kuin yhdistimet mielenkiinnon paremmin yllä.

5. Yhdistetään power kaapeli power lähde- ja PCM.

6. Suojatun kanta relief paaleissa. 

7. Ota käyttöön PCM.

8. Varmista, että korvaa onnistui: Siirry StorSimple hallintapalvelu Azure perinteinen portal **laitteet** > **ylläpito** > **Laitteiston tila**. **Jaetut osat**-PCM tilana pitää olla vihreä. 
                                        
    >[AZURE.NOTE] Voi kestää muutaman minuutin korvaavan PCM täysin alusta.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [StorSimple laitteiston osan korvaaminen](storsimple-hardware-component-replacement.md).
