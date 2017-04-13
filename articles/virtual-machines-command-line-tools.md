<properties
    pageTitle="Azure CLI komentoja hallinnan tilassa | Microsoft Azure"
    description="Azure rivin komentoliittymä (CLI) komentoja hallinnan tilassa hallittavan ominaisuuksissa perinteinen käyttöönotto-mallissa"
    services="virtual-machines-linux,virtual-machines-windows,mobile-services, cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="danlep"/>

# <a name="azure-cli-commands-in-azure-service-management-asm-mode"></a>Azure CLI komentoja Azure Service Management (asm)-tilassa.

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-classic-include.md)]Voit myös [kaikki Resurssienhallinta mallin komennot lisätietoja](virtual-machines/azure-cli-arm-commands.md), ja käyttää CLI siirtämään [resurssien](virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md) perinteinen Resurssienhallinta-malliin.

Tässä artikkelissa on syntaksista ja Azure CLI komennot yleisesti käytetään luominen ja hallinta perinteinen käyttöönoton mallin Azure resursseja asetukset. Voit käyttää näitä komentoja suorittamalla CLI Azure Service Management (asm)-tilassa. Tämä ei ole täydellinen viittauksen ja CLI-versiosi voivat näyttää hieman eri komentoja ja parametreja. 

Pääset alkuun, [Asenna Azure-CLI](xplat-cli-install.md) ensimmäisen ja [Azure tilauksen yhdistäminen](xplat-cli-connect.md).

Nykyisen komennon syntaksin ja vaihtoehdot komentorivillä, kirjoita `azure help` tai, saat näkyviin tietyn komennon `azure help [command]`. Etsi myös ohjeista CLI esimerkkejä luomisen ja ylläpidon Azure palveluja.

Valinnaisten parametrien näkyvät hakasulkeisiin (esimerkiksi `[parameter]`). Kaikki muut parametrit tarvitaan.

Lisäksi komento kielikohtaiset valinnaisten parametrien kuvattu seuraavassa on kolme valinnaisten parametrien, jonka avulla voidaan näyttää tarkemmat tiedot, kuten pyynnön asetukset ja tilakoodin. `-v` Parametri on yksityiskohtainen tuloste ja `-vv` parametri on myös yksityiskohtaisempia yksityiskohtainen tuloste. `--json` Asetus tulostaa tulos raaka json-muodossa.

## <a name="setting-asm-mode"></a>Asetus asm tila

Seuraavalla komennolla voit ottaa käyttöön Azure CLI hallinnan komennot.

    azure config mode asm

>[AZURE.NOTE] CLI Azure Resurssienhallinta tilaa ja Azure hallinnan tilaa ovat toisensa poissulkevia. Toisin sanoen resurssit luodaan yksi tilassa ei voi hallita muiden tilasta.

## <a name="manage-your-account-information-and-publish-settings"></a>Tilitietojen hallinta ja Julkaisuasetukset
Yksi tapa CLI voit muodostaa yhteyden tiliisi on käyttää Azure Tilaustiedot. (Katso [Azure tilauksen Azure-CLI Yhdistä](xplat-cli-connect.md) muita asetuksia.) Näitä tietoja voi hakea Azure perinteinen portaalin Julkaise asetukset tiedostossa seuraavassa kuvatulla tavalla. Voit tuoda Julkaise-asetustiedosto kuin pysyvä paikallisesti asetus, joka CLI käyttää seuraavia toimintoja varten. Haluat tuoda oman Julkaisuasetukset kerran.

**Lataa tilin [Valitsimet]**

Tämä komento Avaa selaimessa, voit ladata .publishsettings tiedoston perinteinen Azure-portaalista.

    ~$ azure account download
    info:   Executing command account download
    info:   Launching browser to https://windows.azure.com/download/publishprofile.aspx
    help:   Save the downloaded file, then execute the command
    help:   account import <file>
    info:   account download command OK

**tilin tuominen [Valitsimet] &lt;tiedosto >**


Tämä komento tuo publishsettings tiedoston tai varmenteen niin, että se voidaan käyttää työkalu tulevaisuudessa istuntoja.

    ~$ azure account import publishsettings.publishsettings
    info:   Importing publish settings file publishsettings.publishsettings
    info:   Found subscription: 3-Month Free Trial
    info:   Found subscription: Pay-As-You-Go
    info:   Setting default subscription to: 3-Month Free Trial
    warn:   The 'publishsettings.publishsettings' file contains sensitive information.
    warn:   Remember to delete it now that it has been imported.
    info:   Account publish settings imported successfully

> [AZURE.NOTE] Publishsettings-tiedoston voi olla useita tilauksia (eli tilauksen nimi ja tunnus) tietoja. Kun tuot publishsettings-tiedoston, ensimmäisen tilauksen käytetään kuvauksen oletusarvo. Jos haluat käyttää eri tilauksen, suorita seuraava komento:
<code>~$ azure config set subscription &lt;other-subscription-id&gt;</code>

**Poista tili [Valitsimet]**

Tämä komento poistaa tallennetut publishsettings, joka on tuotu. Käyttää tätä komentoa, jos olet valmis-työkalulla tässä tietokoneessa ja haluat varmistaa, että työkalu ei voi käyttää tilisi tulevaisuudessa istuntoja.

    ~$ azure account clear
    Clearing account info.
    info:   OK

**Asiakasluettelo [Valitsimet]**

Luettelo tuodut tilaukset

    ~$ azure account list
    info:    Executing command account list
    data:    Name                                    Id
           Current
    data:    --------------------------------------  -------------------------------
    -----  -------
    data:    Forums Subscription                     8679c8be-3b05-49d9-b8fb  true
    data:    Evangelism Team Subscription            9e672699-1055-41ae-9c36  false
    data:    MSOpenTech-Prod                         c13e6a92-706e-4cf5-94b6  false

**tilin määrittäminen [Valitsimet] &lt;tilaus&gt;**

Aseta nykyisen tilauksesi

###<a name="commands-to-manage-your-affinity-groups"></a>Komennot, joilla affiniteetti-ryhmien hallinta

**affiniteetti ryhmän luettelosta [Valitsimet]**

Tämä komento on lueteltu Azure affiniteetti ryhmät.

Affiniteetti ryhmät voidaan määrittää, kun ryhmälle näennäiskoneiden laajuinen useita fyysisten laitteiden. Affiniteetti ryhmän määrittää, että fyysisten laitteiden pitää niin lähellä toisiinsa käytettävissä, kun haluat vähentää verkon latenssin.

    ~$ azure account affinity-group list
    + Fetching affinity groups
    data:   Name                                  Label   Location
    data:   ------------------------------------  ------  --------
    data:   535EBAED-BF8B-4B18-A2E9-8755FB9D733F  opentec  West US
    info:   account affinity-group list command OK

**tilin affiniteetti-ryhmässä Luo [Valitsimet] &lt;nimi&gt;**

Tämä komento luo affiniteetti-ryhmä

    ~$ azure account affinity-group create opentec -l "West US"
    info:    Executing command account affinity-group create
    + Creating affinity group
    info:    account affinity-group create command OK

**tilin affiniteetti-ryhmässä Näytä [Valitsimet] &lt;nimi&gt;**

Tämä komento näkyy affiniteetti ryhmän tiedot

    ~$ azure account affinity-group show opentec
    info:    Executing command account affinity-group show
    + Getting affinity groups
    data:    $ xmlns "http://schemas.microsoft.com/windowsazure"
    data:    $ xmlns:i "http://www.w3.org/2001/XMLSchema-instance"
    data:    Name "opentec"
    data:    Label "b3BlbnRlYw=="
    data:    Description $ i:nil "true"
    data:    Location "West US"
    data:    HostedServices ""
    data:    StorageServices ""
    data:    Capabilities Capability 0 "PersistentVMRole"
    data:    Capabilities Capability 1 "HighMemory"
    info:    account affinity-group show command OK

**tilin affiniteetti-ryhmän Poista [Valitsimet] &lt;nimi&gt;**

Tällä komennolla voit poistaa määritetty affiniteetti-ryhmä

    ~$ azure account affinity-group delete opentec
    info:    Executing command account affinity-group delete
    Delete affinity group opentec? [y/n] y
    + Deleting affinity group
    info:    account affinity-group delete command OK

###<a name="commands-to-manage-your-account-environment"></a>Voit hallita tilin ympäristön komennot

**Kirjekuori luettelosta [Valitsimet]**

Tili-ympäristöissä luettelo

    C:\windows\system32>azure account env list
    info:    Executing command account env list
    data:    Name
    data:    ---------------
    data:    AzureCloud
    data:    AzureChinaCloud
    info:    account env list command OK

**tilin kirjekuori Näytä [Valitsimet] [ympäristön]**

Tilin ympäristön tietojen näyttäminen

    ~$ azure account env show
    info:    Executing command account env show
    Environment name: AzureCloud
    data:    Environment publishingProfile  http://go.microsoft.com/fwlink/?LinkId=2544
    data:    Environment portal  http://go.microsoft.com/fwlink/?LinkId=2544
    info:    account env show command OK

**tili kirjekuori lisätä [Valitsimet] [ympäristön]**

Tämä komento lisää ympäristössä tilille

**tilin kirjekuori määrittäminen [Valitsimet] [ympäristön]**

Tämä komento asettaa tili-ympäristössä

**tilin poistaminen kirjekuori [Valitsimet] [ympäristön]**

Tällä komennolla voit poistaa tietyn ympäristön tililtä

## <a name="commands-to-manage-your-classic-virtual-machines"></a>Voit hallita perinteinen näennäiskoneiden komennot
Seuraavassa kaaviossa näkyy, miten perinteinen Azure näennäiskoneiden isännöidään Azure pilvipalvelussa käyttöönoton tuotantoympäristössä.

![Azure tekniset kaavio](./media/virtual-machines-command-line-tools/architecturediagram.jpg)

**Luo-uusi** Luo asema Blob-objektien tallennustilaan (eli e:\ kaaviossa); **Liitä** liittää levy, joka on jo luotu, mutta irrallisia virtual machine.

**AM luominen [Valitsimet] &lt;dns-nimi > &lt;kuva > &lt;käyttäjänimi > [salasana]**

Tämä komento luo Azure virtuaalikoneen. Oletusarvon mukaan kunkin virtuaalikoneen (AM) luodaan omassa pilvipalvelussa. Voit määrittää, että virtual machine lisätään - c-vaihtoehdon avulla aiemmin luotuun pilvipalveluun suorittimessa tähän.

AM luominen-komento, kuten Azure perinteinen portal, luo vain käyttöönoton tuotantoympäristössä näennäiskoneiden. Ei ei voi luoda virtual machine pilvipalveluun väliaikaisen käyttöönotto-ympäristössä. Jos tilauksesi ei ole käytössä olevan Azure-tallennustilan tilin-komento luo jokin.

Voit määrittää sijainnin--sijainnin parametrin kautta, tai voit määrittää affiniteetti ryhmän parametrin – affiniteetti ryhmän kautta. Jos kumpaakaan ei anneta, sinua kehotetaan tekemään kelvollinen sijainnit-luettelosta.

Annettu salasana on 8 123 merkkiä ja käyttöjärjestelmän, jota käytät virtual tietokoneen salasana monimutkaisuus ehtojen mukaan.

Jos aiot SSH avulla voit hallita käyttöön Linux-virtual machine (Katso on yleensä tapaus), sinun on otettava SSH kautta -e-vaihtoehto, kun luot virtuaalikoneen. Ei ole mahdollista käyttöön SSH virtuaalikoneen luomisen jälkeen.

Windows-näennäiskoneiden ottaa RDP myöhemmin lisäämällä portti 3389 päätepisteen.

Seuraavia valinnaisia parametreja tuetaan tämän komennon:

**- c---yhteyden** virtuaalikoneen sisällä aiemmin luotuja käyttöönoton luominen isännöintipalvelu palvelu. Jos - vmname ei käytetä tässä vaihtoehdossa, uusi virtuaalikoneen nimi luodaan automaattisesti.<br />
**-n,--AM nimi** Määritä virtuaalikoneen nimi. Tämä parametri on oletusarvoisesti isännöintipalvelu palvelunimi. Jos - vmname ei ole määritetty, uusi virtuaalikoneen nimi luodaan kuin &lt;palvelun nimi >&lt;tunnus >, jossa &lt;tunnus > on aiemmin näennäiskoneiden palvelun lisättynä 1 määrä. Esimerkiksi jos tällä komennolla Lisää virtual machine isännöintipalvelu MyService, jossa on yksi aiemmin virtual machine-palveluun, uusi virtuaalikoneen nimeksi MyService2.<br />
**-u,--blob-URL-osoite** Määritä target blob storage URL-osoite, jolle haluat luoda virtuaalikoneen järjestelmälevy. <br />
**z,--AM koko** Määritä virtuaalikoneen koko. Kelvolliset arvot ovat: "ExtraSmall", "Pieni", "Medium", "Suuri", "ExtraLarge", "A5", "A6", "A7", "A8", "A9", "A10", "A11", "Basic_A0", "Basic_A1", "Basic_A2", "Basic_A3", "Basic_A4", "Standard_D1", "Standard_D2", "Standard_D3", "Standard_D4", "Standard_D11", "Standard_D12", "Standard_D13", "Standard_D14", "Standard_DS1", "Standard_DS2", "Standard_DS3", "Standard_DS4", "Standard_DS11", "Standard_DS12", "Standard_DS13", "Standard_DS14", "Standard_G1", "Standard_G2" , "Standard_G3", "Standard_G4", "Standard_G55". Oletusarvo on "Pieni". <br />
**-r** Lisää RDP yhteyden Windows virtual koneeseen. <br />
**– e,--ssh** Lisää SSH yhteyden Windows virtual koneeseen. <br />
**-t,--ssh-varmenne** Määrittää SSH varmenne. <br />
**-s** Tilaus <br />
**-o--yhteisön** Määritetyn kuva on yhteisön kuva <br />
**-w** VPN-nimi <br/>
**-l,--sijainti** määrittää sijainti (esimerkiksi "Pohjois keskitetyn US"). <br />
**-,--affiniteetti ryhmän** määrittää affiniteetti-ryhmä.<br />
**w,--virtual verkko-nimi** Määritä, johon haluat lisätä uuden virtuaalikoneen virtual verkkoon. Virtual verkkojen voit määrittää ja hallita perinteinen Azure-portaalista.<br />
**-b,--aliverkon nimet** Määrittää määrittämään virtuaalikoneen aliverkon nimet.

Tässä esimerkissä MSFT__Win2K8R2SP1-120514-1520-141205-01-en-us-30GB on platform myöntämä kuva. Saat lisätietoja käyttöjärjestelmän AM kuvaluettelo.

    ~$ azure vm create my-vm-name MSFT__Windows-Server-2008-R2-SP1.11-29-2011 username --location "West US" -r
    info:   Executing command vm create
    Enter VM 'my-vm-name' password: ************
    info:   vm create command OK

**AM luominen kohteesta &lt;dns-nimi > &lt;rooli tiedosto >**

Tämä komento luo Azure virtuaalikoneen JSON rooli-tiedostosta.

    ~$ azure vm create-from my-vm example.json
    info:   OK

**AM luettelon [Valitsimet]**

Tämä komento on lueteltu Azure-virtuaalikoneissa. --Json-asetus määrittää, että tulokset palautetaan raaka JSON-muodossa.

    ~$ azure vm list
    info:   Executing command vm list
    data:   DNS Name                          VM Name      Status
    data:   --------------------------------  -----------  ---------
    data:   my-vm-name.cloudapp-preview.net        my-vm        ReadyRole
    info:   vm list command OK

**AM sijaintiluettelosta [Valitsimet]**

Tämä komento on lueteltu kaikki käytettävissä olevat Azure-tili sijainnit.

    ~$ azure vm location list
    info:   Executing command vm location list
    data:   Name                   Display Name
    data:   ---------------------  ------------
    data:   Azure Preview  West US
    info:   account location list command OK

**AM Näytä [Valitsimet] &lt;nimi >**

Tämä komento näyttää Azure virtuaalikoneen tietoja. --Json-asetus määrittää, että tulokset palautetaan raaka JSON-muodossa.

    ~$ azure vm show my-vm
    info:   Executing command vm show
    data:   {
    data:       InstanceSize: 'Small',
    data:       InstanceStatus: 'ReadyRole',
    data:       DataDisks: [],
    data:       IPAddress: '10.26.192.206',
    data:       DNSName: 'my-vm.cloudapp.net',
    data:       InstanceStateDetails: {},
    data:       VMName: 'my-vm',
    data:       Network: {
    data:           Endpoints: [
    data:               {
    data:                   Protocol: 'tcp',
    data:                   Vip: '65.52.250.250',
    data:                   Port: '63238' ,
    data:                   LocalPort: '3389',
    data:                   Name: 'RemoteDesktop'
    data:               }
    data:           ]
    data:       },
    data:       Image: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       OSVersion: 'WA-GUEST-OS-1.18_201203-01'
    data:   }
    info:   vm show command OK

**AM poistaminen [Valitsimet] &lt;nimi >**

Tällä komennolla voit poistaa Azure virtuaalikoneen. Oletusarvoisesti tämä komento ei poista Azure-blob, joista käyttöjärjestelmä-levyn ja tietoja-levyn luodaan. Voit poistaa blob tai johon se perustuu virtuaalikoneen, Määritä -b-vaihtoehtoa.

    ~$ azure vm delete my-vm
    info:   Executing command vm delete
    info:   vm delete command OK

**AM Käynnistä [Valitsimet] &lt;nimi >**

Tämä komento aloittaa Azure virtuaalikoneen.

    ~$ azure vm start my-vm
    info:   Executing command vm start
    info:   vm start command OK

**AM uudelleen [Valitsimet] &lt;nimi >**

Tämä komento käynnistyy Azure virtuaalikoneen.

    ~$ azure vm restart my-vm
    info:   Executing command vm restart
    info:   vm restart command OK

**AM Sammuta [Valitsimet] &lt;nimi >**

Tämä komento sulkee Azure virtuaalikoneen. -P-vaihtoehdon avulla voi määrittää, että Laske resurssin julkaistaan ei sammuta.

```
~$ azure vm shutdown my-vm
info:   Executing command vm shutdown
info:   vm shutdown command OK  
```

**AM sieppaus &lt;AM nimi > &lt;kohde-kuva-name >**

Tämä komento sieppaa Azure virtuaalikoneen kuva.

Virtuaalikoneen kuva voi tallentaa vain, jos virtuaalikoneen tila on **pysäytetty**. Sulje virtuaalikoneen, ennen kuin jatkat.

    ~$ azure.cmd vm capture my-vm mycaptureimagename --delete
    info:   Executing command vm capture
    + Fetching VMs
    + Capturing VM
    info:   vm capture command OK

**Vie AM [Valitsimet] &lt;AM nimi > &lt;tiedoston polku >**

Tämä komento Vie tiedostoon Azure virtuaalikoneen kuva

    ~$ azure vm export "myvm" "C:\"
    info:    Executing command vm export
    + Getting virtual machines
    + Exporting the VM
    info:   vm export command OK

##  <a name="commands-to-manage-your-azure-virtual-machine-endpoints"></a>Voit hallita Azure virtuaalikoneen päätepisteet komennot
Seuraavassa kaaviossa on esitetty eri esiintymissä perinteinen virtual koneen tyypillinen käyttöönoton arkkitehtuuri. Tässä esimerkissä portti 3389 on auki virtual jokaiseen tietokoneeseen (RDP käytön). Kunkin virtual tietokoneeseen, jota käytetään kuormituksen, reitti-liikenne paikalliseen virtuaalikoneen on myös sisäinen IP-osoite (esimerkiksi 168.55.11.1). Tämä sisäinen IP-osoite voidaan myös näennäiskoneiden väliseen viestintään.

![azurenetworkdiagram](./media/virtual-machines-command-line-tools/networkdiagram.jpg)

Ulkoinen pyynnöt näennäiskoneiden käymällä läpi kuormituksen tasauspalvelun. Tästä syystä pyynnöt ei voi määrittää tietyn virtual konetta-käyttöönotoissa useita näennäiskoneiden vastaan. Useita näennäiskoneiden käyttöönotoissa portin yhdistäminen on määritettävä näennäiskoneiden (portti AM) ja kuormituksen (kg portti) välillä.

**AM päätepisteen luominen &lt;AM nimi > &lt;kg portti > [portti AM]**

Tämä komento luo virtuaalikoneen päätepiste. Voit määrittää, onko käyttöön suoraan palvelimen sijoitetun pääoman tämän päätepisteen oletusarvon mukaan poistettu käytöstä voi käyttää myös -u tai – ota käyttöön-suoraan-palvelin-return.

    ~$ azure vm endpoint create my-vm 8888 8888
    azure vm endpoint create my-vm 8888 8888
    info:   Executing command vm endpoint create
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint create command OK

**AM päätepisteen luominen-useita [Valitsimet] &lt;AM nimi > &lt;kg portti > [:&lt;AM portti > [:&lt;protokolla > [:&lt;Ota käyttöön-suoraan-palvelin-return > [:&lt;kg-set-name > [:&lt;näytteenottimen protokolla > [:&lt;näytteenottimen portti > [:&lt;näytteenottimen polku > [:&lt;sisäinen nimi kuin kg >]]] {1 -*}**

Voit luoda useita AM päätepisteet.

**AM päätepisteen poistaminen [Valitsimet] &lt;AM nimi > &lt;päätepisteen nimi >**

Tällä komennolla voit poistaa virtuaalikoneen päätepiste.

    ~$ azure vm endpoint delete my-vm http
    azure vm endpoint delete my-vm http
    info:   Executing command vm endpoint delete
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint delete command OK

**AM päätepisteen luettelon &lt;AM nimi >**

Tämä komento on lueteltu kaikki virtuaalikoneen päätepisteet. --Json-asetus määrittää, että tulokset palautetaan raaka JSON-muodossa.

    ~$ azure vm endpoint list my-linux-vm
    data:   Name  External Port  Local Port
    data:   ----  -------------  ----------
    data:   ssh   22             22

**AM päätepisteen päivitys [Valitsimet] &lt;AM nimi > &lt;päätepisteen nimi >**

Tämä komento päivittää AM päätepisteen uudet arvot näiden asetusten avulla.

    -n, --endpoint-name <name>          the new endpoint name
    -lo, --lb-port <port>                the new load balancer port
    -t, --vm-port <port>                the new local port
    -o, --endpoint-protocol <protocol>  the new transport layer protocol for port (tcp or udp)

**AM päätepisteen Näytä [Valitsimet] &lt;AM nimi >**

Tämä komento näkyy päätepisteet tietoja AM

    ~$ azure vm endpoint show "mycouchvm"
    info:    Executing command vm endpoint show
    + Getting virtual machines
    data:    Network Endpoints 0 LoadBalancedEndpointSetName "CouchDB_EP-5984"
    data:    Network Endpoints 0 LocalPort "5984"
    data:    Network Endpoints 0 Name "CouchDB_EP"
    data:    Network Endpoints 0 Port "5984"
    data:    Network Endpoints 0 Protocol "tcp"
    data:    Network Endpoints 0 Vip "168.61.9.97"
    data:    Network Endpoints 1 LoadBalancedEndpointSetName "CouchEP_2-2020"
    data:    Network Endpoints 1 LocalPort "2020"
    data:    Network Endpoints 1 Name "CouchEP_2"
    data:    Network Endpoints 1 Port "2020"
    data:    Network Endpoints 1 Protocol "tcp"
    data:    Network Endpoints 1 Vip "168.61.9.97"
    data:    Network Endpoints 2 LocalPort "3389"
    data:    Network Endpoints 2 Name "RemoteDesktop"
    data:    Network Endpoints 2 Port "3389"
    data:    Network Endpoints 2 Protocol "tcp"
    data:    Network Endpoints 2 Vip "168.61.9.97"
    info:    vm endpoint show command OK

## <a name="commands-to-manage-your-azure-virtual-machine-images"></a>Voit hallita kuvien Azure virtuaalikoneen komennot

Virtuaalikoneen kuvat ovat jo määritetty näennäiskoneiden, joka voi replikoida kaappaa tarpeen mukaan.

**AM kuvaluettelo [Valitsimet]**

Tämä komento saa virtuaalikoneen kuvia luettelon. On kolmenlaisia kuvat: Microsoft-luonut kuvia, jotka ovat etuliite "MSFT", kuvia, kolmannen osapuolen, jotka ovat etuliite toimittajan nimi ja kuvia, voit luoda luominen. Luo kuvia siepata aiemmin virtuaalikoneen tai kuvan luominen mukautetun .vhd, ladata Blob-objektien tallennustilaan. Lisätietoja mukautetun .vhd käyttämisestä on artikkelissa AM kuvan luominen.
--Json-asetus määrittää, että tulokset palautetaan raaka JSON-muodossa.

    ~$ azure vm image list
    data:   Name                                                                   Category   OS
    data:   ---------------------------------------------------------------------  ---------  -------
    data:   CANONICAL__Canonical-Ubuntu-12-04-20120519-2012-05-19-en-us-30GB.vhd   Canonical  Linux
    data:   MSFT__Windows-Server-2008-R2-SP1.11-29-2011                            Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1-with-SQL-Server-2012-Eval.11-29-2011  Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.en-us.30GB.2012-03-22                      Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.2-17-2012                                  Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1.en-us.30GB.2012-3-22                  Microsoft  Windows
    data:   OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd                 OpenLogic  Linux
    data:   SUSE__SUSE-Linux-Enterprise-Server-11SP2-20120521-en-us-30GB.vhd       SUSE       Linux
    data:   SUSE__OpenSUSE64121-03192012-en-us-15GB.vhd                            SUSE       Linux
    data:   WIN2K8-R2-WINRM                                                        User       Windows
    info:   vm image list command OK

**AM Kuva Näytä [Valitsimet] &lt;nimi >**

Tämä komento näkyy virtuaalikoneen kuva tietoja.

    ~$ azure vm image show MSFT__Windows-Server-2008-R2-SP1.11-29-2011
    + Fetching VM image
    info:   Executing command vm image show
    data:   {
    data:       Label: 'Windows Server 2008 R2 SP1, Nov 2011',
    data:       Name: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       Description: 'Microsoft Windows Server 2008 R2 SP1',
    data:       @: { xmlns: 'http://schemas.microsoft.com/windowsazure', xmlns:i: 'http://www.w3.org/2001/XMLSchema-instance' },
    data:       Category: 'Microsoft',
    data:       OS: 'Windows',
    data:       Eula: 'http://www.microsoft.com',
    data:       LogicalSizeInGB: '30'
    data:   }
    info:   vm image show command OK

**AM Kuva Poista [Valitsimet] &lt;nimi >**

Tällä komennolla voit poistaa virtuaalikoneen kuva.

    ~$ azure vm image delete my-vm-image
    info:   Executing command vm image delete
    info:   VM image deleted: my-vm-image
    info:   vm image delete command OK

**AM kuva luominen &lt;nimi > [lähde-polku]**

Tämä komento luo virtuaalikoneen kuva. Mukautetun .vhd-tiedostoja ladataan blob storage ja sitten virtuaalikoneen kuva luodaan sieltä. Voit käyttää virtuaalikoneen Tässä kuvassa virtual machine luomiseen. Sijainti ja OS parametrit tarvitaan.

>[AZURE.NOTE]Tämä komento tukee tällä hetkellä vain lataus kiinteä .vhd-tiedostoja. Dynaaminen .vhd tiedoston lataaminen käyttää [Azure Näennäiskiintolevyn apuohjelmia, siirry](https://github.com/Microsoft/azure-vhd-utils-for-go).

Jotkin järjestelmät määrätä prosessi tiedoston kuvaus rajoitukset. Jos tämä rajoitus on ylitetty-työkalun näyttää tiedoston kuvauksen rajan-virhe. Voit suorittaa komennon uudelleen käyttämällä -p &lt;numero > Vähennä rinnakkain lataukset-parametri. Rinnakkaisia uloslatauksia oletusarvoinen enimmäismäärä on 96.

    ~$ azure vm image create mytestimage ./Sample.vhd -o windows -l "West US"
    info:   Executing command vm image create
    + Retrieving storage accounts
    info:   VHD size : 13 MB
    info:   Uploading 13312.5 KB
    Requested:100.0% Completed:100.0% Running: 105 Time:    8s Speed:  1721 KB/s
    info:   http://myaccount.blob.core.azure.com/vm-images/Sample.vhd is uploaded successfully
    info:   vm image create command OK

## <a name="commands-to-manage-your-azure-virtual-machine-data-disks"></a>Komennot, joilla Azure virtuaalikoneen tietoja levyjen hallinnasta

Tietoja levyjen ovat .vhd tiedostoja, jotka voidaan käyttää virtual machine Blob-objektien tallennustilaan. Lisätietoja tietojen levyjen on otettu käyttöön blob storage on artikkelissa Azure tekniset kaavio näkyy aiemmin.

Liittäminen tietojen levyjen komennot (azure AM levyn liittäminen ja azure AM levyn liittää uudet) määrittää loogisen yksikön-numeron (LUN) liitettyjä tietoja levylle vaaditulla SCSI-protokollan mukaan. Liitetty virtual machine ensimmäinen tietojen levy on määritetty LUN 0, seuraavan on määritetty LUN 1 ja niin edelleen.

Kun tietojen DVD-levyllä azure AM levyjen irrottaa irrottaa komentoa, käytä &lt;lun&gt; parametri ilmaisee irrottaa levy.

>[AZURE.NOTE] Tietojen levyjen tulee aina irrottaa käänteisessä järjestyksessä alkaen suurin numeroitu LUN, joka on määritetty. Linux SCSI kerros ei tue irrottaminen alemman numeroitu LUN, kun suuremmat numerot LUN edelleen liitetään. Esimerkiksi sinun olisi irrota LUN 0, jos LUN 1 on edelleen liitetty.

**AM levyn Näytä [Valitsimet] &lt;nimi >**

Tämä komento näyttää Azure levyn tietoja.

    ~$ azure vm disk show anucentos-anucentos-0-20120524070008
    info:   Executing command vm disk show
    data:   AttachedTo DeploymentName "mycentos"
    data:   AttachedTo HostedServiceName "myanucentos"
    data:   AttachedTo RoleName "myanucentos"
    data:   OS "Linux"
    data:   Location "Azure Preview"
    data:   LogicalDiskSizeInGB "30"
    data:   MediaLink "http://mystorageaccount.blob.core.azure-preview.com/vhd-store/mycentos-cb39b8223b01f95c.vhd"
    data:   Name "mycentos-mycentos-0-20120524070008"
    data:   SourceImageName "OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd"
    info:   vm disk show command OK

**AM levyn luettelon [Valitsimet] [AM nimi]**

Tämä komento luetteloiden Azure levyjä, tai määritetyn virtual machine yhdistetty levyjä. Jos se on suoritettu virtuaalikoneen name-parametri, se palauttaa kaikki virtuaalikoneen yhdistetty levyjä. LUN 1 luodaan virtuaalikoneen ja luettelossa levyjen on liitetty erikseen.

    ~$ azure vm disk list mycentos
    info:   Executing command vm disk list
    data:   Lun  Size(GB)  Blob-Name
    data:   ---  --------  --------------------------------
    data:   1    30        mycentos-cb39b8223b01f95c.vhd
    data:   2    10        mycentos-e3f0d717950bb78d.vhd
    info:   vm disk list command OK

Kaikkien levyjen suoritetaan Tämä komento ilman virtuaalikoneen name-parametri palauttaa.

    ~$ azure vm disk list
    data:   Name                                        OS
    data:   ------------------------------------------  -------
    data:   mycentos-mycentos-0-20120524070008          Linux
    data:   mycentos-mycentos-2-20120525055052
    data:   mywindows-winvm-20120522223119              Windows
    info:   vm disk list command OK

**AM levylle, Poista [Valitsimet] &lt;nimi >**

Tämä komento poistaa Azure levyn henkilökohtainen tietovarasto. Levy on irrotettu virtuaalikoneen, ennen kuin se poistetaan.

    ~$ azure vm disk delete mycentos-mycentos-2-20120525055052
    info:   Executing command vm disk delete
    info:   Disk deleted: mycentos-mycentos-2-20120525055052
    info:   vm disk delete command OK

**AM levyn luominen &lt;nimi > [lähde-polku]**

Tämä komento latauksia ja rekisteröi Azure levy. --blob-URL-osoite,--sijainti, tai--affiniteetti group on määritettävä. Jos käytät tätä komentoa [lähde-polku], määritetty .vhd-tiedosto on ladattu palvelimeen ja kuvan luodaan. Voit liittää kuvan sitten virtual machine AM-levyn avulla Liitä.

Jotkin järjestelmät määrätä prosessi tiedoston kuvaus rajoitukset. Jos tämä rajoitus on ylitetty-työkalun näyttää tiedoston kuvauksen rajan-virhe. Voit suorittaa komennon uudelleen käyttämällä -p &lt;numero > Vähennä rinnakkain lataukset-parametri. Rinnakkaisia lataukset oletusarvoinen enimmäismäärä on 96.

    ~$ azure vm disk create my-data-disk ~/test.vhd --location "West US"
    info:   Executing command vm disk create
    info:   VHD size : 10 MB
    info:   Uploading 10240.5 KB
    Requested:100.0% Completed:100.0% Running:  81 Time:   11s Speed:   952 KB/s
    info:   http://account.blob.core.azure.com/disks/test.vhd is uploaded successfully
    info:   vm disk create command OK

**AM levyn Lataa [Valitsimet] &lt;lähde-polku > &lt;blob-URL-osoite > &lt;tallennustilan tilin-näppäintä >**

Tämä komento avulla voit ladata AM DVD-levyllä

    ~$ azure vm disk upload "http://sourcestorage.blob.core.windows.net/vhds/sample.vhd" "http://destinationstorage.blob.core.windows.net/vhds/sample.vhd" "DESTINATIONSTORAGEACCOUNTKEY"
    info:   Executing command vm disk upload
    info:   Uploading 12351.5 KB
    info:   vm disk upload command OK

**AM levyn liittäminen &lt;AM nimi > &lt;levy-kuva-name >**

Tämä komento liittää aiemmin luotuun virtuaalikoneen käyttöön pilvipalvelussa olemassa olevaa levyä Blob-objektien tallennustilaan.

    ~$ azure vm disk attach my-vm my-vm-my-vm-2-201242418259
    info:   Executing command vm disk attach
    info:   vm disk attach command OK

**AM levyn liittää uudet &lt;AM nimi > &lt;koon gt > [blob-URL-osoite]**

Tämä komento liittää tietoja DVD-levyllä Azure virtuaalikoneen. Tässä esimerkissä uuden levyn gigatavuina liitettävä koko on 20. Voit käyttää myös Blob-objektien URL-Osoitteen viimeisenä argumenttina määrittäminen eksplisiittisesti kohde Blob-objektien luomiseen. Jos et määritä Blob-objektien URL-osoite-blob-objektien luodaan automaattisesti.

    ~$ azure vm disk attach-new nick-test36 20 http://nghinazz.blob.core.azure-preview.com/vhds/vmdisk1.vhd
    info:   Executing command vm disk attach-new
    info:   vm disk attach-new command OK  

**AM levyn irrottaa &lt;AM nimi > &lt;lun >**

Tämä komento irrottaa Azure virtuaalikoneen liitetyt tiedot DVD-levyllä. &lt;LUN > tunnistaa levy on irrotettu. Saat luettelon käytettävissä liittyvät DVD-levyllä, ennen kuin se irrottaa levyjä, käytä AM levy-luettelo &lt;AM nimi >.

    ~$ azure vm disk detach my-vm 2
    info:   Executing command vm disk detach
    info:   vm disk detach command OK

## <a name="commands-to-manage-your-azure-cloud-services"></a>Voit hallita Azure pilvipalveluihin komennot

Azure pilvipalveluihin ovat sovellusten ja palveluiden isännöimät web roolit ja työntekijä roolit. Seuraavat komennot voidaan hallita Azure pilvipalveluihin.

**palvelun luominen [Valitsimet] &lt;palvelun nimi >**

Tämä komento luo pilvipalveluun

    ~$ azure service create newservicemsopentech
    info:    Executing command service create
    + Getting locations
    help:    Location:
      1) East Asia
      2) Southeast Asia
      3) North Europe
      4) West Europe
      5) East US
      6) West US
      : 6
    + Creating cloud service
    data:    Cloud service name newservicemsopentech
    info:    service create command OK

**palvelun Näytä [Valitsimet] &lt;palvelun nimi >**

Tämä komento näkyy tietoja Azure pilvipalvelussa

    ~$ azure service show newservicemsopentech
    info:    Executing command service show
    + Getting cloud service
    data:    Name newservicemsopentech
    data:    Url https://management.core.windows.net/9e672699-1055-41ae-9c36-e85152f2e352/services/hostedservices/newservicemsopentech
    data:    Properties location West US
    data:    Properties label newservicemsopentech
    data:    Properties status Created
    data:    Properties dateCreated
    data:    Properties dateLastModified
    info:    service show command OK

**Palveluluettelo [Valitsimet]**

Tämä komento on lueteltu Azure pilvipalveluihin.

    ~$ azure service list
    info:   Executing command service list
    data:   Name         Status
    data:   -----------  -------
    data:   service1     Created
    data:   service2     Created
    info:   service list command OK

**palvelun poistaminen [Valitsimet] &lt;nimi >**

Tällä komennolla voit poistaa Azure pilvipalvelussa.

    ~$ azure service delete myservice
    info:   Executing command service delete myservice
    info:   cloud-service delete command OK

Voit pakottaa poiston, käytä `-q` parametria.


## <a name="commands-to-manage-your-azure-certificates"></a>Komennot, joilla Azure varmenteiden hallinta

SSL-varmenteita liitetty tiliisi Azure Azure palvelun varmenteet. Saat lisätietoja Azure varmenteet [Hallinta varmenteet](http://msdn.microsoft.com/library/azure/gg981929.aspx).

**Varmenteen Palveluluettelo [Valitsimet]**

Tämä komento on lueteltu Azure varmenteet.

    ~$ azure service cert list
    info:   Executing command service cert list
    + Fetching cloud services
    + Fetching certificates
    data:   Service   Thumbprint                                Algorithm
    data:   --------  ----------------------------------------  ---------
    data:   myservice  262DBF95B5E61375FA27F1E74AC7D9EAE842916C  sha1
    info:   service cert list command OK

**palvelun varmenteen luominen &lt;dns-etuliite > &lt;tiedosto > [salasana]**

Tämä komento lataa varmenteen. Jätä salasana-kehote tyhjä sertifikaatteja, joita ei ole suojattu salasanalla.

    ~$ azure service cert create nghinazz ~/publishSet.pfx
    info:   Executing command service cert create
    Cert password:
    + Creating certificate
    info:   service cert create command OK

**palvelun varmenteen poistaminen [Valitsimet] &lt;allekirjoitus >**

Tällä komennolla voit poistaa varmenne.

    ~$ azure service cert delete 262DBF95B5E61375FA27F1E74AC7D9EAE842916C
    info:   Executing command service cert delete
    + Deleting certificate
    info:   nghinazz : cert deleted
    info:   service cert delete command OK

## <a name="commands-to-manage-your-web-apps"></a>Komennot, joilla web-sovellusten hallinta

Azure web app-sovelluksessa on web kokoonpano käytettävissä URI. Web Apps-sovellusten isännöidään näennäiskoneiden, mutta ei tarvitse huolehtia luominen ja käyttöönotto virtuaalikoneen tietoja itse. Nämä tiedot ovat käsitellään puolestasi Azure.

**sivustoluettelon [Valitsimet]**

Tämä komento on lueteltu web Apps-sovellusten.

    ~$ azure site list
    info:   Executing command site list
    data:   Name            State    Host names
    data:   --------------  -------  --------------------------------------------------
    data:   mongosite       Running  mongosite.antdf0.antares.windows.net
    data:   myphpsite       Running  myphpsite.antdf0.antares.windows.net
    data:   mydrupalsite36  Running  mydrupalsite36.antdf0.antares.windows.net
    info:   site list command OK

**sivuston määrittäminen [Valitsimet] [nimi]**

Tämä komento asettaa web-sovelluksen [nimi] asetukset

    ~$ azure site set
    info:    Executing command site set
    Web site name: mydemosite
    + Getting sites
    + Updating site config information
    info:    site set command OK

**sivuston deploymentscript [Valitsimet]**

Tämä komento luo mukautettu käyttöönoton komentosarja

    ~$ azure site deploymentscript --node
    info:    Executing command site deploymentscript
    info:    Generating deployment script for node.js Web Site
    info:    Generated deployment script files
    info:    site deploymentscript command OK

**sivuston luominen [Valitsimet] [nimi]**

Tämä komento luo web app- ja paikallinen hakemisto.

    ~$ azure site create mysite
    info:   Executing command site create
    info:   Using location northeuropewebspace
    info:   Creating a new web site
    info:   Created web site at  mysite.antdf0.antares.windows.net
    info:   Initializing repository
    info:   Repository initialized
    info:   site create command OK

> [AZURE.NOTE] Sivuston nimen on oltava yksilöllinen. Et voi luoda sivuston DNS-nimi on sama kuin aiemmin luotuun sivustoon.

**sivuston Selaa [Valitsimet] [nimi]**

Tämä komento Avaa web app selaimessa.

    ~$ azure site browse mysite
    info:   Executing command site browse
    info:   Launching browser to http://mysite.antdf0.antares-test.windows-int.net
    info:   site browse command OK

**Näytä sivuston [Valitsimet] [nimi]**

Tämä komento näyttää web-sovelluksen.

    ~$ azure site show mysite
    info:   Executing command site show
    info:   Showing details for site
    data:   Site AdminEnabled true
    data:   Site HostNames mysite.antdf0.antares-test.windows-int.net
    data:   Site Name mysite
    data:   Site Owner 00060000814EDDEE
    data:   Site RepositorySiteName mysite
    data:   Site SelfLink https://s1.api.antdf0.antares.windows.net:454/subscriptions/444e62ff-4c5f-4116-a695-5c803ed584a5/webspaces/northeuropewebspace/sites/mysite
    data:   Site State Running
    data:   Site UsageState Normal
    data:   Site WebSpace northeuropewebspace
    data:   Config AppSettings
    data:   Config ConnectionStrings
    data:   Config DefaultDocuments 0=Default.htm, 1=Default.asp, 2=index.htm, 3=index.html, 4=iisstart.htm, 5=default.aspx, 6=index.php, 7=hostingstart.aspx
    data:   Config DetailedErrorLoggingEnabled false
    data:   Config HttpLoggingEnabled false
    data:   Config Metadata
    data:   Config NetFrameworkVersion v4.0
    data:   Config NumberOfWorkers 1
    data:   Config PhpVersion 5.3
    data:   Config PublishingPassword rJ}[Er2v[Y]q16B6vTD]n$[C2z}Z.pvgLfRcLnAp%ax]xstiLny};o@vmMAote@d
    data:   Config RequestTracingEnabled false
    data:   Repository https://mysite.scm.antdf0.antares-test.windows-int.net/
    info:   site show command OK

**Poista sivuston [Valitsimet] [nimi]**

Tällä komennolla voit poistaa verkkosovellukseen.

    ~$ azure site delete mysite
    info:   Executing command site delete
    info:   Deleting site mysite
    info:   Site mysite has been deleted
    info:   site delete command OK

 **Vaihda sivuston [Valitsimet] [nimi]**

Tämä komento vaihtaa kaksi web app-paikkaa.

Tämä komento tukee seuraavia lisäasetuksia:

**- q tai **--hiljainen **: Älä Kysy vahvistus. Käytä tätä vaihtoehtoa Automaattiset komentosarjat.


**Aloita sivuston [Valitsimet] [nimi]**

Tämä komento aloittaa verkkosovellukseen.

    ~$ azure site start mysite
    info:   Executing command site start
    info:   Starting site mysite
    info:   Site mysite has been started
    info:   site start command OK

**Sivusto Lopeta [Valitsimet] [nimi]**

Tämä komento lopettaa verkkosovellukseen.

    ~$ azure site stop mysite
    info:   Executing command site stop
    info:   Stopping site mysite
    info:   Site mysite has been stopped
    info:   site stop command OK

**sivuston uudelleenkäynnistyksen [Valitsimet] [nimi]**

Tämä komento päättyy ja alkaa sitten määritetyn verkkosovellukseen.

Tämä komento tukee seuraavia lisäasetuksia:

**– paikka** &lt;paikan >: käynnistämään paikan nimi.


**sivuston sijaintiluettelosta [Valitsimet]**

Tämä komento on lueteltu sovelluksen verkkosijainti.

    ~$ azure site location list
    info:    Executing command site location list
    + Getting locations
    data:    Name
    data:    ----------------
    data:    West Europe
    data:    West US
    data:    North Central US
    data:    North Europe
    data:    East Asia
    data:    East US
    info:    site location list command OK

###<a name="commands-to-manage-your-web-app-application-settings"></a>Komennot, joilla web app-sovelluksen-asetusten hallinta

**sivuston appsetting luettelon [Valitsimet] [nimi]**

Tämä komento luettelo web-sovelluksen lisätään asetus.

    ~$ azure site appsetting list
    info:    Executing command site appsetting list
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Name  Value
    data:    ----  -----
    data:    test  value
    info:    site appsetting list command OK

**sivuston appsetting lisääminen [Valitsimet] &lt;keyvaluepair > [nimi]**

Tämä komento lisää sovellus-asetus avainarvon pari web App-sovellukseen.

    ~$ azure site appsetting add test=value
    info:    Executing command site appsetting add
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    + Updating site config information
    info:    site appsetting add command OK

**sivuston appsetting poistaminen [Valitsimet] &lt;avain > [nimi]**

Tällä komennolla voit poistaa määritetty asetus web Appista.

    ~$ azure site appsetting delete test
    info:    Executing command site appsetting delete
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    Delete application setting test? [y/n] y
    + Updating site config information
    info:    site appsetting delete command OK

**sivuston appsetting Näytä [Valitsimet] &lt;avain > [nimi]**

Tämä komento näyttää tiedot määritetty asetus

    ~$ azure site appsetting show test
    info:    Executing command site appsetting show
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Value:  value
    info:    site appsetting show command OK

###<a name="commands-to-manage-your-web-app-certificates"></a>Komennot, joilla web app-varmenteiden hallinta

**Varmenteen sivustoluettelon [Valitsimet] [nimi]**

Tämä komento näyttää luettelon web app-varmenteet.

    ~$ azure site cert list
    info:    Executing command site cert list
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Subject                       Expiration Date                    Thumbprint
    data:    ----------------------------  -----------------------------------------
    ----------------  ----------------------------------------
    data:    *.msopentech.com              Fri Nov 28 2014 09:49:57 GMT-0800 (Pacific Standard Time)  A40E82D3DC0286D1F58650E570ECF8224F69A148
    data:    msopentech.azurewebsites.net  Fri Jun 19 2015 11:57:32 GMT-0700 (Pacific Daylight Time)  CE1CD6538852BF7A5DC32001C2E26A29B541F0E8
    info:    site cert list command OK

**sivuston varmenteen lisääminen [Valitsimet] &lt;sertifikaatin polku > [nimi]**

**Varmenteen poistaminen [Valitsimet] &lt;allekirjoitus > [nimi]**

**sivuston varmenteen Näytä [Valitsimet] &lt;allekirjoitus > [nimi]**

Tämä komento näkyy varmenteen tiedot

    ~$ azure site cert show CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    Executing command site cert show
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Certificate hostNames 0=msopentech.azurewebsites.net
    data:    Certificate expirationDate
    data:    Certificate friendlyName msopentech.azurewebsites.net
    data:    Certificate issueDate
    data:    Certificate issuer CN=MSIT Machine Auth CA 2, DC=redmond, DC=corp, DC=microsoft, DC=com
    data:    Certificate subjectName msopentech.azurewebsites.net
    data:    Certificate thumbprint CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    site cert show command OK

###<a name="commands-to-manage-your-web-app-connection-strings"></a>Voit hallita web app-yhteyden-merkkijonot komennot

**sivuston connectionstring luettelon [Valitsimet] [nimi]**

**sivuston connectionstring lisääminen [Valitsimet] &lt;yhteyden_nimi > &lt;arvo > &lt;tyyppi > [nimi]**

**sivuston connectionstring poistaminen [Valitsimet] &lt;yhteyden_nimi > [nimi]**

**sivuston connectionstring Näytä [Valitsimet] &lt;yhteyden_nimi > [nimi]**

###<a name="commands-to-manage-your-web-app-default-documents"></a>Komennot, joilla web app-oletusarvo-tiedostojen hallinta

**sivuston defaultdocument luettelon [Valitsimet] [nimi]**

**sivuston defaultdocument lisääminen [Valitsimet] &lt;asiakirjan > [nimi]**

**sivuston defaultdocument poistaminen [Valitsimet] &lt;asiakirjan > [nimi]**

###<a name="commands-to-manage-your-web-app-deployments"></a>Komennot, joilla web-sovellusten käyttöönoton hallinta

**käyttöönoton sivustoluettelon [Valitsimet] [nimi]**

**sivuston käyttöönotto Näytä [Valitsimet] &lt;commitId > [nimi]**

**sivuston käyttöönoton laukaisun [Valitsimet] &lt;commitId > [nimi]**

**sivuston käyttöönoton github [Valitsimet] [nimi]**

**sivuston käyttöönoton käyttäjä määrittäminen [Valitsimet] [käyttäjänimi] [salasana]**

###<a name="commands-to-manage-your-web-app-domains"></a>Komennot, joilla hallinnoi web app

**sivuston toimialueluettelon [Valitsimet] [nimi]**

**sivuston toimialueen lisääminen [Valitsimet] &lt;dn > [nimi]**

**toimialueen poistaminen [Valitsimet] &lt;dn > [nimi]**

###<a name="commands-to-manage-your-web-app-handler-mappings"></a>Komennot, joilla web app-käsittely-yhdistämismääritysten hallinta

**tapahtumakäsittelijän sivustoluettelon [Valitsimet] [nimi]**

**sivuston käsittelijä lisää [Valitsimet] &lt;tunniste > &lt;suoritin > [nimi]**

**sivustojen käsittelijä poistaminen [Valitsimet] &lt;tunniste > [nimi]**

###<a name="commands-to-manage-your-web-jobs"></a>Komennot, joilla verkko-töiden hallinta

**työn sivustoluettelon [Valitsimet] [nimi]**

Tämä komento Luettele kaikki web-kohdassa verkkosovellukseen työt.

Tämä komento tukee seuraavat lisäasetukset:

+ **--Työtyyppi** &lt;Työtyyppi >: valinnainen. Tyyppi webjob. Kelvollinen arvo on "käynnistettävät" tai "jatkuva". Oletusarvoisesti palauttaa webjobs kaikenlaisten.
+ **– paikka** &lt;paikan >: käynnistämään paikan nimi.

**sivuston työn Näytä [Valitsimet] &lt;työ > &lt;jobType > [nimi]**

Tämä komento näyttää tietyn web projektin tiedot.

Tämä komento tukee seuraavat lisäasetukset:

+ **--työn nimi** &lt;työn nimi >: pakollinen. Webjob nimi.
+ **--Työtyyppi** &lt;Työtyyppi >: pakollinen. Tyyppi webjob. Kelvollinen arvo on "käynnistettävät" tai "jatkuva".
+ **– paikka** &lt;paikan >: käynnistämään paikan nimi.

**projektin poistaminen [Valitsimet] &lt;työ > &lt;jobType > [nimi]**

Tällä komennolla voit poistaa määritetyn web-työ.

Tämä komento tukee seuraavat lisäasetukset:

+ **--työn nimi** &lt;työn nimi > pakollinen. Webjob nimi.
+ **--Työtyyppi** &lt;Työtyyppi > pakollinen. Tyyppi webjob. Kelvollinen arvo on "käynnistettävät" tai "jatkuva".
+ **-q** tai **--hiljainen**: Älä Kysy vahvistus. Käytä tätä vaihtoehtoa Automaattiset komentosarjat.
+ **– paikka** &lt;paikan >: käynnistämään paikan nimi.

**sivuston työn Lataa [Valitsimet] &lt;työ > &lt;jobType > <jobFile> [nimi]**

Tällä komennolla voit poistaa määritetyn web-työ.

Tämä komento tukee seuraavat lisäasetukset:

+ **--työn nimi** &lt;työn nimi >: pakollinen. Webjob nimi.
+ **--Työtyyppi** &lt;Työtyyppi >: pakollinen. Tyyppi webjob. Kelvollinen arvo on "käynnistettävät" tai "jatkuva".
+ **--työn tiedoston** &lt;työn tiedosto >: pakollinen. Työ-tiedosto.
+ **– paikka** &lt;paikan >: käynnistämään paikan nimi.

**sivuston työn aloitus [Valitsimet] &lt;työ > &lt;jobType > [nimi]**

Tämä komento aloittaa määritetyn web-työn.

Tämä komento tukee seuraavat lisäasetukset:

+ **--työn nimi** &lt;työn nimi >: pakollinen. Webjob nimi.
+ **--Työtyyppi** &lt;Työtyyppi >: pakollinen. Tyyppi webjob. Kelvollinen arvo on "käynnistettävät" tai "jatkuva".
+ **– paikka** &lt;paikan >: käynnistämään paikan nimi.

**sivusto projektin Lopeta [Valitsimet] &lt;työ > &lt;jobType > [nimi]**

Tämä komento lopettaa määritetyn web-työ. Vain jatkuva työt voidaan pysäyttää.

Tämä komento tukee seuraavat lisäasetukset:

+ **--työn nimi** &lt;työn nimi >: pakollinen. Webjob nimi.
+ **– paikka** &lt;paikan >: käynnistämään paikan nimi.

###<a name="commands-to-manage-your-web-jobs-history"></a>Voit hallita töitä Sivuhistoria komennot

**sivuston työn historialuettelo [Valitsimet] [työ] [nimi]**

Tämä komento näkyy määritetty web-työ suoritetaan historiatiedot.

Tämä komento tukee seuraavat lisäasetukset:

+ **--työn nimi** &lt;työn nimi >: pakollinen. Webjob nimi.
+ **– paikka** &lt;paikan >: käynnistämään paikan nimi.

**sivuston Työhistoria Näytä [Valitsimet] [työ] [suorituksen tunnus] [nimi]**

Tämä komento saa työn määritetty sivusto projektin tiedot.

Tämä komento tukee seuraavat lisäasetukset:

+ **--työn nimi** &lt;työn nimi >: pakollinen. Webjob nimi.
+ **– Suorita tunnus** &lt;Suorita tunnus >: valinnainen. Suorita historia tunnus. Jos ei määritetä, Näytä uusimmat Suorita.
+ **– paikka** &lt;paikan >: käynnistämään paikan nimi.

###<a name="commands-to-manage-your-web-app-diagnostics"></a>Voit hallita web app-diagnostiikka komennot

**sivuston log Lataa [Valitsimet] [nimi]**

Lataa .zip-tiedosto, joka sisältää web-sovelluksen vianmääritys.

    ~$ azure site log download
    info:    Executing command site log download
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    + Downloading diagnostic log to diagnostics.zip
    info:    site log download command OK

**sivuston log kanta [Valitsimet] [nimi]**

Tämä komento muodostaa päätteen log streaming-palveluun.

    ~$ azure site log tail
    info:    Executing command site log tail
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    2013-11-19T17:24:17  Welcome, you are now connected to log-streaming service.

**sivuston log määrittäminen [Valitsimet] [nimi]**

Tämä komento määrittää koodiin diagnostiikan asetuksia.

    ~$ azure site log set -a
    info:    Executing command site log set
    + Getting output options
    help:    Output:
      1) file
      2) storage
      : 1
    Web site name: mydemosite
    + Getting locations
    + Getting sites
    + Getting site information
    + Getting diagnostic settings
    + Updating diagnostic settings
    info:    site log set command OK

###<a name="commands-to-manage-your-web-app-repositories"></a>Voit hallita web app-säilöjen-tietoihin komennot

**sivuston säilö haaraa [Valitsimet] &lt;haaran > [nimi]**

**säilön poistaminen [Valitsimet] [nimi]**

**tietovaraston synkronoinnin [Valitsimet] [nimi]**

###<a name="commands-to-manage-your-web-app-scaling"></a>Voit hallita web app skaalauksen komennot

**sivuston asteikko-tilaa [Valitsimet] &lt;tila > [nimi]**

**sivuston asteikko esiintymät [Valitsimet] &lt;esiintymät > [nimi]**


## <a name="commands-to-manage-azure-mobile-services"></a>Komennot, joilla Azure Mobile palveluiden hallinta

Azure Mobile-palveluihin monipuolisesti joukko Azure palvelut, jotka Taustajärjestelmä käyttämiseksi sovelluksia varten. Mobile-palveluihin komennot jaetaan seuraavat luokat:

+ [Voit hallita mobiilipalvelu esiintymät komennot](#Mobile_Services)
+ [Komennot, joilla mobiilipalvelu määritysten hallinta](#Mobile_Configuration)
+ [Voit hallita mobiilipalvelu taulukoiden komennot](#Mobile_Tables)
+ [Voit hallita mobiilipalvelu komentosarjojen komennot](#Mobile_Scripts)
+ [Voit hallita ajoitetuissa komennot](#Mobile_Jobs)
+ [Komennot, joilla skaalata mobile-palvelu](#Mobile_Scale)

Useimmat Mobile-palvelut-komennot koskevat seuraavat vaihtoehdot:

+ **-h** tai **--Ohje**: Näytä tuloste käyttötiedot.
+ **-s `<id>` ** tai **--tilauksen `<id>` **: Käytä tietyn tilauksen, määritettyä `<id>`.
+ **-v** tai **--yksityiskohtainen**: Kirjoita yksityiskohtainen tuloste.
+ **--json**: Kirjoita JSON tulos.

### <a name="Mobile_Services"></a>Voit hallita mobiilipalvelu esiintymät komennot

**Mobiili sijainnit [Valitsimet]**

Tämä komento näyttää Maantieteellisten sijaintien tukemat Mobile-palvelut.

    ~$ azure mobile locations
    info:    Executing command mobile locations
    info:    East US (default)
    info:    West US
    info:    North Europe

**Mobile luominen [Valitsimet] [palvelun nimi] [sqlAdminUsername] [sqlAdminPassword]**

Tämä komento luo mobiilipalvelu sekä SQL-tietokantaan ja palvelimeen.

    ~$ azure mobile create todolist your_login_name Secure$Password
    info:    Executing command mobile create
    + Creating mobile service
    info:    Overall application state: Healthy
    info:    Mobile service (todolist) state: ProvisionConfigured
    info:    SQL database (todolist_db) state: Provisioned
    info:    SQL server (e96ean1c6v) state: ProvisionConfigured
    info:    mobile create command OK

Tämä komento tukee seuraavat lisäasetukset:

+ **- r `<sqlServer>` ** tai **SQL Server – `<sqlServer>` **: Käytä aiemmin SQL-tietokanta-palvelinta määritettyä `<sqlServer>`.
+ **-d `<sqlDb>` ** tai **--sqlDb `<sqlDb>` **: Käytä aiemmin luotua SQL-tietokantaa, määritettyä `<sqlDb>`.
+ **-l `<location>` ** tai **--sijainti `<location>` **: palvelun luominen tietyssä paikassa, määritettyä `<location>`. Suorita azure mobile sijainnit saat käytettävissä olevat sijainnit.
+ **--sqlLocation `<location>` **: Luo SQL server on tietyn `<location>`; oletukset mobiilipalvelun haluamaasi kohtaan.

**Poista nuolet [Valitsimet] [palvelun nimi]**

Tällä komennolla voit poistaa mobiilipalvelu SQL-tietokantaan ja palvelimen kanssa.

    ~$ azure mobile delete todolist -a -q
    info:    Executing command mobile delete
    data:    Mobile service todolist
    data:    SQL database todolistAwrhcL60azo1C401
    data:    SQL server fh1kvbc7la
    + Deleting mobile service
    info:    Deleted mobile service
    + Deleting SQL server
    info:    Deleted SQL server
    + Deleting mobile application
    info:    Deleted mobile application
    info:    mobile delete command OK

Tämä komento tukee seuraavat lisäasetukset:

+ **+ d** tai **--deleteData**: Poista kaikki tämän mobiilipalvelu tietokannasta.
+ tai **-** **--deleteAll**: Poista SQL-tietokantaan ja palvelimeen.
+ **-q** tai **--hiljainen**: Älä Kysy vahvistus. Käytä tätä vaihtoehtoa Automaattiset komentosarjat.

**Mobiili-luettelosta [Valitsimet]**

Tämä komento on lueteltu mobile-palvelujen.

    ~$ azure mobile list
    info:    Executing command mobile list
    data:    Name          State  URL
    data:    ------------  -----  --------------------------------------
    data:    todolist      Ready  https://todolist.azure-mobile.net/
    data:    mymobileapp   Ready  https://mymobileapp.azure-mobile.net/
    info:    mobile list command OK

**Mobiili Näytä [Valitsimet] [palvelun nimi]**

Tämä komento näyttää tietoja mobile-palvelu.

    ~$ azure mobile show todolist
    info:    Executing command mobile show
    + Getting information
    info:    Mobile application
    data:    status Healthy
    data:    Mobile service name todolist
    data:    Mobile service status ProvisionConfigured
    data:    SQL database name todolistAwrhcL60azo1C401
    data:    SQL database status Linked
    data:    SQL server name fh1kvbc7la
    data:    SQL server status Linked
    info:    Mobile service
    data:    name todolist
    data:    state Ready
    data:    applicationUrl https://todolist.azure-mobile.net/
    data:    applicationKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    masterKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    webspace WESTUSWEBSPACE
    data:    region West US
    data:    tables TodoItem
    info:    mobile show command OK

**Mobiili uudelleenkäynnistyksen [Valitsimet] [palvelun nimi]**

Tämä komento käynnistyy mobiilipalvelu esiintymä.

    ~$ azure mobile restart todolist
    info:    Executing command mobile restart
    + Restarting mobile service
    info:    Service was restarted.
    info:    mobile restart command OK

**Mobiili-loki [Valitsimet] [palvelun nimi]**

Tämä komento palauttaa mobiilipalvelu lokit, suodattaa pois lokin kaikentyyppisillä mutta `error`.

    ~$ azure mobile log todolist -t error
    info:    Executing command mobile log
    data:
    data:    timeCreated 2013-01-07T16:04:43.351Z
    data:    type error
    data:    source /scheduler/TestingLogs.js
    data:    message This is an error.
    data:
    info:    mobile log command OK

Tämä komento tukee seuraavat lisäasetukset:

+ **- r `<query>` ** tai **--kyselyn `<query>` **: suorittaa lokin kyselyn.
+ **-t `<type>` ** tai **--tyyppi `<type>` **: palautetut lokit suodattaminen tapahtuma `<type>`, joka voi olla `information`, `warning`, tai `error`.
+ **-k `<skip>` ** tai **– ohittaa `<skip>` **: ohittaa määritetty rivimäärän `<skip>`.
+ **-p `<top>` ** tai **--top `<top>` **: palauttaa rivit-määrittämää tietty määrä `<top>`.

> [AZURE.NOTE] **--Kyselyn** parametri on nähden **--tyyppi**, **– ohittaa**, ja **--top**.

**Mobile palauttaa [Valitsimet] [unhealthyservicename] [healthyservicename]**

Tämä komento palauttaa perusasemassa mobiilipalvelu siirtämällä se kunnossa mobiilipalvelu toisella alueella.

Tämä komento tukee seuraavia lisäasetuksia:

**-q** tai **--hiljainen**: Piilota Pyydä palautus vahvistus.

**Mobiili-avain uudelleen [Valitsimet] [palvelun nimi] [tyyppi]**

Tämä komento luo uudelleen mobiilipalvelu sovellusnäppäintä.

    ~$ azure mobile key regenerate todolist application
    info:    Executing command mobile key regenerate
    info:    New application key is SmLorAWVfslMcOKWSsuJvuzdJkfUpt40
    info:    mobile key regenerate command OK

Avaintyypit ovat `master` ja `application`.

> [AZURE.NOTE] Näppäimet uudelleen, kun asiakkaat, jotka käyttävät Vanha avain ehkä pysty käyttämään mobile-palvelu. Kun sovellusnäppäintä uudelleen, on päivitettävä sovelluksen tärkeimmät uudet arvot.

**Mobiili avaimen määrittäminen [Valitsimet] [palvelun nimi] [tyyppi] [arvo]**

Tämä komento määrittää tietyn arvon mobiilipalvelu-näppäintä.


### <a name="Mobile_Configuration"></a>Komennot, joilla mobiilipalvelu määritysten hallinta

**Mobiili Hakumääritysten luettelo [Valitsimet] [palvelun nimi]**

Tämä komento on lueteltu Mobiili-palvelun asetukset.

    ~$ azure mobile config list todolist
    info:    Executing command mobile config list
    + Getting mobile service configuration
    data:    dynamicSchemaEnabled true
    data:    microsoftAccountClientSecret Not configured
    data:    microsoftAccountClientId Not configured
    data:    microsoftAccountPackageSID Not configured
    data:    facebookClientId Not configured
    data:    facebookClientSecret Not configured
    data:    twitterClientId Not configured
    data:    twitterClientSecret Not configured
    data:    googleClientId Not configured
    data:    googleClientSecret Not configured
    data:    apnsMode none
    data:    apnsPassword Not configured
    data:    apnsCertifcate Not configured
    info:    mobile config list command OK

**Mobiili-config Hae [Valitsimet] [palvelun nimi] [avain]**

Tämä komento saa tietyn määritysvaihtoehto Mobiili-palvelun tällöin dynaaminen rakenne.

    ~$ azure mobile config get todolist dynamicSchemaEnabled
    info:    Executing command mobile config get
    data:    dynamicSchemaEnabled true
    info:    mobile config get command OK

**Mobiili-config määrittäminen [Valitsimet] [palvelun nimi] [avain] [arvo]**

Tämä komento asettaa tietyn määritysvaihtoehto Mobiili-palvelun tällöin dynaaminen rakenne.

    ~$ azure mobile config set todolist dynamicSchemaEnabled false
    info:    Executing command mobile config set
    info:    mobile config set command OK


### <a name="Mobile_Tables"></a>Voit hallita mobiilipalvelu taulukoiden komennot

**Mobiili taulukkoluettelo [Valitsimet] [palvelun nimi]**

Tämä komento on lueteltu kaikki taulukot mobiili-palvelussa.

    ~$azure mobile table list todolist
    info:    Executing command mobile table list
    data:    Name      Indexes  Rows
    data:    --------  -------  ----
    data:    Channel   1        0
    data:    TodoItem  1        0
    info:    mobile table list command OK

**Mobiili-taulukossa on esitetty [Valitsimet] [palvelun nimi] [taulukon nimi]**

Tämä komento näkyy palauttaa tietyn taulukon tietoja.

    ~$azure mobile table show todolist
    info:    Executing command mobile table show
    + Getting table information
    info:    Table statistics:
    data:    Number of records 5
    info:    Table operations:
    data:    Operation  Script       Permissions
    data:    ---------  -----------  -----------
    data:    insert     1900 bytes   user
    data:    read       Not defined  user
    data:    update     Not defined  user
    data:    delete     Not defined  user
    info:    Table columns:
    data:    Name  Type           Indexed
    data:    ----  -------------  -------
    data:    id    bigint(MSSQL)  Yes
    data:    text      string
    data:    complete  boolean
    info:    mobile table show command OK

**Mobiili-taulukon luominen [Valitsimet] [palvelun nimi] [taulukon nimi]**

Tämä komento luo taulukon.

    ~$azure mobile table create todolist Channels
    info:    Executing command mobile table create
    + Creating table
    info:    mobile table create command OK

Tämä komento tukee seuraavia lisäasetuksia:

+ **-p `&lt;permissions>` ** tai **--käyttöoikeudet `&lt;permissions>` **: pilkuilla erotettu luettelo `<operation>` = `<permission>` parit, jossa `<operation>` on `insert`, `read`, `update`, tai `delete` ja `&lt;permissions>` on `public`, `application` (oletus) `user`, tai `admin`.

**Mobiili tietoja lukea [Valitsimet] [palvelun nimi] [taulukon nimi] [kyselyn]**

Tämä komento lukee tiedot taulukkoon.

    ~$azure mobile data read todolist TodoItem
    info:    Executing command mobile data read
    data:    id  text     complete
    data:    --  -------  --------
    data:    1   item #1  false
    data:    2   item #2  true
    data:    3   item #3  false
    data:    4   item #4  true
    info:    mobile data read command OK

Tämä komento tukee seuraavat lisäasetukset:

+ **-k `<skip>` ** tai **– ohittaa `<skip>` **: ohittaa määritetty rivimäärän `<skip>`.
+ **-t `<top>` ** tai **--top `<top>` **: palauttaa rivit-määrittämää tietty määrä `<top>`.
+ **-l** tai **--luettelon**: palauttaa tietoja luettelo-muodossa.

**Mobiili päivitys [Valitsimet] [palvelun nimi] [taulukon nimi]**

Tämä komento muuttuu Poista taulukon oikeudet vain järjestelmänvalvojat.

    ~$azure mobile table update todolist Channels -p delete=admin
    info:    Executing command mobile table update
    + Updating permissions
    info:    Updated permissions
    info:    mobile table update command OK

Tämä komento tukee seuraavat lisäasetukset:

+ **-p `&lt;permissions>` ** tai **--käyttöoikeudet `&lt;permissions>` **: pilkuilla erotettu luettelo `<operation>` = `<permission>` parit, jossa `<operation>` on `insert`, `read`, `update`, tai `delete` ja `&lt;permissions>` on `public`, `application` (oletus) `user`, tai `admin`.
+ **--deleteColumn `<columns>` **: sarakkeet, jos haluat poistaa, jonka arvot on erotettu luettelo kuin `<columns>`.
+ **-q** tai **--hiljainen**: poistaa sarakkeita kysymättä vahvistusta varten.
+ **--addIndex `<columns>` **: hakemiston lisättävät sarakkeet pilkuilla erotettu luettelo.
+ **--deleteIndex `<columns>` **: sarakkeiden jättäminen pois indeksistä pilkuilla erotettu luettelo.

**Mobiili-taulukon poistaminen [Valitsimet] [palvelun nimi] [taulukon nimi]**

Tällä komennolla voit poistaa taulukon.

    ~$azure mobile table delete todolist Channels
    info:    Executing command mobile table delete
    Do you really want to delete the table (yes/no): yes
    + Deleting table
    info:    mobile table delete command OK

Määritä Poista taulukko ilman vahvistusta - q-parametrin. Näin voit estää automaatio-komentosarjojen estäminen.

**Mobiili tietojen Katkaise [Valitsimet] [palvelun nimi] [taulukon nimi]**

Tämä komento poistaa taulukosta kaikki tietorivejä.

    ~$azure mobile data truncate todolist TodoItem
    info:    Executing command mobile data truncate
    info:    There are 7 data rows in the table.
    Do you really want to delete all data from the table? (y/n): y
    info:    Deleted 7 rows.
    info:    mobile data truncate command OK


### <a name="Mobile_Scripts"></a>Voit hallita komentosarjojen komennot

Tämän osan komennot käytetään hallinta server-komentosarjoja, jotka kuuluvat mobile-palvelu. Lisätietoja on aiheessa [käsitteleminen server-komentosarjoja Mobile-palvelut](https://github.com/Azure/azure-mobile-services/blob/master/docs/mobile-services-how-to-use-server-scripts.md).

**Mobiili-komentosarjan luettelosta [Valitsimet] [palvelun nimi]**

Tämä komento on lueteltu rekisteröity komentosarjoja, kuten taulukko- ja ajoitus-komentosarjoja.

    ~$azure mobile script list todolist
    info:    Executing command mobile script list
    + Getting script information
    info:    Table scripts
    data:    Name                   Size
    data:    ---------------------  ----
    data:    table/TodoItem.delete  256
    data:    table/Devices.insert   1660
    error:   Unable to get shared scripts
    info:    Scheduler scripts
    data:    Name                 Status     Interval   Last run   Next run
    data:    -------------------  ---------  ---------  ---------  ---------
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    info:    mobile script list command OK

**Mobiili-komentosarjan lataaminen [Valitsimet] [palvelun nimi] [scriptname]**

Tämä komento lataa Lisää komentosarja TodoItem taulukosta tiedoston nimeltä `todoitem.insert.js` - `table` alikansioon.

    ~$azure mobile script download todolist table/todoitem.insert.js
    info:    Executing command mobile script download
    info:    Saved script to ./table/todoitem.insert.js
    info:    mobile script download command OK

Tämä komento tukee seuraavat lisäasetukset:

+ **-p `<path>` ** tai **--polku `<path>` **: sijaintia, johon haluat tallentaa komentosarja-tiedostot, joiden käytön hakemiston on oletusarvo.
+ **-f `<file>` ** tai **--tiedoston `<file>` **: tiedoston, johon haluat tallentaa komentosarja nimi.
+ **+ o** tai **– ohittaa**: Korvaa aiemmin luotu tiedosto.
+ **-c** tai **--konsolin**: komentosarja konsoliin sijaan tiedostoon.

**Lataa Mobile komentosarjan [Valitsimet] [palvelun nimi] [scriptname]**

Tämä komento lataa komentosarja nimeltä `todoitem.insert.js` kohteesta `table` alikansioon.

    ~$azure mobile script upload todolist table/todoitem.insert.js
    info:    Executing command mobile script upload
    info:    mobile script upload command OK

Tiedoston nimi on koostuttava taulukon ja toiminta-nimistä. Se on sijaittava taulukon alikansioon suhteessa sijainti, johon komento suoritetaan. Voit käyttää myös **-f `<file>` ** tai **--tiedoston `<file>` ** parametri Määritä eri tiedostonimi ja rekisteröi komentosarjan sisältävän tiedoston polku.


**Mobiili-komentosarjan poistaminen [Valitsimet] [palvelun nimi] [scriptname]**

Tämä komento poistaa aiemmin Lisää komentosarjan TodoItem-taulukosta.

    ~$azure mobile script delete todolist table/todoitem.insert.js
    info:    Executing command mobile script delete
    info:    mobile script delete command OK

### <a name="Mobile_Jobs"></a>Voit hallita ajoitetuissa komennot

Tämän osan komennot käytetään ajoitetuissa, jotka kuuluvat langattoman palvelun hallinta. Lisätietoja on artikkelissa [Ajoita työt](http://msdn.microsoft.com/library/windowsazure/jj860528.aspx).

**Mobiili työn luettelon [Valitsimet] [palvelun nimi]**

Tämä komento on lueteltu ajoitetuissa.

    ~$azure mobile job list todolist
    info:    Executing command mobile job list
    info:    Scheduled jobs
    data:    Job name    Script name           Status    Interval     Last run              Next run
    data:    ----------  --------------------  --------  -----------  --------------------  --------------------
    data:    getUpdates  scheduler/getUpdates  enabled   15 [minute]  2013-01-14T16:15:00Z  2013-01-14T16:30:00Z
    info:    You can manipulate scheduled job scripts using the 'azure mobile script' command.
    info:    mobile job list command OK

**Mobiili luominen [Valitsimet] [palvelun nimi] [työ]**

Tämä komento luo nimetty työn `getUpdates` , joka on suunniteltu toimimaan kerran tunnissa.

    ~$azure mobile job create -i 1 -u hour todolist getUpdates
    info:    Executing command mobile job create
    info:    Job was created in disabled state. You can enable the job using the 'azure mobile job update' command.
    info:    You can manipulate the scheduled job script using the 'azure mobile script' command.
    info:    mobile job create command OK

Tämä komento tukee seuraavat lisäasetukset:

+ **-i `<number>` ** tai **--aikavälin `<number>` **: aikaväli, kokonaisluku. Oletusarvo on `15`.
+ **-u `<unit>` ** tai **--intervalUnit `<unit>` **: yksikkö _aikaväli_, joka voi olla jokin seuraavista arvoista:
    + **minuutti** (oletus)
    + **Tunti**
    + **päivä**
    + **kuukausi**
    + **ei mitään** (tarvittaessa työt)
+ **-t `<time>`** **--aloitusajan `<time>` ** Ensimmäinen alkamispäivää suorittamalla komentosarja ISO-muodossa. Oletusarvo on `now`.

> [AZURE.NOTE] Uusia töitä luodaan käytöstä poistettuun vaiheeseen, koska komentosarjan edelleen voi ladata. **Mobiili komentosarjan lataaminen** -komennon avulla voit ladata komentosarjan ja voi työn **mobile työn Päivitä** -komento.

**Mobiili työn päivitys [Valitsimet] [palvelun nimi] [työ]**

Seuraava komento ottaa poistettu käytöstä `getUpdates` työn.

    ~$azure mobile job update -a enabled todolist getUpdates
    info:    Executing command mobile job update
    info:    mobile job update command OK

Tämä komento tukee seuraavat lisäasetukset:

+ **-i `<number>` ** tai **--aikavälin `<number>` **: aikaväli, kokonaisluku. Oletusarvo on `15`.
+ **-u `<unit>` ** tai **--intervalUnit `<unit>` **: yksikkö _aikaväli_, joka voi olla jokin seuraavista arvoista:
    + **minuutti** (oletus)
    + **Tunti**
    + **päivä**
    + **kuukausi**
    + **ei mitään** (tarvittaessa työt)
+ **-t `<time>`** **--aloitusajan `<time>` ** Ensimmäinen alkamispäivää suorittamalla komentosarja ISO-muodossa. Oletusarvo on `now`.
+ **- `<status>` ** tai **--tila `<status>` **: projektin tila, joka voi olla `enabled` tai `disabled`.

**Poista nuolet työn [Valitsimet] [palvelun nimi] [työ]**

Tämä komento poistaa getUpdates ajoitetun työn TodoList palvelimeen.

    ~$azure mobile job delete todolist getUpdates
    info:    Executing command mobile job delete
    info:    mobile job delete command OK

> [AZURE.NOTE] Projektin poistaminen poistaa myös ladatut komentosarja.

### <a name="Mobile_Scale"></a>Komennot, joilla skaalata mobile-palvelu

Tämän osan komennot käytetään skaalata mobile-palvelu. Lisätietoja on artikkelissa [Skaalaus mobile-palvelu](http://msdn.microsoft.com/library/windowsazure/jj193178.aspx).

**Mobiili asteikko Näytä [Valitsimet] [palvelun nimi]**

Tämä komento näkyy asteikko-tiedot, kuten Laske tilan ja esiintymien määrän.

    ~$azure mobile scale show todolist
    info:    Executing command mobile scale show
    data:    webspace WESTUSWEBSPACE
    data:    computeMode Free
    data:    numberOfInstances 1
    info:    mobile scale show command OK

**Mobiili asteikon muuttaminen [Valitsimet] [palvelun nimi]**

Tämä komento muuttuu mobiilipalvelun asteikon vapaa premium-tilaan.

    ~$azure mobile scale change -c Reserved -i 1 todolist
    info:    Executing command mobile scale change
    + Rescaling the mobile service
    info:    mobile scale change command OK

Tämä komento tukee seuraavat lisäasetukset:

+ **- c `<mode>` ** tai **--computeMode `<mode>` **: Laske-tilan on oltava joko `Free` tai `Reserved`.
+ **-i `<count>` ** tai **--numberOfInstances `<count>` **: käytetään, kun varattu tilassa esiintymien määrän.

> [AZURE.NOTE] Kun määrität Laske tilassa `Reserved`, kaikki saman alueen mobile palvelut Suorita premium-tilassa.


###<a name="commands-to-enable-preview-features-for-your-mobile-service"></a>Komennot, joilla esikatselutoiminnot käyttöön mobiilipalvelun ominaisuudet

**Mobile Preview-version luettelon [Valitsimet] [palvelun nimi]**

Tämä komento näyttää esikatselun ominaisuuksista määritetyn palvelun ja onko ne ovat käytössä.

    ~$ azure mobile preview list mysite
    info:    Executing command mobile preview list
    + Getting preview features
    data:    Preview feature  Enabled
    data:    ---------------  -------
    data:    SourceControl    No
    data:    Users            No
    info:    You can enable preview features using the 'azure mobile preview enable' command.
    info:    mobile preview list command OK

**Mobile Preview-version käyttöön [Valitsimet] [palvelun nimi] [featurename]**

Tämä komento mahdollistaa Mobiili-palvelun määritetyn esikatselu-toimintoa. Kun otettu käyttöön, esikatselutoiminnot ei voi poistaa Mobiili-palvelun käytöstä.

###<a name="commands-to-manage-your-mobile-service-apis"></a>Voit hallita mobiilipalvelu ohjelmointirajapinnan komennot

**Mobiili api luettelon [Valitsimet] [palvelun nimi]**

Tämä komento näyttää mobile luettelon palvelun mukautetun API, jotka olet luonut mobile-palvelu.

    ~$ azure mobile api list mysite
    info:    Executing command mobile api list
    + Retrieving list of APIs
    info:    APIs
    data:    Name                  Get          Put          Post         Patch        Delete
    data:    --------------------  -----------  -----------  -----------  -----------  -----------
    data:    myCustomRetrieveAPI   application  application  application  application  application
    info:    You can manipulate API scripts using the 'azure mobile script' command.
    info:    mobile api list command OK

**Mobiili-ohjelmointirajapinnan luominen [Valitsimet] [palvelun nimi] [apiname]**

Luo mukautettu mobiilipalvelu-Ohjelmointirajapinta

    ~$ azure mobile api create mysite myCustomRetrieveAPI
    info:    Executing command mobile api create
    + Creating custom API: 'myCustomRetrieveAPI'
    info:    API was created successfully. You can modify the API using the 'azure mobile script' command.
    info:    mobile api create command OK

Tämä komento tukee seuraavia lisäasetuksia:

**-p** tai **--käyttöoikeudet** &lt;käyttöoikeudet >: CSV-luettelon &lt;menetelmä > =&lt;käyttöoikeus > parit.

**Mobiili api päivitys [Valitsimet] [palvelun nimi] [apiname]**

Tämä komento päivittää määritetyn mobiilipalvelu mukautetun Ohjelmointirajapinnan.

Tämä komento tukee seuraavia lisäasetuksia:

Tämä komento tukee seuraavat lisäasetukset:

+ **-p** tai **--käyttöoikeudet** &lt;käyttöoikeudet >: CSV-luettelon &lt;menetelmä > =&lt;käyttöoikeus > parit.
+ **-f** tai **--pakottaa**: ohittaa omia muutoksia käyttöoikeudet metatiedot-tiedostoon.

**Mobiili-ohjelmointirajapinnan poistaminen [Valitsimet] [palvelun nimi] [apiname]**

    ~$ azure mobile api delete mysite myCustomRetrieveAPI
    info:    Executing command mobile api delete
    + Deleting API: 'myCustomRetrieveAPI'
    info:    mobile api delete command OK

Tällä komennolla voit poistaa määritetyn mobiilipalvelu mukautetun Ohjelmointirajapinnan.

###<a name="commands-to-manage-your-mobile-application-app-settings"></a>Komennot, joilla mobiilisovelluksen app-asetusten hallinta

**Mobiili appsetting luettelon [Valitsimet] [palvelun nimi]**

Tämä komento näyttää määritetyn palvelun mobiilisovelluksen app-asetukset.

    ~$ azure mobile appsetting list mysite
    info:    Executing command mobile appsetting list
    + Retrieving app settings
    data:    Name               Value
    data:    -----------------  -----
    data:    enablebetacontent  true
    info:    mobile appsetting list command OK

**Mobiili appsetting lisääminen [Valitsimet] [palvelun nimi] [nimi] [arvo]**

Tämä komento lisää mukautetun sovelluksen asetus mobile-palvelu.

    ~$ azure mobile appsetting add mysite enablebetacontent true
    info:    Executing command mobile appsetting add
    + Retrieving app settings
    + Adding app setting
    info:    mobile appsetting add command OK

**Poista nuolet appsetting [Valitsimet] [palvelun nimi] [nimi]**

Tämä komento poistaa mobile-palvelu määritetyn sovelluksen-asetusta.

    ~$ azure mobile appsetting delete mysite enablebetacontent
    info:    Executing command mobile appsetting delete
    + Retrieving app settings
    + Removing app setting 'enablebetacontent'
    info:    mobile appsetting delete command OK

**Näytä Mobile appsetting [Valitsimet] [palvelun nimi] [nimi]**

Tämä komento poistaa mobile-palvelu määritetyn sovelluksen-asetusta.

    ~$ azure mobile appsetting show mysite enablebetacontent
    info:    Executing command mobile appsetting show
    + Retrieving app settings
    info:    enablebetacontent: true
    info:    mobile appsetting show command OK

## <a name="manage-tool-local-settings"></a>Työkalun paikallisen asetusten hallinta

Paikalliset asetukset ovat Tilaustunnus ja oletus tallennustilan tilin nimi.

**Hakumääritysten luettelo [Valitsimet]**

Tämä komento näyttää config asetukset.

    ~$ azure config list
    info:   Displaying config settings
    data:   Setting                Value
    data:   ---------------------  ------------------------------------
    data:   subscription           32-digit-subscription-key
    data:   defaultStorageAccount  name

**Config määrittäminen [Valitsimet] &lt;nimi&gt;,&lt;arvo&gt;**

Tämä komento muuttuu määritys-asetusta.

    ~$ azure config set defaultStorageAccount myname
    info:   Setting 'defaultStorageAccount' to value 'myname'
    info:   Changes saved.

## <a name="commands-to-manage-service-bus"></a>Voit hallita palvelua Bus komennot

Nämä komennot avulla voit hallita palvelua Bus tilisi

**SB nimitilan valintaruudun [Valitsimet] &lt;nimi >**

Tarkista, että palvelun bus nimitila on turvallista eikä käytettävissä.

**SB nimitilan luominen &lt;nimi > &lt;sijainti >**

Luo palvelun Bus nimitila.

    ~$ azure sb namespace create mysbnamespacea-test "West US"
    info:    Executing command sb namespace create
    + Creating namespace mysbnamespacea-test in region West US
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Activating
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    _: [object Object]
    info:    sb namespace create command OK


**Poista SB nimitilan &lt;nimi >**

Poista nimitila.

    ~$ azure sb namespace delete mysbnamespacea-test
    info:    Executing command sb namespace delete
    Delete namespace mysbnamespacea-test? [y/n] y
    + Deleting namespace mysbnamespacea-test
    info:    sb namespace delete command OK

**SB nimitilan luettelo**

Luettele kaikki nimitilan luotu tilissäsi.

    ~$ azure sb namespace list
    info:    Executing command sb namespace list
    + Getting namespaces
    data:    Name                 Region   Status
    data:    -------------------  -------  ------
    data:    mysbnamespacea-test  West US  Active
    info:    sb namespace list command OK


**SB nimitilan sijaintiluettelosta**

Näyttää kaikki käytettävissä olevat nimitilan sijaintien luettelon.

    ~$ azure sb namespace location list
    info:    Executing command sb namespace location list
    + Getting locations
    data:    Name              Code
    data:    ----------------  ----------------
    data:    East Asia         East Asia
    data:    West Europe       West Europe
    data:    North Europe      North Europe
    data:    East US           East US
    data:    Southeast Asia    Southeast Asia
    data:    North Central US  North Central US
    data:    West US           West US
    data:    South Central US  South Central US
    info:    sb namespace location list command OK

**SB nimitilan Näytä &lt;nimi >**

Näyttää tietyn nimitilan tietoja.

    ~$ azure sb namespace show mysbnamespacea-test
    info:    Executing command sb namespace show
    + Getting namespace
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Active
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    UpdatedAt: 2013-11-14T16:25:37.85Z
    info:    sb namespace show command OK

**Tarkista SB nimitilan &lt;nimi >**

Tarkista, onko nimitilan käytettävissä.

## <a name="commands-to-manage-your-storage-objects"></a>Voit hallita tallennustilan objekteja komennot

###<a name="commands-to-manage-your-storage-accounts"></a>Komennot, joilla tallennustilan tilien hallinta

**tallennustilan luettelosta [Valitsimet]**

Tämä komento näyttää tilauksen tallennustilan tilit.

    ~$ azure storage account list
    info:    Executing command storage account list
    + Getting storage accounts
    data:    Name             Label  Location
    data:    ---------------  -----  --------
    data:    mybasestorage           West US
    info:    storage account list command OK

**tallennustilan tilin Näytä [Valitsimet]<name>**

Tämä komento näkyy myös URI ja tilin ominaisuudet määritetyn tallennustilan tilin tiedot.

**tallennustilan tilin luominen [Valitsimet]<name>**

Tämä komento luo tallennustilan-tilin määrittämisessä asetusten perusteella.

    ~$ azure storage account create mybasestorage --label PrimaryStorage --location "West US"
    info:    Executing command storage account create
    + Creating storage account
    info:    storage account create command OK

Tämä komento tukee seuraavat lisäasetukset:

+ **-e** tai **--otsikko** &lt;selite >: tallennustilan tilin nimi.
+ **-d** tai **--kuvaus** &lt;kuvaus >: kuvaus-tallennustilan tilin.
+ **-l** tai **--sijainti** &lt;nimi >: maantieteellisen alueen, jossa tallennustila-tilin luominen.
+ **-** tai **--affiniteetti group** &lt;nimi >: liitettävä tallennustilan tilin affiniteetti-ryhmä. 
+ **--tyyppi**: ilmaisee, voit luoda tilin tyyppi: joko vakio tallennustilan redundancy vaihtoehto (LRS/ZRS/GRS/RAGRS) tai Premium Storage (PLRS).

**tallennustilan tilin määrittäminen [Valitsimet]<name>**

Tämä komento päivittää määritetyn tallennustilan tilin.

    ~$ azure storage account set mybasestorage --kind Storage --sku-name GRS
    info:    Executing command storage account set
    + Updating storage account
    info:    storage account set command OK

Tämä komento tukee seuraavat lisäasetukset:

+ **-e** tai **--otsikko** &lt;selite >: tallennustilan tilin nimi.
+ **-d** tai **--kuvaus** &lt;kuvaus >: kuvaus-tallennustilan tilin.
+ **-l** tai **--sijainti** &lt;nimi >: maantieteellisen alueen, jossa tallennustila-tilin luominen.
+ **--tyyppi**: ilmaisee uuden tilin tyyppi: joko vakio tallennustilan redundancy vaihtoehto (LRS/ZRS/GRS/RAGRS) tai Premium Storage (PLRS).

**tallennustilan tilin poistaminen [Valitsimet]<name>**

Tällä komennolla voit poistaa määritetyn tallennustilan tilin.

Tämä komento tukee seuraavia lisäasetuksia:

**-q** tai **--hiljainen**: Älä Kysy vahvistus. Käytä tätä vaihtoehtoa Automaattiset komentosarjat.

###<a name="commands-to-manage-your-storage-account-keys"></a>Voit hallita tallennustilan tilin avaimien komennot

**tallennustilan tilin näppäimet luettelon [Valitsimet]<name>**

Tämä komento on lueteltu määritetty tallennustilan tilin ensisijainen ja toissijainen näppäimet.

**tallennustilan tilin näppäimet uusia [Valitsimet]<name>**

###<a name="commands-to-manage-your-storage-container"></a>Voit hallita tallennustilan säiliön komennot

**tallennustilan säilö luettelon [Valitsimet] [etuliite]**

Tämä komento näyttää määritetyn tallennustilan tilin tallennustilan säilö-luettelosta. Tallennustilan tilin määrittämän yhteysmerkkijonon tai tallennustilan tilin nimi ja tili-näppäintä.

Tämä komento tukee seuraavat lisäasetukset:

+ **-p** tai **-etuliitteen** &lt;etuliite >: tallennustilan säilö etuliite.
+ **-** tai **--tilin nimi** &lt;accountName >: tallennustilan tilin nimi.
+ **-k** tai **--tiliavain tiliavain** &lt;accountKey >: tallennustilan tilin-näppäintä.
+ **-c** tai **--yhteysmerkkijono** &lt;connectionString >: tallennustilan yhteysmerkkijonon.
+ **--Virheenkorjaus**: suorittaa tallennustilan komennon virheenkorjaus-tilassa.

**tallennustilan säilö Näytä [Valitsimet] [säilö]**
**tallennustilan säiliön luominen [Valitsimet] [säilö]**

Tämä komento luo määritetyn tallennustilan tilin tallennustilan säilö. Tallennustilan tilin määrittämän yhteysmerkkijonon tai tallennustilan tilin nimi ja tili-näppäintä.

Tämä komento tukee seuraavat lisäasetukset:

+ **--säilö** &lt;säilö >: Luo säilytykseen nimi.
+ **-p** tai **-etuliitteen** &lt;etuliite >: tallennustilan säilö etuliite.
+ **-** tai **--tilin nimi** &lt;accountName >: tallennustilan tilin nimi
+ **-k** tai **--tiliavain tiliavain** &lt;accountKey >: tallennustilan tilin avain
+ **-c** tai **--yhteysmerkkijono** &lt;connectionString >: tallennustilan yhteysmerkkijono
+ **--Virheenkorjaus**: suorittaa tallennustilan komennon virheenkorjaus-tilassa.

**tallennustilan säilö Poista [Valitsimet] [säilö]**

Tällä komennolla voit poistaa määritetyn tallennustilan säilö. Tallennustilan tilin määrittämän yhteysmerkkijonon tai tallennustilan tilin nimi ja tili-näppäintä.

Tämä komento tukee seuraavat lisäasetukset:

+ **--säilö** &lt;säilö >: Luo säilytykseen nimi.
+ **-p** tai **-etuliitteen** &lt;etuliite >: tallennustilan säilö etuliite.
+ **-** tai **--tilin nimi** &lt;accountName >: tallennustilan tilin nimi.
+ **-k** tai **--tiliavain tiliavain** &lt;accountKey >: tallennustilan tilin-näppäintä.
+ **-c** tai **--yhteysmerkkijono** &lt;connectionString >: tallennustilan yhteysmerkkijonon.
+ **--Virheenkorjaus**: suorittaa tallennustilan komennon virheenkorjaus-tilassa.

**tallennustilan säiliön määrittäminen [Valitsimet] [säilö]**

Tämä komento määrittää käyttöoikeusluettelo tallennustilan säilö. Tallennustilan tilin määrittämän yhteysmerkkijonon tai tallennustilan tilin nimi ja tili-näppäintä.

Tämä komento tukee seuraavat lisäasetukset:

+ **--säilö** &lt;säilö >: Luo säilytykseen nimi.
+ **-p** tai **-etuliitteen** &lt;etuliite >: tallennustilan säilö etuliite.
+ **-** tai **--tilin nimi** &lt;accountName >: tallennustilan tilin nimi.
+ **-k** tai **--tiliavain tiliavain** &lt;accountKey >: tallennustilan tilin-näppäintä.
+ **-c** tai **--yhteysmerkkijono** &lt;connectionString >: tallennustilan yhteysmerkkijonon.
+ **--Virheenkorjaus**: suorittaa tallennustilan komennon virheenkorjaus-tilassa.

###<a name="commands-to-manage-your-storage-blob"></a>Voit hallita tallennustilan Blob-objektien komennot

**tallennustilan Blob-objektien luettelon säilö [etuliite] [Valitsimet]**

Tämä komento palauttaa määritetyn tallennustilan säiliön BLOB storage luettelo.

Tämä komento tukee seuraavat lisäasetukset:

+ **--säilö** &lt;säilö >: Luo säilytykseen nimi.
+ **-p** tai **-etuliitteen** &lt;etuliite >: tallennustilan säilö etuliite.
+ **-** tai **--tilin nimi** &lt;accountName >: tallennustilan tilin nimi.
+ **-k** tai **--tiliavain tiliavain** &lt;accountKey >: tallennustilan tilin-näppäintä.
+ **-c** tai **--yhteysmerkkijono** &lt;connectionString >: tallennustilan yhteysmerkkijonon.
+ **--Virheenkorjaus**: suorittaa tallennustilan komennon virheenkorjaus-tilassa.

**tallennustilan Blob-objektien näyttäminen [Valitsimet] [säilö] [blob]**

Tämä komento näyttää määritetyn tallennustilan Blob-objektien tiedot.

Tämä komento tukee seuraavat lisäasetukset:

+ **--säilö** &lt;säilö >: Luo säilytykseen nimi.
+ **-p** tai **-etuliitteen** &lt;etuliite >: tallennustilan säilö etuliite.
+ **-** tai **--tilin nimi** &lt;accountName >: tallennustilan tilin nimi.
+ **-k** tai **--tiliavain tiliavain** &lt;accountKey >: tallennustilan tilin-näppäintä.
+ **-c** tai **--yhteysmerkkijono** &lt;connectionString >: tallennustilan yhteysmerkkijonon.
+ **--Virheenkorjaus**: suorittaa virheenkorjaus tallennustilan-komennon.

**tallennustilan Blob-objektien poistaminen säilö [blob] [Valitsimet]**

Tämä komento tukee seuraavat lisäasetukset:

+ **--säilö** &lt;säilö >: Luo säilytykseen nimi.
+ **-b** tai **--Blob-objektien** &lt;blobName >: tallennustilan Blob-objektien poistaminen nimi.
+ **-q** tai **--hiljainen**: ilman vahvistusta määritetyn tallennustilan Blob-objektien poistaminen.
+ **-** tai **--tilin nimi** &lt;accountName >: tallennustilan tilin nimi.
+ **-k** tai **--tiliavain tiliavain** &lt;accountKey >: tallennustilan tilin-näppäintä.
+ **-c** tai **--yhteysmerkkijono** &lt;connectionString >: tallennustilan yhteysmerkkijonon.
+ **--Virheenkorjaus**: suorittaa virheenkorjaus tallennustilan-komennon.

**tallennustilan blob Lataa tiedosto säilöön [blob] [Valitsimet]**

Tämä komento lataa määritetyn tiedoston specified\ Blob-objektien tallennustilaan.

Tämä komento tukee seuraavat lisäasetukset:

+ **--säilö** &lt;säilö >: Luo säilytykseen nimi.
+ **-b** tai **--Blob-objektien** &lt;blobName >: lataa tallennustilan Blob-objektien nimi.
+ **-t** tai **--blobtype** &lt;blobtype >: blob tallennustyyppi: sivun tai lohkon.
+ **-p** tai **--Ominaisuudet** &lt;ominaisuudet >: ladatun tiedoston blob storage ominaisuudet. Ominaisuudet ovat avainasemassa = arvo pari s ja semicolon(;) erottimena. Käytettävissä olevat ominaisuudet ovat contentType, contentEncoding tai contentLanguage cacheControl.
+ **m** tai **--metatiedot** &lt;metatietojen >: ladatun tiedoston tallennustilan blob-metatiedot. Metatietojen osat ovat avainasemassa = arvo-pareina d erottimena puolipiste (;).
+ **--concurrenttaskcount** &lt;concurrenttaskcount >: samanaikainen Lataa pyynnöt enimmäismäärä.
+ **-q** tai **--hiljainen**: Korvaa ilman vahvistusta määritetyn tallennustilan Blob-objektien.
+ **-** tai **--tilin nimi** &lt;accountName >: tallennustilan tilin nimi.
+ **-k** tai **--tiliavain tiliavain** &lt;accountKey >: tallennustilan tilin-näppäintä.
+ **-c** tai **--yhteysmerkkijono** &lt;connectionString >: tallennustilan yhteysmerkkijonon.
+ **--Virheenkorjaus**: suorittaa virheenkorjaus tallennustilan-komennon.

**tallennustilan blob Lataa säilö blob [kohde] [Valitsimet]**

Tämä komento lataa määritetyn tallennustilan Blob-objektien.

Tämä komento tukee seuraavat lisäasetukset:

+ **--säilö** &lt;säilö >: Luo säilytykseen nimi.
+ **-b** tai **--Blob-objektien** &lt;blobName >: tallennustilan blob-nimi.
+ **-d** tai **--kohde** [kohde]: Lataa kohteeseen tiedoston tai kansion polun.
+ **m** tai **--checkmd5**: Tarkista md5sum ladatun tiedoston.
+ **--concurrenttaskcount** &lt;concurrenttaskcount > samanaikainen Lataa pyynnöt enimmäismäärä
+ **-q** tai **--hiljainen**: Korvaa kohdetiedosto ilman vahvistusta.
+ **-** tai **--tilin nimi** &lt;accountName >: tallennustilan tilin nimi.
+ **-k** tai **--tiliavain tiliavain** &lt;accountKey >: tallennustilan tilin-näppäintä.
+ **-c** tai **--yhteysmerkkijono** &lt;connectionString >: tallennustilan yhteysmerkkijonon.
+ **--Virheenkorjaus**: suorittaa virheenkorjaus tallennustilan-komennon.

## <a name="commands-to-manage-sql-databases"></a>Voit hallita SQL-tietokantoja komennot

Nämä komennot avulla voit hallita Azure SQL-tietokannat

###<a name="commands-to-manage-sql-servers"></a>Voit hallita SQL-palvelimet komennot.

Nämä komennot avulla voit hallita SQL-palvelimet

**SQL server Luo &lt;administratorLogin > &lt;administratorPassword > &lt;sijainti >**

Luo tietokantapalvelimeen

    ~$ azure sql server create test T3stte$t "West US"
    info:    Executing command sql server create
    + Creating SQL Server
    data:    Server Name i1qwc540ts
    info:    sql server create command OK

**SQL server Näytä &lt;nimi >**

Näytä tiedot.

    ~$ azure sql server show xclfgcndfg
    info:    Executing command sql server show
    + Getting SQL server
    data:    SQL Server Name xclfgcndfg
    data:    SQL Server AdministratorLogin msopentechforums
    data:    SQL Server Location West US
    data:    SQL Server FullyQualifiedDomainName xclfgcndfg.database.windows.net
    info:    sql server show command OK

**SQL server-luettelo**

Pyydä palvelinten luettelon.

    ~$ azure sql server list
    info:    Executing command sql server list
    + Getting SQL server
    data:    Name        Location
    data:    ----------  --------
    data:    xclfgcndfg  West US
    info:    sql server list command OK

**SQL server Poista &lt;nimi >**

Poistaa palvelimen

    ~$ azure sql server delete i1qwc540ts
    info:    Executing command sql server delete
    Delete server i1qwc540ts? [y/n] y
    + Removing SQL Server
    info:    sql server delete command OK

###<a name="commands-to-manage-sql-databases"></a>Voit hallita SQL-tietokantoja komennot

Nämä komennot avulla voit hallita SQL-tietokannat.

**SQL-db luominen [Valitsimet] &lt;palvelimen nimi > &lt;nimi > &lt;administratorPassword >**

Luo tietokannan esiintymän

    ~$ azure sql db create fr8aelne00 newdb test
    info:    Executing command sql db create
    Administrator password: ********
    + Creating SQL Server Database
    info:    sql db create command OK

**SQL-db Näytä [Valitsimet] &lt;palvelimen nimi > &lt;nimi > &lt;administratorPassword >**

Näyttää tietokannan tiedot.

    C:\windows\system32>azure sql db show fr8aelne00 newdb test
    info:    Executing command sql db show
    Administrator password: ********
    + Getting SQL server databases
    data:    Database _ ContentRootElement=m:properties, id=https://fr8aelne00.datab
    ase.windows.net/v1/ManagementService.svc/Server2('fr8aelne00')/Databases(4), ter
    m=Microsoft.SqlServer.Management.Server.Domain.Database, scheme=http://schemas.m
    icrosoft.com/ado/2007/08/dataservices/scheme, link=[rel=edit, title=Database, hr
    ef=Databases(4), rel=http://schemas.microsoft.com/ado/2007/08/dataservices/relat
    ed/Server, type=application/atom+xml;type=entry, title=Server, href=Databases(4)
    /Server, rel=http://schemas.microsoft.com/ado/2007/08/dataservices/related/Servi
    ceObjective, type=application/atom+xml;type=entry, title=ServiceObjective, href=
    Databases(4)/ServiceObjective, rel=http://schemas.microsoft.com/ado/2007/08/data
    services/related/DatabaseMetrics, type=application/atom+xml;type=entry, title=Da
    tabaseMetrics, href=Databases(4)/DatabaseMetrics, rel=http://schemas.microsoft.c
    om/ado/2007/08/dataservices/related/DatabaseCopies, type=application/atom+xml;ty
    pe=feed, title=DatabaseCopies, href=Databases(4)/DatabaseCopies], title=, update
    d=2013-11-18T19:48:27Z, name=
    data:    Database Id 4
    data:    Database Name newdb
    data:    Database ServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database AssignedServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database ServiceObjectiveAssignmentState 1
    data:    Database ServiceObjectiveAssignmentStateDescription Complete
    data:    Database ServiceObjectiveAssignmentErrorCode
    data:    Database ServiceObjectiveAssignmentErrorDescription
    data:    Database ServiceObjectiveAssignmentSuccessDate
    data:    Database Edition Web
    data:    Database MaxSizeGB 1
    data:    Database MaxSizeBytes 1073741824
    data:    Database CollationName SQL_Latin1_General_CP1_CI_AS
    data:    Database CreationDate
    data:    Database RecoveryPeriodStartDate
    data:    Database IsSystemObject
    data:    Database Status 1
    data:    Database IsFederationRoot
    data:    Database SizeMB -1
    data:    Database IsRecursiveTriggersOn
    data:    Database IsReadOnly
    data:    Database IsFederationMember
    data:    Database IsQueryStoreOn
    data:    Database IsQueryStoreReadOnly
    data:    Database QueryStoreMaxSizeMB
    data:    Database QueryStoreFlushPeriodSeconds
    data:    Database QueryStoreIntervalLengthMinutes
    data:    Database QueryStoreClearAll
    data:    Database QueryStoreStaleQueryThresholdDays
    info:    sql db show command OK

**SQL db luettelon [Valitsimet] &lt;palvelimen nimi > &lt;administratorPassword >**

Luettele tietokannat.

    ~$ azure sql db list fr8aelne00 test
    info:    Executing command sql db list
    Administrator password: ********
    + Getting SQL server databases
    data:    Name    Edition  Collation                     MaxSizeInGB
    data:    ------  -------  ----------------------------  -----------
    data:    master  Web      SQL_Latin1_General_CP1_CI_AS  5
    info:    sql db list command OK

**Poista SQL db [Valitsimet] &lt;palvelimen nimi > &lt;nimi > &lt;administratorPassword >**

Poistaa tietokannan.

    ~$ azure sql db delete fr8aelne00 newdb test
    info:    Executing command sql db delete
    Administrator password: ********
    Delete database newdb? [y/n] y
    + Getting SQL server databases
    + Removing database
    info:    sql db delete command OK

###<a name="commands-to-manage-your-sql-server-firewall-rules"></a>Komennot, joilla SQL Server-palomuurin sääntöjen hallinta

Nämä komennot avulla voit hallita SQL Server-palomuurisäännöt

**SQL-firewallrule luominen [Valitsimet] &lt;palvelimen nimi > &lt;SäännönNimi > &lt;startIPAddress > &lt;endIPAddress >**

Palomuurisäännön luominen SQL Serveriä varten.

    ~$ azure sql firewallrule create fr8aelne00 allowed 131.107.0.0 131.107.255.255
    info:    Executing command sql firewallrule create
    + Creating Firewall Rule
    info:    sql firewallrule create command OK

**SQL-firewallrule Näytä [Valitsimet] &lt;palvelimen nimi > &lt;SäännönNimi >**

Näytä palomuurin säännön tiedot.

    ~$ azure sql firewallrule show fr8aelne00 allowed
    info:    Executing command sql firewallrule show
    + Getting firewall rule
    data:    Firewall rule Name allowed
    data:    Firewall rule Type Microsoft.SqlAzure.FirewallRule
    data:    Firewall rule State Normal
    data:    Firewall rule SelfLink https://management.core.windows.net/9e672699-105
    5-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00/firewallrules/allowed
    data:    Firewall rule ParentLink https://management.core.windows.net/9e672699-1
    055-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00
    data:    Firewall rule StartIPAddress 131.107.0.0
    data:    Firewall rule EndIPAddress 131.107.255.255
    info:    sql firewallrule show command OK

**SQL firewallrule luettelon [Valitsimet] &lt;palvelimen nimi >**

Luettele palomuurisäännöt.

    ~$ azure sql firewallrule list fr8aelne00
    info:    Executing command sql firewallrule list
    \data:    Name     Start IP address  End IP address
    data:    -------  ----------------  ---------------
    data:    allowed  131.107.0.0       131.107.255.255
    +
    info:    sql firewallrule list command OK

**SQL-firewallrule poistaminen [Valitsimet] &lt;palvelimen nimi > &lt;SäännönNimi >**

Tällä komennolla voit poistaa palomuurisäännön.

    ~$ azure sql firewallrule delete fr8aelne00 allowed
    info:    Executing command sql firewallrule delete
    Delete rule allowed? [y/n] y
    + Removing firewall rule
    info:    sql firewallrule delete command OK

## <a name="commands-to-manage-your-virtual-networks"></a>Komennot, joilla Virtual verkkojen hallinta

Nämä komennot avulla voit hallita Virtual lopettaminen

**verkon vnet luominen [Valitsimet] &lt;sijainti >**

Luo virtuaalisia verkkoon.

    ~$ azure network vnet create vnet1 --location "West US" -v
    info:    Executing command network vnet create
    info:    Using default address space start IP: 10.0.0.0
    info:    Using default address space cidr: 8
    info:    Using default subnet start IP: 10.0.0.0
    info:    Using default subnet cidr: 11
    verbose: Address Space [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/8 (16777216)
    verbose: Subnet [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/11 (2097152)
    verbose: Fetching Network Configuration
    verbose: Fetching or creating affinity group
    verbose: Fetching Affinity Groups
    verbose: Fetching Locations
    verbose: Creating new affinity group AG1
    info:    Using affinity group AG1
    verbose: Updating Network Configuration
    info:    network vnet create command OK

**verkon vnet Näytä &lt;nimi >**

Näytä Virtual verkon tiedot.

    ~$ azure network vnet show vnet1
    info:    Executing command network vnet show
    + Fetching Virtual Networks
    data:    Name "vnet1"
    data:    Id "25786fbe-08e8-4e7e-b1de-b98b7e586c7a"
    data:    AffinityGroup "AG1"
    data:    State "Created"
    data:    AddressSpace AddressPrefixes 0 "10.0.0.0/8"
    data:    Subnets 0 Name "subnet-1"
    data:    Subnets 0 AddressPrefix "10.0.0.0/11"
    info:    network vnet show command OK

**verkon vnet luettelo**

Luettele kaikki Virtual verkoissa.

    ~$ azure network vnet list
    info:    Executing command network vnet list
    + Fetching Virtual Networks
    data:    Name        Status   AffinityGroup
    data:    ----------  -------  -------------
    data:    vnet1      Created  AG1
    data:    vnet2      Created  AG1
    data:    vnet3      Created  AG1
    data:    vnet4      Created  AG1
    info:    network vnet list command OK


**verkon vnet poistaminen &lt;nimi >**

Poistaa määritetyn Virtual verkon.

    ~$ azure network vnet delete opentechvn1
    info:    Executing command network vnet delete
    + Fetching Network Configuration
    Delete the virtual network opentechvn1 ?  (y/n) y
    + Deleting the virtual network opentechvn1
    info:    network vnet delete command OK

**verkon vieminen [tiedostopolku]**

Lisäasetukset verkon määrittäminen, kun viet verkon määritysten paikallisesti. Viedyt verkon määritys on DNS-palvelinasetukset, VPN-asetukset, lähiverkossa sivuston asetukset ja muita asetuksia.

**verkon tuo [tiedostopolku]**

Voit tuoda paikallisen verkon määritys.

**verkon dnsserver rekisteröidä [Valitsimet] &lt;dnsIP >**

Rekisteröi DNS-palvelimeen, jota aiot käyttää nimenselvitys verkko-kokoonpanoa.

    ~$ azure network dnsserver register 98.138.253.109 --dns-id FrontEndDnsServer
    info:    Executing command network dnsserver register
    + Fetching Network Configuration
    + Updating Network Configuration
    info:    network dnsserver register command OK

**verkon dnsserver luettelo**

Luettele kaikki verkon määritysten rekisteröity DNS-palvelimet.

    ~$ azure network dnsserver list
    info:    Executing command network dnsserver list
    + Fetching Network Configuration
    data:    DNS Server ID         DNS Server IP
    data:    --------------------  --------------
    data:    DNS-bb39b4ac34d66a86  44.55.22.11
    data:    FrontEndDnsServer     98.138.253.109
    info:    network dnsserver list command OK

**verkon dnsserver unregister [Valitsimet] &lt;dnsIP >**

Poistaa DNS server-merkintä, verkon määritysten.

    ~$ azure network dnsserver unregister 77.88.99.11
    info:    Executing command network dnsserver unregister
    + Fetching Network Configuration
    Delete the DNS server entry dns-4 ( 77.88.99.11 ) %s ? (y/n) y
    + Deleting the DNS server entry dns-4 ( 77.88.99.11 )
    info:    network dnsserver unregister command OK

