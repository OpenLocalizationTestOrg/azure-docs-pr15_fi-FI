<properties
    pageTitle="Ennen kuin otat Azure pinon Käsitteiden | Microsoft Azure"
    description="Näytä ympäristön ja laitteiston vaatimukset Azure pinon Käsitteiden (palvelun järjestelmänvalvoja)."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="erikje"/>

# <a name="azure-stack-deployment-prerequisites"></a>Azure pinon käyttöönoton edellytykset

Ennen kuin otat Azure pinon Käsitteiden ([Käsite kuitti](azure-stack-poc.md)), varmista, että tietokoneesi täyttää seuraavat vaatimukset.
Tekninen esikatselu-2-käyttöönottovaatimukset Käsitteiden ovat samat kuin vaaditaan Technical Preview-1. Sen vuoksi voit käyttää saman laite, jota käytit edellisen single-ruudussa esikatselun.

## <a name="hardware"></a>Laite

| Osa | Pienin  | Suositellaan |
|---|---|---|
| Levyasemien: käyttöjärjestelmä | 1 200 gt käytettävissä osion (Suoritettaessa tai Kiintolevylle) vähintään OS levy | 1 200 gt käytettävissä osion (Suoritettaessa tai Kiintolevylle) vähintään OS levy |
| Levyasemien: Yleiset Azure pinon Käsitteiden tiedot | 4 levyjä. Kunkin levyn on vähintään 140 Gigatavua kapasiteetti (Suoritettaessa tai Kiintolevy). Kaikkien käytettävissä levyjen käytetään. | 4 levyjä. Kunkin levyn on vähintään 250 Gigatavua kapasiteetti (Suoritettaessa tai Kiintolevy). Kaikkien käytettävissä levyjen käytetään.|
| Laske: Suoritin | Kahden-Socket: 12 fyysinen sydämiä (summa)  | Kahden-Socket: 16 fyysinen sydämiä (summa) |
| Laske: muistia | 96 GT RAM-MUISTIA  | 128 GT RAM-MUISTIA |
| Laske: BIOS | Hyper-V käytössä (ja SLAT tuki)  | Hyper-V käytössä (ja SLAT tuki) |
| Verkko: NIC: ssä | Windows Server 2012 R2: n sertifikaatin vaaditaan NIC; ei ole pakollinen erikoisominaisuuksia | Windows Server 2012 R2: n sertifikaatin vaaditaan NIC; ei ole pakollinen erikoisominaisuuksia |
| KV logon varmennusta | [Sertifioitu Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Sertifioitu Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

**Tietojen levyaseman määritys:** Kaikki tiedot asemat on oltava sama tyyppi (kaikki Suojaussidokset tai kaikki SATA) ja kapasiteetti. Jos SAS levyasemien käytetään, levyasemien on liitettävä yksittäisen polun (ei MPIO usean polku tuki on annettu) kautta.

**HBA kokoonpanoasetusten määrittäminen**
 
- (Ensisijainen) Yksinkertainen HBA
- RAID HBA – sovittimen on määritettävä "kulkevat"-tilassa
- RAID HBA – levyjen on määritettävä kuin levylle, RAID 0

**Tuetut bus ja media kirjoittamalla yhdistelmät**

-   SATA KIINTOLEVY

-   SAS KIINTOLEVY

-   RAID KIINTOLEVY

-   RAID Suoritettaessa (Jos mediatyyppi on määrittämätön tai Tuntematon\*)

-   SATA SUORITETTAESSA + SATA KIINTOLEVY

-   SAS SUORITETTAESSA + SAS KIINTOLEVY

\*RAID ohjaimet ilman läpivienti mahdollisuutta ei tunnista mediatyyppi. Näiden ohjaimet merkitsee Kiintolevylle ja Suoritettaessa määrittämätön. Siinä tapauksessa Suoritettaessa voi käyttää pysyvän säilön sijaan välimuistiin laitteet. Sen vuoksi voit ottaa näiden SSD Microsoft Azure pino-Käsitteiden.

**Esimerkki isäntäväyläsovittimet**: LSI 9207 8i, LSI 9300-8i tai LSI 9265-8i läpivienti-tilassa

Esimerkki OEM-määrityksiä ovat käytettävissä.

## <a name="operating-system"></a>Käyttöjärjestelmä

| | **Vaatimukset**  |
|---|---|
| **Käyttöjärjestelmäversio** | Windows Server 2012 R2: n tai sitä uudemmalla versiolla. Käyttöjärjestelmäversio ei ole kriittinen käyttöönotto alkaa, ennen kuin käynnistät isäntätietokoneen näennäiskiintolevylle, joka sisältyy Azure pinon asennuksen zip. Kuva jo integroitu käyttöjärjestelmän ja kaikki tarvittavat korjaukset. Älä käytä näppäinten aktivoida Käsitteiden käyttää Windows Server kaikki esiintymät.|

## <a name="deployment-requirements-check-tool"></a>Käyttöönoton vaatimusten tarkistus-työkalu

Asennettuasi käyttöjärjestelmä, varmista, että laitteen täyttää kaikki vaatimukset [Käyttöönoton tarkistaminen Azure pinon tekninen esikatselu 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) avulla.



## <a name="microsoft-azure-active-directory-accounts"></a>Microsoft Azure Active Directory-tilit

Microsoft Azure pinon Käsitteiden käyttöönotto on oltava yhdistettynä Azure. Sen vuoksi sinun täytyy laatia Microsoft Azure Active Directory-tilin ennen kuin suoritat käyttöönoton PowerShell-komentosarjaa. Tällä tilillä on yleisen järjestelmänvalvojan Azure Active Directory-vuokraajan. Sitä käytetään valmistelu ja delegoida sovellukset ja palvelun ansaitun kaikissa Azure Active Directory- ja kuvan Ohjelmointirajapinnan kanssa toimivat Azure pino-palveluissa. Sitä käytetään myös oletus tarjoajan tilaus (joka voit myöhemmin muuttaa) omistajaksi. Voit kirjautua Azure pino-järjestelmän hallintaportaalissa käyttämällä tähän tiliin.

1. Luo Azure AD-tili, joka on vähintään yksi Azure Active Directory directory-järjestelmänvalvoja. Jos sinulla on jo jokin, voit käyttää joka. Muussa tapauksessa voit luoda sen maksutta osoitteessa [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (Kiinassa, osoitteessa <http://go.microsoft.com/fwlink/?LinkID=717821> sijaan.)

    Nämä tunnistetiedot käytettäväksi [Suorita PowerShell-käyttöönoton komentosarja](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script)vaiheessa 6. Tämän *palvelun järjestelmänvalvoja* -tilin voit määrittää ja hallita resurssin paveikslėlis, käyttäjätilit, vuokraajan suunnitelmien, kiintiön ja hinnoittelua. Portaalissa ne voit luoda sivuston paveikslėlis virtuaalikoneen yksityinen paveikslėlis, luodaan Palvelupaketit ja käyttäjän tilausten hallinta.

2. [Luo](azure-stack-add-new-user-aad.md) vähintään yksi tilin niin, että voit kirjautua sisään Azure pinon Käsitteiden alihallinnan kuin haluat.

  	| **Azure Active Directory-tili**  | **Tukee?** |
  	|---|---| 
  	| Työpaikan tai oppilaitoksen tilin, joka on kelvollinen julkisen Azure-tilaus  | Kyllä |
  	| Microsoft-Account kelvollinen julkisen Azure-tilaus  | Ei |
  	| Työpaikan tai oppilaitoksen tilin, joka on kelvollinen Kiinan Azure-tilaus  | Kyllä |
  	| Työpaikan tai oppilaitoksen tilin, joka on kelvollinen US Government Azure tilaus  | Kyllä |


## <a name="network"></a>Verkon

### <a name="switch"></a>Vaihda

Vaihda Käsitteiden koneen yksi käytettävissä porttiin.  

Azure pinon Käsitteiden koneen tukee yhteyden valitsin access portin tai yhdysjohdon-porttiin. Vaihda on käytettävä ei erikoisominaisuuksia. Jos käytössäsi on yhdysjohdon portin tai jos sinun on määritettävä VLAN-tunnus, sinun on annettava käyttöönoton parametrina VLAN-tunnus. Näet esimerkkejä [käyttöönoton parametrien luettelon](azure-stack-run-powershell-script.md).

### <a name="subnet"></a>Aliverkon

Käsitteiden koneen ei muodostaa seuraavat aliverkosta:
- 192.168.200.0/24
- 192.168.100.0/27
- 192.168.101.0/26
- 192.168.102.0/24
- 192.168.103.0/25
- 192.168.104.0/25

Nämä aliverkosta varataan sisäisiin verkkoihin Microsoft Azure pinon Käsitteiden-ympäristössä.

### <a name="ipv4ipv6"></a>IPv4 ja IPv6

Tuetaan vain IPv4. Et voi luoda IPv6-verkoissa.

### <a name="dhcp"></a>DHCP

Varmista, että verkossa, joka yhdistää Verkkokortti on DHCP-palvelimeen. Jos DHCP ei ole käytettävissä, sinun täytyy laatia muut staattista IPv4-verkossa käyttämä host lisäksi. Sinun on määritettävä, IP-osoite ja yhdyskäytävän käyttöönoton parametrina. Näet esimerkkejä [käyttöönoton parametrien luettelon](azure-stack-run-powershell-script.md).

### <a name="internet-access"></a>Internet-yhteys

Azure pinon tarvitaan Internet-yhteys joko suoraan tai läpinäkyvä välityspalvelimen kautta. Azure pinoa ei tue web-välityspalvelimen käyttöön Internet-yhteyden määrittäminen. Isännän IP ja uusi IP määrittämän MAS-BGPNAT01 (DHCP tai staattinen IP) on oltava voivat käyttää Internet. Portit 80 ja 443 käytetään graph.windows.net ja login.windows.net toimialueet-kohdassa.

### <a name="telemetry"></a>Telemetriatietojen

Tukemaan telemetriatietojen tiedonkulun porttiin 443 (HTTPS) on oltava avoinna verkossa. Asiakas-päätepiste on https://vortex-win.data.microsoft.com.


## <a name="next-steps"></a>Seuraavat vaiheet

[Lataa Azure pinon Käsitteiden asennuspaketin](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Azure pinon Käsitteiden käyttöönotto](azure-stack-run-powershell-script.md)
