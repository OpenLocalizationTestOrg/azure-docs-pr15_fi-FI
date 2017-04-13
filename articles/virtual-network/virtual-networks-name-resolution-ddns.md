<properties
   pageTitle="Dynaaminen DNS: n avulla voit rekisteröidä isäntänimet"
   description="Tällä sivulla antaa tiedot voit määrittää dynaamisen DNS isäntänimet rekisteröidään DNS-palvelimet."
   services="dns"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="" />
<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="using-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Dynaaminen DNS: n avulla voit rekisteröidä isäntänimet DNS-palvelimeen

[Azure tarjoaa nimenselvitys](virtual-networks-name-resolution-for-vms-and-role-instances.md) näennäiskoneiden (VMs) ja roolin esiintymät. Kun nimi tarkkuus on muutakin Azure tulostusvaihtoehtoja, voit kirjoittaa omia DNS-palvelimet. Näin voit sovittaa DNS-ratkaisun haluamallasi tietyn tavalla potenssiin. Esimerkiksi tarvitset paikallisen resursseihin Active Directory-toimialueen ohjauskoneen kautta.

Kun mukautettu DNS-palvelimet isännöidään Azure VMs kuin voit lähettää saman vnet hostname kyselyt edelleen Azure isäntänimet ratkaisemiseksi. Jos et halua käyttää tätä vaihtoehtoa, toimi, voit rekisteröidä AM isäntänimet DNS-palvelimen dynaamisen DNS: n avulla.  Azure ei ole mahdollista (kuten tunnistetiedot) suoraan Luo tietueet DNS-palvelimet niin vaihtoehtoinen järjestelyt tarvitaan usein. Seuraavassa on joitakin yleisiä tilanteita, joissa vaihtoehtojen avulla.

## <a name="windows-clients"></a>Windows-asiakkaat

Muut toimialueeseen liittyneet Windows asiakkaat yrittävät suojaamaton dynaaminen DNS (DDNS)-päivitykset, kun ne käynnistys tai niiden IP-osoite muuttuu. DNS-nimi on isäntänimi plus ensisijainen DNS-liite. Azure jättää ensisijaisen DNS-liitteen tyhjäksi, mutta voit tehdä tämän AM [Käyttöliittymän](https://technet.microsoft.com/library/cc794784.aspx) tai [käyttämällä automaatiota](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).

Toimialueen liittyneet Windows-asiakkaiden rekisteröidä niiden IP-osoitteiden toimialueen ohjauskoneen käyttämällä suojatun dynaaminen DNS. Toimialueen liity-prosessi määrittää asiakkaan ensisijainen DNS-liite ja luo ja ylläpitää luottamussuhde.

## <a name="linux-clients"></a>Linux asiakkaat

Linux Asiakkaat yleensä ei rekisteröityvät käynnistettäessä DNS-palvelimeen, niiden oletetaan luokaksi tekee. Azure's DHCP-palvelimia ei ole mahdollista tai tunnistetietoja, Rekisteröi-tietueet DNS-palvelimeen.  Kutsutaan *nsupdate*, joka sisältyy sidonta-paketti-työkalun avulla voit lähettää dynaamisen DNS-päivitykset. Koska dynaaminen DNS-protokollan on standardoitu, voit käyttää *nsupdate* myös silloin, kun et käytä sitominen DNS-palvelimeen.

Voit käyttää koukut, jotka toimittaa DHCP-asiakas, voit luoda ja ylläpitää hostname tapahtuman DNS-palvelimeen. Asiakkaan suorittaa aikana DHCP-komentosarjat */etc/dhcp/dhclient-exit-hooks.d/*. Tämä voidaan rekisteröidä uuden IP-osoitteen avulla *nsupdate*. Esimerkki:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on the primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        #done
        exit 0;

*Nsupdate* -komennon avulla voit myös suorittaa suojatun dynaaminen DNS-päivitykset. Esimerkiksi kun käytät sitoa DNS-palvelimeen, julkisen ja yksityisen avaimen pari on [luotu](http://linux.yyz.us/nsupdate/).  DNS-palvelin on [määritetty](http://linux.yyz.us/dns/ddns-server.html) avaimen julkisen osan niin, että se tarkistaa pyynnön allekirjoitus. Anna avain pari *nsupdate* järjestyksessä dynaaminen DNS-, Päivitä allekirjoitusta, sinun on käytettävä *-k* -vaihtoehto.

Kun käytät Windowsin DNS-palvelimeen, voit käyttää Kerberos-todennus (ei käytettävissä *nsupdate*Windows-version) *nsupdate* *-g* -parametrin. Voit tehdä tämän ladata tunnistetiedot (esimerkiksi palauttamalla [keytab tiedoston](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)) *kinit* avulla. Valitse *nsupdate g* noutaa välimuistista tunnistetiedot.

Tarvittaessa voit lisätä DNS-haun liitteen että VMs. DNS-liite on määritetty */etc/resolv.conf* -tiedosto. Sisällön hallinta automaattisesti useimmat Linux distros tiedoston, joten yleensä et voi muokata sitä. Voit kuitenkin ohittaa liitteen DHCP asiakkaan *korvaavat* -komennon avulla. Voit tehdä tämän */etc/dhcp/dhclient.conf*lisääminen:

        supersede domain-name <required-dns-suffix>;

