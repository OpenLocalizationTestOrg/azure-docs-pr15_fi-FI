<properties
   pageTitle="Suositellut nimeämiskäytännöt Azure resurssien | Ohjeet | Microsoft Azure"
   description="Suositeltava nimeämiskäytännöt Azure resurssien. Nimi näennäiskoneiden, tallennustilan tilit, verkkojen, virtual verkkojen, aliverkosta ja Azure muihin kohteisiin"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/27/2016"
   ms.author="christb"/>
   
# <a name="recommended-naming-conventions-for-azure-resources"></a>Suositellut Azure resurssien nimeämiskäytännöt

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Minkä tahansa Microsoft Azure resurssin nimi valinta on tärkeää koska:

- On vaikeaa nimen muuttaminen myöhemmin.
- Nimet on täyttävät tietyt resurssin lajin.

Johdonmukaisesti helpottaa resurssien löytämistä. Hän voi myös ilmaista ratkaisussa resurssin rooli.

Tässä artikkelissa on yhteenveto nimeämiskäytäntö säännöt ja Azure resurssien rajoitukset ja nimeämiskäytännöt suosituksia perusaikataulun joukko.  Voit käyttää suositukset pohjana varten tietyn oman nimeämiskäytännön tarpeitasi.  

Kanssa nimeämiskäytännöt menestyksen salaisuus vahvistamiseksi ja seuraa niiden sovellukset ja organisaatioita. 

## <a name="naming-subscriptions"></a>Tilaukset nimeäminen

Azure tilaukset nimeämiseen yksityiskohtainen nimet helpottavat tietoja kontekstin ja poista jokaisen tilauksen käyttötarkoituksen.  Kun työskentelet ympäristössä, jossa on useita tilauksia, seuraavien jaetun nimeämiskäytäntöä parantaa selkeyttä.

Nimeämällä tilaukset suositellut malli on:

`<Company> <Department (optional)> <Product Line (optional)> <Environment>`

- Yrityksen yleensä olisi sama jokaisen tilauksen. Jotkin yritykset voi kuitenkin olla lapsen yritysten organisaation rakenteessa. Näiden yritysten hallitsee keskitetyn IT-ryhmä. Tällaisissa tapauksissa ne voi voidaan eritellä siten, että ylemmän tason yrityksen nimi (*Contoso*)- ja ali yrityksen nimi (*Pohjoinen tuulen*).

- Osasto on organisaation nimi, jossa henkilöryhmän toimi. Kohteen nimitilan sellaisena kuin se on valinnainen.

- Tuotteen rivi on-tuotteita tai -funktiota, joka suoritetaan osaston sisällä tiettyä nimi.
Tämä on yleensä valinnainen sisäisten palvelujen ja sovellusten. Kuitenkin on erittäin suositeltavaa käytettävät julkisen palvelut, jotka tarvitsevat helposti erottaminen ja tunnus (kuten Tyhjennä erottaminen Laskutus tietueita).

- Ympäristön on käyttöönottoprosessin, sovelluksia ja palveluja, kuten keskihajonta, kysymysten ja vastausten tai tuot kuvaava nimi.

| Yrityksen | Osasto | Rivin tuotteen tai palvelun | Ympäristön | Koko nimi  |
----------| ---------- | ----------------------- | ----------- | ---------- |
| Contoso | SocialGaming | AwesomeService | Tuotannon | Contoson SocialGaming AwesomeService tuotannon |
| Contoso | SocialGaming | AwesomeService | Keskihajonta | Contoson SocialGaming AwesomeService keskihajonta |
| Contoso | SE | InternalApps | Tuotannon | Contoson IT InternalApps tuotannon |
| Contoso | SE | InternalApps | Keskihajonta | Contoson IT InternalApps keskihajonta |

<!-- TODO; include more information about organizing subscriptions for application deployment, pods, etc. -->

## <a name="use-affixes-to-avoid-ambiguity"></a>Jälkiliitteet avulla voit välttää moniselitteisyyden

Resurssien Azure nimeämiseen kannattaa suorittaa yleisiä etuliitteitä ja liitteet tyyppi ja resurssin kontekstin selvittäminen.  Tyyppi-metatiedot tietoja aikana kontekstissa, ei käytettävissä ohjelmallisesti, käyttämällä yleisiä jälkiliitteet yksinkertaistaa visual tunnistus.  Kun nimeämiskäytännön mukaisesti on sisällytetty jälkiliitteet, on tärkeää selkeästi Määritä, onko kiinnitettävä nimi (etuliite) alussa tai lopussa (jälkiliite).  

Seuraavassa on esimerkiksi kaksi mahdollista nimet palvelun isännöinnin laskenta-ohjelma:

- SvcCalculationEngine (etuliite)
- CalculationEngineSvc (jälkiliite)

Jälkiliitteet voidaan viitata eri ominaisuuksia, jotka kuvaavat tarvittavat resurssit. Seuraavassa taulukossa on joitakin esimerkkejä tavanomaisesta käytöstä.

| Kuvasuhde | Esimerkki | Huomautuksia |
| ------ | ------- | ----- |
| Ympäristön | keskihajonta-tuot Vastauksiin | Määrittää resurssin ympäristössä |
| Sijainti | UW (US Länsi), luokkaan UE asti (US Itä) | Määrittää alueen, johon resurssi on otettu käyttöön |
| Esiintymän | 01, 02 | Resurssien, joka on useampi kuin yksi nimetty esiintymä (WWW-palvelimien jne.). |
| Tuotteen tai palvelun | palvelun | Tunnistaa tuotteen, sovelluksen tai palvelun, joka tukee resurssi |
| Rooli | SQL-web-viestit | Tunnistaa liittyvän resurssin rooli |

Kun kehittäminen yrityksen tai projektien tietyn nimeämiskäytäntö on seurannan aikana voit valita joukkoa jälkiliitteet ja niiden sijainti (jälkiliite tai etuliite).

## <a name="naming-rules-and-restrictions"></a>Nimeäminen säännöt ja rajoitukset

Kirjoita kunkin resurssi tai palvelun Azure pakottaa tietyt rajoitukset ja laajuus; nimeäminen mikä tahansa nimeämiskäytäntöä tai kuvion on noudatettava edeltävät nimeämiskäytäntö säännöt ja laajuus.  Esimerkiksi nimi, AM yhdistää DNS-nimen (ja täten täytyy olla yksilöllinen kaikissa Azure), VNET nimi on määritetty, se luodaan resurssiryhmä.

Yleensä Vältä tiettyjä merkkejä (`-` tai `_`) minkä tahansa nimen ensimmäinen tai viimeinen merkiksi. Merkeistä aiheuttavat useimmissa kelpoisuussääntöjen epäonnistuu.

| Luokka | Palvelun tai kohteen | Laajuus | Pituus | Kannen | Kelvolliset merkit | Ehdotettu kuvio | Esimerkki |
| ------------- | ----------------- | ----- | ------ | ------ | ---------------- | ----------------- | ------- |
| Resurssiryhmä | Resurssiryhmä | Yleinen | 1-64 | Kirjainkokoa | Alphanumeric alaviiva ja yhdysmerkki | `<service short name>-<environment>-rg` | `profx-prod-rg` |
| Resurssiryhmä | Käytettävyys määrittäminen | Resurssiryhmä | 1-80 | Kirjainkokoa | Alphanumeric alaviiva ja yhdysmerkki | `<service-short-name>-<context>-as` | `profx-sql-as` |
| Yleiset | Tunniste | Liitetty kohde | 512 (nimi), 256 (arvo) | Kirjainkokoa | Aakkosnumeerinen | `"key" : "value"` | `"department" : "Central IT"` |
| Laske | Virtuaalikoneen | Resurssiryhmä | 1-15 | Kirjainkokoa | Alphanumeric alaviiva ja yhdysmerkki | `<name>-<role>-<instance>` | `profx-sql-001` |
| Tallennustilan | Tallennustilan tilin nimi (tiedot) | Yleinen | 3 – 24 | Pienet kirjaimet | Aakkosnumeerinen | `<service short name><type><number>` | `profxdata001` |
| Tallennustilan | Tallennustilan tilin nimi (levyä) | Yleinen | 3 – 24 | Pienet kirjaimet | Aakkosnumeerinen | `<vm name without dashes>st<number>` | `profxsql001st0` |
| Tallennustilan | Säilön nimi | Tallennustilan tilin | 3-63 |   Pienet kirjaimet | Alphanumeric ja viiva | `<context>` | `logs` |
| Tallennustilan | Blob-objektien nimi | Säilö | 1-1 024 | Sama kirjainkoko | Kaikki URL-merkki | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Tallennustilan | Jonon nimi | Tallennustilan tilin | 3-63 | Pienet kirjaimet | Alphanumeric ja viiva | `<service short name>-<context>-<num>` | `awesomeservice-messages-001` |
| Tallennustilan | Taulukkonimi | Tallennustilan tilin | 3-63 |Kirjainkokoa | Aakkosnumeerinen | `<service short name>-<context>` | `awesomeservice-logs` |
| Tallennustilan | Tiedostonimi | Tallennustilan tilin | 3-63 | Pienet kirjaimet | Aakkosnumeerinen | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Verkko | Virtual Network (VNet) | Resurssiryhmä | 2-64 | Asennustavan | ALPHANUMERIC, viivan tai alaviiva piste | `<service short name>-[section]-vnet` | `profx-vnet` |
| Verkko | Aliverkon | Ylemmän tason VNet | 2-80 | Asennustavan | ALPHANUMERIC, alaviiva viiva tai piste | `<role>-subnet` | `gateway-subnet` |
| Verkko | Verkkokäyttöliittymä | Resurssiryhmä | 1-80 | Asennustavan | ALPHANUMERIC, viivan tai alaviiva piste | `<vmname>-<num>nic` | `profx-sql1-1nic` |
| Verkko | Verkko-käyttöoikeusryhmän | Resurssiryhmä | 1-80 | Asennustavan | ALPHANUMERIC, viivan tai alaviiva piste | `<service short name>-<context>-nsg` | `profx-app-nsg` |
| Verkko | Verkko-käyttöoikeusryhmän sääntö | Resurssiryhmä | 1-80 | Asennustavan | ALPHANUMERIC, viivan tai alaviiva piste | `<descriptive context>` | `sql-allow` |
| Verkko | Julkiseen IP-osoite | Resurssiryhmä | 1-80 | Asennustavan | ALPHANUMERIC, viivan tai alaviiva piste | `<vm or service name>-pip` | `profx-sql1-pip` |
| Verkko | Kuormituksen | Resurssiryhmä | 1-80 | Asennustavan | ALPHANUMERIC, viivan tai alaviiva piste | `<service or role>-lb` | `profx-lb` |
| Verkko | Lataa tasapainoinen säännöt Config | Kuormituksen | 1-80 | Asennustavan | ALPHANUMERIC, viivan tai alaviiva piste | `descriptive context` | `http` |

<!-- TODO fill in the rest of these resources
| Networking | Azure Application Gateway | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `<service or role>-aag` | `profx-aag`
| Networking | Azure Application Gateway Connection | Azure Application Gateway | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `` | TODO
| Networking | Traffic Manager Profile | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | TODO | TODO
-->

## <a name="organizing-resources-with-tags"></a>Järjesteleminen tunnisteiden resursseja

Azure-Resurssienhallinta tukee tunnisteiden kohteiden haluamaansa tekstimerkkijonoja kontekstin selvittäminen ja nopeuttaa automaatio kanssa.  Esimerkiksi tunniste `"sqlVersion: "sql2014ee"` voi tunnistaa VMs käynnissä automaattinen komentosarjojen suorittamisen yksinkertaisella SQL Server 2014 Enterprise Edition-yhdistelmäympäristössä.  Tunnisteita käytetään verkkotunnistetietojen ja parantaa valinnut nimeämiskäytännöt reunassa kontekstissa.

> [AZURE.TIP]Tunnisteiden välisenä ajankohtana resurssiryhmät, joilla voit linkittää ja yhdistää kohteiden eri ympäristöissä eri on muita tunnisteita etu.

Kunkin resurssi tai resurssiryhmä voi olla enintään **15** tunnisteet. Tunnisteen nimi on 512 merkkiä ja tunniste-arvo on enintään 256 merkkiä.

Lisätietoja resurssi tunnisteita viitata [käyttäminen tunnisteen, kun haluat järjestää Azure resurssit](../resource-group-using-tags.md).

Joitakin yleisiä tunnisteiden Käyttötapaukset ovat seuraavat:

- **Laskutus**; Resurssien ryhmitteleminen ja niiden liittäminen laskutus tai Kulu takaisin koodit.
- **Palvelun kontekstin tunnistus**; Selvitä ryhmiä resurssien resurssin ryhmien Yleiset toiminnot ja ryhmittely
- **Käyttöoikeuksien hallinta ja suojauksen kontekstissa**. Järjestelmänvalvojan rooli tunnuksen perusteella portfolion, järjestelmä, palvelu, sovellus, esiintymän jne.

> [AZURE.TIP]Tunnisteen etuajassa - tunniste usein.  Parempi on tunnisteita värimallin paikassa perusaikataulun ja säädä päälle aika sen sijaan, että sinun tarvitsee muuttelemaan sen jälkeen.  

Esimerkki joitakin yleisiä tunnisteiden tavoista:

| Tunnistenimi | Avain | Esimerkki | Kommentti |
| -------- | --- | ------- | ------- |
| Laskun tai sisäinen palautus tunnus | billTo  | `IT-Chargeback-1234` | Sisäinen i/o tai laskutuksen koodi |
| Operaattori tai suoraan vastaava henkilö (DRI) | managedBy | `joe@contoso.com`  | Alias tai sähköpostiosoite |
| Projektinimi | projektin nimi | `myproject`  | Projektin tai tuotteen nimi |
| Project-versio | Project-versio | `3.4`  | Projektin tai tuotteen versio |
| Ympäristön | ympäristön | `<Production, Staging, QA >` | Ympäristön tunnus | 
| Taso | taso | `Front End, Back End, Data` | Taso tai rooli/kontekstin tunnistus |
| Profiilin tiedot | dataProfile | `Public, Confidential, Restricted, Internal` | Tiedot tallennetaan resurssin suojaustasoa |
 
## <a name="tips-and-tricks"></a>Vihjeitä ja vinkkejä

Jotkin resurssityyppiä saattaa edellyttää muita tarkkaan nimeämisestä ja nimeämiskäytännön.

### <a name="virtual-machines"></a>Näennäiskoneiden

Erityisesti suurempi topologioissa nimeäminen huolellisesti näennäiskoneiden tehostaa rooli ja Jokaisessa koneessa käyttötarkoituksen ja ottamalla käyttöön aiempaa helpommin ennakoitavia komentosarjan.

> [AZURE.WARNING]Azure virtual jokaiseen tietokoneeseen on sekä Azure resurssinimi ja käyttöjärjestelmän-isäntänimi.  
> Jos resurssinimi ja isäntänimi ovat eri, hallinta VMs voi olla hankalaa ja voidaan välttää.
> Jos virtual machine on luotu .vhd jo on määritetty käyttöjärjestelmä, jossa isäntänimi.

- [Windows Server VMs nimeämiskäytännöt](https://support.microsoft.com/en-us/kb/188997)

<!-- TODO - recommendations on naming VMs. -->

### <a name="storage-accounts-and-storage-entities"></a>Tallennustilan asiakkaat ja tallennustilaa yksiköt

Tilit - levyjä varmuuskopioiminen VMs varten ja BLOB-objektit ja olevien taulukoiden tiedot tallennetaan tallennustilan ensisijainen tapauksista on.  Tallennustilan tilejä AM levyjen olisi noudattamalla nimeämiskäytännön liittäminen ylätason AM nimi (mahdollisten tarvetta tallennustilan useita tilejä tehokkaimpia AM varastointiyksikköjen, myös ja puhelinnumeron jälkiliite).

> [AZURE.TIP]Tallennustilan tilit - tietoja tai levyjen - varten pitäisi seurata nimeämiskäytäntöä, joka sallii useiden tallennustilan tilit ja hyödyntää (eli aina joko numeerinen liite).

Se mahdollista määrittää mukautetun toimialuenimen Azure-tallennustilan tilin tietojen Blob-objektien käyttäminen.
Oletusarvoinen päätepiste Blob-palvelun on `https://mystorage.blob.core.windows.net`.

Jos mukautetun toimialueen (kuten www.contoso.com) yhdistäminen blob-päätepisteen tallennustilan tilin, voit myös avata, mutta blob storage-tilisi tiedot käyttämällä kyseisen toimialueen. Mukautettu toimialuenimi, esimerkiksi kanssa `http://mystorage.blob.core.windows.net/mycontainer/myblob` saattaa voi käyttää `http://www.contoso.com/mycontainer/myblob`.

Lisätietoja tämän ominaisuuden viitata [määrittäminen Blob storage päätepiste mukautettua toimialuenimeä](../storage/storage-custom-domain-name.md).

Lisätietoja BLOB-objektit, säilöjä ja taulukoiden nimeämisestä:

- [Nimeämisestä ja viittaava säilöjen BLOB-objektit ja metatiedot](https://msdn.microsoft.com/library/dd135715.aspx)
- [Nimeäminen olevien ja metatiedot](https://msdn.microsoft.com/library/dd179349.aspx)
- [Taulukoiden nimeäminen](https://msdn.microsoft.com/library/azure/dd179338.aspx)

Blob-nimessä voi olla minkä tahansa merkkiyhdistelmän, mutta varattu URL-osoitteessa merkkien on ohitettuja oikein. Vältä Blob-objektien nimet, jotka loppua pisteeseen (.), vinoviiva (/) tai sarjan tai kahden yhdistelmä. Käytännön mukaan vinoviiva on **virtuaalisen** kansion erottimen. Älä käytä taaksepäin vinoviiva (\) Blob-objektien nimen. Asiakkaan ohjelmointirajapinnan saattaa sallia sen, mutta hash oikein epäonnistuu ja allekirjoitukset ei vastaa.

Ei voida muokata tallennustilan tilin tai säilön nimen, sen luomisen jälkeen.
Jos haluat käyttää uuden nimen, poista se ja luoda uuden.

> [AZURE.TIP] On suositeltavaa muodostaa nimeämiskäytäntö kaikki tallennustilan asiakkaat ja tyypit ennen uuden palvelun tai sovelluksen kehittäminen koulusi.

## <a name="example---deploying-an-n-tier-service"></a>Esimerkki: n-taso-palvelun käyttöönotto

Tässä esimerkissä on määrittäminen n tason service-määritys-koostuva edusta IIS-palvelimiin (ylläpidettävä Windows Server VMs), SQL Server (ylläpidettävä kaksi Windows Server VMs), (ylläpidettävä 6 Linux VMs) ElasticSearch-klusterin ja liittyvän tallennustilan tilit, virtual verkkojen resurssien ryhmittely ja kuormituksen.

Asetetaan ensin määrittämällä tilannekohtaiset nimeämiskäytännön tämän sovelluksen:

| Kohteen | Konferenssi | Kuvaus  |
| ------ | ---------- | ------------ |  
| Palvelunimi | `profx` | Sovelluksen tai palvelun otetaan käyttöön lyhyt nimi |
| Ympäristön | `prod` | Tämä on tuotannon käyttöönoton (eikä kysymysten ja vastausten, Testaa jne.) |

Kyseisen perusaikataulun-olemme sitten kartoittaa sopimusten kunkin resurssityyppejä:

| Resurssilaji | Konferenssi Base | Esimerkki |
| ------------- | --------------- | ------- |
| Tilauksen | `<Company> <Department (optional)> <Product Line (optional)> <Environment>` | `Contoso IT InternalApps Profx Production` |
| Resurssiryhmä | `servicename-rg` | `profx-rg` |
| VPN | `servicename-vnet` | `profx-vnet` |
| Aliverkon | `role-subnet` | `sql-vnet` |
| Kuormituksen | `servicename-lb` | `profx-lb` |
| Virtuaalikoneen | `servicename-role[number]` | `profx-sql0` |
| Tallennustilan tilin | `<vmnamenodashes>st<num>` | `profxsql0st0` |

Kaavion seuraava komento:

![sovelluksen topologian kaavio] (media/guidance-naming-conventions/guidance-naming-convention-example.png "Esimerkki sovelluksen topologian")

## <a name="sample---azure-cli-script-for-deploying-the-sample-above"></a>Malli - Azure CLI komentosarjan yllä otosten käyttöönotto

```bash
#!/bin/sh

#####################################################################
# Sample script using the Azure CLI to build out an application 
# demonstrating naming conventions.  
#
# Note; this script is not intended for production deployment, as it does 
# not create availability sets, configure network security rules, etc.
#####################################################################

# Set up variables to build out the naming conventions for deploying
# the cluster  
LOCATION=eastus2
APP_NAME=profx
ENVIRONMENT=prod
USERNAME=testuser
PASSWORD="testpass"

# Set up the tags to associate with items in the application
TAG_BILLTO="InternalApp-ProFX-12345"
TAGS="billTo=${TAG_BILLTO}"

# Explicitly set the subscription to avoid confusion as to which subscription
# is active/default
SUBSCRIPTION=3e9c25fc-55b3-4837-9bba-02b6eb204331

# Set up the names of things using recommended conventions
RESOURCE_GROUP="${APP_NAME}-${ENVIRONMENT}-rg"
VNET_NAME="${APP_NAME}-vnet"

# Set up the postfix variables attached to most CLI commands
POSTFIX="--resource-group ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}"

##########################################################################################
# Set up the VM conventions for Linux and Windows images

# For Windows, get the list of URN's via 
# azure vm image list ${LOCATION} MicrosoftWindowsServer WindowsServer 2012-R2-Datacenter
WINDOWS_BASE_IMAGE=MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20160126

# For Linux, get the list or URN's via 
# azure vm image list ${LOCATION} canonical ubuntuserver
LINUX_BASE_IMAGE=canonical:ubuntuserver:16.04.0-DAILY-LTS:16.04.201602130

#########################################################################################
## Define functions 
create_vm ()
{
    vm_name=$1
    vnet_name=$2
    subnet_name=$3
    os_type=$4
    vhd_path=$5
    vm_size=$6
    diagnostics_storage=$7

    # Create the network interface card for this VM
    azure network nic create --name "${vm_name}-0nic" --subnet-name ${subnet_name} --subnet-vnet-name ${vnet_name} \
        --tags="${TAGS}" ${POSTFIX}

    # Create the storage account for this vm's disks (premium locally redundant storage -> PLRS)
    # Note the ${var//-/} syntax to remove dashes from the vm name
    storage_account_name=${vm_name//-/}st01
    azure storage account create --type=PLRS --tags "${TAGS}" ${POSTFIX} "${storage_account_name}"

    # Map the name of the diagnostics storage account to a blob URI for boot diagnostics
    # This is (currently) required when deploying with a named premium storage account 
    diag_blob="https://${diagnostics_storage}.blob.core.windows.net/"

    # Create the VM
    azure vm create --name ${vm_name} --nic-name "${vm_name}-0nic" --os-type ${os_type} \
        --image-urn ${vhd_path} --vm-size ${vm_size} --vnet-name ${vnet_name} --vnet-subnet-name ${subnet_name} \
        --storage-account-name "${storage_account_name}" --storage-account-container-name vhds --os-disk-vhd "${vm_name}-osdisk.vhd" \
        --admin-username "${USERNAME}" --admin-password "${PASSWORD}" \
        --boot-diagnostics-storage-uri "${diag_blob}" \
        --tags="${TAGS}" ${POSTFIX} 
}

###################################################################################################
# Create resources

# Step 1 - create the enclosing resource group
azure group create --name "${RESOURCE_GROUP}" --location "${LOCATION}" --tags "${TAGS}" --subscription "${SUBSCRIPTION}"

# Step 2 - create the network security groups

# Step 3 - create the networks (VNet and subnets)
azure network vnet create --name "${VNET_NAME}" --address-prefixes="10.0.0.0/8" --tags "${TAGS}" ${POSTFIX}
# TODO - does subnet support tagging?
azure network vnet subnet create --name gateway-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.1.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-master-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.2.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-data-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.3.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name sql-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.4.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}

# Step 4 - define the load balancer and network security rules
azure network lb create --name "${APP_NAME}-lb" ${POSTFIX}
# In a production deployment script, we'd create load balancer rules and 
# network security groups here

# Step 5 - create a diagnostics storage account
diagnostics_storage_account=${APP_NAME//-/}diag
azure storage account create --type=LRS --tags "${TAGS}" ${POSTFIX} "${diagnostics_storage_account}"

# Step 6.1 - Create the gateway VMs
for i in `seq 1 4`;
do
    create_vm "${APP_NAME}-gw${i}" "${APP_NAME}-vnet" "gateway-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}" 
done    

# Step 6.2 - Create the ElasticSearch master and data VMs
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-master${i}" "${APP_NAME}-vnet" "es-master-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-data${i}" "${APP_NAME}-vnet" "es-data-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done

# Step 6.3 - Create the SQL VMs
create_vm "${APP_NAME}-sql0" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
create_vm "${APP_NAME}-sql1" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
```
