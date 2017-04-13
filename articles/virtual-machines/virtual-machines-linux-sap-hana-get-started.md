<properties
   pageTitle="Pika-aloitusopas-Azure VMs SAP HANA manuaalinen asennus | Microsoft Azure"
   description="Pika-aloitusopas-Azure VMs SAP HANA manuaalinen asennus"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="quickstart-guide-for-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Manuaalinen asennus yhden esiintymän SAP HANA Azure VMs-pika-aloitusopas

## <a name="introduction"></a>Johdanto

Pikaopas tämän oppaan avulla voit määrittää yhden esiintymän SAP HANA prototyypin/esittely järjestelmän Azure VMs-manuaalisen asennuksen SAP NetWeaver 7.5 ja SAP HANA SP12.
Oppaan presumes lukuohjelma on tuttuja Azure IaaS perustoimintojen, kuten käyttöönottamisesta näennäiskoneiden tai virtual verkkojen joko Azure portal tai Powershell/CLI, mukaan lukien asetus, jos haluat käyttää json malleja. Lisäksi oletetaan, että lukija tuntee SAP HANA ja SAP NetWeaver asennusohjeet paikallisen.

Oletetaan, että lukija tuntee Yleiset SAP-Azure-asiakirjat, yleiset tiedot on artikkelin lopussa olevassa kohdassa.

Tuotannon järjestelmiä rajoitus vuoksi tässä oppaassa ei aiheet aiheet HA, kuten Varmuuskopioi DR, erinomainen suorituskyky tai erityinen suojausasiat.

Esimerkki asennus on valmis käyttämällä kahta näennäiskoneiden suorittamaan hajautetun SAP NetWeaver-asennettavissa Azure Resurssienhallinta-mallin kuin SAP-Linux-Azure tuetaan vain Azure Resurssienhallinta ja perinteinen malli. Lisätietoja Azure resurssin Managerin linkkejä löytyvät yleistiedot-osiossa on tämän artikkelin lopussa.

Nämä on kaksi VMs, malli-asennus:

* hana-appsrv (tyyppi DS3) isännöimiseen NW 7.5 ASCS esiintymän + PAS
* hana-dbsrv (tyyppi GS4) isäntään HANA SP12
* molemmat VMs kuulunut yhteen Azure virtual verkkoon (azure-hana-testi-vnet)
* Kummassakin tapauksessa OS on SLES 12 SP1


Ota huomioon siitä kuitenkin, että vuodesta heinäkuussa 2016 SAP HANA vain kokonaan tuetaan OLAP (Mustavalkoinen) tuotannon Systemsin Azure AM tyypin GS5. Testausta, jossa yksi ei odottaa virallinen SAP-tuki on hieno käyttää jotakin pienempi, kuten esimerkiksi GS4.
Mikä on aina käytettävä SAP HANA Azure-Azure premium HANA tietojen ja lokitiedostojen - tallennustila on ohjeaiheessa "levyn asetukset-osassa edelleen alas. Lisätietoja siitä, mitä SAP tuotteiden tuetaan Azure koskeva Tarkista yleistiedot-osiossa on tämän artikkelin lopussa.


Oppaassa kerrotaan SAP HANA asennetaan manuaalisesti Azure VMs kahdella eri tavalla:

* Asenna hajautettu NetWeaver asennuksen "tietokannan esiintymän" vaiheessa SAP HANA kautta SAP ohjelmiston valmistelu hallinta (SWPM)
* Asenna SAP HANA hdblcm HANA elinkaaren hallinta-työkalun avulla ja asenna sitten NetWeaver jälkeenpäin

On myös mahdollista SWPM ja asenna kaikki komponentit (SAP HANA, SAP sovelluspalvelin, ASCS esiintymän SAP Graafisen) yhden yksittäisen AM. Tämä vaihtoehto ei ole kuvattu tämän artikkelin mutta kohteet, jotka on otettava huomioon ovat samat.

Ennen kuin aloitat asennuksen jälkeen tarkistusluetteloiden Azure testi VMs määrittämisestä osan luetaan tapahtuvien basic virheiden joka tapahtuu, kun käytät vain Azure AM oletusasetukset.


## <a name="checklist-sap-hana-installation-via-sap-swpm"></a>Tarkistusluettelon SAP HANA asennettavissa SAP SWPM

Tämä on yksinkertaisen tarkistusluettelon manuaalista yhden esiintymän SAP HANA asennusta varten esittely tai prototyyppiä pursposes kautta SAP SWPM tekevät hajautettu SAP-NW 7.5 liittyviä kohteita asentaa avain. Yksittäisten kohteiden kuvaus ja näyttökuvat tarkemmin artikkelin koko lomakkeessa näkyvät:

* Azure virtual verkon, joka sisältää kaksi VMs luominen 
* ottaa käyttöön kaksi Azure VMs OS SLES 12 SP1: een kautta Azure resurssien hallinnan malli 
* liittää kaksi vakio tallennustilan levyä app-palvelin AM (kuten 75 gt ja 500 gt)
* Liitä neljä levyjen HANA DB palvelimeen AM - 2 vakio tallennustilan levyjä, kuten sovelluksen palvelimen AM + 2 premium-tallennustilan levyä (kuten 2x512GB)
* sen mukaan, koon ja/tai siirtonopeuden vaatimukset liittäminen useille levyille ja luo Raidallinen asemat joko käyttämällä lvm tai mdadm OS tasolla AM sisällä
* Luo XFS tiedoston varten liitetyn levyille / looginen tietomääristä
* ottaa käyttöön uuden XFS tiedostojärjestelmien OS tasolla. Yksi tiedostojärjestelmän avulla voit pitää kaikki SAP-ohjelmistojen ja toinen esimerkiksi sapmnt-hakemistosta ja ehkä varmuuskopiot. SAP-HANA DB-palvelimen asennustapa XFS tiedoston järjestelmien /hana ja tämä on kaikki tarvittavat välttämiseksi, joka ei ole liian suuri Linux Azure VMs-pääkansio tiedostojärjestelmän täyttyy /usr/sap premium tallennustilan-levyille
* Kirjoita/jne/isännät testin VMs paikallinen ip-osoitteet
* Kirjoita /etc/fstab nofail-parametri
* Määritä ydin parametrien HANA-SLES – 12 SAP-huomautus (Katso ydin paramters osan edelleen tiedot)
* Vaihda tilan
* Jos etsimäsi - asentaminen testi VMs graafinen työpöydän. Käytä muussa remote sapinst asentaminen
* SAP-ohjelmiston lataaminen SAP-palvelun Marketplacesta
* SAP-ASCS esiintymän asentaminen app-palvelin AM
* jakaa sapmnt hakemiston kautta NFS kesken testi VMs (app-palvelin AM on NFS-palvelin)
* Asenna tietokannan esiintymän, mukaan lukien HANA kautta SWPM DB-palvelimeen AM
* Asenna sovellus-palvelimeen AM PAS
* Käynnistä SAP MC ja muodosta esimerkiksi SAP Graafisen kautta / HANA Studio 



## <a name="checklist-sap-hana-installation-via-hdblcm"></a>Tarkistusluettelon SAP HANA asennettavissa hdblcm

Tämä on yksinkertaisen tarkistusluettelon manuaalista yhden esiintymän SAP HANA asennusta varten esittely tai prototyyppiä pursposes kautta SAP SWPM tekevät hajautettu SAP-NW 7.5 liittyviä kohteita asentaa avain. Yksittäisten kohteiden kuvaus ja näyttökuvat tarkemmin artikkelin koko lomakkeessa näkyvät:

* Azure virtual verkon, joka sisältää kaksi VMs luominen 
* ottaa käyttöön kaksi Azure VMs OS SLES 12 SP1: een kautta Azure resurssien hallinnan malli 
* liittää kaksi vakio tallennustilan levyä app-palvelin AM (kuten 75 gt ja 500 gt)
* Liitä neljä levyjen HANA DB palvelimeen AM - 2 vakio tallennustilaa, kuten app-palvelin AM + 2 premium-tallennustilan levyä (kuten 2x512GB)
* sen mukaan, koon ja/tai siirtonopeuden vaatimukset liittäminen useille levyille ja luo Raidallinen asemat joko käyttämällä lvm tai mdadm OS tasolla AM sisällä
* Luo XFS tiedoston varten liitetyn levyille / looginen tietomääristä
* ottaa käyttöön uuden XFS tiedostojärjestelmien OS tasolla. Yksi tiedostojärjestelmän avulla voit pitää kaikki SAP-ohjelmistojen ja toinen esimerkiksi sapmnt-hakemistosta ja ehkä varmuuskopiot. SAP-HANA DB-palvelimen asennustapa XFS tiedoston järjestelmien /hana ja tämä on kaikki tarvittavat välttämiseksi, joka ei ole liian suuri Linux Azure VMs-pääkansio tiedostojärjestelmän täyttyy /usr/sap premium tallennustilan-levyille
* Kirjoita/jne/isännät testin VMs paikallinen ip-osoitteet
* Kirjoita /etc/fstab nofail-parametri
* Määritä ydin parametrien HANA-SLES – 12 SAP-huomautus (Katso ydin paramters osan edelleen tiedot)
* Vaihda tilan
* Jos etsimäsi - asentaminen testi VMs graafinen työpöydän. Käytä muussa remote sapinst asentaminen
* SAP-ohjelmiston lataaminen SAP-palvelun Marketplacesta
* Luo ryhmä "sapsys" HANA DB palvelimen AM ryhmätunnus 1001
* Asenna SAP HANA DB-palvelimeen AM hdblcm käyttäminen
* SAP-ASCS esiintymän asentaminen app-palvelin AM
* jakaa sapmnt hakemiston kautta NFS kesken testi VMs (app-palvelin AM on NFS-palvelin)
* Asenna tietokannan esiintymän, mukaan lukien HANA kautta SWPM DB-palvelimeen AM
* Asenna sovellus-palvelimeen AM PAS
* Käynnistä SAP MC ja muodosta esimerkiksi SAP Graafisen kautta / HANA Studio 




## <a name="prepare-azure-vms-for-manual-installation-of-sap-hana"></a>Manuaalinen asennus SAP HANA Azure VMs valmisteleminen

Tämän luvun Azure VMs valmistelemisesta SAP HANA manuaalinen asennus koostuu viisi osat, jotka kattavat on seuraavissa artikkeleissa:

* Levyn asetukset
* Ydin parametrit
* FILESYSTEMS
* / jne/isännät
* / jne/fstab


### <a name="disk-setup"></a>Levyn asetukset

Linux-AM Azure-pääkansio-tiedostojärjestelmässä on rajoitettu koko. Tämän vuoksi on tarpeen levytilaa liittäminen AM SAP suorittamista varten. Kyseessä AM käytetään ympäristössä, jossa on täysin prototyypin/esittely SAP app-palvelin on Azure vakio tallennustilan levyjen ongelmitta. SAP: in HANA DB tiedot ja kirjaudu tiedostojen - Azure Premium tallennustilan levyjen tulisi käyttää myös tuotannon vaaka.

Joitakin lisätietoja levyjen liittämisestä Linux AM [tähän](virtual-machines-linux-add-disk.md)

Jos haluat Azure levyn välimuisti - jokin käytettävä "Ei", jos levyjen, johon voidaan tallentaa HANA tapahtumalokit. HANA voidaan käyttää datatiedostojen jäljempänä välimuistiin. HANA sellaisenaan ladatun-tietokantaan, se on riippuvainen yleinen käyttö kuvion paljonko luku Azure levyn tason välimuisti parantaa suorituskykyä (kuten käynnistäminen HANA ja tietojen lukeminen levyltä muistiin).

Lisätietoja Azure Premium tallennustilan [tähän](../storage/storage-premium-storage.md)

[Tätä](https://github.com/Azure/azure-quickstart-templates) löydät luominen VMs json esimerkkimallit.
"101-AM-yksinkertainen-linux" näyttää, miten perusmallin näyttää myös tallennustila-kohdasta, joka lisää 100 Gigatavua tietoja DVD-levyllä.

[Tässä artikkelissa](virtual-machines-linux-sap-on-suse-quickstart.md) on tietoja Powershell tai CLI SUSE kuvan etsiminen ja liitettävä DVD-levyllä kautta UUID tärkeys.


Järjestelmä-ja siirtonopeuden koon mukaan voi olla tarpeen useita levyjä yhden sijaan ja luoda myöhemmin raita, määrittää kyseiset levyille OS tasolla. Miksi jokin loisi raita, määrittää useille Azure levyille kaksi ominaisuuksia ovat seuraavat:

* Suurenna siirtonopeuden
* yksittäinen tiedostojärjestelmän > 1 TT on Azure levyn nykyisen kokorajoituksen on 1 TT (vuodesta heinäkuussa 2016)


Löytyy lisätietoja kaksi tärkeimpien työkalujen raitasarjoittamista määrittäminen:

[Mdadm avulla voit määrittää Linux ohjelmiston raid Azure-AM-artikkeli](virtual-machines-linux-configure-raid.md)

[Artikkeli tietoja määrittäminen Linux Azure-AM loogisen levyn hallinta](virtual-machines-linux-configure-lvm.md)



![](./media/virtual-machines-linux-sap-hana-get-started/image003.jpg)

Testi ympäristön kaksi Azure vakio tallennustilan levyä on liitetty SAP app-palvelin AM. Yksi käytettiin tallentaa kaikki SAP-ohjelmiston asennus (kuten NetWeaver 7.5, SAP Graafisen SAP HANA... ) ja toinen on tarpeeksi tilaa, joista voi olla pakollinen (esimerkiksi varmuuskopioinnin, testitiedot) sekä kaikki VMs, jotka kuuluvat samaan SAP-vaaka kesken jaettavan sapmnt kansion (esimerkiksi SAP-profiilit).

![](./media/virtual-machines-linux-sap-hana-get-started/image004.jpg)

Toisin kuin app-palvelin AM neljä levyjen on liitetty SAP HANA palvelimeen AM. Kuten ennen kaksi levyä käytettiin on tarpeeksi tilaa esimerkiksi varmuuskopioinnin sekä pitäminen SAP-ohjelmiston (yksi voi jakaa myös SAP-ohjelmiston levyn NFS kautta). Lisää kaksi levyä on Azure Premium tallennustilan levyä, jos haluat säilyttää SAP HANA tietojen ja lokitiedostojen sekä /usr/sap-kansio.


### <a name="kernel-parameters"></a>Ydin parametrit


SAP-HANA vaatii tietyn Linux-ydin asetuksia, jotka eivät kuulu vakio Azure valikoiman kuvia ja on määritettävä manuaalisesti. Tällä tietyn SAP muistiinpano, joka on kuvattu asetukset. 


SAP Huomautus SAP HANA DB: Suositellut asetukset OS SLES 12 / SLES SAP-sovellusten 12: [SAP Huomautus 2205917](https://launchpad.support.sap.com/#/notes/2205917)

Yksi muita aiheen reagrding sivun välimuistin liittyvät SAP HANA käytössä SLES löytyy [tähän](https://www.suse.com/documentation/sles_for_sap/singlehtml/sles_for_sap_guide/sles_for_sap_guide.html#sec.s4s.configure.page-cache) luku 6.1 ydin: sivun välimuistin raja

On myös sivun välimuistin raja [SAP Huomautus 1557506](https://service.sap.com/sap/support/notes/1557506) SAP huomautuksen

SLES 12 on uutta työkalua, joka korvaa vanhan sapconf-apuohjelma. Se on "virittää-adm" ja määräten SAP HANA profiili on käytettävissä. Yksi voit etsiä lisätietoja tämän työkalun seuraavan kahden alla olevia linkkejä.

SLES ohjeista virittää adm profiilin sap-hana löytyy [tähän](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html)

SLES ohjeista virittää adm profiilin sap-hana - SAP-toiminnoista ja virittää-adm - luvun 6.2 säätäminen järjestelmien löytyy [tähän](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf)


![](./media/virtual-machines-linux-sap-hana-get-started/image005.jpg)

Tähän jokin voi nähdä, miten "virittää-adm" muuttuvat transparent_hugepage sekä numa_balancing arvot tarvittavat SAP HANA asetusten mukaan.


Voit tehdä SAP HANA ydin asetukset pysyvän kukaan grub2 käyttäminen SLES 12. Lisätietoja grub2 löytyy [tähän](https://www.suse.com/documentation/sled-12/book_sle_admin/data/sec_grub2_file_structure.html)


![](./media/virtual-machines-linux-sap-hana-get-started/image006.jpg)

Tässä näyttökuvassa näkyy, miten ydin asetuksia on muutettu määritystiedostossa ja sitten "käännetty" grub2 mkconfig kautta


![](./media/virtual-machines-linux-sap-hana-get-started/image007.jpg)

Voit muuttaa kautta Yast ja käynnistyksessä ydin parametrin asetusten on jokin muu vaihtoehto.


### <a name="filesystems"></a>FILESYSTEMS 

![](./media/virtual-machines-linux-sap-hana-get-started/image008.jpg)

Tässä jokin näkyvät kahden tiedostojärjestelmät, jotka on luotu SAP-sovelluksen palvelimen AM kaksi liitetty Azure vakio tallennustilan levyä päälle. Molemmat filesystems tyyppi XFS ja /sapdata ja /sapsoftware lisätallennustilaa.

Ei ole välttämätöntä, voit tehdä näin. Useilla eri tavoilla rakenteelle levytila poistamisesta.
Tärkeimmät kuvasuhde on välttää pääkansion tiedostojärjestelmän tila loppuu. 


![](./media/virtual-machines-linux-sap-hana-get-started/image009.jpg)

Vain yksinkertaisia "Normaali" asennus-vaihtoehtoa käytettäessä se asennetaan tiedostot oletusarvoisesti /hana ja /usr/sap koskeva SAP HANA DB AM on tärkeää tietää, että tietokannan kautta sapinst (swpm) asennuksen aikana. SAP-HANA log varmuuskopiointi oletusarvo on esimerkiksi /usr/sap.
Tykkää, ennen kuin se on välttää pääkansion tiedostojärjestelmän loppuu tila-näppäintä. Tämän vuoksi jokin Varmista, että ennen asennusta SAP HANA swpm kautta on tarpeeksi tilaa, valitse /hana ja /usr/sap.

SAP- [Tässä artikkelissa](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm) kuvataan SAP HANA vakio tiedostojärjestelmän-asettelu 


![](./media/virtual-machines-linux-sap-hana-get-started/image010.jpg)

Kun asennat SAP NetWeaver vakio SLES 12 Azure-valikoiman kuva ole, ei ole Vaihda tilaa viestin. Vahingossa viestiä jokin voi esimerkiksi lisätä manuaalisesti Vaihda tiedoston kuvatulla tavalla tämän asiakirjan dd, mkswap ja swapon kautta. Vain hakukenttään "Lisääminen vaihdettava tiedoston manuaalisesti" [Tässä artikkelissa](https://www.suse.com/documentation/sled-12/book_sle_deployment/data/sec_yast2_i_y2_part_expert.html)

Toinen vaihtoehto on määritettävä Vaihda tilaa Linux AM-agentti kautta. Lisätietoja löytyy [tähän](virtual-machines-linux-agent-user-guide.md)


### <a name="etchosts"></a>/ jne/isännät

![](./media/virtual-machines-linux-sap-hana-get-started/image011.jpg)

Toisen projektinhallintaa, ennen kuin alat asentaa SAP on/ETC/Hosts-tiedostoon sisällytettävät isäntänimet ja SAP-VMs IP-osoitteet. Yksi tulee käyttöön kaikissa SAP yhden Azure virtual verkossa oleville VMs ja käytä sitten sisäisiä IP-osoitteita.

### <a name="etcfstab"></a>/ jne/fstab

![](./media/virtual-machines-linux-sap-hana-get-started/image000c.jpg)

Testauksessa vaiheessa se poissa on hyvä nofail parametrin lisääminen fstab. Jos vikaa levyjä AM tapauksessa tulee edelleen ja ripustaa ei käynnistyksen yhteydessä. Mutta kukaan kiintolevytilan lisääminen eivät välttämättä ole käytettävissä ja prosessien voitu täyttävät pääkansion tiedostojärjestelmän varo kuin tässä tapauksessa. Siltä varalta, että /hana olisi puuttuu SAP HANA Eikö Aloita mutta ollenkaan.


## <a name="install-graphical-gnome-desktop-on-sles-12"></a>Graafisen Gnome työpöydän asentaminen SLES 12

Tämän luvun kuuluu kaksi secitions, jotka kattavat on seuraavissa artikkeleissa:

* Gnome työpöytä- ja xrdp SLES 12-asennus
* Java-pohjaisen SAP MC Firefoxin käyttäminen SLES 12 käynnissä

Yksi voi käyttää myös vaihtoehtoja, kuten Xterminal, VNC, mutta tässä artikkelissa kuvataan vain xrdp vuodesta syys 2016.

### <a name="installation-of-gnome-desktop-and-xrdp-on-sles-12"></a>Gnome työpöytä- ja xrdp SLES 12-asennus

Erityisesti niiden käyttäjille, joilla on Microsoft Windows-tausta ja haluat käyttää graafinen työpöydän suoraan SAP Linux VMs Firefox, Sapinst, SAP Graafisen, SAP MC tai HANA Studio ja ehkä muodostaa yhteyden kautta RDP AM Microsoft Windows-tietokoneessa on helppo tapa saavuttaa. Vaikka tämä ei ehkä sovellu esimerkiksi tuotannon tietokantapalvelimeen voi ok täysin prototyypin/esittely-ympäristössä. Näin Gnome työpöydän asennetaan Azure SLES 12-AM:

Asenna gnome työpöydän mukaan (esimerkiksi-painovärit, muste ikkuna) seuraava komento:

   Valitse kuvio -t gnome basic zypper

Asenna xrdp, jotta yhteyden AM RDP kautta:

   xrdp zypper

Muokkaa /etc/sysconfig/windowmanager ja Määritä oletusarvoinen Windowsin hallinnan Gnome:

   DEFAULT_WM = "gnome"

Suorita chkconfig, varmista, että xrdp käynnistyy automaattisesti uudelleenkäynnistyksen jälkeen: 

  chkconfig-tason 3 xrdp käyttöön

siltä varalta, että ongelma RDP-yhteys on oltava yrittää käynnistää uudelleen (tämä ei ehkä painovärit, muste ikkuna):

  /ETC/xrdp/xrdp.SH uudelleen

siltä varalta, että xrdp uudelleenkäynnistyksen kuin edellä mainituista ei toimi valintaruutu, jos on .pid tiedosto ja poista ne:

  Tarkista /var/run ja Etsi xrdp.pid   
  Poista se ja yritä sitten uudelleen käynnistäminen uudelleen



### <a name="sap-mc"></a>SAP-MC


Aloita graafinen Java-pohjaisen SAP MC ulos Firefox käynnissä Azure SLES 12-AM asennettuasi Gnome työpöydän jokin edellisessä kohdassa kuvatulla tavalla saavat virheen vuoksi Java-selainlaajennuksesta puuttuu.

Aloita SAP-MC URL-osoite on <server>: 5 < instance_number > 13

Lisätietoja löytyy [tähän](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm)


![](./media/virtual-machines-linux-sap-hana-get-started/image013.jpg)

Yllä olevassa näyttökuvassa jokin nähdä kuinka virheilmoitus saattaa näyttää samalta kun Java-selainlaajennuksesta puuttuu. 

![](./media/virtual-machines-linux-sap-hana-get-started/image014.jpg)

Yksi vaihtoehto ongelman ratkaisemiseen on yksinkertaisesti Asenna puuttuvat laajennus Yast kautta.

![](./media/virtual-machines-linux-sap-hana-get-started/image015.jpg)

Avaa toistuva SAP MC URL-osoite hieman valintaikkunan ensimmäisen kerran, pyytää aktivoimaan laajennus.


Yksi muita ongelman, joka saattaa ponnahdusikkuna on puuttuvan tiedoston koskeva virhesanoma: Tämä on todennäköisesti Java 1.8 joka vaaditaan SAP Graafisen 7.4 asennukseen liittyviä javafx.properties

Nähdä kautta Yast IBM Java-versioon ei kuulu tämän tiedoston. Ratkaisu on Oracle Java lataaminen.
Artikkeliin, jossa puheen tietoja tämän ongelman löytyy [tähän](https://scn.sap.com/thread/3908306)



## <a name="manual-sap-hana-installation-via-swpm-as-part-of-a-netweaver-75-installation"></a>Manuaalinen SAP HANA asennettavissa SWPM NetWeaver 7.5 asennuksen


Näyttökuvat seuraavassa luettelossa näkyy tärkeimmät vaiheet SAP NetWeaver 7.5 ja SAP HANA SP12 SWPM (sapinst) kautta. Osana NW 7.5 asennuksen SWPM on asennettava myös HANA tietokannan yhden esiintymän ominaisuuksia.


![](./media/virtual-machines-linux-sap-hana-get-started/image012.jpg)

Esimerkki testin ympäristön yhden ABAP app-palvelin on asennettu. Vaihtoehdon "Jaettu järjestelmän" käytettiin asennettava ASCS esiintymän ja ensisijainen App server-esiintymän yhdessä Azure AM ja SAP HANA toiseen Azure-AM tietokannan mukaisena.


![](./media/virtual-machines-linux-sap-hana-get-started/image016.jpg)

Kun ASCS esiintymän app-palvelin AM asennetaan ja määritetään "vihreä" SAP MC sapmnt hakemiston esimerkiksi SAP profiilin hakemiston mukaan lukien on jaettava AM SAP HANA DB-palvelimen kanssa.
DB-asennuksen vaihe on käyttää tietoja. Paras tapa on käytettävä NFS, joka on määritetty Yast.


![](./media/virtual-machines-linux-sap-hana-get-started/image017b.jpg)

Sovelluksesta palvelimen AM sapmnt hakemiston tulisi jakaa NFS kautta käyttämällä sekä "no_root_squash" asetukset "rw". Oletusarvo on "ro" ja "root_squash" joka saattaa aiheuttaa ongelmia asennettaessa tietokannan esiintymä.


![](./media/virtual-machines-linux-sap-hana-get-started/image018b.jpg)

SAP HANA DB palvelimessa AM sapmnt jakaa sovelluksen palvelimesta AM on määritettävä kautta "NFS-asiakas" (kuten kanssa Yast ohje)


![](./media/virtual-machines-linux-sap-hana-get-started/image019.jpg)

Valitse kukaan SAP HANA DB palvelimen AM kirjautua sisään ja Käynnistä SWPM suorittamaan hajautetun NW 7.5-asennus - "Tietokannan esiintymän" seuraavaan vaiheeseen.


![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Liity SAP HANA asennus ei ole mahdollista liian paljon antamaan valittuasi "Normaali" asennus. Paitsi useimpien yritysten installatiom mediatiedostojen yksi polku on DB SUOJAUSTUNNUS, isäntänimi, esiintymän numero ja DB järjestelmänvalvoja-salasanalla.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Seuraava vaihe on annettava salasana DBACOCKPIT rakenteen.



![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Seuraa kysymyksen SAPABAP1 rakenteen salasana.


![](./media/virtual-machines-linux-sap-hana-get-started/image023.jpg)

Lopussa on oltava vain vihreä Verbitoimintovalikon avattavassa luettelossa jokaisen vaiheen DB asentunut eteen ja jotka tulee pieni sanomaruudussa pitäisi näkyä versionumeron "suorittamisen... Tietokannassa on päättynyt".

 
![](./media/virtual-machines-linux-sap-hana-get-started/image024.jpg)

SAP-MC pitäisi näyttää "vihreä" ja SAP HANA prosesseja (kuten hdbindexserver, hdbcompileserver) täysi luettelo DB-esiintymän onnistuneen asennuksen jälkeen


![](./media/virtual-machines-linux-sap-hana-get-started/image025.jpg)

Tässä näyttökuvassa näkyy osat tiedostorakenne-kohdassa /hana/shared, jolla SWPM luotu HANA asennuksen aikana. Oli ei voi määrittää eri polun. Tämän vuoksi on tärkeä levytilaa /hana ennen SWPM välttämiseksi pääkansion tiedostojärjestelmän loppuu tilaa SAP HANA asennettavissa-kohdassa Ota käyttöön.


![](./media/virtual-machines-linux-sap-hana-get-started/image026.jpg)

Yksi tässä nähdä samojen ohjeiden ennen /usr/sap-kansion.


![](./media/virtual-machines-linux-sap-hana-get-started/image027b.jpg)

Hajautettu ABAP asennuksen viimeinen vaihe on sen "ensisijainen sovelluksen Server-esiintymän"


![](./media/virtual-machines-linux-sap-hana-get-started/image028b.jpg)

Kun PAS sekä SAP Graafisen onko asennettu jokin tarkistaa tapahtuman "dbacockpit", kaikki näyttää oikealle SAP HANA asennuksessa kautta.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Viimeksi vaihe voi asentaa SAP HANA Studio SAP app-palvelin AM ja DB palvelimessa AM SAP HANA esiintymään.




## <a name="manual-sap-hana-installation-via-hana-life-cycle-manager-tool-hdblcm"></a>Manuaalinen SAP HANA asennettavissa HANA elinkaaren hallinta työkalu hdblcm


Lisäksi asentaminen SAP HANA hajautettu asennuksen SWPM kautta on myös laskentamenetelmä Asenna HANA erillisenä käyttäen hdblcm ja sitten Asenna esimerkiksi SAP NetWeaver 7.5 jälkeenpäin. Näyttökuvat alla luettelosta näkyy, miten tämä toimii.

Seuraavassa on tietoja HANA hdblcm työkalu kolme lähteet:

[Tehtävän oikea SAP-HANA HDBLCM valitseminen](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)

[SAP: in HANA elinkaari hallintatyökalut](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)

[SAP: in HANA asennus- ja Update-opas](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)



![](./media/virtual-machines-linux-sap-hana-get-started/image030.jpg)

Voit välttää ongelmia myöhemmin ryhmän tunnus oletusasetus siten \<HANA SUOJAUSTUNNUS\>adm käyttäjän (luotu hdblcm-työkalulla) jokin kannattaa määrittää uuteen ryhmään nimeltä "sapsys" ryhmätunnus 1001 kanssa ennen asennusta käyttämällä hdblcm SAP HANA.




![](./media/virtual-machines-linux-sap-hana-get-started/image031.jpg)

Aloitusaika hdblcm ensimmäisen yksinkertaisen Käynnistä-valikko, jossa kukaan Valitse kohde ole 1 "Asenna uusi"



![](./media/virtual-machines-linux-sap-hana-get-started/image032.jpg)

Tässä näyttökuvassa jokin voit tarkastella kaikkia avaimen vaihtoehtoja, joilla on lisätty ennen. Tärkeää - kansioita, jotka olivat nimetyt HANA log ja tietojen asemat sekä asennuspolku (/ hana/jaetut tässä esimerkissä) ja/usr/sap ei saa olla osa pääkansion tiedostojärjestelmän, mutta kuuluvat Azure tietojen levyjä, johon on liitetty AM Azure AM asetukset-kohdassa kuvatulla tavalla. Riskin, että pääkansio tiedostojärjestelmän saattaa toimia loppumassa välttämiseksi.
Yksi näet myös, että HANA järjestelmänvalvojan käyttäjällä on käyttäjätunnus 1005 ja kuuluu sapsys ryhmään (id 1001) joka oli määritetty ennen asennusta.



![](./media/virtual-machines-linux-sap-hana-get-started/image033.jpg)

Yksi voit tarkistaa HANA \<HANA SUOJAUSTUNNUS\>adm (azdadm tässä esimerkissä) käyttäjätiedot-/ jne /: mm: ss



![](./media/virtual-machines-linux-sap-hana-get-started/image034.jpg)

SAP-HANA käyttämällä hdblcm asentamisen jälkeen se näkevät SAP HANA Studiossa. SAPABAP1 rakenteen mukaan lukien esimerkiksi kaikki SAP NetWeaver taulukot ei ole vielä.



![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Asennettuasi SAP HANA jokin asentaa SAP NetWeaver päällä. Tässä esimerkissä on tehty käyttäminen "jaettu asennuksen" kautta SWPM vastaavan edellisen osan ohjeiden mukaisesti.
Kun asentaminen tietokannan kautta SWPM yhden esiintymän vain on voit kirjoittaa samat tiedot kuin ennen hdblcm (kuten isäntänimi, HANA SUOJAUSTUNNUS esiintymän numero) kanssa. SWPM sitten olemassa olevan HANA asennuksen käyttäminen ja lisätä uusia malleja.



![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Tämä on SWPM asennuksen vaihe kuvan kohtaa, johon voit kirjoittaa tietoa koskeva DBACOCKPIT rakenteen kukaan.


![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Valitse SWPM odottaa syöttämään tietoja SAPABAP1 rakenne.


![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Kun SWPM tietokannan esiintymä-asennus on valmis jokin voi nähdä HANA Studiossa SAPABAP1 rakenne.



![](./media/virtual-machines-linux-sap-hana-get-started/image039b.jpg)

Ja lopuksi SAP app-palvelin ja SAP Graafisen asennuksen jälkeen jokin pitäisi vahvistamiseksi HANA DB-esiintymää tapahtuman "dbacockpit".




## <a name="general-information-related-to-sap-azure-certifications-running-sap-hana-on-azure-and-sap-software-download"></a>Yleisiä tietoja, jotka liittyvät SAP Azure kaltaisilta, SAP HANA käytössä Azure ja SAP-ohjelmiston lataaminen

* Yleiset SAP Azure helppokäyttötoimintoa suorittamisesta Azure Windows-Käyttöjärjestelmää ja SAP-perinteinen tila: [Käyttämällä SAP-näennäiskoneiden Windows Azure-tietokannassa](virtual-machines-windows-classic-sap-get-started.md)

* olemassa olevan SAP-mallien käytön asiakkaiden tietojen: [SAP Azure pikaopas mallit](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/)

* Yleiset SAP Azure helppokäyttötoimintoa suorittamisesta SAP Azure kanssa Linux OS Azure Resurssienhallinta-mallissa: [Käyttämällä SAP-Linux näennäiskoneiden (VMs)](virtual-machines-linux-sap-get-started.md)

* sertifioitu SAP HANA laitteiston directory jossa luetellaan Azure AM tiedostotyyppejä tuetaan tuotannon: [Sertifioitu SAP HANA® laitteisto-kansio](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)

* tietoja virtuaalikoneen koot erityisesti Linux toiminnoista: [koot näennäiskoneiden Azure-tietokannassa](virtual-machines-linux-sizes.md)

* SAP-muistiinpano, joka näyttää kaikki tukemien SAP-tuotteiden Azure ja tuetut Azure AM tyypit SAP: [SAP Huomautus 1928533](https://launchpad.support.sap.com/#/notes/1928533/E)

* SAP: in huomautuksen SAP "parannetun seuranta" kanssa Linux VMs Azure-: [SAP Huomautus 2191498](https://launchpad.support.sap.com/#/notes/2191498/E)

* SAP-HANA toiminnon uudesta Azure "Suuri esiintymät". On tärkeää ymmärtää, että tämä ei ole suorittamisesta SAP HANA Azure VMs mutta hybrid ympäristössä, jossa SAP-sovelluksen palvelinten suorittaa Azure VMs mutta SAP HANA suoritetaan paljas palvelimissa: [SAP Huomautus 2316233](https://launchpad.support.sap.com/#/notes/2316233/E)

* Tietoja Linux SAPOSCOL Huomautus SAP: [SAP Huomautus 1102124](https://launchpad.support.sap.com/#/notes/1102124/E)

* Avaimen seurantaa arvot Microsoft Azure-SAP: [SAP Huomautus 2178632](https://launchpad.support.sap.com/#/notes/2178632/E)

* Lisätietoja Azure resurssin hallinnasta: [Azure resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md)

* Lisätietoja käyttöönotosta Linux VMs kautta mallit: [käyttöönotto ja hallita näennäiskoneiden Azure Resurssienhallinta mallit ja Azure-CLI](virtual-machines-linux-cli-deploy-templates.md)

* Käyttöönoton vertailu mallit Azure Resurssienhallinta ja perinteinen: [Azure Resurssienhallinta ja perinteinen käyttöönoton: käyttöönoton mallit ja resurssien tilan](../resource-manager-deployment-model.md)

* Lataa NetWeaver 7.5 Linux/HANA SAP-palvelun Marketplacesta:![](./media/virtual-machines-linux-sap-hana-get-started/image001.jpg)

* Lataa HANA SP12 Platform Edition SAP-palvelun Marketplace:![](./media/virtual-machines-linux-sap-hana-get-started/image002.jpg)

