<properties
    pageTitle="Vahvistettava Linux jaot | Microsoft Azure"
    description="Lisätietoja Linux-ohjeiden mukaan lukien Ubuntu, OpenLogic tai Oracle SUSE Azure vahvistettava jaot."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager"
    />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="szark"/>



#<a name="linux-on-azure-endorsed-distributions"></a>Valitse Azure vahvistettava jaot Linux

> [AZURE.NOTE] Jos sinulla on hetkeksi, Auta meitä kehittämään Azure Linux AM ohjeista tehokkaasta tämän oman kokemukset [nopeasti kyselyn](https://aka.ms/linuxdocsurvey) . Jokaisen vastauksen auttaa meitä ohjeen saat tekemäsi työt.

Azure-valikoiman tai Marketplace Linux-kuvia annetaan luvulla kumppaneita ja yritämme eri Linux yhteisöjen paremmin flavors lisääminen vahvistettava jakeluluettelon kanssa. Tässä vaiheessa jaot valikoimasta ei ole käytettävissä, voit aina tuo-Your-omistaja-Linux [tällä](virtual-machines-linux-classic-create-upload-vhd.md)sivulla ohjeiden mukaisesti.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="supported-distributions--versions"></a>Tuetut jakelu ja versiot ##

Seuraavassa taulukossa on lueteltu Linux jakelu ja versiot, joita tuetaan Azure. Myös Tutustu [Microsoft Azure Linux kuvat tuki](https://support.microsoft.com/en-us/kb/2941892) yksityiskohtaisempia tietoja.

Hyper-V ja Azure Linux Integration Services (LIS) ohjaimia ei ydin moduulit, jotka Microsoft osaltaan suoraan ylöspäin Linux ydin.  LIS ohjaimet joko sisältyvät jakauman ydin oletusarvoisesti tai vanhempi RHEL/CentOS-pohjainen jaot ovat käytettävissä ladata erikseen [tähän](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409).  Katso [tämän artikkelin](virtual-machines-linux-create-upload-generic.md#linux-kernel-requirements) lisätietoja LIS-ohjaimet.

Azure Linux-agentti on jo valmiiksi asennettu Azure-valikoiman kuvia ja ovat yleensä käytettävissä jakauman paketin säilöstä.  Lähdekoodin löytyy [GitHub](https://github.com/azure/walinuxagent).

Jakauman.|Versio|Ohjaimet|Agentti
---|---|---|---
CentOS OpenLogic mukaan | CentOS 6.3 + 7.0 + | CentOS 6.3: [LIS lataaminen](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +:-Ytimessä | Paketin:-Kohdassa "WALinuxAgent" [OpenLogic repo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) <br/>Lähdekoodin: [GitHub](https://github.com/Azure/WALinuxAgent)
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) | 494.4.0+ | Valitse ydin | Lähdekoodin: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Debian | Debian 7.9 +, 8.2 + | Valitse ydin | Paketin:-kohdassa "waagent" repo <br/>Lähdekoodin: [GitHub](https://github.com/Azure/WALinuxAgent)
Oracle Linux | 6.4 +, 7.0 + | Valitse ydin | Paketin:-kohdassa "WALinuxAgent" repo <br/>Lähdekoodin: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Punainen on Enterprise Linux | RHEL 6,7 + 7.1 + | Valitse ydin|Paketin:-kohdassa "WALinuxAgent" repo <br/>Lähdekoodin: [GitHub](https://github.com/Azure/WALinuxAgent)
SUSE Linux Enterprisen | SLES 11 SP4, SLES 12 SP1 + ja <p> SLES for SAP 11 SP3 + | Valitse ydin | Paketin:-Kohdassa "python-azure-agentti" repo [Cloud: Työkalut](https://build.opensuse.org/project/show/Cloud:Tools) <br/>Lähdekoodin: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE | openSUSE 13.2 + | Valitse ydin | Paketin:-Kohdassa "python-azure-agentti" repo [Cloud: Työkalut](https://build.opensuse.org/project/show/Cloud:Tools) <br/>Lähdekoodin: [GitHub](https://github.com/Azure/WALinuxAgent)
Ubuntu|Ubuntu 12.04, 14.04, 16.04, 16.10 | Valitse ydin | Paketin:-kohdassa "walinuxagent" repo <br/>Lähdekoodin: [GitHub](https://github.com/Azure/WALinuxAgent)


## <a name="partners"></a>Kumppaneille

### <a name="openlogic"></a>OpenLogic
[http://www.openlogic.com/Azure](http://www.openlogic.com/azure)

OpenLogic on Avaa lähde yritysratkaisuja pilveen ja tietokeskuksen johtavan. OpenLogic auttaa satoja alussa yrityksen arvoalueella teollisuuden turvallisesti hankkia, tukevat ja hallita Avaa lähde-ohjelmisto. OpenLogic tarjoaa kaupallisen arvosanojen tekniseen tukeen ja korvausten palautettua OpenLogic-asiantuntija yhteisö, mukaan lukien yrityksen CentOS tason tuki sekä niiden julkaisun kumppanin tarjoamiseksi CentOS-pohjaisia kuvia Azure-600 Avaa lähde-paketit.

### <a name="coreos"></a>CoreOS
[https://coreos.com/Docs/Running-coreos/cloud-Providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

CoreOS-sivustosta:

*CoreOS on suunniteltu suojaus, yhdenmukaisuuden ja luotettavuutta. CoreOS käyttää sen sijaan, että paketteja yum tai piharakennus asennettaessa Linux säilöjen ylempänä artikkeli palvelujen hallinta. Yksittäisen palvelun koodin ja kaikki riippuvuudet pakattu säilö, joka voidaan suorittaa yhden tai usean CoreOS tietokoneissa.*


### <a name="credativ"></a>Credativ
[http://www.credativ.co.UK/credativ-blog/debian-Images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ on itsenäinen konsultointipalveluita ja palveluiden yrityksen erikoisjulkaisuihin kehittäminen ja ilmaisen ohjelmiston käytön professional ratkaisujen käyttöönoton. Credative on kuin ensimmäistä Avaa lähde asiantuntijoiden kansainväliset tunnistuksen monta niiden tukea avulla IT-osastoille. Yhdessä Microsoft Credativ on tällä hetkellä valmisteleminen vastaavan Debian kuvia Debian 8 (Jessie) ja Debian ennen 7 (Wheezy), joka on suunniteltu toimimaan Azure ja hallittavissa helposti platform kautta. Credativ tukee myös pitkällä aikavälillä ylläpito ja päivitys Debian kuvat – Avaa lähde tuki sen keskukset Azure varten.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/Topics/cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle's strategia on tarjota ratkaisuja julkisista ja yksityisistä paveikslėlis laajan projektiportfolioiden aikana asiakkaiden valinta ja miten ne ottaa Oracle ohjelmiston Oracle paveikslėlis sekä muita paveikslėlis joustavuutta.  Oracle's partnership Microsoft mahdollistaa asiakkaat voivat ottaa käyttöön Microsoft julkisista ja yksityisistä paveikslėlis sertifikaatin LUOTTAMUSVÄLI Oracle ohjelmiston ja Oracle-tukea.  Oracle's sitoumus ja Oracle julkisista ja yksityisistä cloud ratkaisuja sijoitus ei muutu.

### <a name="red-hat"></a>Punainen on
[http://www.redhat.com/en/partners/Strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Maailman johtavan Avaa lähde-ratkaisuja, punainen on auttaa yli 90 prosenttia Fortune 500 yritysten ratkaiseminen business haasteisiin, Tasaa niiden IT ja business strategioita ja valmisteleminen tekniikka tulevaisuudessa. Punainen on tekee tämän tarjoamalla turvallisten ratkaisujen – Avaa business-malli ja edullinen, ennakoitavissa tilaus-malli.

### <a name="suse"></a>SUSE
[http://www.sUSE.com/SuSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server Azure on osoitettu, joka sisältää luotettavuuden suojaus ja tietosuoja cloud tietojenkäsittely. SUSE's monipuolisia Linux-ympäristö on integroitu saumattomasti Azure pilvipalveluihin aikana helposti ohjattavan cloud-ympäristössä. Ja yli 9,200 sertifioitu sovellusten yli 1 800 riippumattomat toimittajien SUSE Linux Enterprise Serveriin, SUSE varmistaa, että käytössä tuetut tietokeskuksen työmääriä voit eri neljänneksien ottaa käyttöön Azure.

### <a name="canonical"></a>Kanoninen
[http://www.Ubuntu.com/cloud/Azure](http://www.ubuntu.com/cloud/azure)

Kanoninen tekniikka ja avoin yhteisö hallinnon asema Ubuntu onnistumista asiakkaan ja palvelimen käyttäminen, mukaan lukien Omat pilvipalveluihin kuluttajille cloud. Yhdistetty vapaa-ympäristö Ubuntu, puhelimessa cloud-puhelin, lehtiö-PC:, TV ja työpöydän johdonmukaisen liittymät joukkoon canonical's näkö on Ubuntu monipuolisen laitokset julkisen cloud tarjoajien kuluttaja electronics valmistajat ja suosikiksi kesken yksittäisiä technologists ensimmäinen vaihtoehto.

Kehittäjät ja eri puolilla maailmaa suunnitteluryhmät keskikohdan mukaan-Canonical sijoitetaan yksilöllisesti toimimaan laitteiston valmistajat, sisältöpalveluista ja ohjelmistokehittäjät tuomaan Ubuntu ratkaisuja markkinointi-palvelimiin ja kannettavia laitteita PC.

