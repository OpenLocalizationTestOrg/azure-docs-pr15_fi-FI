<properties 
   pageTitle="Epäonnistua takaisin VMware näennäiskoneiden ja paikallisen sivuston fyysinen palvelinten | Microsoft Azure"
   description="Tietoja epäonnistumista VMware VMs automaattisesti jälkeen takaisin paikalliseen-sivustoon ja Azure fyysinen palvelinten." 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required" 
   ms.date="08/22/2016"
   ms.author="ruturajd"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Edellinen VMware näennäiskoneiden ja paikallisen sivuston fyysinen palvelinten epäonnistuu

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-failback-azure-to-vmware.md)
- [Azure perinteinen Portal](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure perinteinen Portal (vanha)](site-recovery-failback-azure-to-vmware-classic-legacy.md)



Tässä artikkelissa kerrotaan, miten voi epäonnistua takaisin Azuren näennäiskoneiden azuren paikallisen-sivustoon. Noudata tämän artikkelin ohjeita, kun olet valmis epäonnistuu takaisin VMware näennäiskoneiden tai Windows-tai Linux fyysiset palvelimet sen jälkeen, kun ne on epäonnistui päälle tiloista sivuston käyttäminen tässä [opetusohjelmassa](site-recovery-vmware-to-azure-classic.md)Azure.



## <a name="overview"></a>Yleiskatsaus

Tässä kaaviossa on esitetty tässä skenaariossa tuntisesta arkkitehtuuri.

Käytä tätä arkkitehtuuri, kun prosessi-palvelin on paikallisen ja käytössäsi on ExpressRoute.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Käytä tätä arkkitehtuuri, kun prosessi-palvelin ei ole Azure ja sinulla on VPN- tai ExpressRoute yhteys.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Saat näkyviin täydellisen luettelon portit ja tuntisesta architechture kaavion viitata alla olevassa kuvassa

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Näin tuntisesta toiminnasta:

- Kun olet epäonnistui päälle Azure-asiakas ei takaisin paikalliseen sivustoon muutaman vaiheittain:
    - **Vaihe 1**: Azure VMs reprotect niin, että ne alkavat replikoiminen takaisin VMware virtuaalilaitteiksi paikallisen sivuston. Ottaminen käyttöön reprotection siirtyy AM tuntisesta suojaus-ryhmä, joka on luotu automaattisesti, kun alun perin luotu automaattisesti suojaus-ryhmä. Jos olet lisännyt suunnitelman automaattisesti myös lisätään automaattisesti suojaus-ryhmä palautus-palvelupakettia sitten tuntisesta suojaus-ryhmä.  Voit määrittää, miten aiot epäonnistua reprotect aikana takaisin.
    - **Vaihe 2**: Azure VMs ovat replikoiminen paikallisen sivustoon, kun suoritat epäonnistumisen takaisin Azure-epäonnistuu.
    - **Vaihe 3**: tietojen päivitys epäonnistuu takaisin, kun olet reprotect paikallisen VMs, joka epäonnistui takaisin, niin, että ne alkavat replikoiminen Azure.

> [AZURE.VIDEO enhanced-vmware-to-azure-failback]


### <a name="failback-to-the-original-or-alternate-location"></a>Tuntisesta alkuperäisen tai vaihtoehtoinen sijaintiin

Jos epäonnistui VMware AM kautta takaisin samasta lähteestä AM saattaa epäonnistua, jos se on edelleen olemassa paikallisen. Tässä skenaariossa vain delta muutokset epäonnistuu takaisin. Huomaa, että:

- Jos fyysinen palvelimissa epäonnistui sitten tuntisesta on aina uuden VMware AM.
    - Huomaa, että ennen kaatuvat takaisin fyysinen machine
    - Suojattu fyysinen tietokoneen toimitetaan takaisin Virtual machine epäonnistunut päälle VMware takaisin Azure-
    - Varmista löytämään vähintään yksi perustyyli kohde palvelimen kanssa, johon haluat tuntisesta tarvittavat ESX/ESXi-isännät.
- Jos takaisin alkuperäiseen AM epäonnistua seuraavista tarvitaan:
    - Jos AM hallitsee vCenter palvelimen perustyyli kohde ESX host pitäisi olla VMs datastore käyttöoikeus.
    - Jos AM on ESX isännän, mutta se ei ole käytössä vCenter AM kovalevy on oltava käytettävissä datastore MT isäntä.
    - Jos yhteyttä AM on ESX isännän ja ei käytä vCenter ESX isäntä MT etsiminen on suoritettava ennen kuin voit reprotect. Tämä koskee, jos olet kaatuvat takaisin fyysiset palvelimet liian.
    - Toinen vaihtoehto (Jos on olemassa paikallisen AM) on poistettava ennen tuntisesta. Valitse tuntisesta Luo uusi AM sama kuin perusmuodon kohde ESX isäntä isännän.
    
- Kun olet tuntisesta vaihtoehtoiseen sijaintiin tiedot voidaan palauttaa saman datastore ja saman ESX host, jota paikallisen perusmuodon kohde-palvelin. 


## <a name="prerequisites"></a>Edellytykset

- Sinun on VMware ympäristön jotta epäonnistua takaisin VMware VMs ja fyysinen palvelimiin. Palaa fyysisiä palvelimen epäonnistumista ei tueta.
- Jotta epäonnistua takaisin olisi luomasi Azure verkon silloin, kun suojaus. Tuntisesta tarvitsee VPN- tai ExpressRoute yhteyden Azure verkosta, jossa Azure VMs sijaitsevat paikallisen-sivustoon.
- Jos VMs haluat takaisin hallitaan vCenter-palvelin, sinun on Varmista, että sinulla on VMs etsiminen vCenter palvelimissa tarvittavia oikeuksia. [Lue lisää](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
- Jos tilannevedoksia on AM reprotection epäonnistuu. Voit poistaa tilannevedosten tai levyjä. 
- Ennen epäonnistua takaisin sinun täytyy luoda joukon osat:
    - **Luo prosessi-server Azure-tietokannassa**. Tämä on Azure-AM, jota tarvitaan luominen ja säilyttää suorittaminen tuntisesta aikana. Voit poistaa tietokoneen tuntisesta päätyttyä.
    - **Luo pää kohdepalvelin**: perustyylin palvelimesta lähettää ja vastaanottaa tuntisesta tietoja. Asiakirjanhallinnan palvelimeen, voit luoda paikallisen on asennettu oletusarvoisesti perustyylin kohde palvelimeen. Kuitenkin epäonnistui takaisin liikenne äänenvoimakkuuden mukaan voit joutua luomaan tuntisesta kohdepalvelin, joka erillisessä perustyylin.
    - Jos haluat luoda Linux käytössä muita perusmuodon kohde-palvelimeen, tarvitset määrittämään Linux AM ennen kuin asennat perusmuodon kohdepalvelimen seuraavalla tavalla.
- Määrityspalvelimen on pakollinen paikallisen, kun teet tuntisesta. Aikana tuntisesta virtuaalikoneen on oltava määritys server-tietokantaan, kaatuvat mitä tuntisesta ei onnistu. Varmista näin ollen tehdä säännöllisesti ajoitetun varmuuskopioinnin Serverin. Jos kyseessä on huono sinun on voit palauttaa sen samaa IP-osoitetta, jotta tuntisesta toimivat oikein.

## <a name="set-up-the-process-server-in-azure"></a>Azure prosessi-palvelimen määrittäminen

Sinun täytyy asentaa prosessin server Azure, niin, että Azure VMs voit lähettää tiedot takaisin paikalliseen perusmuodon kohde palvelimeen. 

1.  Sivuston palauttaminen portaalissa > **Configuration Servers** valitsemalla Lisää uusi prosessi-palvelin.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)

2.  Prosessin palvelimen nimi ja kirjoita nimi ja salasana käytetään yhdistäminen Azure-AM järjestelmänvalvojana. Valitse **Määrityspalvelimen** paikallisen asiakirjanhallinnan palvelimeen ja määrittää Azure verkon, jonka prosessin palvelimen käyttöön. Tämä on oltava verkko, jossa Azure VMs sijaitsevat. Määritä yksilöllinen IP-osoite, valitse aliverkosta ja aloita käyttöönotto.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

    Työn prosessin Serverin käyttöönotto käynnistyy

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

    Kun olet kirjautunut sen avulla voit määritetyt tunnistetiedot Azure prosessi-palvelin on otettu käyttöön. Suorittaa valintaikkunan prosessin palvelimen kirjaudut ensimmäisen kerran. Kirjoita IP-osoite on paikallinen hallinta-palvelin ja sen salasana. Jätä porttiin 443 oletusasetusta. Oletusarvo voit myös jättää tietojen replikoinnin 9443 portti paitsi muokkaamasi asetusta erikseen, kun määrität paikallisen asiakirjanhallinnan palvelimeen. 

    >[AZURE.NOTE] Palvelin ei ole näkyvissä **AM ominaisuudet**-kohdasta. Se on näkyvissä vain **palvelimet** -välilehden hallinta-palvelin, johon se on rekisteröity. Tietoja voi kestää 10-15 minuuttia prosessin palvelimen näkyvän.


## <a name="set-up-the-master-target-server-on-premises"></a>Määritä perustyylin kohde palvelimen paikallisen

Perusmuodon kohdepalvelin vastaanottaa tuntisesta tiedot. Perusmuodon kohdepalvelin asennetaan automaattisesti paikallisen asiakirjanhallinnan palvelimeen, mutta jos sinun on viallinen takaisin paljon tietoja sinun on ehkä muita perusmuodon kohdepalvelimen määrittäminen. Tehdä seuraavasti:

>[AZURE.NOTE] Jos haluat asentaa perustyylin kohdepalvelimen Linux seuraavaan vaiheeseen ohjeiden mukaisesti.

1. Jos asennat Windows perusmuodon palvelimesta, Avaa AM kohdalle, johon olet asentamassa perusmuodon kohdepalvelin ja lataa asennustiedosto Azure sivuston palautus-yhdistetyn ohjatun Pika-aloitus-sivulla.
2. Suorita asennus ja valitse **ennen kuin aloitat** **Lisää prosessit-palvelinten skaalata käyttöönoton ulos**.
3. Suorita ohjattu teit milloin samalla tavalla voit [määrittää asiakirjanhallinnan palvelimeen](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). Määritä IP-osoitteen perusmuodon kohde tähän palvelimeen ja salasana, voit käyttää AM **Määritysten palvelimen tiedot** -sivulla.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Määritetty Linux AM perusmuodon palvelimesta
Voit määrittää hallinta-palvelimen kuin Linux-AM, sinun on asennettava prosenttia perustyylin kohdepalvelin) S 6.6 mahdollisimman vähän käyttöjärjestelmässä, noutaa SCSI kullakin kiintolevyllä SCSI-tunnukset, asentaa joitakin muita paketteja ja käyttää mukautettuja muutoksia.

#### <a name="install-centos-66"></a>Asenna CentOS 6.6

1.  Asenna CentOS 6.6 mahdollisimman vähän käyttöjärjestelmä hallinta palvelimessa AM. Säilytä ISO DVD-asemaan ja Käynnistä järjestelmä. Ohita media testaus, valitse US englannin kielen, valitse **Perus tallennuslaitteet**, tarkista, että kiintolevyä ei ole tärkeitä tietoja ja valitse **Kyllä**, Hylkää kaikki tiedot. Kirjoita isäntänimi asiakirjanhallinnan palvelimeen, ja valitse server verkkosovittimen.  Valitse **Järjestelmän muokkaaminen** -valintaikkunan** muodostaa automaattisesti** ja lisää staattinen IP-osoite, verkon ja DNS-asetukset. Määritä aikavyöhykkeen ja pääkansion salasanan hallinta-palvelinta. 
2.  Kysyttäessä asennustapa haluat valita **Mukautetun asettelun luominen** osioksi. Kun valitset **Seuraava** Valitse **Vapauta** ja sitten Luo. Luo **/**, **/var/crash** ja **/ home osioiden** kanssa **FS tyyppi:** **ext4**. Luo kuin Vaihda-partion **FS tyyppi: Vaihda**.
3.  Jos olemassa laitteiden löytyy varoitussanoma tulee näkyviin. Valitse **Muotoile** alusta asemaa osio-asetuksilla. Valitse osion muutokset **kirjoittaa muuta levylle** .
4.  Valitse **Asenna käynnistyksessä** > **seuraavan** asentaa käynnistyksessä päämyöntäjien-osioon.
5.  Kun asennus on valmis, valitse **Käynnistä tietokone uudelleen**.


#### <a name="retrieve-the-scsi-ids"></a>Noutaa SCSI-tunnukset

1. Asennuksen jälkeen noutaa AM SCSI kunkin kiintolevy SCSI tunnukset. Voit tehdä tämän Sammuta asiakirjanhallinnan palvelimeen AM VMware AM-ominaisuuksiin kakkospainikkeella AM tapahtuma > **Muokkaa asetuksia** > **asetukset**.
2. Valitse **Lisäasetukset** > **Yleiset kohde** ja valitse **Määrityksiä**. Tämä vaihtoehto ole de-active, kun laite on käynnissä. Voit aktivoida sen koneen on suljettava.
3. Jos rivi **levy. EnableUUID** olemassa Varmista, että arvo on **Tosi** (kirjainkoko on merkitsevä). Jos se on jo, voit peruuttaa ja testaa sisällä vieras-käyttöjärjestelmän SCSI-komento, kun se käynnistetään. 
4.  Jos rivi ei ole olemassa **Lisää rivi** – ja lisää se arvo **Tosi** . Älä käytä lainausmerkkejä.

#### <a name="install-additional-packages"></a>Asenna muita paketit

Sinun on ladattava ja asennettava joitakin muita paketit. 

1.  Varmista, että perusmuodon kohdepalvelin on yhteydessä Internetiin.
2.  Suorita tämä komento ladattava ja asennettava 15 pakettien CentOS säilöstä: **# yum – y xfsprogs perl lsscsi rsync wget kexec-työkalut asennetaan**.
3.  Jos lähde-tietokoneissa on suojaaminen käynnissä olevat Linux käyttäjälle Reiser tai XFS tiedostojärjestelmän pääkansion tai käynnistys-laitteen kannattaa ladata ja asentaa muita paketteja seuraavasti:

    - # <a name="cd-usrlocal"></a>CD-levylle /usr/local
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
    - # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>jälleenmyyntihinnan – ivh kmod-reiserfs-0,0-1.el6.elrepo.x86_64.rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm
    - # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
    - # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>jälleenmyyntihinnan – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Mukautetun muutokset

Tee seuraavat muutosten mukautetun sen jälkeen, kun olet vaiheiden asennuksen jälkeiset ja asennettu paketit:

1.  Kopioi RHEL 6 64 yhdistetyn agentti binaarinen AM. Suorita tämä komento untar binaaritiedosto: **Kohdekeh – zxvf <file name> **
2.  Suorita tämä komento ja anna käyttöoikeudet: **# chmod 755./ApplyCustomChanges.sh**
3.  Suorita komentosarja: **#./ApplyCustomChanges.sh**. Komentosarja on suoritettava vain kerran. Käynnistä palvelin uudelleen, kun komentosarja on suoritettu onnistuneesti.


## <a name="run-the-failback"></a>Suorita tuntisesta

### <a name="reprotect-the-azure-vms"></a>Reprotect Azure VMs

1.  Sivuston palauttaminen portaalissa > **koneet** -välilehdessä Valitse AM, joka on epäonnistui päälle ja valitse sitten **Suojaa uudelleen**.
2.  Valitse **Perustyyli kohde-palvelin** ja **Prosessin palvelin** on paikallinen perusmuodon kohde-palvelin ja Azure AM prosessin palvelimen.
3.  Valitse tili AM muodostamisesta määritetty.
4.  Valitse Suojaus-ryhmä tuntisesta versio. Jos esimerkiksi jos AM suojataan PG1, sinun on valita PG1_Failback.
5.  Jos haluat palauttaa vaihtoehtoiseen sijaintiin, valitse säilytys-asema ja datastore määritetty perusmuodon palvelimesta. Kun olet epäonnistua takaisin paikalliseen sivustoon VMware VMs tuntisesta suojaus-suunnitelmassa käyttää samaa datastore perusmuodon palvelimesta. Jos haluat palauttaa se Azure AM samaan paikalliset AM sitten paikallisen AM pitäisi olla jo saman datastore perusmuodon kohde-palvelimeksi. Jos ei ole AM paikallisen uuden luodaan reprotection aikana.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)

6.  Kun valitset **OK** , jos haluat aloittaa reprotection työn alkaa replikoida AM azuren paikallisen sivuston. Voit seurata edistymistä **työt** -välilehden.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-to-the-on-premises-site"></a>Suorita paikallisen sivuston automaattisesti

Reprotection jälkeen AM siirretään tuntisesta version sen suojaus-ryhmä ja lisätään automaattisesti käytit vikasietotilaa Azure, jos sellainen on palautus suunnitelma.

1.  Palautus-palvelupaketti, sisältävä tuntisesta ryhmän **Palauttaminen suunnitelmien** -sivulla ja valitse sitten **automaattisesti** > **Suunnittelematon automaattisesti**.
2.  Tarkista **Vahvista automaattisesti** automaattisesti suunnan (Azure) ja valitse palautus-kohtaan haluat käyttää vikasietotilaa (uusin). Jos **Usean AM** otettu käyttöön, kun olet määrittänyt replikoinnin ominaisuudet voit palauttaa sen uusin sovelluksen tai kaatumisen yhdenmukaisia palautuspiste. Valitse Aloita vikasietotilaa valintamerkkiä.
3.  Azure-VMs suljetaan palauttaminen aikana automaattisesti. Kun olet tarkistanut, että tuntisesta on päättynyt odotetulla tavalla voit voit voit tarkistaa, että Azure VMs on suljettu odotetulla tavalla.

### <a name="reprotect-the--on-premises-site"></a>Paikallisen sivuston reprotect

Tuntisesta päätyttyä tietosi ovat takaisin paikalliseen sivustossa, mutta ei voi suojata. Aloita replikoiminen Azure uudelleen seuraavasti:

1.  Sivuston palauttaminen portaalissa > **koneet** välilehden Valitse VMs, jotka eivät ole läpäisseet takaisin ja valitse sitten **Suojaa uudelleen**. 
2.  Kun olet varmistanut, että replikoinnin Azure toimii oikein, voit poistaa Azure-VMs (ei ole käynnissä) Azure, takaisin epäonnistui.


### <a name="common-issues-in-failback"></a>Tuntisesta yleisiä ongelmia

1. Jos suoritat vain luku-käyttäjän vCenter etsiminen ja suojata näennäiskoneiden, se onnistuu ja automaattisesti toimii. Reprotect aikaa se ei onnistu datastores ei voi etsiä, koska. Ongelma kuin et näe luettelossa aikana uudelleen suojaaminen datastores. Voit ratkaista tämän Päivitä vCenter tunnistetieto haluamasi tili, jolla on oikeudet ja yritä työn. [Lisätietoja on artikkelissa](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
2. Kun tuntisesta Linux AM ja suorita paikallinen, näet, että verkon hallinta-paketin asennus poistetaan tietokoneesta. Tämä johtuu siitä AM hyödynnetään Azure-verkon hallinta-paketin poistetaan.
3. Kun AM on määritetty staattinen IP-osoite ja on epäonnistui Azure päälle, IP-osoite on hankittu DHCP kautta. Kun paikallinen takaisin päälle, AM säilyy IP-osoitteen DHCP avulla. Sinun on manuaalisesti kirjautumista machine ja Määritä IP-osoite takaisin staattinen osoite tarvittaessa.
4. Jos käytössäsi on ilmainen Editionin ESXi 5.5 tai vSphere 6 Hypervisor vapaa-version, toiminto onnistuu automaattisesti, mutta tuntisesta ei onnistu. Se ned päivittämään tuntisesta käyttöön joko laskenta-käyttöoikeutta.

## <a name="failing-back-with-expressroute"></a>Kaatuvat Edellinen ExpressRoute kanssa

Voi epäonnistua takaisin VPN-yhteyden tai Azure ExpressRoute päälle. Jos haluat käyttää ExpressRoute huomautuksen seuraavasti:

- ExpressRoute on määritettävä mitä lähteeseen koneet virheiden päälle ja mitkä Azure VMs sijaitsevat jälkeen vikasietotilaa Azure virtual verkossa.
- Tietojen replikoinnin Azure-tallennustilan tilin julkisen päätepisteen. Voit määrittää tulee julkinen peering ExpressRoute-kohteen tietokeskuksen palauttaminen replikoinnin käyttämään ExpressRoute kanssa.



