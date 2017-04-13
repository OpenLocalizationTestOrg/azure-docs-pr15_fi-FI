<properties
    pageTitle="Asenna Active Directory-metsää Azure virtual verkossa | Microsoft Azure"
    description="Opetusohjelma, jossa kerrotaan, miten voit luoda uuden Active Directory-metsää Azure Virtual verkossa virtual tietokoneeseen (AM)."
    services="active-directory, virtual-network"
    keywords="Active Directoryn virtuaalikoneen, asenna active Directoryn metsää azure active directory-videot "
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    tags=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Asenna uusi Active Directory-metsää Azure virtual verkossa

Tässä ohjeaiheessa esitellään, miten voit luoda uuden Windows Server Active Directory-ympäristön Azure virtual verkossa virtual machine (AM) [Azure virtual verkon](../virtual-network/virtual-networks-overview.md). Tässä tapauksessa Azure virtual network ei ole yhdistetty paikalliseen verkkoon.

Hyödyllisiä myös näiden liittyvien ohjeaiheiden:

- Videon, joka sisältää näitä ohjeita Katso [asennusohjeet uuden Active Directory-metsää Azure virtual verkossa](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
- Voit halutessasi [määrittää sivuston sivusto-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) ja sitten joko Asenna uusi metsää tai laajentaa paikallisen-metsää Azure virtual verkkoon. Näitä ohjeita on kohdassa [asentaminen replikan Active Directory-toimialueen ohjauskoneen Azure Virtual verkossa](../active-directory/active-directory-install-replica-active-directory-domain-controller.md).
-  Saat ohjeita asennuksen Active Directory-toimialueen palveluista (AD DS) Azure virtual verkossa, [käyttöönotto Windows Server Active Directory-Azuren näennäiskoneiden ohjeet](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Skenaario kaavio

Tässä skenaariossa ulkoisten käyttäjien pystyttävä käyttämään toimialueen liittyneet palvelimessa suoritettavien sovellusten. VMs, jotka suoritetaan sovelluksen palvelimet ja VMs, jotka suoritetaan toimialueen ohjaimet asennetaan asennettu omia pilvipalvelussa Azure virtual verkossa. He ovat myös parannettu vikasietoa määrittäminen saatavuus mukana.

![Active Directory metsää näennäiskoneiden Virtual Azure-verkossa käyttöön ][1] 7
## <a name="how-does-this-differ-from-on-premises"></a>Miten tämä eroaa paikallisen?

Ei ole paljon eroa toimialueen ohjauskoneen asentaminen Azure-ympäristöön vai. Seuraavassa taulukossa on lueteltu tärkeimmät erot.

Voit määrittää...  | Paikallisen  | Azure virtual verkossa
------------- | -------------  | ------------
**Toimialueen ohjauskoneen IP-osoite**  | Staattinen IP-osoitteen verkkosovittimen ominaisuuksien määrittäminen   | Suorita staattinen IP-osoitteiden määrittäminen AzureStaticVNetIP cmdlet-komento
**Asiakkaan DNS-tulkintatoiminnon**  | Asettaa ensisijainen ja Vaihtoehtoinen DNS-palvelimen osoitteen verkossa toimialuejäsenten sovittimen ominaisuudet   | Määrittää DNS-palvelimen osoite, VPN-ominaisuudet
**Active Directory-tietokannan säilön**  | Voit myös muuttaa oletuskansio C:\  | Sinun on muutettava oletuskansio C:\



## <a name="create-an-azure-virtual-network"></a>Azure virtual verkoston luominen

1. Kirjaudu Azure perinteinen-portaaliin.
2. Luo virtuaalisia verkkoon. Valitse **verkkojen** > **Luo virtuaalisia verkkoon**. Suorita ohjattu seuraavan taulukon arvot avulla.

    Tämän ohjatun toiminnon sivulla...  | Määritä nämä arvot
    ------------- | -------------
    **VPN-tiedot**  | <p>Nimi: Syötä virtual verkon nimi</p><p>Alue: Valitse lähinnä alue</p>
    **DNS- ja VPN**  | <p>Jätä DNS-palvelin</p><p>Älä valitse joko VPN-vaihtoehto</p>
    **VPN osoite välilyöntejä**  | <p>Aliverkon nimi: Kirjoita nimi käytettävissä myös aliverkon ulkopuolella</p><p>Ensimmäinen IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p>



## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>Toimialueen ohjauskoneen ja DNS server rooleja VMs luominen

Toista seuraavien ohjeiden avulla luoda VMs isännöimiseen Ohjauskoneen rooli tarpeen mukaan. Vähintään kaksi virtual ohjauskoneita vikasietoa ja redundancy tulisi ottaa käyttöön. Jos Azure virtual verkossa on vähintään kaksi ohjauskoneita, jotka on määritetty vastaavasti (eli ne ovat molemmat kokoonpanojaan Suorita DNS-palvelin ja kumpaakaan pitää FSMO-roolin ja niin edelleen), jotka suoritetaan ne ohjauskoneita parannettu vikasietoa määrittäminen saatavuus VMs Aseta.

Luomisesta VMs Windows PowerShell-toiminnolla sen sijaan, että Käyttöliittymä on artikkelissa [Azure PowerShellin Luo ja määritä Windows-pohjaisesta näennäiskoneiden valmiiksi](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. Perinteinen-portaalissa, valitse **Uusi** > **Laske** > **virtuaalikoneen** > **-Valikoima**. Käytä seuraavia arvoja ohjattu toiminto loppuun. Hyväksy oletusarvoa asetusta, ellei toinen arvo on ehdotetut tai.

    Tämän ohjatun toiminnon sivulla...  | Määritä nämä arvot
    ------------- | -------------
    **Valitse kuva**  | Windows Server 2012 R2 Palvelinkeskukseen
    **Virtuaalikoneen määritys**  | <p>Virtuaalikoneen nimi: Kirjoita Yksittäinen tarra nimi (esimerkiksi AzureDC1).</p><p>Uuden käyttäjänimi: Kirjoita käyttäjän nimi. Tämä käyttäjä saa AM paikallisen Järjestelmänvalvojat-ryhmän jäsen. Tarvitset tätä nimeä AM kirjautuminen ensimmäistä kertaa. Valmiin tili nimeltä järjestelmänvalvoja ei toimi.</p><p>Uusi salasana ja vahvista: Kirjoita salasana</p>
    **Virtuaalikoneen määritys**  | <p>Pilvipalvelussa: <b>Luo uusi pilvipalvelussa</b> ensimmäisen AM, ja valitse samaa pilvessä palvelun nimeä luodessasi Lisää VMs, joka isännöi Ohjauskoneen rooli.</p><p>Cloud palvelun DNS-nimi: Määritä yksilöivä nimi</p><p>Alue/affiniteetti ryhmän/Virtual verkon: Määritä VPN-nimen (esimerkiksi WestUSVNet).</p><p>Tallennustilan tili: Valitse <b>automaattisesti luotu tallennustilan tilin käyttäminen</b> ensimmäisen AM ja valitse sitten samaa tallennustilan tilin nimeä, kun luot Lisää VMs, joka isännöi Ohjauskoneen rooli.</p><p>Käytettävyys joukko: Valitse <b>käytettävyys-joukon luominen</b>.</p><p>Käytettävyys määritetty nimi: Kirjoita nimi käytettävyys määrittäminen kun luot ensimmäisen AM ja valitse sitten sama nimi Lisää VMs luodessasi.</p>
    **Virtuaalikoneen määritys**  | <p>Valitse <b>Asenna AM-agentti</b> ja muut tarvitset tunnisteesta.</p>
2. Liittää DVD-levyllä kunkin AM, joka suoritetaan toimialueen Ohjauskoneen rooli. Lisää disk tarvitaan AD-tietokantaan, lokit ja SYSVOL tallentamiseen. Määritä koko levyn (esimerkiksi 10 gt) ja jätä **Host välimuisti-asetus** **ei mitään**. Ohjeita on artikkelissa [tietojen DVD-levyllä, Windows Virtual Machine liittäminen](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Kun ensin kirjautua sisään AM, Avaa **Serverin hallinta** > **tiedoston ja tallennustilaa palvelujen** aseman luominen käyttämällä NTFS levyn.
4. Varaa staattinen IP-osoite, joka suoritetaan Ohjauskoneen rooli VMs varten. Voit varata staattinen IP-osoite, lataa Microsoft Web ympäristö Installer ja [Asenna PowerShellin Azure](../powershell-install-configure.md) ja suorita määrittäminen AzureStaticVNetIP cmdlet-komento. Esimerkki:

    "Get-AzureVM - palvelun nimi AzureDC1-nimen AzureDC1 | Määritä AzureStaticVNetIP - IP-osoite 10.0.0.4 | Päivitä AzureVM

Lisätietoja staattinen IP-osoitteen määrittämisestä on artikkelissa [määrittäminen staattiset sisäinen IP-osoite, AM](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-windows-server-active-directory"></a>Asenna Windows Server Active Directory

Käytä samaa säännöllisesti [AD DS: N asentaminen](https://technet.microsoft.com/library/jj574166.aspx) , käytä paikallista (toisin sanoen voit käyttää Käyttöliittymän, vastaustiedosto tai Windows PowerShellin). Sinun täytyy asentaa uusi metsää järjestelmänvalvojan tunnistetietoja. Active Directory-tietokanta, lokit ja SYSVOL paikan määrittäminen muuttaa oletuskansio käyttöjärjestelmän asemasta tiedot, jotka AM liitetty levylle.

Toimialueen Ohjauskoneen asennuksen päätyttyä AM yritysominaisuudet ja Palvelinkeskus kirjautua. Muista määrittää toimialueen tunnistetietoja.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>Palauttaa Azure virtual verkon DNS-palvelin

1. Palauttaa palvelimessa Ohjauskoneen/DNS DNS forwarder-asetus.
  1. Palvelimen hallinnan valitsemalla **Työkalut** > **DNS**.
  2. **DNS-hallintaan**ja DNS-palvelimen nimeä hiiren kakkospainikkeella ja valitse **Ominaisuudet**.
  3. **Välittäjiä** -välilehden Valitse forwarder IP-osoite ja valitse **Muokkaa**.  Valitse IP-osoite ja valitse **Poista**.
  4. Valitse **OK** ja sulje editorin ja **Ok** Sulje DNS-palvelimen ominaisuudet.
2. Päivitä virtual verkon DNS server-asetusta.
  1. Valitse **Virtual verkkojen** > luomasi virtual verkko > **Määritä** > **DNS-palvelimet**-nimen ja DIP yhden VMs, joka suoritetaan Ohjauskoneen/DNS server rooli ja valitse **Tallenna**.
  2. Valitse AM ja valitsemalla **uudelleen** käynnistettävän AM DNS-tulkintatoiminnon asetusten uuden DNS-palvelimen IP-osoite.


## <a name="create-vms-for-domain-members"></a>Luo VMs toimialueen jäsenet

1. Toista luominen VMs sovelluksen palvelinten toimii seuraavasti. Hyväksy oletusarvoa asetusta, ellei toinen arvo on ehdotetut tai.

    Tämän ohjatun toiminnon sivulla...  | Määritä nämä arvot
    ------------- | -------------
    **Valitse kuva**  | Windows Server 2012 R2 Palvelinkeskukseen
    **Virtuaalikoneen määritys**  | <p>Virtuaalikoneen nimi: Kirjoita Yksittäinen tarra nimi (esimerkiksi AppServer1).</p><p>Uuden käyttäjänimi: Kirjoita käyttäjän nimi. Tämä käyttäjä saa AM paikallisen Järjestelmänvalvojat-ryhmän jäsen. Tarvitset tätä nimeä AM kirjautuminen ensimmäistä kertaa. Valmiin tili nimeltä järjestelmänvalvoja ei toimi.</p><p>Uusi salasana ja vahvista: Kirjoita salasana</p>
    **Virtuaalikoneen määritys**  | <p>Pilvipalvelussa: **Luo uusi pilvipalvelussa** valitseminen ensimmäisen AM, ja valitse samaa pilvessä palvelun nimeä, Lisää VMs, joka isännöi sovelluksen luodessasi.</p><p>Cloud palvelun DNS-nimi: Määritä yksilöivä nimi</p><p>Alue/affiniteetti ryhmän/Virtual verkon: Määritä VPN-nimen (esimerkiksi WestUSVNet).</p><p>Tallennustilan tili: Valitse **automaattisesti luotu tallennustilan tilin käyttäminen** ensimmäisen AM ja valitse sitten samaa tallennustilan tilin nimeä, kun luot Lisää VMs, joka isännöi sovelluksen.</p><p>Käytettävyys joukko: Valitse **käytettävyys-joukon luominen**.</p><p>Käytettävyys määritetty nimi: Kirjoita nimi käytettävyys määrittäminen kun luot ensimmäisen AM ja valitse sitten sama nimi Lisää VMs luodessasi.</p>
    **Virtuaalikoneen määritys**  | <p>Valitse <b>Asenna AM-agentti</b> ja muut tarvitset tunnisteesta.</p>
2. Kun kunkin AM on valmisteltu, kirjaudu sisään ja liittyä toimialueeseen. **Palvelimen hallinta**ja valitse **Paikallisen palvelimen** > **TYÖRYHMÄN** > **Muuta...** Valitse **toimialue** ja paikallisen toimialueen nimi. Toimialueen käyttäjän tunnistetietoja ja Käynnistä AM Viimeistele toimialueen liity.

Luomisesta VMs Windows PowerShell-toiminnolla sen sijaan, että Käyttöliittymä on artikkelissa [Azure PowerShellin Luo ja määritä Windows-pohjaisesta näennäiskoneiden valmiiksi](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

Saat lisätietoja Windows PowerShellin [Azure cmdlet-komentojen käytön aloittaminen](https://msdn.microsoft.com/library/azure/jj554332.aspx) ja [Azure Cmdlet-viittaus](https://msdn.microsoft.com/library/azure/jj554330.aspx).


## <a name="see-also"></a>Katso myös

-  [Uusi Active Directory-metsää asentamisesta Azure virtual verkossa](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
-  [Ohjeet Windows Azure-Virtuaalikoneissa Server Active Directory käyttöönotto](https://msdn.microsoft.com/library/azure/jj156090.aspx)

-  [Määritä sivusto sivusto VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)
-  [Asenna replikan Active Directory toimialueen ohjauskoneen Azure virtual verkossa](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
-  [Microsoft Azure IT Pro IaaS: (01) virtuaalikoneen perusteet](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) luominen Virtual verkkojen ja paikallisen-yhteys](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [VPN-yleiskatsaus](../virtual-network/virtual-networks-overview.md)
-  [Asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure Cmdlet-viittaus](https://msdn.microsoft.com/library/azure/jj554330.aspx)
-  [Määritä Azure AM staattinen IP-osoite](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
-  [Azure AM staattinen IP määrittäminen](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
-  [Asenna uusi Active Directory-metsää](https://technet.microsoft.com/library/jj574166.aspx)
-  [Johdanto Active Directoryn toimialueen palveluiden (AD DS) Virtualization (taso 100)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
