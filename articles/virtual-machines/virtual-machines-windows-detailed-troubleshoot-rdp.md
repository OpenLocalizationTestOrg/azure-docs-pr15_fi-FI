<properties
    pageTitle="Yksityiskohtainen Etätyöpöydän vianmääritys | Microsoft Azure"
    description="Tarkista yksityiskohtaisia vianmääritysohjeita remote työpöydän virheet, jos et pysty, Windows-näennäiskoneiden Azure-tietokannassa"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"
    keywords="ei voi muodostaa yhteyttä etätyöpöytään, vianmääritys etätyöpöydän kautta, Etätyöpöydän voi muodostaa etäyhteyden työpöydän virheitä, Etätyöpöydän vianmääritys, etäyhteyden työpöydän ongelmia
"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Yksityiskohtaiset vianmääritysohjeet Etätyöpöytäyhteys ongelmien Windows Azure-tietokannassa VMs

Tässä artikkelissa on yksityiskohtaiset vianmääritysohjeita, joiden vianmäärityksen ja korjauksen Windows-pohjaisesta Azuren näennäiskoneiden monimutkaisia etätyöpöydän virheet.

> [AZURE.IMPORTANT] Poistamiseksi tavallisimmista etätyöpöydän virheet muista [basic vianmääritysartikkeliin etätyöpöydän](virtual-machines-windows-troubleshoot-rdp-connection.md) ennen kuin jatkat.

Etätyöpöytä virhesanoma, joka ei näytä mitään kattaa [basic etätyöpöydän vianmäärityksen](virtual-machines-windows-troubleshoot-rdp-connection.md)oppaasta virheilmoituksista-viestejä. Noudattamalla seuraavia ohjeita voit selvittää, miksi Remote Desktop (RDP) asiakas ei voi muodostaa Azure AM RDP-palveluun.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Jos tarvitset apua milloin tahansa tämän artikkelin, ota Azure asiantuntijoille [MSDN-Azure](https://azure.microsoft.com/support/forums/)ja Pinon ylivuoto-keskustelupalstoilla. Voit vaihtoehtoisesti myös jättää Azure tuki tapahtuma. Siirry [Azure tukisivustossa](https://azure.microsoft.com/support/options/) ja valitse **Pyydä tuki**. Lue lisätietoja Azure tuki- [Microsoft Azure tuki usein kysytyt kysymykset](https://azure.microsoft.com/support/faq/).


## <a name="components-of-a-remote-desktop-connection"></a>Etätyöpöytäyhteys osat

RDP-yhteys sisältyy seuraavat osat:

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_0.png)

Ennen kuin jatkat, se voi olla apua tarkistettava henkisesti mikä on muuttunut AM viimeisen onnistuneen etätyöpöytä yhteys. Esimerkki:

- Julkinen IP-osoite AM tai pilvipalvelussa, joka sisältää AM (kutsutaan myös virtual IP-osoite [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) on muuttunut. RDP-virhe voi johtua siitä asiakkaan DNS-välimuisti on edelleen *Vanha IP-osoite* , rekisteröity DNS-nimi. DNS-asiakasohjelman välimuistin tyhjentäminen ja yritä AM uudelleen. Tai yritä yhteys muodostetaan suoraan uusi VIP.
- Käytössäsi on kolmannen osapuolen sovelluksen, voit hallita etätyöpöydän yhteyksiä Azure portaalin luoma yhteyden sijaan. Tarkista, että sovelluksen määritys sisältää oikeat porttinumeroa etätyöpöydän tietoliikenteen. Voit tarkistaa tämän perinteinen virtual machine [Azure portal](https://portal.azure.com)portti valitsemalla AM asetukset > Päätepisteet.


## <a name="preliminary-steps"></a>Alustava vaiheet

Ennen kuin jatkat yksityiskohtaiset vianmääritys

- Tarkista virtuaalikoneen Azure perinteinen portaalin tai ilmeisimmät ongelmat Azure portaali tila.
- Tee [RDP yleisten virheiden basic vianmäärityksen oppaan vaiheet Nopea korjaus](virtual-machines-windows-troubleshoot-rdp-connection.md#quick-troubleshooting-steps).


Yritä yhdistää uudelleen etätyöpöydän kautta AM jälkeen seuraavasti.


## <a name="detailed-troubleshooting-steps"></a>Yksityiskohtaiset vianmääritysohjeet

Etätyöpöydän asiakkaan ei ehkä saavuttamiseksi vuoksi seuraavista lähteistä asioista Azure-AM Etätyöpöytä-palvelu:

- [Remote työpöydän asiakastietokone](#source-1-remote-desktop-client-computer)
- [Organisaation intranet reuna-laite](#source-2-organization-intranet-edge-device)
- [Cloud palvelupäätepiste ja käyttää hallinta-luettelossa (Käyttöoikeusluettelon)](#source-3-cloud-service-endpoint-and-acl)
- [Verkon käyttöoikeusryhmät](#source-4-network-security-groups)
- [Windows-pohjaisesta Azure AM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>Lähteen 1: Etäyhteyden työpöydän asiakastietokone

Varmista, että tietokoneen voi tehdä etätyöpöydän yhteydet paikalliseen toiseen, Windows-tietokoneella.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_1.png)

Jos et voi tarkistaa tietokoneen seuraavat asetukset:

- Paikallinen palomuuri-asetus, joka estää etätyöpöydän liikenne.
- Paikallisesti asennettu välityspalvelimen Asiakasohjelmistoa, joka estää etätyöpöydän yhteydet.
- Paikallisesti asennettu verkon seuranta ohjelmiston, joka estää etätyöpöydän yhteydet.
- Muuntyyppisten suojausohjelmisto toisella, joka liikennettä tai Salli/disallow tietyntyyppiset liikenteestä, joka estää Etätyöpöytä yhteydet.

Kaikissa näissä tapauksissa tilapäisesti käytöstä ohjelmiston ja yritä muodostaa yhteys paikalliseen tietokoneeseen etätyöpöydän kautta. Jos saat selville todellinen syy tällä tavalla, käsitellä verkonvalvoja korjaa Ohjelmistoasetukset sallimaan etätyöpöydän yhteyksiä.

## <a name="source-2-organization-intranet-edge-device"></a>Lähteen 2: Organisaation intranet reuna-laite

Tarkista, että tietokone, joka on suoraan yhteydessä Internetiin voivat luoda Azure virtuaalikoneen etätyöpöydän yhteyksiä.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_2.png)

Jos sinulla ei ole tietokoneeseen, jossa on suoraan yhteydessä Internetiin, Luo ja testaa uuden Azure virtual-koneen resurssien ryhmä- tai cloud-palvelussa. Lisätietoja on artikkelissa [Create virtual machine käynnissä Windows Azure-tietokannassa](virtual-machines-windows-hero-tutorial.md). Voit poistaa virtuaalikoneen ja resurssiryhmän tai pilvipalveluun, testin jälkeen.

Jos luot Etätyöpöytäyhteys suorassa Internet tietokoneen kanssa, tarkista organisaation intranet reunan laitteen varten:

- Sisäinen palomuuri estää HTTPS-yhteyksiä Internetiin.
- Estää etätyöpöydän yhteydet välityspalvelinta.
- Tunkeutumisen tunnistus tai verkon seuranta, mikä verkkoa reunan, joka estää etätyöpöydän yhteydet-ohjelmiston.

Käsitellä verkonvalvoja korjaa organisaation intranet reunan laitteen asetukset sallivat HTTPS-pohjainen etätyöpöydän Internet.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Lähteen 3: Cloud palvelupäätepiste ja Käyttöoikeusluettelon

VMs luotu perinteinen käyttöönotto-malli Varmista, että toisen Azure AM, joka on saman pilvipalvelussa tai VPN tehdä etätyöpöydän yhteydet Azure AM.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_3.png)

> [AZURE.NOTE] Näennäiskoneiden luotiin resurssien hallinnan, siirry [lähde 4: verkon käyttöoikeusryhmät](#source-4-network-security-groups).

Jos sinulla ei ole virtual käyttäjäprofiilista saman pilvipalvelussa tai VPN, luo sellainen. [Luo virtual tietokoneen käyttöjärjestelmä on Windows Azure-tietokannassa](virtual-machines-windows-hero-tutorial.md)noudattamalla. Poista testi virtuaalikoneen testi päättymisen jälkeen.

Jos pystyt muodostamaan etätyöpöydän kautta saman pilvipalvelussa tai VPN virtual tietokoneeseen, tarkista nämä asetukset:

- Etätyöpöytä liikenne aikataulussa AM päätepisteen määrityskohde: yksityinen porttinumeroa päätepisteen on vastattava porttinumeroa, joina AM etätyöpöydän palvelu Kuuntele (oletusarvo on 3389).
- Etätyöpöytä liikenne päätepisteen aikataulussa AM Käyttöoikeusluettelon: käyttöoikeusluettelot avulla voit määrittää sallia tai estää saapuvan liikenteen Internetistä perusteella sen lähde-IP-osoite. Väärä käyttöoikeusluettelot estää etätyöpöydän saapuvan liikenteen päätepisteelle. Tarkista, että käyttöoikeusluettelot varmistaa, että saapuvan liikenteen julkiseen IP-osoitteista sekä välityspalvelimen tai muita edge Serverin on sallittu. Lisätietoja on artikkelissa [verkon luettelon Käyttöoikeusluettelon (Access Control) ominaisuudet?](../virtual-network/virtual-networks-acl.md)

Voit tarkistaa, onko päätepisteen ongelman aiheuttaja, poista nykyinen päätepiste ja luoda uuden, valitseminen satunnaisen portin alueen 49152 – 65535 ulkoisen portin numeroa. Lisätietoja on artikkelissa [virtual machine päätepisteet määrittämisestä](virtual-machines-windows-classic-setup-endpoints.md).

## <a name="source-4-network-security-groups"></a>Lähteen 4: Verkon käyttöoikeusryhmät

Verkon käyttöoikeusryhmät Salli sallittu saapuva ja lähtevä liikenne eritellympiä ohjausobjektin. Voit luoda sääntöjä, jotka kestävät aliverkosta ja cloud services Azure virtual verkossa. Tarkista varmistaa, että Internet-liikenne Etätyöpöytä sallitaan verkon käyttöoikeusryhmän säännöt:

- Valitse oman AM Azure-portaalissa
- Valitse **kaikkia asetuksia** | **verkkoliittymät** ja valitse Verkkokäyttöliittymän.
- Valitse **kaikkia asetuksia** | **verkon käyttöoikeusryhmän** ja valitse verkko-käyttöoikeusryhmän.
- Valitse **kaikkia asetuksia** | **saapuvien suojaussäännöt** ja varmista, että TCP-porttiin 3389 RDP salliminen sääntöä.
    - Jos sinulla ei ole säännön, valitse **Lisää** Luo sääntö. Kirjoita **TCP** protokolla ja sitten **3389** kohde Port (portti)-alueella.
    - Varmista, että toiminto on määritetty **Salli** ja Tallenna uusi saapuva sääntö valitsemalla OK.


Lisätietoja on artikkelissa [verkon suojauksen ryhmän (NSG) ominaisuudet?](../virtual-network/virtual-networks-nsg.md)

## <a name="source-5-windows-based-azure-vm"></a>Lähteen 5: Windows-pohjaisesta Azure AM

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_5.png)

[Azure IaaS (Windows) diagnostiikka package](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) avulla voit tarkistaa, onko virheen vuoksi Azure virtuaalikoneen itse. Jos **RDP yhteys Azure-AM (perusarvoa)** ongelman ratkaisemiseksi ei diagnostiikka-paketti, [tämän artikkelin](virtual-machines-windows-reset-rdp.md)ohjeiden mukaisesti. Tässä artikkelissa palauttaa virtuaalikoneen Etätyöpöytä-palvelu:

- Ota käyttöön "Etätyöpöydän" Windowsin palomuuri oletusarvoiset säännöt (TCP-portin 3389).
- Etätyöpöytä tietoyhteyksien ottaminen käyttöön määrittämällä HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections rekisteriarvoa 0.

Kokeile tietokoneesta yhteyttä uudelleen. Jos et ole vielä ei voi muodostaa etätyöpöydän kautta, tarkista seuraavat mahdolliset ongelmat:

- Etätyöpöytä-palvelu ei ole käynnissä olevat samannimiset AM.
- Etätyöpöytä-palvelu ei ole Kuuntele porttinumeroa 3389.
- Windowsin palomuuri tai jokin muu paikallinen palomuuri on lähtevän säännön, joka estää Etätyöpöytä liikenne.
- Tunkeutumisen tunnistus tai verkon seuranta Azure virtuaalikoneen-ohjelmiston estää Etätyöpöytä yhteydet.

VMs luotu perinteinen käyttöönoton mallia voit käyttää Azure PowerShell-etäistunnon, voit Azure virtuaalikoneen. Ensin sinun täytyy asentaa virtual machine pilvipalvelussa sijaitsevaan varmennetta. [Määrittää suojatun PowerShell etäkäyttöpalvelimen Azuren näennäiskoneiden,](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) Siirry ja lataa **InstallWinRMCertAzureVM.ps1** -komentosarjatiedosto paikalliseen tietokoneeseen.

Asenna PowerShellin Azure seuraavaksi, jos omistajuus on vielä vahvistamatta. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).

Avaa PowerShellin Azure-komentokehote ja vaihtaa nykyisen kansion **InstallWinRMCertAzureVM.ps1** komentosarja-tiedoston sijainti. Oikea suorittamisen käytäntö on määritettävä suorittamaan Azure PowerShell-komentosarjaa. Määrittää käytännön tasoa **Get suorituskäytäntöä** -komennon suorittaminen. Lisätietoja sopivalle tasolle määrittämisestä on artikkelissa [Määrittäminen suorituskäytäntöä](https://technet.microsoft.com/library/hh849812.aspx).

Seuraavaksi täytä Azure tilauksen nimi ja virtuaalikoneen nimesi cloud palvelunimi (poistaminen < ja > merkkiä), ja Suorita nämä komennot.

    $subscr="<Name of your Azure subscription>"
    $serviceName="<Name of the cloud service that contains the target virtual machine>"
    $vmName="<Name of the target virtual machine>"
    .\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName

Saat oikeat tilauksen nimi **Hae AzureSubscription** -komennossa _SubscriptionName_ ominaisuudesta. Saat virtuaalikoneen cloud-palvelunimi **Hae AzureVM** -komennossa _palvelun nimi_ -sarakkeessa.

Tarkistaa, onko uutta varmennetta. Avaa Varmenteet laajennuksen nykyisen käyttäjän ja Etsi **Luotetut varmenteiden päämyöntäjät\varmenteet** -kansioon. Raportissa pitäisi näkyä cloud-palvelun myöntänyt-sarakkeessa DNS-nimi varmenteen (Esimerkki: cloudservice4testing.cloudapp.net).

Seuraavaksi aloittaa Azure PowerShell-etäistunnon näitä komentoja käyttämällä.

    $uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
    $creds = Get-Credential
    Enter-PSSession -ConnectionUri $uri -Credential $creds

Kun olet lisännyt kelvollinen järjestelmänvalvojan tunnistetiedot, pitäisi näkyä kaltaisen PowerShellin Azure seuraava kehote:

    [cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>

Tämä kehote ensimmäinen osa on cloud palvelunimi, joka sisältää kohteen AM, jotka voivat olla erilaisia kuin "cloudservice4testing.cloudapp.net". Voit nyt myöntää PowerShellin Azure tämän pilvipalvelussa, voit tutkia ongelmaa komennot mainituista ja korjaa määritykset.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>Jos haluat korjata manuaalisesti listening porttinumeroa Etätyöpöytäpalvelut

Jos et voi suorittaa **Azure-AM (perusarvoa) yhteys RDP** -ongelma [Azure IaaS (Windows) diagnostiikka paketti](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) remote PowerShellin Azure istunnon kehotteessa, suorita tämä komento.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

PortinNumero-ominaisuus näyttää nykyisen porttinumero. Muuta tarvittaessa Etätyöpöytä portti sen oletusarvo (3389) luvun takaisin tämän-komennon avulla.

    Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389

Varmista, että portti on vaihdettu 3389 tämän-komennon avulla.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

Lopeta Azure PowerShell-etäistunnon tämän-komennon avulla.

    Exit-PSSession

Varmista, että Azure AM etätyöpöydän päätepisteen on myös porttinumeroa 3398 sen sisäinen porttina. Azure-AM uudelleen ja yritä etätyöpöydän yhteyttä uudelleen.


## <a name="additional-resources"></a>Lisäresursseja

[Azure IaaS (Windows) diagnostiikka-paketti](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)

[Windowsin näennäiskoneiden Etätyöpöytä-palvelun tai salasanan nollaaminen](virtual-machines-windows-reset-rdp.md)

[Asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md)

[Koneen Azure virtual Linux-pohjaiset suojattu runko (SSH)-yhteyksien vianmääritys](virtual-machines-linux-troubleshoot-ssh-connection.md)

[Access-sovellukseen Azure virtuaalikoneen käytössä vianmääritys](virtual-machines-linux-troubleshoot-app-connection.md)
