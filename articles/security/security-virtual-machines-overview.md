<properties
   pageTitle="Azuren näennäiskoneiden suojauksen yleiskatsaus | Microsoft Azure"
   description=" Azure-Virtuaalikoneissa antaa sinulle virtualization joustavuutta eikä sinun tarvitse ostaa ja ylläpitää fyysinen laitteisto, joka suoritetaan virtuaalikoneen.  Tässä artikkelissa on yleiskatsaus tärkeä Azure suojausominaisuuksia, jotka voidaan käyttää Azuren näennäiskoneiden kanssa. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-virtual-machines-security-overview"></a>Azuren näennäiskoneiden suojauksen yleiskatsaus

Azure-Virtuaalikoneissa voit ottaa käyttöön monien tietojenkäsittely ratkaisuja joustava tavalla. Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP ja Azure BizTalk palvelut-tuen minkä tahansa työmäärää ja minkä tahansa kielen lähes minkä tahansa käyttöjärjestelmä käyttöönotto.

Azure virtuaalikoneen lisää joustavuutta virtualization, eikä sinun tarvitse ostaa ja ylläpitää fyysinen laitteisto, joka suoritetaan virtuaalikoneen.  Voit muodosta ja ota käyttöön ja varmistaa, että tiedot on suojattu ja tutustu suojatuissa palvelinkeskusten turvassa sovellukset.

Azure, jossa voit luoda suojatun, yhteensopiva ratkaisuja, jotka:

- Suojaa oman näennäiskoneiden viruksilta ja haittaohjelmilta
- Salaa luottamukselliset tiedot
- Verkon tietoliikenteen
- Tunnistaa ja uhkien tunnistaminen
- Vaatimusten noudattaminen

Tässä artikkelissa on antamaan yleiskatsaus tärkeä Azure suojausominaisuuksia, jotka voidaan käyttää näennäiskoneiden. Annamme myös linkkejä artikkeleihin, joissa kuvailla kullekin ominaisuudelle, joten voit lukea lisää.  

Tässä artikkelissa kattama core Azure virtuaalikoneen suojaus-ominaisuuksia:

- Antimalware
- Laitteiston suojaus-moduuli
- Virtuaalikoneen salauksen
- Virtuaalikoneen varmuuskopiointi
- Azure sivuston palauttaminen
- Virtuaalinen verkko
- Suojauksen hallinta ja raportointi
- Yhteensopivuus

## <a name="antimalware"></a>Antimalware

Azure-voit suojata oman näennäiskoneiden haittaohjelmien tiedostoja, mainos ja muut uhkien antimalware ohjelmiston suojauksen toimittajista, kuten Microsoft, Symantec, trendi mikro, McAfee ja Kaspersky avulla. Katso lisätietoja jäljempänä olevaan kohtaan Etsi kumppani artikkeleita ratkaisuja.

Microsoft Antimalware Azure pilvipalveluihin ja näennäiskoneiden on reaaliaikainen ominaisuus, joka auttaa tunnistamaan ja poista viruksilta, vakoiluohjelmilta ja muilta haittaohjelmilta.  Microsoft Antimalware on määritettävä ilmoitukset, kun tunnetut haittaohjelmien tai ei-toivottu ohjelma yrittää asentaa tai suorittaa Azure-järjestelmiin.

Microsoft Antimalware on yksi edustaja ratkaisu sovellusten ja vuokraajan ympäristöissä, joka on suunniteltu toimimaan ilman ihmisen taustalla. Voit ottaa käyttöön sovelluksen toiminnoista, kanssa joko basic suojatun--oletusarvoisesti tarpeiden mukaan tai advanced mukautettu konfigurointi, mukaan lukien antimalware seuranta.

Kun otetaan käyttöön ja ota käyttöön Microsoft Antimalware, core seuraavat toiminnot ovat käytettävissä:

- Reaaliaikainen - näytöt tehtävän pilvipalveluihin ja näennäiskoneiden tunnistaa ja estää haittaohjelmien suorittamisen.
- Ajoitettu scanning - suorittaa säännöllisesti kohdennettujen scanning esiintyvien haittaohjelmia, kuten aktiivisesti käynnissä olevat ohjelmat.
- Haittaohjelmien korjaus - kestää toiminnon automaattisesti olemassa haittaohjelmia, kuten poistaminen tai ehkäisytoimenpiteet haittaohjelmien tiedostoja ja Siivotaan haittaohjelmien rekisterimerkinnät.
- Allekirjoituksen päivitykset – automaattisesti asentaa uusimman suojaus allekirjoitukset (virus määritelmät) suojaamisen on ajan tasalla ennalta korkojakso.
- Antimalware ohjelma päivitykset – automaattisesti päivittää Microsoft Antimalware-ohjelma.
- Antimalware ympäristö päivitykset – automaattisesti päivittää Microsoft Antimalware-ympäristössä.
- Aktiivinen suojaus - raportteja Azure telemetriatietojen metatietojen olemassa uhkien ja epäilyttävistä resurssien varmistaakseen nopean vastauksen ja reaaliaikaisen synkronisen allekirjoituksen toimittamisen – Microsoft Active suojaus järjestelmän (KARTAT).
- Esimerkit raportointi - tarjoaa ja raportoi näytteiden palvelu ja ota käyttöön vianmäärityksen Microsoft Antimalware-palveluun.
- Poikkeukset – avulla voit määrittää tiettyjen tiedostojen prosessit-sovelluksen ja palvelun Järjestelmänvalvojat ja ohjaa ne pois suojaus ja skannaaminen suorituskykyä ja muista syistä.
- Antimalware tapahtuman sivustokokoelman - tallentaa antimalware palvelun kuntoa, epäilyttävistä toiminnot ja käyttöjärjestelmän tapahtumalokiin korjaus toimia ja kerää ne asiakkaan Azuren tallennustilaan huomioon.

Lue lisää: Lisätietoja antimalware ohjelmiston suojaaminen oman näennäiskoneiden on artikkelissa:

- [Microsoft Azure pilvipalveluihin ja näennäiskoneiden Antimalware](../security/azure-security-antimalware.md)
- [Azuren näennäiskoneiden ratkaisujen Antimalware käyttöönotto](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Asentaa ja määrittää trendi mikro laaja suojauksen palveluna Windows AM](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Asentaminen ja määrittäminen Windows-AM Symantec Endpoint Protection](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Uudet Antimalware vaihtoehdot Azuren näennäiskoneiden – McAfee Endpoint Protection suojaaminen](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)
- [Security-ratkaisujen Azure Marketplacesta](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Laitteiston suojauksen moduuli

Salaus- ja parantaa suojaus, ellei itse näppäimet on suojattu. Hallinta ja tärkeitä tietoja ja näppäimet suojaus voidaan yksinkertaistaa tallentamalla ne Azure avaimen säilö. Avaimen säilö on asetus, jos haluat tallentaa avaimien laitteiston suojauksen moduuleissa (HSMs) oikeus FIPS 140-2 tason 2 mukaisia. SQL Server salausavaimet varmuuskopiointi-ja [Läpinäkyvä salauksen](https://msdn.microsoft.com/library/bb934049.aspx) kaikki voidaan tallentaa avaimen säilöön tai avaimet tai tietoja-sovellusten kanssa. Käyttöoikeudet ja suojatun tällaisia pääsy hallitaan [Azure Active Directoryn](https://azure.microsoft.com/documentation/services/active-directory/)kautta.

Opi lisää:

- [Mikä on Azure avaimen säilö?](../key-vault/key-vault-whatis.md)
- [Azure avaimen säilö käytön aloittaminen](../key-vault/key-vault-get-started.md)
- [Azure avain säilöön-blogi](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Virtuaalikoneen salauksen

Azure salauksen on uusi ominaisuus, jonka avulla voit salata Windows ja Linux Azure virtuaalikoneen levyjen. Azure salauksen käyttää alan [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) -Normaali ominaisuus, Windowsin ja Linux [dm crypt](https://en.wikipedia.org/wiki/Dm-crypt) -ominaisuus säätää äänenvoimakkuutta salaus käyttöjärjestelmän ja tietoja-levyjä.

Ratkaisu on integroitu Azure avaimen säilö avulla voit hallita ja levyn salausavaimet ja tietoja avaimen säilö tilaukseesi, ja varmistaa, että, että kaikki tiedot virtuaalikoneen kohtauksen salataan loput Azure-tallennustilan hallinta.

Opi lisää:

- [Windows-ja Linux IaaS VMs Azure salauksen](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
- [Linux ja näennäiskoneiden Windows Azure salauksen](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
- [Salaa virtual machine](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Virtuaalikoneen varmuuskopiointi

Azure varmuuskopio on skaalattava ratkaisu, joka suojaa sovelluksen tietojen nolla pääoman sijoitus ja mahdollisimman vähän kustannuksia. Sovellusvirheet voit vioittunut tiedot ja ihmisten virheet voivat esitellä virheet sovellustesi kyselyjä. Azure varmuuskopioimalla näennäiskoneiden oman Windows- ja Linux on suojattu.

Opi lisää:

- [Azure varmuuskopioinnin-ominaisuudet](../backup/backup-introduction-to-azure-backup.md)
- [Azure varmuuskopion Oppimispolku](https://azure.microsoft.com/documentation/learning-paths/backup/)
- [Azure varmuuskopion palvelu - usein kysytyt kysymykset](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure sivuston palauttaminen

Organisaation BCDR strategia tärkeä osa, kuinka voit seurata yrityksen toiminnoista ja sovellusten käyttäminen ja suunnitellut ja suunnittelemattomat katkokset toteutuessa käynnissä. Azure palauttaminen auttaa orchestrate replikoinnin ja toiminnoista ja sovellukset automaattisesti niin, että ne ovat käytettävissä toissijainen sijainnista, jos ensisijainen sijainti siirtyy.

Sivuston palauttaminen:

- **Simplifies BCDR strategia** , sivuston palautus on helppo käsittelemään replikoinnin automaattisesti ja useiden liiketoiminnan toiminnoista ja sovellusten palauttamista yhdestä paikasta. Sivuston palauttaminen orchestrates replikointi ja automaattisesti, mutta ei leikkauspiste-sovelluksen tietojen tai ole mitään tietoja.
- **Sisältävät joustavia replikoinnin** – sivuston palauttaminen avulla voit toistaa työmääriä Hyper-V näennäiskoneiden, VMware näennäiskoneiden ja Windows-tai Linux fyysiset palvelimet.
- **Tukee automaattisesti ja hyödyntämistä** – sivuston palautus on testi failovers tukemaan vaikuttamatta tuotannon ympäristöissä tietojen palauttaminen työkaluja. Voit suorittaa suunnitellun failovers odotettu katkokset tappion nolla-tietojen kanssa tai suunnittelematon failovers mahdollisimman vähän tietojen menettämisen (sen mukaan, replikoinnin taajuus) odottamattomia Luonnonvahinkoja koskevat kanssa. Automaattisesti, kun voit tuntisesta ensisijainen sivustoihin. Sivuston palautus on palautus-Palvelupaketit, joihin voit sisältyy komentosarjojen ja Azure automaatio työkirjojen niin, että voit mukauttaa automaattisesti ja monitasoisten sovellusten palauttaminen.
- **Eliminates toissijainen palvelinkeskukseen** , voit toistaa toissijainen paikallisen sivuston tai Azure. Kustannus- ja monimutkaisuudesta toissijaisen sivuston ylläpito käyttämällä Azure kohteeksi palauttaminen jättää pois. Replikoitua tiedot tallennetaan Azuren tallennustilaan.
- **Olemassa olevan BCDR-tekniikoiden kanssa on liitetty** – palauttaminen kumppaneille ja muiden sovellusten BCDR ominaisuudet. Sivuston palauttaminen avulla voit suojata SQL Server yrityksen työmääriä takaisin loppuun. Tämä sisältää SQL Server-AlwaysOn hallittavan käytettävyys ryhmien vikasietotilaa tuki.

Opi lisää:

- [Mikä on Azure palauttaminen?](../site-recovery/site-recovery-overview.md)
- [Miten Azure palauttaminen toimii?](../site-recovery/site-recovery-components.md)
- [Mitä toiminnoista on suojattu Azure palauttaminen?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuaalinen verkko

Näennäiskoneiden on verkkoyhteys. Vaatimus tukemaan Azure edellyttää näennäiskoneiden on muodostettava yhteys Virtual Azure-verkkoon. Virtuaalinen Azure-verkko on looginen rakenteen laadittuihin fyysinen Azure verkko-kangasta päälle. Kunkin loogisen Azure Virtual verkon eristetään kaikki muut Azure Virtual verkot. Tämä eristystaso avulla voit varmistaa, että verkkoliikennettä käyttöönoton ei ole käytettävissä muissa Microsoft Azure-asiakkaille.

Opi lisää:

- [Azure verkon suojauksen yleiskatsaus](security-network-overview.md)
- [VPN-yleiskatsaus](../virtual-network/virtual-networks-overview.md)
- [Verkko-ominaisuuksia ja paranee sektorin](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Suojauksen hallinta ja raportointi

Azure Tietoturvakeskus auttaa estämään, haku- ja uhkien vastaaminen ja on lisätty näkyvyys- ja päättää, Azure resurssien suojaus. Siinä on integroitu suojaus seuranta ja käytäntöjen hallinta Azure-tilauksissa, auttaa tunnistamaan uhkien, jotka muuten huomaamatta ja laaja valikoima security-ratkaisujen mediatyökaluja toimii.

Azure Tietoturvakeskus avulla voit optimoida ja seurata virtuaalikoneen suojausta:

- Antamisen virtuaalikoneen [suojausta koskevia suosituksia](../security-center/security-center-recommendations.md) kuten järjestelmän päivitysten, määrittää käyttöoikeusluettelot päätepisteet, käyttöön antimalware, verkon käyttöoikeusryhmät ottaminen käyttöön ja käyttää salauksen.
- Oman näennäiskoneiden tilan seuranta

Opi lisää:

- [Johdanto Azure Tietoturvakeskus](../security-center/security-center-intro.md)
- [Azure Tietoturvakeskus usein kysytyt kysymykset](../security-center/security-center-faq.md)
- [Azure Tietoturvakeskus suunnittelu ja toiminnot](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Yhteensopivuus

Azuren näennäiskoneiden on sertifioitu fisma –, FedRAMP, HIPAA, PCI DSS tason 1 ja avaimen yhteensopivuuden muissa ohjelmissa. Tämän sertifikaatin on helppo vaatimukset täyttävän oman Azure-sovellusten ja yrityksen osoite ja lainmukaisia monien.

Opi lisää:

- [Microsoft Valvontakeskus: vaatimustenmukaisuus](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
- [Luotettu Cloud: Microsoft Azure tietoturva, tietosuoja ja vaatimustenmukaisuus](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
