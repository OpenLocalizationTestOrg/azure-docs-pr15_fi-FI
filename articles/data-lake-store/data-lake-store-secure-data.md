<properties 
   pageTitle="Azure tietojen järvi säilöön tallennettuja tietojen suojaaminen | Microsoft Azure" 
   description="Opi Azure Lake Tietosäilölle ryhmien käyttäminen tietojen suojaamiseen ja käyttöoikeusluettelot" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/29/2016"
   ms.author="nitinme"/>

# <a name="securing-data-stored-in-azure-data-lake-store"></a>Azure tietojen järvi säilöön tallennettuja tietojen suojaamisesta

Azure järvi tietovaraston tietojen suojaaminen on kolme vaihe vaiheelta.

1. Aloita luomalla käyttöoikeusryhmät-Azure Active Directory (AAD). Nämä käyttöoikeusryhmät käytetään toteuttamisesta Roolipohjainen käyttöoikeuksien valvonta (RBAC) Azure-portaalissa. Katso lisätietoja [Roolipohjainen käyttöoikeuksien valvonta Microsoft Azure-tietokannassa](../active-directory/role-based-access-control-configure.md).

2. Määritä AAD käyttöoikeusryhmät Azure järvi tietosäilö-tili. Tämä ohjaa portal tai ohjelmointirajapinnan portaali- ja -toimintojen järvi tietosäilö-tilille.

3. Määritä AAD käyttöoikeusryhmät access järvi tietovaraston tiedostojärjestelmässä luetteloihin (käyttöoikeusluettelot).

4. Lisäksi voit myös määrittää IP-osoitealueita asiakkaille, jotka voivat käyttää järvi tietovaraston tiedot.

Tämän artikkelin ohjeita opit käyttämään Azure portaalin Suorita edellä mainitut toimet. Katso tarkempia tietoja kuinka järvi tietovaraston toteuttaa suojauksen tili ja tietojen tasolla, [Azure järvi tietovaraston suojaus](data-lake-store-security-overview.md). Ohjaamme tietoja siitä, miten käyttöoikeusluettelot toteutettu Azure järvi tietosäilö on artikkelissa [Yleiskatsaus Access hallinnan järvi säilössä](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).
- **Azure Lake Tietosäilölle tili**. Ohjeita siitä, miten voit luoda, on artikkelissa [Azure järvi tietovaraston käytön aloittaminen](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Luo käyttöoikeusryhmät Azure Active Directory

Katso ohjeet AAD käyttöoikeusryhmät luomisesta ja käyttäjien lisääminen ryhmään [Azure Active Directoryn hallinta käyttöoikeusryhmät](../active-directory/active-directory-accessmanagement-manage-groups.md).

## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Määrittää käyttäjät tai käyttöoikeusryhmät Azure järvi tietovaraston tilit

Kun määrität käyttäjät tai käyttöoikeusryhmät Azure järvi tietovaraston tilit, voidaan hallita Azure portaalin ja Azure Resurssienhallinta ohjelmointirajapinnan käyttäminen tilin hallintatoiminnot. 

1. Avaa Azure järvi tietosäilö-tiliä. Vasemmanpuoleisessa ruudussa valitsemalla **Selaa**, valitse **Järvi tietosäilö**, ja valitse sitten järvi tietosäilö-sivu-tilin nimi, jolle haluat määrittää käyttäjän tai suojaus-ryhmä.

2. Valitse järvi tietosäilö-tilin sivu **asetukset**. Valitse **asetukset** -sivu **käyttäjät**.

    ![Määritä käyttöoikeusryhmän Azure järvi tietosäilö-tilille] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Määritä käyttöoikeusryhmän Azure järvi tietosäilö-tilille")

3. Oletusarvon mukaan **käyttäjä** -sivu Luetteloi **tilauksen järjestelmänvalvojat** -ryhmän omistaja. 

    ![Lisää käyttäjät ja roolit] (./media/data-lake-store-secure-data/adl.add.group.roles.png "Lisää käyttäjät ja roolit")
 
    Voit lisätä ryhmään ja liittyvien roolien kahdella eri tavalla.

    * Käyttäjän tai ryhmän lisääminen tiliin ja Määritä rooli- tai
    * Lisää rooli ja määritä sitten roolin käyttäjät ja ryhmät.

    Tässä osassa on katsomalla ensimmäisestä tavasta lisäämisessä ja sitten roolien. Voit suorittaa vastaavat vaiheet, valitse ensin rooli ja määritä sitten roolin ryhmät.
    
4. Valitse **käyttäjät** -sivu valitsemalla **Lisää** Avaa **Lisää access** -sivu. Valitse **Lisää access** -sivu valitsemalla **Valitse rooli**ja valitse sitten käyttäjän/ryhmän rooli.

     ![Lisää käyttäjän rooli] (./media/data-lake-store-secure-data/adl.add.user.1.png "Lisää käyttäjän rooli")

    **Omistaja** ja **osallistujan** rooli antaa tietyt hallinnan toiminnot pääsy tietojen järvi-tili. Käyttäjille, joilla tietoja järvi tietojen käsitteleminen voit lisätä ne **lukija **. Rooli alue on rajoitettu Azure järvi tietosäilö-tiliin liittyvät hallintatoiminnot.

    Tietoja toimintojen yksittäisiä tiedostojärjestelmän käyttöoikeudet määrittää mitä käyttäjät voivat tehdä. Vuoksi ottaa lukija käyttäjä voi tarkastella vain näkyy tiliin liitetty hallinta-asetukset, mutta voit mahdollisesti Kirjoita ja Lue tietoja järjestelmän käyttöoikeuksien määritetty. Tietosäilö järvi tiedostojärjestelmän käyttöoikeudet on kuvattu osoitteessa [määrittää käyttöoikeusryhmän kuin käyttöoikeusluettelot Azure järvi tietovaraston tiedostojärjestelmän](#filepermissions).



5. Valitse **Lisää access** -sivu, **Lisää käyttäjät** voivat avata **Lisää käyttäjiä** -sivu. Etsi käyttöoikeusryhmän, jonka loit aiemmin Azure Active Directory tämä sivu. Jos sinulla on useita ryhmiä haku tehdään, käytä tekstiruudun yläosassa suodatettavan ryhmänimi. Valitse **Valitse**.

    ![Käyttöoikeusryhmän lisääminen] (./media/data-lake-store-secure-data/adl.add.user.2.png "Käyttöoikeusryhmän lisääminen")

    Jos haluat lisätä ryhmän/käyttäjä, joka ei ole luettelossa, voit kutsua niitä käyttämällä **Kutsu** -kuvaketta ja määrittämällä käyttäjän/ryhmän sähköpostiosoite.

6. Valitse **OK**. Raportissa pitäisi näkyä käyttöoikeusryhmän, lisätä alla kuvatulla tavalla.

    ![Käyttöoikeusryhmän lisätty] (./media/data-lake-store-secure-data/adl.add.user.3.png "Käyttöoikeusryhmän lisätty")

7. Käyttäjän tai käyttöoikeusryhmän on nyt Azure järvi tietovaraston tilin käyttöoikeus. Jos haluat antaa tietyille käyttäjille, voit lisätä ne suojaus-ryhmään. Vastaavasti, jos haluat kumota pääsyn käyttäjän, voit poistaa ne suojaus-ryhmästä. Voit määrittää useita käyttöoikeusryhmät myös tiliin. 

## <a name="filepermissions"></a>Määritä käyttäjät tai käyttöoikeusryhmän käyttöoikeusluettelot Azure järvi tietovaraston tiedostojärjestelmän

Määrittämällä käyttäjän ja mitkä käyttöoikeusryhmät Azure tietojen järvi tiedostojärjestelmän määritetään käytönvalvonta Azure järvi tietovaraston tallennetuista tiedoista.

1. Valitse **Tietoresurssien**järvi tietosäilö-tilin sivu.

    ![Luo hakemistoja järvi tietovaraston tili] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Tietojen järvi tilin kansioiden luominen")

2. Valitse **Hallinta** -sivu napsauttamalla tiedostoa tai kansiota, jolle haluat määrittää Käyttöoikeusluettelon ja valitse sitten **Access**. Jos haluat liittää tiedoston Käyttöoikeusluettelon, sinun on valittava **Access** - **Tiedoston esikatselu** -sivu.

    ![Määritä käyttöoikeusluettelot tietojen järvi tiedostojärjestelmässä] (./media/data-lake-store-secure-data/adl.acl.1.png "Määritä käyttöoikeusluettelot tietojen järvi tiedostojärjestelmässä")

3. **Access** -sivu on lueteltu vakio access ja mukautetun access pääkansio on jo määritetty. Valitse **Lisää** -kuvaketta, jos haluat lisätä käyttöoikeusluettelot Mukautettu taso.

    ![Luettelon vakio- ja mukautettuja access] (./media/data-lake-store-secure-data/adl.acl.2.png "Luettelon vakio- ja mukautettuja access")

    * **Standard käyttää** on UNIX-pohjaiset, access, jossa voit määrittää luku, kirjoitus, suorita (rwx) kolme eri käyttäjän luokkien: omistaja ja ryhmä.
    * **Mukautettu access** vastaa POSIX käyttöoikeusluettelot, jonka avulla voit määrittää nimetyn tietyille käyttäjille tai ryhmille, ja paitsi tiedoston omistaja tai ryhmän käyttöoikeudet. 
    
    Lisätietoja on artikkelissa [HDFS käyttöoikeusluettelot](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Lisätietoja siitä, miten käyttöoikeusluettelot toteutettu järvi tietosäilö on artikkelissa [Käyttöoikeuksien valvonta järvi säilössä](data-lake-store-access-control.md).

4. Valitse **Lisää** -kuvaketta, voit avata **Lisää mukautettu Access** -sivu. Valitse tämä sivu **valitsemalla käyttäjän tai ryhmän**ja sitten **Valitse käyttäjä tai ryhmä** -sivu, näkyykö käyttöoikeusryhmän, jonka loit aiemmin Azure Active Directory. Jos sinulla on useita ryhmiä haku tehdään, käytä tekstiruudun yläosassa suodatettavan ryhmänimi. Valitse Lisää ja valitse sitten **Valitse**ryhmä.

    ![Lisää ryhmä] (./media/data-lake-store-secure-data/adl.acl.3.png "Lisää ryhmä")

5. **Valitse käyttöoikeudet**, valitse käyttöoikeudet, ja kysytään, haluatko käyttöoikeuksien oletukseksi Käyttöoikeusluettelon käyttää Käyttöoikeusluettelon tai molemmat. Valitse **OK**.

    ![Käyttöoikeuksien ryhmään] (./media/data-lake-store-secure-data/adl.acl.4.png "Käyttöoikeuksien ryhmään")

    Saat lisätietoja järvi tietovaraston ja oletuskäyttöoikeudet ja käyttöoikeusluettelot käyttöoikeuksia [Käytönvalvonta järvi säilössä](data-lake-store-access-control.md).


6. **Lisää mukautettu Access** -sivu valitsemalla **OK**. Lisätyn-ryhmässä ja niihin kuuluvat oikeudet näkyvät nyt **Access** -sivu.

    ![Käyttöoikeuksien ryhmään] (./media/data-lake-store-secure-data/adl.acl.5.png "Käyttöoikeuksien ryhmään")

    > [AZURE.IMPORTANT] Nykyinen versio voi olla vain **Mukautetun Access**-kohdan 9 merkinnät. Jos haluat lisätä enintään 9 käyttäjiä, sinun on luotava käyttöoikeusryhmät, käyttäjien lisääminen SharePoint-ryhmät, Lisää antaa suojausryhmät järvi tietosäilö-tilille.

7. Tarvittaessa voit myös muokata käyttöoikeudet, kun olet lisännyt ryhmä. Valitse kunkin käyttöoikeustyyppi (luku, kirjoitus-Execute) mukaan, haluatko poistaa tai määrittää, että oikeudet käyttöoikeusryhmän-valintaruutu tai poista sen valinta. Valitse Tallenna muutokset **Tallenna** tai **hylätä** Kumoa muutokset.

## <a name="set-ip-address-range-for-data-access"></a>Määritä IP-osoitealueita tietojen käyttö

Azure järvi tietovaraston voit edelleen käyttää tiedot tallennetaan verkkoon tasolla lukitseminen. Voit ottaa palomuuri, Määritä IP-osoite tai Määritä IP-osoitealueita luotettujen asiakkaiden. Kun otettu käyttöön, vain asiakkaat, jotka on määritetty alueella IP-osoitteet muodostaa kauppa.

![Palomuurin asetusten ja IP-osoitetta] (./media/data-lake-store-secure-data/firewall-ip-access.png "Palomuurin asetusten ja IP-osoite")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Poista Azure järvi tietovaraston tilin käyttöoikeusryhmät

Kun poistat käyttöoikeusryhmät Azure järvi tietovaraston tileistä, muutat vain access tilillä Azure-portaalin ja Azure Resurssienhallinta ohjelmointirajapinnan hallinta-toimintoja.

1. Valitse järvi tietosäilö-tilin sivu **asetukset**. Valitse **asetukset** -sivu **käyttäjät**.

    ![Määritä käyttöoikeusryhmän Azure tietojen järvi-tilille] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Määritä käyttöoikeusryhmän Azure tietojen järvi-tilille")

2. Valitse **käyttäjät** -sivu käyttöoikeusryhmän, jonka haluat poistaa.

    ![Poista suojaus-ryhmä] (./media/data-lake-store-secure-data/adl.add.user.3.png "Poista suojaus-ryhmä")

3. Valitse Suojaus-ryhmän sivu Valitse **Poista**.

    ![Käyttöoikeusryhmän poistettu] (./media/data-lake-store-secure-data/adl.remove.group.png "Käyttöoikeusryhmän poistettu")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Käyttöoikeusryhmän käyttöoikeusluettelot poistaminen Azure järvi tietovaraston tiedostojärjestelmässä

Kun poistat käyttöoikeusryhmät käyttöoikeusluettelot Azure järvi tietovaraston tiedostojärjestelmässä, voit muuttaa access järvi tietovaraston tiedot.

1. Valitse **Tietoresurssien**järvi tietosäilö-tilin sivu.

    ![Tietojen järvi tilin kansioiden luominen] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Tietojen järvi tilin kansioiden luominen")

2. Valitse **Hallinta** -sivu tiedostoa tai kansiota, jonka haluat poistaa Käyttöoikeusluettelon ja valitse tili-sivu, **Access** -kuvaketta. Jos haluat poistaa Käyttöoikeusluettelon tiedostolle, sinun on valittava **Access** - **Tiedoston esikatselu** -sivu.

    ![Määritä käyttöoikeusluettelot tietojen järvi tiedostojärjestelmässä] (./media/data-lake-store-secure-data/adl.acl.1.png "Määritä käyttöoikeusluettelot tietojen järvi tiedostojärjestelmässä")

3. Valitse **Access** -sivu **Mukautetun Access** -osiosta käyttöoikeusryhmän, jonka haluat poistaa. Valitse **Mukautettu Access** -sivu valitsemalla **Poista** ja valitse sitten **OK**.

    ![Käyttöoikeuksien ryhmään] (./media/data-lake-store-secure-data/adl.remove.acl.png "Käyttöoikeuksien ryhmään")


## <a name="see-also"></a>Katso myös

- [Yleistä Azure järvi tietosäilö](data-lake-store-overview.md)
- [Tietojen kopioiminen Azure-tallennustilan BLOB järvi tietosäilö](data-lake-store-copy-data-azure-storage-blob.md)
- [Azure tietojen järvi Analytics käyttäminen järvi tietosäilö](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure Hdinsightiin käyttäminen järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Tietosäilö järvi PowerShellin avulla aloittaminen](data-lake-store-get-started-powershell.md)
- [Tietosäilö järvi käyttämällä .NET SDK käytön aloittaminen](data-lake-store-get-started-net-sdk.md)
- [Accessin vianmäärityslokit järvi tietovaraston varten](data-lake-store-diagnostic-logs.md)
