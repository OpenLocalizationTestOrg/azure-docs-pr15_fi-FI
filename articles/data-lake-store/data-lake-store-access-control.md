<properties
   pageTitle="Yleistä käytönvalvonta järvi säilössä | Microsoft Azure"
   description="Tietoja siitä, miten käyttää Azure järvi tietovaraston ohjausobjektiin"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="access-control-in-azure-data-lake-store"></a>Azure tietojen järvi kaupan käyttöoikeuksien valvonta

Tietosäilö järvi toteuttaa access-ohjausobjektin malli, joka hakee HDFS ja puolestaan POSIX access ohjausobjektin mallista. Tässä artikkelissa on yhteenveto järvi tietovaraston access-ohjausobjektin mallin perusteet. Saat lisätietoja HDFS käyttää ohjausobjektin mallin oppaassa [HDFS käyttöoikeudet](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>Tiedostojen ja kansioiden käyttöoikeusluettelot

On kahdenlaisia olevalle ohjausobjektin luettelot (käyttöoikeusluettelot) - **Access käyttöoikeusluettelot** ja **Käyttöoikeusluettelot oletus**.

* **Access-käyttöoikeusluettelot** – nämä ohjausobjektin objektin käyttöä. Tiedostojen ja kansioiden on Access käyttöoikeusluettelot.

* **Oletusarvoiset käyttöoikeusluettelot** – "Mallin" käyttöoikeusluettelot liittyviä kansion, jotka määrittävät, minkä tahansa lapsen kansion alla olevat kohteet Access käyttöoikeuksia. Tiedostoja ei ole oletusarvoinen käyttöoikeusluettelot.

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Accessin käyttöoikeusluettelot ja käyttöoikeusluettelot oletusarvo on sama rakenne.

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-acls-2.png)

>[AZURE.NOTE] Jossa oletusarvo-Käyttöoikeusluettelon muuttaminen ei vaikuta käytön Käyttöoikeusluettelon tai oletus Käyttöoikeusluettelon alikohteista, jotka ovat jo.

## <a name="users-and-identities"></a>Käyttäjien ja jäsenyyksistä

Jokaisen tiedostojen ja kansioiden on nämä käyttäjätietojen eri käyttöoikeudet:

* Tiedoston omistavan käyttäjän
* Omistavan ryhmän
* Nimetty käyttäjät
* Nimetty ryhmät
* Kaikki muut käyttäjät

Käyttäjien ja ryhmien käyttäjätietojen ovat Azure Active Directory (AAD) käyttäjätietojen niin ellei muuta ole mainittu "käyttäjä"-järvi tietovaraston kontekstissa voi joko tarkoittaa AAD-käyttäjän tai käyttöoikeusryhmän AAD.

## <a name="permissions"></a>Käyttöoikeudet

Tiedostojärjestelmän objektin käyttöoikeuksia on **luku**, **kirjoittaa**, ja **Suorita** ja niitä voidaan käyttää tiedostot ja kansiot, kuten alla olevassa taulukossa on esitetty.

|            |    Tiedoston     |   Kansion |
|------------|-------------|----------|
| **Read (R)** | Voit lukea tiedoston sisältöä | Edellyttää **luku** ja **suoritus** -kansion sisältö-luettelo.|
| **Kirjoita (W)** | Voit kirjoittaa tai liittää tiedostoon | Vaatii, **Kirjoita & Suorita** luoda lapsen kohteita kansiossa. |
| **Suoritus (X)** | Ei tarkoita mitään järvi tietovaraston yhteydessä | Pakollinen voidaan siirtää kansion ali-kohteita. |

### <a name="short-forms-for-permissions"></a>Lyhyt lomakkeiden käyttöoikeudet

**RWX**käytetään osoittamaan **lukea + kirjoittaa + Suorita**. Lisää tiivistetty numeerinen muoto on olemassa missä **luku = 4**, **kirjoittaa = 2**, ja **suoritus = 1** ja niiden summa käyttöoikeudet. Seuraavassa on joitakin esimerkkejä.

| Numeerinen lomake | Lyhyessä muodossa |      Mitä tämä merkitsee?     |
|--------------|------------|------------------------|
| 7            | RWX        | Lue + kirjoittaa + suorittaminen |
| 5            | R-X        | Lue + suorittaminen         |
| 4            | R--        | Luku                   |
| 0            | ---        | Ei ole oikeuksia         |


### <a name="permissions-do-not-inherit"></a>Peri käyttöoikeudet

Järvi tietovaraston käyttämä POSIX-tyyppiseksi mallissa kohteen käyttöoikeudet tallennetaan itse kohteeseen. Toisin sanoen kohteen käyttöoikeudet, et voi periytyy ylemmän tason kohteiden.

## <a name="common-scenarios-related-to-permissions"></a>Yleisiä tilanteita, joissa liittyvät oikeudet

Seuraavassa on joitakin yleisiä tilanteita, joissa ymmärtää, mitä oikeuksia tarvitaan järvi tietovaraston tilin tiettyjen toimintojen suorittamiseen.

### <a name="permissions-needed-to-read-a-file"></a>Voit lukea tiedoston tarvittavat oikeudet

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Tiedoston voi lukea - soittajan on **lukuoikeudet**
* Kaikki kansiorakenne kansiot, jotka sisältävät tiedosto - soittajan on **Execute** -oikeudet

### <a name="permissions-needed-to-append-to-a-file"></a>Tiedoston liittäminen tarvittavat oikeudet

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* Tiedosto, joka lisätään - soittajan tarvitsee **kirjoittaa** käyttöoikeudet
* Kaikki kansiot, jotka sisältävät tiedosto - soittajan on **Execute** -oikeudet

### <a name="permissions-needed-to-delete-a-file"></a>Voit poistaa tiedoston tarvittavat oikeudet

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Pääkohde - kansion soittajan tarvitsee **suorittaa kirjoitus** -käyttöoikeudet
* Tiedoston polku - muiden kansioiden soittajan tarvitsee **Execute** -oikeudet

>[AZURE.NOTE] Kirjoita tiedoston käyttöoikeuksia ei tarvitse poistaa tiedoston, kunhan edellä mainitut kaksi ehdot ovat tosia.

### <a name="permissions-needed-to-enumerate-a-folder"></a>Luetteloi kansion tarvittavat oikeudet

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Luettelointi - kansion soittajan tarvitsee **luku + suoritus** käyttöoikeudet
* Kaikki ylemmän tason kansiot - soittajan on **Execute** -oikeudet

## <a name="viewing-permissions-in-the-azure-portal"></a>Käyttöoikeuksien tarkasteleminen Azure-portaalissa

Napsauta **Accessin** saat näkyviin tiedoston tai kansion käyttöoikeuksia järvi tietovaraston tilin **Hallinta** -sivu. Valitse seuraavassa näyttökuvassa, saat näkyviin **luettelon** kansion **mydatastore** tilissä käyttöoikeusluettelot Access.

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Tämän jälkeen **Access** -sivu, napsauta **Yksinkertainen näkymän** yksinkertaisempi näkymää.

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Valitse **Näkymän Lisäasetukset** monimutkaisemman näkymää.

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a>Erittäin käyttäjä

Erittäin käyttäjällä on eniten kaikille käyttäjille oikeudet järvi tietovaraston. Erittäin käyttäjä:

* RWX oikeudet **kaikkien** tiedostojen ja kansioiden
* Voit muuttaa minkä tahansa tiedoston tai kansion käyttöoikeuksia.
* Voit muuttaa omistavan käyttäjän tai minkä tahansa tiedoston tai kansion omistavan ryhmän.

Azure-järvi tietosäilö-tili on useita Azure rooleja:

* Omistajat
* Osallistujat
* Lukijat
* Jne.

Kaikkien järvi tietovaraston tilin **omistajat** -roolin on automaattisesti pääkäyttäjän tilin. Lisää tietoja Azure roolin perusteella Access ohjausobjektin (RBAC) kohdassa [Roolipohjainen käytön hallinta](../active-directory/role-based-access-control-configure.md).

## <a name="the-owning-user"></a>Omistavan käyttäjän

Kohteen luonut käyttäjä on automaattisesti omistavan käyttäjän kohteen. Omistavan käyttäjän tehdä seuraavia toimia:

* Tiedosto, joka omistaa käyttöoikeuksien muuttaminen
* Muuta tiedoston, joka omistaa omistavan ryhmän, kunhan omistavan käyttäjä on myös kohderyhmän jäsen.

>[AZURE.NOTE] Toisen omistama tiedoston omistavan käyttäjän omistavan käyttäjä **ei voi** muuttaa. Vain super-käyttäjät voivat muuttaa tiedoston tai kansion omistavan käyttäjän.

## <a name="the-owning-group"></a>Omistavan ryhmän

POSIX-käyttöoikeusluettelot jokaiselle käyttäjälle liittyy "ensisijaisen ryhmän". Esimerkiksi "Anneli" käyttäjä voi kuulua "rahoitus-ryhmä. Anneli voivat kuulua useita ryhmiä, mutta hänen ensisijaisen ryhmän aina määritetty ryhmä. POSIX, kun Anneli Luo tiedosto, tiedoston omistavan ryhmän asetetaan hänen ensisijaisen ryhmän, joka on tässä tapauksessa "talous".
 
Tiedostojärjestelmän uuden kohteen luomisen jälkeen järvi tietovaraston määrittää arvon omistavan ryhmän. 

* **Tapaus 1** - pääkansion "/". Tämä kansio luodaan, kun järvi tietosäilö-tili on luotu. Tässä tapauksessa omistavan ryhmän on määritetty käyttäjälle, joka on luotu tili.
* **Tapaus 2** (kaikki muut laatikko) - uuden kohteen luomisen jälkeen omistavan ryhmän kopioidaan pääkansion.

Omistavan ryhmän voi muuttaa:
* Super-käyttäjät
* Omistavan käyttäjä, jos omistavan käyttäjä on myös kohderyhmän jäsen.

## <a name="access-check-algorithm"></a>Access-valintaruudun algoritmin

Seuraavassa kuvassa on access valintaruudun salausalgoritmi järvi tietovaraston tilit.

![Tietosäilö järvi käyttöoikeusluettelot algoritmin](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a>Syöttörajoitteen ja "voimassa olevien käyttöoikeuksien"

**Syöttörajoitteen** on RWX arvo, jota käytetään rajoittaa **nimeltä käyttäjille**, **ryhmän toimialueesi omistajuutta**ja **nimeltä ryhmät** , Access tarkistaa algoritmin suoritettaessa. Seuraavassa on käsitteistä rajoitteen varten. 

* Rajoite Luo "voimassa olevien käyttöoikeuksien", eli se Muokkaa käyttöoikeuksia, Access tarkistaa aikaan.
* Rajoite niitä voidaan muokata suoraan tiedoston omistaja ja super-käyttäjiä.
* Rajoite on mahdollisuus poistaa oikeudet luoda käyttöoikeudet. Käyttöoikeuksien peite **ei voi** lisätä käyttöoikeudet. 

Tarkista uutiskirjeistä esimerkkejä. Alla rajoite on määritetty **RWX**, mikä tarkoittaa, että rajoite ei poista mitään käyttöoikeuksia. Huomaa, että nimetyn, omistavan ryhmän ja ryhmän voimassa olevien käyttöoikeuksien ei ole muutettu käyttöoikeuksien tarkistuksen aikana.

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

Alla olevassa esimerkissä rajoite on määritetty **R-X**. Näin on sen **käyttäjän nimi**, **ryhmän toimialueesi omistajuutta**ja milloin Access **-ryhmä** **poistaa käytöstä kirjoitusoikeus** Tarkista.

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Käyttöä seuraavassa on tiedoston tai kansion syöttörajoitteen kohtaa, jossa näkyy Azure-portaalissa.

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

>[AZURE.NOTE] Järvi tietovaraston uuden tilin käytön Käyttöoikeusluettelon ja pääkansio ("/") oletus Käyttöoikeusluettelon peite on oletusarvo RWX.

## <a name="permissions-on-new-files-and-folders"></a>Uusien tiedostojen ja kansioiden käyttöoikeuksien

Kun uusi tiedosto tai kansio luodaan aiemmin luotuun kansioon, oletus Käyttöoikeusluettelon pääkansion määrittää:

* Alikansion oletus Käyttöoikeusluettelon ja käytön Käyttöoikeusluettelon
* Ali-tiedostojen käytön Käyttöoikeusluettelon (tiedostoja ei ole oletusarvoinen Käyttöoikeusluettelon)

### <a name="a-child-file-or-folders-access-acl"></a>Lapsen tiedoston tai kansion käytön Käyttöoikeusluettelon

Lapsen tiedoston tai kansion luomisen ylätason oletus Käyttöoikeusluettelon kopioidaan alitiedoston tai kansion käytön Käyttöoikeusluettelon. Myös, jos **toinen** käyttäjä on RWX käyttöoikeudet ylätason oletusarvon Käyttöoikeusluettelon, se kokonaan poistetaan ali-kohteen käytön Käyttöoikeusluettelon.

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

Edellä olevat tiedot on useimmissa skenaarioissa olisi tarvitsee tietää, miten alatason kohteen käytön Käyttöoikeusluettelon määräytyy. Jos POSIX järjestelmien tuttu ja haluat ymmärtää perusteellisempaa miten muunnos saavutetaan, kuitenkin nähdä jäljempänä tämän artikkelin kohtaan [Umask henkilön rooli luominen Access-Käyttöoikeusluettelon uusia tiedostoja ja kansioita](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) .
 

### <a name="a-child-folders-default-acl"></a>Alikansion oletus Käyttöoikeusluettelon

Alikansion luomisen ylätason kansiosta-kohdassa haluamaasi Yläkansiota oletus Käyttöoikeusluettelon kopioidaan päälle, ennalleen ja alikansion oletus Käyttöoikeusluettelon.

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Lisäohjeita ymmärtämään järvi tietovaraston käyttöoikeusluettelot

Seuraavassa on lisäohjeita voi auttaa sinua ymmärtämään, miten käyttöoikeusluettelot määritetään järvi tietovaraston tiedostoja tai kansioita usealla.

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a>Luoda uusia tiedostoja ja kansioita Käyttöoikeusluettelon Access Umask henkilön rooli

POSIX-yhteensopivaa system Yleiset käsitteestä on kyseisen umask on 9-bittinen ylätason kansiota, muunna **omistaa käyttäjän**, **ryhmän toimialueesi omistajuutta**ja **muiden** uuden lapsen tiedoston tai kansion käytön Käyttöoikeusluettelon käyttöoikeudet. Umask bittien Selvitä, mitä bittien ali-kohteen käytön Käyttöoikeusluettelon käytöstä. Näin on käytetty estäminen valikoivasti käyttöoikeudet omistavan käyttäjän, ryhmän omistaa välitys ja muut.
  
HDFS-järjestelmässä umask on yleensä koko sivuston määritysvaihtoehto, joka ohjaa järjestelmänvalvojat. Tietosäilö järvi käyttää **tilillä kattava umask** , joka ei voi muuttaa. Seuraavassa taulukossa on tietoja järvi kaupan umask.

| Käyttäjäryhmän  | Asetus | Uuden ali-kohteen käytön Käyttöoikeusluettelon vaikutus |
|------------ |---------|---------------------------------------|
| Omistaa käyttäjä | ---     | Ei vaikuta                             |
| Ryhmän omistaa| ---     | Ei vaikuta                             |
| Muut       | RWX     | Poista luku + kirjoittaa + suorittaminen         | 

Seuraavassa kuvassa tämän umask toiminnassa. Nettonykyarvon vaikuttaa poistaa **toisen** käyttäjän **lukea + kirjoittaa + Suorita** . Koska umask ei ole määrittänyt bittien **omistaa käyttäjän** ja **ryhmän omistaa**, kyseiset käyttöoikeudet ei muuntaa.

![Tietosäilö järvi käyttöoikeusluettelot](./media/data-lake-store-access-control/data-lake-store-acls-umask.png) 

### <a name="the-sticky-bit"></a>Yhden sormen bittinen

Yhden sormen bitti on enemmän lisäominaisuudet POSIX tiedostojärjestelmässä. Tietosäilö järvi kontekstissa on epätodennäköistä, että muistilappuja bittinen tarvitaan.

Alla olevassa taulukossa näkyy muistilappuja bittinen toiminta järvi tietosäilö.

| Käyttäjäryhmän         | Tiedoston    | Kansion |
|--------------------|---------|-------------------------|
| Yhden sormen bittinen **ei käytössä** | Ei vaikuta   | Ei vaikuta           |
| Yhden sormen bittinen **edelleen**  | Ei vaikuta   | Estää **super-käyttäjien** ja **käyttäjä omistaa** , poistaminen tai nimeäminen uudelleen alatason kohteen alikohde lukuun ottamatta.               |

Yhden sormen bittinen ei näy Azure-portaalissa.

## <a name="common-questions-for-acls-in-data-lake-store"></a>Tietosäilö järvi käyttöoikeusluettelot kysymyksiin

Seuraaviin kysymyksiin, jotka tulee usein järvi tietovaraston käyttöoikeusluettelot osalta.

### <a name="do-i-have-to-enable-support-for-acls"></a>Onko minun käyttöoikeusluettelot tuen ottaminen käyttöön?

Ei. Access-ohjausobjektin kautta käyttöoikeusluettelot on aina käytössä järvi tietovaraston tilin.

### <a name="what-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Mitä käyttöoikeudet ovat rekursiivisesti Poista kansio ja sen sisältöä?

* Pääkansio on oltava **Kirjoitus ja suorita**.
* Kansio poistetaan, ja kaikki siihen tarvitaan **luku + kirjoittaminen ja suorita**.
>[AZURE.NOTE] Kansiot-tiedostojen poistaminen ei edellyttää kirjoittaminen tiedostojen. Lisäksi pääkansio "/" **ei koskaan** voi poistaa.

### <a name="who-is-set-as-the-owner-of-a-file-or-folder"></a>Kuka on määritetty tiedoston tai kansion omistajan?

Tiedoston tai kansion luoja tulee omistaja.

### <a name="who-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Kuka on määritetty tiedoston tai kansion omistavan ryhmän luomisen?

Se on kopioitu kohdassa uusi tiedosto tai kansio luodaan pääkansion omistavan ryhmän.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Olen omistavan käyttäjän tiedoston, mutta minulla ei ole on RWX käyttöoikeudet. Mitä teen?

Omistavan käyttäjä voi muuttaa vain tiedoston käyttöoikeuksia ja anna itse kaikki RWX käyttöoikeudet, he tarvitsevat.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Tukeeko järvi tietovaraston käyttöoikeusluettelot periytyminen?

Ei.

### <a name="what-is-the-difference-between-mask-and-umask"></a>Mikä on syöttörajoitteen ja umask välinen ero?

| syöttörajoitteen | umask|
|------|------|
| **Mask** -ominaisuus on käytettävissä kaikissa tiedosto ja kansio. | **Umask** on järvi tietovaraston tilin-ominaisuus. Siis on vain yksi umask järvi tietovaraston.    |
| Voit muuttaa tiedoston tai kansion mask-ominaisuus käyttämällä omistavan käyttäjän tai tiedoston tai pääkäyttäjän omistavan ryhmän. | Kuka tahansa käyttäjä, erittäin käyttäjä ei voi muokata umask-ominaisuus. Se on vaihtoehdoksi, vakion arvo.|
| Syöttörajoitteen-ominaisuutta käytetään Access tarkistaa algoritmin suorituksen aikana voit selvittää, onko käyttäjälle oikeuden suorittaa toiminnon tiedoston tai kansion. Rajoitteen rooli on luotava "voimassa olevien käyttöoikeuksien", kun access-valintaruutu. | Umask ei käytetä aikana Access tarkistaa ollenkaan. Umask käytetään käytön Käyttöoikeusluettelon alikohteista uuden kansion. |
| Rajoite on 3-bittiseen RWX arvo, joka koskee nimetty käyttäjän, ryhmän ja omistavan käyttäjän käyttöoikeuksien tarkistuksen aikana.| Umask on 9 bittinen arvo, joka koskee omistavan käyttäjän, omistavan ryhmän ja muut uusi alikohde.| 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Jos Lisätietoja POSIX malli voit lukea?

* [http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.HTML](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [HDFS oikeudet opas](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) 

* [POSIX USEIN KYSYTYT KYSYMYKSET](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1e 1997](http://users.suse.com/~agruen/acl/posix/Posix_1003.1e-990310.pdf)

* [POSIX Käyttöoikeusluettelon Linux](http://users.suse.com/~agruen/acl/linux-acls/online/)

* [Access-ohjausobjekti näyttää käyttäminen Linux Käyttöoikeusluettelon](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Katso myös

* [Yleistä Azure järvi tietosäilö](data-lake-store-overview.md)

* [Azure tietojen järvi Analytics käytön aloittaminen](../data-lake-analytics/data-lake-analytics-get-started-portal.md)





