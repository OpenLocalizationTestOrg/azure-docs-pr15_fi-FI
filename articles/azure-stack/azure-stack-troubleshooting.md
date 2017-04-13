<properties
    pageTitle="Microsoft Azure pinon vianmääritys | Microsoft Azure"
    description="Azure pinon vianmääritys."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="microsoft-azure-stack-troubleshooting"></a>Microsoft Azure pinon vianmääritys

Tässä tiedostossa on yleisiä Azure pinon vianmääritystiedot.

Saat parhaan kokemuksen Varmista, että käyttöönotto-ympäristösi täyttää kaikki [vaatimukset](azure-stack-deploy.md) ja [valmistelut](azure-stack-run-powershell-script.md) ennen asennusta. 

Suosituksia vianmääritys, joka on kuvattu tämän osion useista lähteistä peräisin, ja ongelmaasi ei ehkä Ratkaise. Jos ongelma ei ole mainittu, varmista, että [Azure pinon MSDN-keskustelupalsta](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) edelleen tuki-ja muita tietoja.

Koodiesimerkkejä toimitetaan sellaisenaan ja odotetut tulokset voidaan taata. Tässä osassa on usein käytetyt muokkaukset ja päivitykset tuotteen parannuksia on otettu käyttöön.

  

## <a name="known-issues"></a>Tunnetut ongelmat

 - Näkyviin voi tulla seuraava-lopetetaan virheet käyttöönoton aikana joka käyttöönoton onnistumiseen vaikuttavat:
     - "" C:\WinRM\Start-Logging.ps1"ei tunnisteta termi"
     - "Ongelma EceAction: ei voi indeksoida null taulukon yhdeksi" 
     - "InvokeEceAction: ei voi liittyä argumentin parametrin 'Viestin', koska se on tyhjä merkkijono."
 - Voit nähdä epäonnistua virheen "Tunnus"URI"ei löydy sovellusten." 60.61.93 vaiheessa käyttöönotto Tämä ongelma on rekisteröity sovelluksia Azure Active Directoryn tavalla.  Jos saat tämän virheilmoituksen, edelleen [uudelleen asennuksen komentosarja](azure-stack-rerun-deploy.md) vaiheesta 60.61.93 käyttöönoton pistekokoa.
 - Näet, että **virtualMachine ARM** -luokassa näkyy **Saatavuus** resurssin on Marketplace – tämä ulkoasu on vain kosmeettisia ongelma.
 - Luotaessa uusia virtual machine portaalissa **perusteet** vaiheessa tallennustilan-asetuksen oletusarvo on Suoritettaessa.  Tämä asetus on vaihdettava kiintolevylle tai **koon** vaiheessa, AM käyttöönoton, et näe AM koot käytettävissä ja jatka käyttöönotto. 
 - Näet AzureRM PowerShell-moduuleja ei enää asenneta oletusarvoisesti MAS CON01 AM (TP1: Tämä on nimeltään ClientVM). Tämä ongelma on suunniteltu ominaisuus, koska on vaihtoehtoinen menetelmä [Asenna nämä moduulit ja liitä](azure-stack-connect-powershell.md).  
 - Näet, että **Microsoft.Insights** resurssi-palvelu ei ole automaattisesti rekisteröity vuokraajan tilaukset. Jos haluat nähdä tietojen seurannan AM, kuin palvelutili käyttöön, suorita seuraava komento PowerShell-(jälkeen voit [asentaa ja Yhdistä](azure-stack-connect-powershell.md) palvelutili): 
       
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights 

 - Näet Vie toimintoja portaalissa resurssin ryhmien, mutta ei ole teksti on näkyvissä ja käytettävissä vienti.      
 - Voit aloittaa tallennustilan resurssien suurempi kuin käytettävissä kiintiön käyttöönotto.  Tämä käyttöönotto epäonnistuu, ja tilin resurssit keskeytetään.  Käytettävissä on kaksi korjaus-asetusta:
     - Palvelun järjestelmänvalvoja voi parantaa kiintiö, mutta muutokset voimaan heti ja yleisesti vievät tunneiksi leviäminen ei.
     - Palvelun järjestelmänvalvoja voit luoda lisäosa suunnitelman muita kiintiön, vuokraajan jälkeen voit lisätä tilaukseen.
 - Kun portaalin avulla voit luoda VMs Azure pinon ympäristöissä, joissa jäsenyyden "Azure - Kiina", ei näy käytettävissä AM käyttöönoton **koon** vaiheessa AM koot ja ei voi jatkaa käyttöönotto.
 - Näkyviin voi tulla käyttöönoton epäonnistui-portaalissa AM on todella käyttöönotto onnistui.
 - Kun poistat suunnitelma, tarjouksen tai tilauksen, VMs ei voi poistaa.
 - Näet AM-laajennukset Marketplacesta.
 - AM tallennetun AM kuvasta ei voi ottaa käyttöön.
 - Alihallinnat, jotka voi tulla palveluja, jotka eivät sisälly tilauksen.  Kun alihallinnat yrittää ottaa käyttöön näiden resurssien, he saavat virheen.  Esimerkki: Vuokraajan tilaus sisältää vain tallennustilan resurssit.  Vuokraajan tulee näkyviin, kun haluat luoda muita resursseja, kuten VMs.  Tässä skenaariossa alihallinnan yrittää ottaa käyttöön AM, näyttöön tulee sanoma, joka ilmaisee, AM ei voi luoda. 
 - Asennettaessa TP2 olisi Aktivoi Näennäiskiintolevyn, jossa Azure pinon asennuksen komentosarjan suorittaminen tai voit saada virhesanoman messaging maininta tilastollisesti Windows-käyttöjärjestelmän päättyy pian host.


## <a name="deployment"></a>Käyttöönotto

### <a name="deployment-failure"></a>Käyttöönoton virhe
Jos virhe toistuu asennuksen aikana, Azure pinon asennuksen avulla voit jatkaa asennuksen [suorittamalla käyttöönoton ohjeita](azure-stack-rerun-deploy.md)noudattamalla.

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Käyttöönoton lopussa PowerShell-istunnon on edelleen avoinna ja kaikki tulos ei näy

Tämä ongelma on todennäköisesti juuri tulos PowerShell-komentoikkunassa oletustoiminnon, kun se on valittu. Käsitteiden käyttöönotto onnistui, mutta komentosarja keskeytettiin ikkunan valittaessa. Voit varmistaa näin hakemalla sanaa "select-komennon ikkunan otsikkorivillä.  Painamalla ESC-näppäintä, poista sen valinta ja valmistuminen viesti näkyy sen jälkeen.

## <a name="templates"></a>Mallit

### <a name="azure-template-wont-deploy-to-azure-stack"></a>Azure-malli ei ota käyttöön Azure pino

Varmista seuraavat seikat:

- Malli on käytettävä Microsoft Azure-palvelu, joka on jo käytettävissä tai Azure Pinotut esikatselussa.
- Käyttää tiettyä resurssia ohjelmointirajapinnan tukemat paikallisen Azure pinon esiintymän ja että kohdennat kelvollinen sijainti ("Paikallinen"-Azure pinon tekninen esikatselu (TP) 2 ja "Itä USA" tai "Etelä Intia" Azure-tietokannassa).
- Voit tarkistaa [tämän artikkelin](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) testi AzureRmResourceGroupDeployment cmdlet-komennot, jotka todellisen Azure Resurssienhallinta syntaksi pieni erot.

Voit käyttää myös jo antanut [GitHub säilöön](http://aka.ms/AzureStackGitHub/) Azure pinon mallien avulla pääset alkuun.

## <a name="virtual-machines"></a>Näennäiskoneiden

### <a name="after-starting-my-microsoft-azure-stack-poc-host-all-my-tenants-vms-are-gone-from-hyper-v-manager-and-come-back-automatically-after-waiting-a-bit"></a>Microsoft Azure pinon Käsitteiden-isännän käynnistyksen jälkeen kaikki omat alihallinnat VMs ovat kadonneet Hyper-V vastuuhenkilöltä ja sisältyvät takaisin automaattisesti jälkeen odotetaan hieman?

Kun järjestelmä tulee takaisin ylös RPs pitää määrittää yhdenmukaisuuden ja tallennustilaa alirakenne. Tarvittavan ajan, riippuu siitä, että laitteet ja kuvaus käytössä, mutta se saattaa olla jonkin aikaa vuokraajan VMs tunnistetaan ja palata isännän uudelleenkäynnistyksen jälkeen.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Voin poistanut joitakin näennäiskoneiden kuitenkin yhä nähdä näennäiskiintolevytiedostoja levylle. Tämä ongelma odotetun?

Kyllä, tämä on ongelman oikein. Se on suunniteltu tällä tavalla, koska:

- Kun poistat AM, näennäiskiintolevyjen ei poisteta. Levyjen on erillinen resursseja resurssiryhmän.
- Tallennustilan tilin saa poistamisen, poisto on näkyvissä heti kautta Azure Resurssienhallinta (portaalin PowerShell), mutta siinä voi olla levyjen säilytetään edelleen tallennustilan, kunnes muistista suoritetaan.

Jos näet "orporivit" näennäiskiintolevyjen, on tärkeää tietää, jos ne ovat osa tallennustilan tilin, joka on poistettu kansio. Jos tallennustilan tiliä ei poisteta, on tavallinen ovat edelleen postilaatikossa.

Voit lukea lisää määrittämisestä säilytys raja- [tallennustilan tilien hallinta](azure-stack-manage-storage-accounts.md).

## <a name="installation-steps"></a>Asennusohjeita
Azure-pino asennusohjeita seuraavia tietoja voi olla hyötyä vianmääritystä varten:  

| Indeksi | Nimi | Kuvaus|
| ----- | ----- | -----|
|0.11 | (DEP) Vahvista fyysisten laitteiden | Laitteiston ja käyttöjärjestelmän määritys Fyysinen solmuissa vahvistaminen. |
| 0,12 | (DEP) Fyysisten laitteiden verkko, Käsitteiden määrittäminen | Määritä VPN valitsimet ja liittymät. |
| 0.14 | (DEP) Toimialueen käyttöönotto | Ota käyttöön Active Directory virtuaalikoneen. |
| 0,15 | (DEP) Määritä toimialue-palvelin | Määritä toimialueen palvelimen käyttöoikeusryhmät jne. |
| 0,16 | (DEP) Fyysinen koneen | Määritä verkko, liity toimialueen ja asennuksen paikallisen Järjestelmänvalvojat. |
| 0.18 | (LOPETA) Tallennustilan klusterin määrittäminen | Luo tallennustilan klusterin, tallennustilan resurssivaranto ja tiedosto palvelimeen. |
| 0.19 | (CPI) Asennuksen kangasta infrastruktuuri | Määritä edellytykset kangasta käyttöönottoa varten. |
| 0.21 | (VERKON) Määritä erityisen ja NAT | Asentaa erityisen ja NAT - tarvitaan vain yksi solmu. |
| 0.22 | (VERKON) NAT- ja aikapalvelimen asetusten määrittäminen | Synkronoi aikapalvelin ja määrittää NAT-tapahtumat. |
| 40.41 | (CPI) Luo vieraskäytön VMs | Luo hallinta VMs. |
| 40.42 | (FBI: LLE) PowerShellin JEA määrittäminen | Määritä PowerShell JEA rooleista. |
| 40.43 | (FBI: LLE) Azure-pino myöntäjä määrittäminen | Asentaa Azure pinon varmenteiden myöntäjä. |
| 40.44 | (FBI: LLE) Määritä Azure pinon myöntäjä | Määrittää Azure pinon varmenteiden myöntäjä. |
| 40.45 | (VERKON) Valitse VMs NC määrittäminen | Asentaa NC Vieras VMs |
| 40.46 | (VERKON) NC määrittämistä VMs | NC määrittämistä Vieras VMs |
| 40.47 | (VERKON) Määritä Vieras VMs | Määritä hallinta VMs NC käyttöoikeusluettelot. |
| 60.61.81 | (FBI: LLE) Ottaa käyttöön Azure pinon kangasta soi palvelut - FabricRing edellytyksenä | Luo FabricRing VIPs |
| 60.61.82 | (FBI: LLE) Azure pinon kangasta soi Services ‑palveluiden käyttöönottaminen - kangasta soi klusterin käyttöönotto | Asentaa ja määrittää Azure pinon kangasta soi klusterin. |
| 60.61.83 | (FBI: LLE) Ottaa käyttöön järjestelmänvalvojan tunnisteet resurssin toimittajien | Asentaminen resurssin toimittajien järjestelmänvalvoja-laajennukset |
| 60.61.84 | (ACS) Määritä Azure yhdenmukaisia tallennustilan solmu tasolla. | Asentaa ja määrittää Azure yhdenmukaisia tallennustilan solmu taso. |
| 60.61.85 | (ACS) Määritä Azure yhdenmukaisia tallennustilan klusterin tasolla. | Asentaa ja määrittää Azure yhdenmukaisia tallennustilan klusterin taso. |
| 60.61.86 | (FBI: LLE) Ottaa käyttöön Azure pinon kangasta soi ohjauskoneen palvelut - edellytyksenä | InfraServiceController edellytyksistä |
| 60.61.87 | (FBI: LLE) Ottaa käyttöön Azure pinon kangasta rengas ohjauskoneen palvelut - edellytyksenä | KHI edellytyksistä |
| 60.61.88 | (FBI: LLE) Ottaa käyttöön Azure pinon kangasta soi ohjauskoneen palvelut - edellytyksenä | ASAppGateway edellytyksistä |
| 60.61.89 | (FBI: LLE) Ottaa käyttöön Azure pinon kangasta rengas ohjauskoneen palvelut - edellytyksenä | Edellytyksistä ohjain |
| 60.61.90 | (FBI: LLE) Ottaa käyttöön Azure pinon kangasta soi ohjauskoneen palvelut - edellytyksenä | HealthMonitoring edellytyksistä |
| 60.61.91 | (FBI: LLE) Ottaa käyttöön Azure pinon kangasta soi ohjauskoneen palvelut - edellytyksenä | ECE edellytyksistä |
| 60.61.92 | (FBI: LLE) Ottaa käyttöön Azure pinon kangasta rengas ohjauskoneen palvelut - edellytyksenä | PMM edellytyksistä |
| 60.61.93 | (Katal) Luo AzureStack palvelun ansaitun | Luo Azure Graph-sovellukset ja palvelun ansaitun AAD. |
| 60.61.94 | (VERKON) Asennuksen GW VMs | Asentaa GW Vieras VMs. |
| 60.61.95 | (VERKON) Määritä GW VMs | Määrittää GW Vieras VMs. |
| 60.61.96 | (VERKON) Isännät-IDN käyttöönotto | IDN-infrastruktuurin isännät käyttöönotto |
| 60.61.97 | (VERKON) Määritä IDN | IDN-roolin määrittäminen |
| 60.61.98 | (FBI: LLE) Asennuksen WSUS VMs | Asentaa Vieras VMs WSUS-palvelimeen. |
| 60.61.99 | (FBI: LLE) Määritä WSUS VMs | Määrittää Vieras VMs WSUS-palvelimeen. |
| 60.61.100 | (FBI: LLE) Määritä Azure SQL-VMs | Asentaa Azure SQL Serverin Vieras VMs |
| 60.61.101 | (Katal) Määritys on VMs edellytyksistä. | Määrittää Microsoft Azure-pino-Vieras VMs edellytyksistä. |
| 60.61.102 | (Katal) Asennusohjelma ei VMs | Asentaa Microsoft Azure pinon Vieras VMs. |
| 60.120.121 | (FBI: LLE) Ottaa käyttöön resurssin tarjoajat ja ohjaimet | Asentaa resurssin tarjoajat ja ohjaimet |
| 60.120.121 | (FBI: LLE) Ottaa käyttöön resurssin tarjoajat ja ohjaimet | Asentaa resurssin tarjoajat ja ohjaimet |
| 60.120.121 | (FBI: LLE) Ottaa käyttöön resurssin tarjoajat ja ohjaimet | Asentaa resurssin tarjoajat ja ohjaimet |
| 60.120.121 | (FBI: LLE) Ottaa käyttöön resurssin tarjoajat ja ohjaimet | Asentaa resurssin tarjoajat ja ohjaimet |
| 60.120.121 | (FBI: LLE) Ottaa käyttöön resurssin tarjoajat ja ohjaimet | Asentaa resurssin tarjoajat ja ohjaimet |
| 60.120.121 | (FBI: LLE) Ottaa käyttöön resurssin tarjoajat ja ohjaimet | Asentaa resurssin tarjoajat ja ohjaimet |
| 60.120.121 | (FBI: LLE) Ottaa käyttöön resurssin tarjoajat ja ohjaimet | Asentaa resurssin tarjoajat ja ohjaimet |
| 60.120.121 | (FBI: LLE) Ottaa käyttöön resurssin tarjoajat ja ohjaimet | Asentaa resurssin tarjoajat ja ohjaimet |
| 60.120.122 | (FBI: LLE) Ohjaimen määritys | Määrittää ohjaimet |
| 60.120.123 | (Katal) Määritä WAS VMs | Määrittää Microsoft Azure pinon Vieras VMs. |
| 60.120.124 | (Katal) Azure pinon AAD määritys. | Määrittää Azure pinon Azure AD kanssa. |
| 60.120.125 | (Katal) ADFS: N asentaminen | Asentaa Active Directory-liittoutumispalvelut (ADFS) |
| 60.120.126 | (Katal) Asentaminen ADFS ja kaavio | Asentaa Azure pinon Graph |
| 60.120.127 | (Katal) ADFS: N määrittäminen | Määrittää Active Directory-liittoutumispalvelut (ADFS) |
| 60.140.141 | (FBI: LLE) Määritä OVH | Määrittää tallennustilan resurssin tarjoajaan |
| 60.140.142 | (ACS) Määritä Azure yhdenmukaisia tallennustilan. | Määrittää Azure yhdenmukaisia tallennustilan. |
| 60.140.143 | (FBI: LLE) Tallennustilan tilien luominen | Luo kaikki eri tarjoajien käytettävän tallennustilan tilit. |
| 60.140.144 | (FBI: LLE) Rekisteröidy OVH käyttö | Rekisteröidy Storage Provider käyttö. |
| 60.140.145 | (CPI) Siirtää luotu VMs isäntien ja klusterin KHI | Siirtää objekteja luotu VMs, isäntien ja klusterin KHI |
| 60.140.146 | (FBI: LLE) Windows Defenderin määrittäminen | Määrittää Windows Defender |
| 60.160.161 | (MA) Määritä valvonta agentti | Määrittää agentti seuranta |
| 60.160.162 | (FBI: LLE) NRP edellytyksenä | Asentaa NRP edellytykset |
| 60.160.163 | (FBI: LLE) NRP käyttöönotto | Asentaa NRP  |
| 60.160.164 | (FBI: LLE) NRP määritys | Määrittää NRP |
| 60.160.165 | (FBI: LLE) Edellytyksenä kap. | Asentaa edellytykset kap. |
| 60.160.166 | (FBI: LLE) Käyttöönoton kap. | Asentaa kap. |
| 60.160.167 | (FBI: LLE) Määritysten kap. | Määrittää kap. |
| 60.160.168 | (FBI: LLE) FRP edellytyksenä | Asentaa FRP edellytykset |
| 60.160.169 | (FBI: LLE) FRP käyttöönotto | Asentaa FRP  |
| 60.160.170 | (FBI: LLE) FRP määritys | Määrittää FRP |
| 60.160.174 | (FBI: LLE) URP valmistelevat | Asentaa URP edellytykset |
| 60.160.175 | (FBI: LLE) URP käyttöönotto | Asentaa URP  |
| 60.160.176 | (FBI: LLE) URP määritys | Määrittää URP |
| 60.160.171 | (FBI: LLE) HRP edellytyksenä | Asentaa HRP edellytykset |
| 60.160.172 | (FBI: LLE) HRP käyttöönotto | Asentaa HRP  |
| 60.160.173 | (FBI: LLE) HRP määritys | Määrittää HRP |
| 60.160.177 | (KV) KeyVault edellytyksenä | Asentaa KeyVault edellytykset |
| 60.160.178 | (KV) KeyVault käyttöönotto | Asentaa KeyVault  |
| 60.160.179 | (KV) KeyVault määritys | Määrittää KeyVault | 
| 60.190.191 | (FBI: LLE) Määritä valikoima | Määritä valikoima |
| 60.190.192 | (FBI: LLE) Kangasta soi palvelujen määrittäminen | Kangasta soi palvelujen määrittäminen |
| 60.221 | (FBI: LLE) Konsolin VMs asetukset | Asentaa Vieras VMs Console-palvelimeen. |
| 60.222 | (FBI: LLE) Konsolin VMs asetukset | Siirtää DVM sisällön konsolin AM. |
| 251 | Valmistele varten tulevien host käynnistetään uudelleen | Käynnistä käytännön määrittäminen |


## <a name="next-steps"></a>Seuraavat vaiheet

[Usein kysytyt kysymykset](azure-stack-FAQ.md)
