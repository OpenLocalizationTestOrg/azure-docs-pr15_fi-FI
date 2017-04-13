<properties 
   pageTitle="Ota yhteys Microsoftin tuotetukeen | Microsoft Azure"
   description="Lue, miten voit luoda tukipyynnön ja tuki-istunnon aloittaminen StorSimple laitteen."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="contact-microsoft-support"></a>Ota yhteys Microsoftin tuotetukeen

Jos kohtaat ongelmia Microsoft Azure StorSimple-ratkaisuun, voit luoda palvelupyynnön tekniseen tukeen. Oman tukihenkilö kanssa online istunnossa myös joudut ehkä tuki-istunnon aloittaminen StorSimple laitteen. Tässä artikkelissa käydään läpi:

- Voit luoda tukipyynnön.
- Miten tuki-istunnon aloittaminen StorSimple laitteesi Windows PowerShell-käyttöliittymän.

Tarkista [StorSimple 8000 sarjan tuki palvelutasosopimuksia ja tietoja](https://msdn.microsoft.com/library/mt433077.aspx) ennen kuin luot tukipyynnön.

## <a name="create-a-support-request"></a>Luo tukipyynnön

Seuraavien toimien tukipyynnön luomiseen:

#### <a name="to-create-a-support-request"></a>Voit luoda tukipyynnön

1. Valitse [Azure perinteinen portal](https://manage.windowsazure.com/)-oikeassa yläkulmassa napsauttamalla tilin nimeä ja valitse sitten **Ota yhteyttä Microsoftin tukeen**.

    ![Yhteyshenkilön MS-tuen ManagementPortal kautta](./media/storsimple-contact-microsoft-support/Ibiza1.png)

2. Sinut ohjataan uuteen Azure-portaaliin (portal.azure.com). Valitse **Uusi tue pyyntöä** -ruutu.

    ![Yhteyshenkilön MS-tuen uuden portaalin kautta](./media/storsimple-contact-microsoft-support/Ibiza2.png)

    **Uusi tue pyyntöä** -ruudussa näkyy näytön oikeaan reunaan. 

    ![Uusi pyyntö tuki-ruutu](./media/storsimple-contact-microsoft-support/Ibiza3a.png)

3. Täytä **Perustiedot** -valintaikkunassa seuraavasti:                                
    1. **Ongelman tyyppi** avattavasta luettelosta Valitse **tekniset**.
    2. Valitse **tilauksen** avattavasta luettelosta.
    3. Valitse pudotusvalikosta **palvelun** **StorSimple**. 
    4. Valitse **tuki suunnitelman** avattavasta luettelosta. Sinun on maksettu tuki suunnitelma käyttöön tuotetukeen.

4. Valitse **Seuraava**. **Ongelma** -valintaikkuna.

    ![Uusi pyyntö tuki-ruutu](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 

5. Täytä **ongelma** -valintaikkunassa seuraavasti:

    1.  Valitse **vakavuus** tason avattavasta luettelosta.
    2.  Valitse **ongelmatyyppi** avattavasta luettelosta.
    3.  Valitse avattavasta luettelosta **luokka** . 
    4.  **Tiedot** -ruudussa lyhyesti ongelmaa.
    5.  **Aikaväli** -ruutuun osoittaa päivämäärän, kellonajan ja aikavyöhykkeen, joka vastaa ongelmaa uusimman esiintymän.
    6.  Valitse **tiedoston lataaminen**-kohdassa kansiokuvaketta tuki-paketti selaamalla.
    7.  Valitse **Jaa vianmääritystietoja** -valintaruutu.

6. Valitse **Seuraava**. **Yhteystiedot** -valintaikkuna.

    ![Uusi pyyntö tuki-ruutu](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 

7. Kirjoita yhteystiedon tiedot ja valitse yhteydenottotapa (puhelin tai sähköpostin). 

8. Valitse **Tallenna yhteyshenkilön muutokset tulevat tukipyyntöjä** -valintaruutu.

9. Valitse **Luo**.

Kun olet lähettänyt pyynnön, tukihenkilö sinuun mahdollisimman pian, jos haluat jatkaa puhelinnumeroon.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Tuki-istunnon aloittaminen Windows PowerShellin StorSimple varten

Ratkaista ongelmat, jotka voi kärsiä StorSimple-laitteen kanssa, sinun on Microsoft Support team kanssa. Microsoft Support ehkä käyttää tuki-istunnon kirjautua laitteeseen. 

Seuraavien toimien tuki-istunnon aloittaminen:

#### <a name="to-start-a-support-session"></a>Tuki-istunnon aloittaminen

1. Käyttää laite suoraan käyttämällä serial konsolin tai telnet istunnon etätietokoneista kautta. Voit tehdä tämän ohjeiden [Käyttäminen painovärit, muste muodostaa laitteen serial konsolin](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Istunnon aikana, joka avautuu paina **Enter** -näppäintä saat komentorivi-ikkuna.

3. Valitse sarja konsoli-valikon vaihtoehto 1, **Kirjaudu sisään täydet oikeudet**.

4. Kehotettaessa salasana seuraavasti: 

    `Password1`

5. Tulee näkyviin Kirjoita seuraava komento:

    `Enable-HcsSupportAccess`

6. Salattu merkkijono esitetään sinulle. Kopioi tämä merkkijono tekstieditorissa, esimerkiksi Muistiossa.

7. Merkkijono Tallenna ja lähetä se Microsoft Support sähköpostiviestissä. 

> [AZURE.IMPORTANT] Poista tuki-tila käytöstä suorittamalla `Disable-HcsSupportAccess`. StorSimple laitteen yrittää myös käytöstä tuki access kahdeksan tuntia, kun istunto on käynnistetty. Paras käytäntö muutatkin StorSimple laitteen tunnistetiedot tuki-istunnon aloittaminen on.
