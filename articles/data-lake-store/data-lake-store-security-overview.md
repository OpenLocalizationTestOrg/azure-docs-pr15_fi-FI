<properties
   pageTitle="Tietosäilö järvi security yleiskatsaus | Microsoft Azure"
   description="Tietoja siitä, miten Azure järvi tietosäilö on turvallisempi big datasta säilö"
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
   ms.date="08/02/2016"
   ms.author="nitinme"/>

# <a name="security-in-azure-data-lake-store"></a>Azure järvi tietovaraston suojaus

Monet yritykset ovat hyödyntämistä big datasta analysointitietoja business havainnollistamisen käyttäjille smart päätösten tekemiseen. Organisaatiossa voi olla monimutkaisia ja säännellyillä-ympäristön, jossa monipuolisen käyttäjien kasvava määrä. On tärkeää varmistaaksesi, että tärkeitä tietoja tallennettujen yksittäisten käyttäjien käyttöoikeustasot oikea taso Lisää turvallisesti yrityksen. Azure järvi tietosäilö on suunniteltu helpottamaan täyttävät nämä suojausvaatimukset. Lisätietoja tämän artikkelin tiedot järvi säilön tietoturvaominaisuudet mukaan lukien:

* Todennus
* Todennus
* Verkon eristystaso
* Tietojen suojaaminen
* Valvonta

## <a name="authentication-and-identity-management"></a>Todennus ja tunnistetietojen hallinta

Todennus on prosessi, jolla käyttäjän tunnistetiedot on vahvistettu, kun käyttäjä käsittelee järvi tietosäilö tai palvelu, joka muodostaa yhteyden järvi tietovaraston kanssa. Tunnistetietojen hallinta ja todennusta varten järvi tietovarasto käyttää [Azure Active Directory](../active-directory/active-directory-whatis.md), täydellinen käyttäjätieto- ja access cloud hallintaratkaisu, joka helpottaa käyttäjien ja ryhmien hallinta.

Jokaisen Azure tilauksen voi olla liitetty Azure Active Directory-esiintymä. Käyttäjät ja palvelun käyttäjätietojen, jotka on määritetty Azure Active Directory-palvelun voivat käyttää järvi tietosäilö-tilisi avulla Azure-portaalissa komentorivin työkalut tai asiakassovelluksissa kautta organisaatiosi muodostaa käyttämällä Azure tietojen järvi kaupan SDK-paketissa. Käytä Azure Active Directory keskitetyt ohjausobjektin järjestelmä avaimen edut ovat seuraavat:

* Vaivaton tunnistetietojen elinkaaren hallinta. Käyttäjän tai palvelu (palvelun pääasiallista käyttäjätiedot) voit luoda nopeasti ja kumottu nopeasti yksinkertaisesti poistamalla tai hakemistossa käyttäjätilin poistaminen käytöstä.

* Monimenetelmäisen todentamisen. [Monimenetelmäisen todentamisen](../multi-factor-authentication/multi-factor-authentication.md) antaa arvopaperin käyttäjän kirjautumiset ja tapahtumia varten.

* Todennus minkä tahansa asiakaskoneelta vakio Avaa protokolla, kuten OAuth tai OpenID kautta.

* Yrityksen hakemistopalvelujen ja cloud tunnistetietojen toimittajat Federation.

## <a name="authorization-and-access-control"></a>Todennus ja käyttöoikeuksien hallinta

Kun Azure Active Directory todentaa käyttäjän niin, että käyttäjä voi käyttää Azure järvi tietosäilö, luvan ohjaa järvi tietovaraston käyttöoikeudet. Tietosäilö järvi erottaa luvan tiliin liittyvät ja tietoihin liittyviä toimintoja seuraavalla tavalla:

* [Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-what-is.md) (RBAC) myöntämä Azure tilin hallinta
* POSIX Käyttöoikeusluettelon tietojen säilöön


### <a name="rbac-for-account-management"></a>Tilinhallinta RBAC

Neljä perus rooleihin määritetään järvi tietovaraston oletusarvoisesti. Roolien Salli järvi tietosäilö-tilin lisäämiseen Azure portaalin, PowerShellin cmdlet-komennot ja REST API eri toimintoja. Omistaja- ja osallistuja-roolien suorittaa tietyt hallinnan toiminnot tili. Voit määrittää lukija käyttäjille, joiden käsitellä vain tiedot.

![RBAC roolit] (./media/data-lake-store-security-overview/rbac-roles.png "RBAC roolit")

Huomaa, että vaikka roolit määritetään tilinhallinta-tietyille rooleille vaikuta tietojen käytön. Sinun on käytettävä käyttöoikeusluettelot hallintaa toiminnoista, joita käyttäjä voi suorittaa tiedostojärjestelmässä. Seuraavassa taulukossa on hallintaoikeudet ja tietojen oletusroolit käyttöoikeuksia.

| Roolit                    | Sisältöoikeuksien hallinta               | Tietojen käyttöoikeudet | SELITYS |
| ------------------------ | ------------------------------- | ------------------ | ----------- |
| Ei ole määritetty roolia         | Ei mitään                            | Käyttöoikeusluettelon noudatettava    | Käyttäjä voi käyttää Azure portal tai Azure PowerShellin cmdlet-komennot järvi tietovaraston selaamalla. Käyttäjä voi käyttää vain komentorivin työkalut.
| Omistaja  | Kaikki  | Kaikki  | Omistajan rooli on pääkäyttäjän. Tämä rooli voi hallita kaikki ja käyttää tietoja.
| Lukija   | Vain luku-tilassa  | Käyttöoikeusluettelon noudatettava    | Lukija voi tarkastella kaikkea koskeva tilinhallinta, kuten, jossa käyttäjä on määritetty roolin. Lukija voi tehdä muutoksia.   |
| Avustaja              | Kaikki paitsi Lisää ja poista roolit | Käyttöoikeusluettelon noudatettava    | Osallistujan rooli hallita tietyiltä tiliä, kuten ominaisuuksissa ja luomisen ja ylläpidon ilmoitukset. Osallistujan rooli ei voi lisätä tai poistaa roolit.
| Käyttäjän Access-järjestelmänvalvoja | Lisää ja poista roolit            | Käyttöoikeusluettelon noudatettava    | Accessin järjestelmänvalvoja-rooliin voit hallita tilien käyttöoikeuden. |

Katso ohjeet [määrittää käyttäjät tai käyttöoikeusryhmät järvi tietovaraston tileihin](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Käyttämällä käyttöoikeusluettelot tiedostojärjestelmän toimenpiteet

Tietosäilö järvi on hierarkkinen tiedostojärjestelmässä kuten Hadoop Distributed tiedoston järjestelmän (HDFS), ja se tukee [POSIX käyttöoikeusluettelot](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Ne ohjaavat luku (r), kirjoittaa (w) ja suorita (x) resurssien omistajan rooli, omistajat-ryhmään ja muiden käyttäjien ja ryhmien käyttöoikeudet. Tietoja järvi kaupan julkisen esikatselussa (nykyinen versio) käyttöoikeusluettelot ovat käytettävissä pääkansion, alikansiot ja yksittäiset tiedostot. Käyttöoikeusluettelot, joita haluat käyttää pääkansion koskevat myös kaikki alikansiot ja tiedostot.

On suositeltavaa määrittäminen käyttöoikeusluettelot useille käyttäjille käyttämällä [käyttöoikeusryhmät](../active-directory/active-directory-accessmanagement-manage-groups.md). Käyttäjien lisääminen käyttöoikeusryhmän ja määritä sitten tiedoston tai kansion käyttöoikeuksia, käyttöoikeusryhmän. Tästä on hyötyä, kun haluat lisätä mukautetun access koska sinulla on rajoitettu lisääminen enintään yhdeksän mukautetun access laskujen määrä. Saat lisätietoja paremmin suojattu järvi tietovaraston käyttämällä Azure Active Directory-käyttöoikeusryhmät tallennettuja tietoja [käyttäjien tai käyttöoikeusryhmän käyttöoikeusluettelot Azure järvi tietovaraston tiedostojärjestelmän nimellä](data-lake-store-secure-data.md#filepermissions).

![Luettelon vakio- ja mukautettuja access] (./media/data-lake-store-security-overview/adl.acl.2.png "Luettelon vakio- ja mukautettuja access")

## <a name="network-isolation"></a>Verkon eristystaso

Käytä tietojen järvi säilön avulla hallita tiedot tallennetaan verkkoon tasolla. Voit vahvistaa palomuurien ja määrittää IP-osoitealueita luotettujen asiakkaille. IP-osoitealueita vain asiakkaat, joilla IP-osoitteen määritetyn alueen sisällä voit liittää järvi tietosäilö.

![Palomuurin asetusten ja IP-osoitetta] (./media/data-lake-store-security-overview/firewall-ip-access.png "Palomuurin asetusten ja IP-osoite")

## <a name="data-protection"></a>Tietojen suojaaminen

Organisaatioiden haluat varmistaa, että niiden business tärkeät tiedot on suojattu koko sen elinkaaren aikana. Jos salataanko siirrettävät tiedot, järvi tietovarasto käyttää yleisesti käytetty Transport Layer Security (TLS)-protokolla suojaa tiedot, joka siirtää asiakkaan ja järvi tietovaraston välillä.

Tietojen suojauksen loput tiedot ovat käytettävissä tulevissa versioissa.

## <a name="auditing-and-diagnostic-logs"></a>Valvonnan ja diagnostiikan lokit

Voit käyttää valvonta- tai diagnostiikan lokit sen mukaan, onko hakua lokit hallintaan liittyviä toimintoja tai tietoihin liittyviä toimintoja.

*  Hallintaan liittyviä toimintoja käyttämällä Azure Resurssienhallinta ohjelmointirajapinnan ja Azure-portaalissa kautta valvontalokien esiin.
*  Tietoihin liittyviä toimintoja käyttämällä WebHDFS REST API ja Azure-portaalissa kautta vianmäärityslokit esiin.

### <a name="auditing-logs"></a>Valvonta-lokit

Noudattamaan säädösten mukaan organisaation saattaa edellyttää riittävä kirjausketjut, jos se on tarkastella tietyn tapaukset. Tietosäilö järvi on valmiita seuranta ja se kirjaa kaikki tilin hallintatoimintoihin.

Tilin hallinta kirjausketjuissa Näytä ja valitse sarakkeet, jotka haluat kirjautua. Voit myös viedä valvontalokien Azure-tallennustilan.

![Valvontalokien] (./media/data-lake-store-security-overview/audit-logs.png "Valvontalokien")

### <a name="diagnostic-logs"></a>Vianmäärityslokit

Voit määrittää tietojen käytön kirjausketjut Azure-portaalissa (diagnostiikan asetuksissa) ja lokit tallennuspaikkaa Azure Blob storage tilin luominen.

![Vianmäärityslokit] (./media/data-lake-store-security-overview/diagnostic-logs.png "Vianmäärityslokit")

Kun määrität diagnostiikan asetuksia, voit tarkastella lokit **Vianmäärityslokit** -välilehdessä.

Katso lisätietoja käsittelystä vianmäärityslokit Azure tietojen järvi kaupan, [Access vianmäärityslokit järvi tietovaraston varten](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Yhteenveto

Yritysasiakkaille pyytää tietoja analytics cloud ympäristöä, joka on suojattu ja käyttöä. Azure järvi tietosäilö on suunniteltu helpottamaan osoite näitä vaatimuksia jäsenyyksien hallinta ja todentaminen vakioverkko Azure Active Directory-integrointi, Käyttöoikeusluettelon-pohjainen todennus, verkon eristystaso salauksen siirron ja tietyssä Vie (tulevaisuudessa tulossa) ja valvonta.

Jos haluat nähdä järvi tietovaraston uusista ominaisuuksista, Lähetä meille palautetta [tietojen järvi kaupan Palautesivuston keskustelupalstalle](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Katso myös

- [Yleistä Azure järvi tietosäilö](data-lake-store-overview.md)
- [Tietosäilö järvi käytön aloittaminen](data-lake-store-get-started-portal.md)
- [Tietojen suojaaminen järvi säilössä](data-lake-store-secure-data.md)
