<properties
 pageTitle="Aloita Azure ajoituksen Azure-portaalissa | Microsoft Azure"
 description="Aloita Azure ajoituksen Azure-portaalissa"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/10/2016"
 ms.author="deli"/>

# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Aloita Azure ajoituksen Azure-portaalissa

On helppo luoda ajoitetuissa Azure ajoitus. Tässä opetusohjelmassa opit luomaan projektin. Opit myös ajoituksen valvonnan ja hallinnan ominaisuuksia.

## <a name="create-a-job"></a>Työn luominen

1.  Kirjaudu [Azure-portaaliin](https://portal.azure.com/).  

2.  Valitse **+ Uusi** > Kirjoita hakukenttään _ajoitus_ > Valitse **ajoitus** tulokset > valitsemalla **Luo**.

     ![][marketplace-create]

3.  Luo työn, joka käynnit yksinkertaisesti http://www.microsoft.com/ GET-pyynnössä kanssa. **Työn ajoitus** -näytön Lisää seuraavat tiedot:

    1.  **Nimi:**`getmicrosoft`  

    2.  **Tilaus:** Azure-tilauksesi   

    3.  **Projektin sivustokokoelman:** Valitse aiemmin luotu projekti kokoelma tai **Luo** Uusi > nimi.

4.  Seuraavaksi **Toimintoasetukset**, Määritä seuraavat arvot:

    1.  **Toiminnon tyyppi:**` HTTP`  

    2.  **Menetelmä:**`GET`  

    3.  **URL-osoite:**` http://www.microsoft.com`  

      ![][action-settings]

5.  Määritä lopuksi japanin aikataulu. Työn voidaan määrittää kuin erikseen työ, mutta valitse japanin toistuvan aikataulun:

    1. **Toistuminen**:`Recurring`

    2. **Aloittaminen**: kuluvan päivän päivämäärän

    3. **Toistuvaksi jokaisen**:`12 Hours`

    4. **Lopeta ennen**: on kahden päivän päivämäärä  

      ![][recurrence-schedule]

6.  Valitse **Luo**

## <a name="manage-and-monitor-jobs"></a>Hallinta ja töiden seuranta

Kun työ on luotu, se näkyy tärkeimmät Azure Raporttinäkymät-ikkunan. Valitse projektin ja avautuu uusi ikkuna, jossa seuraavilla välilehdillä:

1.  Ominaisuudet:  

2.  Toimintoasetukset  

3.  Aikataulu  

4.  Historia

5.  Käyttäjät

    ![][job-overview]

### <a name="properties"></a>Ominaisuudet:

Vain luku-ominaisuuksien kuvaus ajoitus projektin hallinta-metatiedot.

   ![][job-properties]


### <a name="action-settings"></a>Toimintoasetukset

Projektin **työt** -näytössä napsauttamalla voit määrittää, että projektin. Näillä oikeuksilla voit määrittää lisäasetuksia, jos et ole määrittänyt niitä nopea luominen ohjatun toiminnon.

Kaikki tyypit voivat muuttua, yritä käytäntö ja Virhetoiminto.

HTTP- ja HTTPS-työ toiminnon tyypeille voivat muuttua menetelmä minkä tahansa sallitun HTTP-toiminnon avulla. Voit myös lisätä, poistaa tai muuttaa ylä- ja perustodentamista tiedot.

Tallennustilan jonon tyypit voivat muuttua, tallennustilan tilin, jonon nimi, SAS tunnuksen ja tekstissä.

Palvelun bus tyypit voivat muuttua, nimitilan, aiheen tai jonon polun, todennusasetukset, siirtotyyppi, viestin ominaisuudet ja viestin tekstiosaan.

   ![][job-action-settings]

### <a name="schedule"></a>Aikataulu

Näillä oikeuksilla voit uudelleen aikataulussa, jos haluat muuttaa aikataulun, jonka loit nopea luominen ohjatun toiminnon.

Tämä on mahdollisuus luoda [monimutkaisia aikatauluja ja Lisäasetukset toistuminen-työ](scheduler-advanced-complexity.md)

Voit muuttaa alkamispäivä ja ajan, toistuvan aikataulun ja loppuun päivämäärän ja kellonajan (Jos työ on toistuva.)

   ![][job-schedule]


### <a name="history"></a>Historia

**Historia** -välilehteen näyttää projektin jokaisen suorittamisen valitun mittaukset valitun työn järjestelmä. Nämä arvot on oman ajoituksen kunto reaaliaikainen arvot:

1.  Tila  

2.  Tiedot  

3.  Yritä yritykset

4.  Esiintymä: 1st, 2nd, 3, jne.

5.  Aloitusaika suorittaminen  

6.  Suorittamisen päättymisaika

   ![][job-history]

Voit valita suorittamisen, voit tarkastella sen **Historia-tiedot**, kuten jokaisen suorittamisen koko vastaus. Tämän valintaikkunan avulla voit kopioida Leikepöydälle vastaus.

   ![][job-history-details]

### <a name="users"></a>Käyttäjät

Azure Roolipohjainen käytön hallinta (RBAC) mahdollistaa tarkasti rajattuja käyttöoikeuksien hallinnan Azure ajoitus. Opettele käyttämään käyttäjät-välilehti, katso [Azure Role-Based käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md)


## <a name="see-also"></a>Katso myös

 [Ajoituksen ominaisuudet](scheduler-intro.md)

 [Ajoituksen käsitteitä ja termejä kohteen hierarkia](scheduler-concepts-terms.md)

 [Suunnitelmien ja Azure ajoituksen Laskutus](scheduler-plans-billing.md)

 [Miten voit luoda monimutkaisia aikatauluja ja Lisäasetukset toistuminen Azure schedulerilla](scheduler-advanced-complexity.md)

 [Ajoituksen REST API-viittaus](https://msdn.microsoft.com/library/mt629143)

 [Ajoituksen PowerShell cmdlet-viittaus](scheduler-powershell-reference.md)

 [Ajoituksen suuren käytettävyyden ja luotettavuutta](scheduler-high-availability-reliability.md)

 [Ajoituksen rajoitukset, oletusarvot ja virhekoodit](scheduler-limits-defaults-errors.md)

 [Ajoituksen lähtevien yhteyksien todennusta](scheduler-outbound-authentication.md)


[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
