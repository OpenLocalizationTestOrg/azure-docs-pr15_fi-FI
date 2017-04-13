<properties
    pageTitle="Asenna replikan Active Directory-toimialueen ohjauskoneen Azure | Microsoft Azure"
    description="Opetusohjelma, jossa kerrotaan, miten Asenna toimialueen ohjauskoneen paikallisen Active Directory-metsää Azure virtual-laitteeseen."
    services="virtual-network"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Asenna replikan Active Directory-toimialueen ohjauskoneen Azure virtual verkossa

Tässä ohjeaiheessa esitellään asentaminen lisätoimialuetta ohjaimet (tunnetaan myös nimellä replikan ohjauskoneita)-Azure-virtuaalikoneissa (VMs) Azure virtual verkossa paikallisen Active Directory-toimialueen osalta.

Hyödyllisiä myös näiden liittyvien ohjeaiheiden:

-  Voit asentaa myös uuden Active Directory-metsää Azure virtual verkossa. Näitä ohjeita on kohdassa [Asenna uusi Active Directory-metsää Azure virtual verkossa](../active-directory/active-directory-new-forest-virtual-machine.md).
-  Saat ohjeita asennuksen Active Directory-toimialueen palveluista (AD DS) Azure virtual verkossa, [käyttöönotto Windows Server Active Directory-Azuren näennäiskoneiden ohjeet](https://msdn.microsoft.com/library/azure/jj156090.aspx).


## <a name="scenario-diagram"></a>Skenaario-kaavio

Tässä skenaariossa ulkoisten käyttäjien pystyttävä käyttämään toimialueen liittyneet palvelimessa suoritettavien sovellusten. VMs, suorita sovelluspalvelimet ja replikan ohjauskoneet asennetaan Azure virtual verkossa. Virtual verkon voit on yhteydessä Internetiin paikallisen [sivuston sivuston VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) -yhteyden seuraavassa kaaviossa esitetyllä tavalla tai voit käyttää [ExpressRoute](../expressroute/expressroute-locations-providers.md) nopeampi yhteys.

Sovelluksen-palvelimet ja ohjauskoneet on otettu käyttöön erillinen pilvipalveluihin jakaa Laske käsittely ja parannettu vikasietoa [käytettävyys joukot](../virtual-machines/virtual-machines-windows-manage-availability.md) sisäpuolelle.
Ohjauskoneet replikoida keskenään ja paikallisen ohjauskoneita käyttämällä Active Directory-replikointi. Synkronointi ei ole työkaluja ei tarvita.

![Kaavio muistiinpanon replikan Active Directory-toimialueen ohjauskoneen Azure vnet][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Azure virtual verkon Active Directory-sivuston luominen

Se on hyvä luoda sivuston Active Directoryssa, joka edustaa vastaava verkon alueen virtual verkkoon. Auttavat optimointi todennuksen, replikoinnin ja muita Ohjauskoneen sijainti-toimintoja. Seuraavassa kerrotaan, kuinka sivuston luominen ja Lisää tausta-kohdassa [lisääminen uuteen sivustoon](https://technet.microsoft.com/library/cc781496.aspx).

1. Avaa Active Directory-sivustojen ja -palvelut: **Palvelimen hallinnan** > **Työkalut** > **Active Directory-sivustot ja -palvelut**.
2. Edustavan alue, jossa loit Azure virtual verkon sivuston luominen: Valitse **sivustot** > **toiminto** > **uuden sivuston** > uuden sivuston, kuten Azure US Länsi nimi > Valitse linkin > **OK**.
3. Luo aliverkon ja liitä uuteen sivustoon: Kaksoisnapsauta **sivustot** > hiiren kakkospainikkeella **aliverkosta** > **Uusi aliverkon** > Kirjoita IP-osoitealueita virtual verkoston (kuten 10.1.0.0/16 skenaario kaaviossa) > Uusi Azure-sivusto > **OK**.

## <a name="create-an-azure-virtual-network"></a>Azure virtual verkoston luominen

1. [Azure perinteinen portal](https://manage.windowsazure.com), valitse **Uusi** > **Verkkopalveluita** > **VPN** > **Mukautettu luominen** ja käytä seuraavia arvoja ohjattu toiminto loppuun.

    Tämän ohjatun toiminnon sivulla...  | Määritä nämä arvot
    ------------- | -------------
    **VPN-tiedot**  | <p>Nimi: Kirjoita virtual verkkoon, kuten WestUSVNet nimi.</p><p>Alue: Valitse lähinnä alue.</p>
    **DNS- ja VPN-yhteys**  | <p>DNS-palvelimet: Anna nimi ja vähintään yhden paikallisen DNS-palvelimilta IP-osoite.</p><p>Yhteys: Valitse **Määritä sivusto sivusto VPN-yhteyttä**.</p><p>Paikalliseen verkkoon kirjauduttaessa: Määritä uusi paikalliseen verkkoon.</p><p>Jos käytät ExpressRoute sijaan VPN-kohdassa [Configure ExpressRoute-yhteyden Exchange-palvelun kautta](../expressroute/expressroute-locations-providers.md).</p>
    **Sivusto yhteys**  | <p>Nimi: Kirjoita paikallisen verkon nimi.</p><p>VPN-laitteen IP-osoite: Määritä laitteella, joka muodostaa yhteyden virtual verkkoon julkiseen IP-osoite. VPN-laite ei löydy, takana tulkintatoiminnon</p><p>Osoite: Määritä osoitealueet (kuten 192.168.0.0/16 skenaarion kaaviossa) paikallista verkkoa varten.</p>
    **VPN osoite välilyöntejä**  | <p>Osoitetilan: Määritä IP-osoitealueita VMs, jonka haluat suorittaa Azure virtual verkon (kuten 10.1.0.0/16 skenaarion kaaviossa). Tämä osoitealueita ei voi olla paikallisen verkoston osoitealueet.</p><p>Aliverkosta: Määritä nimi ja osoite sovellus palvelimia aliverkon (kuten edusta-10.1.1.0/24) ja ohjauskoneet (taustatietokannan, kuten 10.1.2.0/24).</p><p>Valitse **Lisää yhdyskäytävän aliverkon**.</p>

2. Seuraavaksi määrittää suojattu sivusto sivusto VPN-yhteyden luominen VPN-yhdyskäytävä. Lisätietoja on kohdassa [Configure Virtual yhdyskäytävien](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) ohjeita.
3. Luo uusi virtual verkko-ja paikallisen VPN-laite sivusto sivusto VPN-yhteyden. Lisätietoja on kohdassa [Configure Virtual yhdyskäytävien](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) ohjeita.


## <a name="create-azure-vms-for-the-dc-roles"></a>Luo Azure VMs Ohjauskoneen roolit

Toista seuraavien ohjeiden avulla luoda VMs isännöimiseen Ohjauskoneen rooli tarpeen mukaan. Vähintään kaksi virtual ohjauskoneita vikasietoa ja redundancy kannattaa ottaa käyttöön. Jos Azure virtual verkossa on vähintään kaksi ohjauskoneita, jotka on määritetty vastaavasti (eli ne ovat molemmat kokoonpanojaan Suorita DNS-palvelin ja kumpaakaan pitää FSMO-roolin ja niin edelleen), jotka suoritetaan ne ohjauskoneita parannettu vikasietoa määrittäminen saatavuus VMs Aseta.
Luomisesta VMs Windows PowerShell-toiminnolla sen sijaan, että Käyttöliittymä on artikkelissa [Azure PowerShellin Luo ja määritä Windows-pohjaisesta näennäiskoneiden valmiiksi](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. [Azure perinteinen portal](https://manage.windowsazure.com), valitse **Uusi** > **Laske** > **virtuaalikoneen** > **-Valikoima**. Käytä seuraavia arvoja ohjattu toiminto loppuun. Hyväksy oletusarvoa asetusta, ellei toinen arvo on ehdotetut tai.

    Tämän ohjatun toiminnon sivulla...  | Määritä nämä arvot
    ------------- | -------------
    **Valitse kuva**  | Windows Server 2012 R2 Palvelinkeskukseen
    **Virtuaalikoneen määritys**  | <p>Virtuaalikoneen nimi: Kirjoita Yksittäinen tarra nimi (esimerkiksi AzureDC1).</p><p>Uuden käyttäjänimi: Kirjoita käyttäjän nimi. Tämä käyttäjä saa AM paikallisen Järjestelmänvalvojat-ryhmän jäsen. Tarvitset tätä nimeä AM kirjautuminen ensimmäistä kertaa. Valmiin tili nimeltä järjestelmänvalvoja ei toimi.</p><p>Uusi salasana ja vahvista: Kirjoita salasana</p>
    **Virtuaalikoneen määritys**  | <p>Pilvipalvelussa: Valitseminen ensimmäisen AM <b>Luo uusi pilvipalvelussa</b> , ja valitse samaa pilvessä palvelun nimeä, kun luot Lisää VMs, joka isännöi Ohjauskoneen rooli.</p><p>Cloud palvelun DNS-nimi: Määritä yksilöivä nimi</p><p>Alue/affiniteetti ryhmän/Virtual verkon: Määritä VPN-nimen (esimerkiksi WestUSVNet).</p><p>Tallennustilan tili: Valitse <b>automaattisesti luotu tallennustilan tilin käyttäminen</b> ensimmäisen AM ja valitse sitten samaa tallennustilan tilin nimeä, kun luot Lisää VMs, joka isännöi Ohjauskoneen rooli.</p><p>Käytettävyys joukko: Valitse <b>käytettävyys-joukon luominen</b>.</p><p>Käytettävyys määritetty nimi: Kirjoita nimi käytettävyys määrittäminen kun luot ensimmäisen AM ja valitse sitten sama nimi Lisää VMs luodessasi.</p>
    **Virtuaalikoneen määritys**  | <p>Valitse <b>Asenna AM-agentti</b> ja muut tarvitset tunnisteet.</p>
2. Liittää DVD-levyllä kunkin AM, joka suoritetaan toimialueen Ohjauskoneen rooli. Lisää disk tarvitaan AD-tietokantaan, lokit ja SYSVOL tallentamiseen. Määritä koko levyn (esimerkiksi 10 gt) ja jätä **Host välimuisti-asetus** **ei mitään**. Ohjeita on artikkelissa [tietojen DVD-levyllä, Windows Virtual Machine liittäminen](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Kun ensin kirjautua sisään AM, Avaa **Serverin hallinta** > **tiedoston ja tallennustilaa palvelujen** aseman luominen käyttämällä NTFS levyn.
4. Varaa staattinen IP-osoite, joka suoritetaan Ohjauskoneen rooli VMs varten. Voit varata staattinen IP-osoite, lataa Microsoft Web ympäristö Installer ja [Asenna PowerShellin Azure](../powershell-install-configure.md) ja suorita määrittäminen AzureStaticVNetIP cmdlet-komento. Esimerkki:

    "Get-AzureVM - palvelun nimi AzureDC1-nimen AzureDC1 | Määritä AzureStaticVNetIP - IP-osoite 10.0.0.4 | Päivitä AzureVM

Lisätietoja staattinen IP-osoitteen määrittämisestä on artikkelissa [määrittäminen staattinen sisäinen IP-osoite, AM](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Azure VMs AD DS: N asentaminen

Kirjaudu sisään AM ja tarkista on yhteys yli resurssien sivusto sivusto VPN- tai ExpressRoute yhteys paikalliseen verkossa. Asenna Azure VMs AD DS: ÄÄN. Voit käyttää samoja ohjeita, jonka avulla muut Ohjauskoneen asentaminen paikallisen verkon (UI, Windows PowerShellin tai vastaustiedosto). Kun olet asentanut AD DS: ÄÄN, varmista, että uuden sijainnin AD-tietokantaan, lokit ja SYSVOL äänenvoimakkuutta. Jos tarvitset perusvalintakyselyiden AD DS: N asennuksen, tutustu [Asenna Active Directory-toimialueen palveluista (taso 100)](https://technet.microsoft.com/library/hh472162.aspx) tai jos [asennat replikan Windows Server 2012: n toimialueen ohjauskoneen aiemmin toimialueen (taso 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>Määritä virtual verkon DNS-palvelin

1. [Azure perinteinen portal](https://manage.windowsazure.com)virtual verkon nimi ja valitse sitten **Määritä** välilehteä käyttämään myönnetyt se ohjauskoneita sen sijaan, että paikallisen DNS-palvelimilta IP-osoitteiden staattinen IP-osoitteiden [määrittäminen uudelleen DNS-palvelinten IP-osoitteet virtual verkkoa varten](../virtual-network/virtual-networks-manage-dns-in-vnet.md) .

2. Voit varmistaa, että kaikki se Ohjauskoneen VMs virtual verkossa on määritetty käyttämään DNS-palvelimet virtual verkossa, **näennäiskoneiden**, valitse tila-sarakkeessa kunkin AM ja valitse sitten **uudelleen**. Odota, kunnes AM näyttää **käytössä** -tilan, ennen kuin yrität kirjautua siihen.

## <a name="create-vms-for-application-servers"></a>Luo VMs sovelluksen palvelimia

1. Toista luominen VMs sovelluksen palvelinten toimii seuraavasti. Hyväksy oletusarvoa asetusta, ellei toinen arvo on ehdotetut tai.

    Tämän ohjatun toiminnon sivulla...  | Määritä nämä arvot
    ------------- | -------------
    **Valitse kuva**  | Windows Server 2012 R2 Palvelinkeskukseen
    **Virtuaalikoneen määritys**  | <p>Virtuaalikoneen nimi: Kirjoita Yksittäinen tarra nimi (esimerkiksi AppServer1).</p><p>Uuden käyttäjänimi: Kirjoita käyttäjän nimi. Tämä käyttäjä saa AM paikallisen Järjestelmänvalvojat-ryhmän jäsen. Tarvitset tätä nimeä AM kirjautuminen ensimmäistä kertaa. Valmiin tili nimeltä järjestelmänvalvoja ei toimi.</p><p>Uusi salasana ja vahvista: Kirjoita salasana</p>
    **Virtuaalikoneen määritys**  | <p>Pilvipalvelussa: **Luo uusi pilvipalvelussa** valitseminen ensimmäisen AM, ja valitse samaa pilvessä palvelun nimeä, Lisää VMs, joka isännöi sovelluksen luodessasi.</p><p>Cloud palvelun DNS-nimi: Määritä yksilöivä nimi</p><p>Alue/affiniteetti ryhmän/Virtual verkon: Määritä VPN-nimen (esimerkiksi WestUSVNet).</p><p>Tallennustilan tili: Valitse **automaattisesti luotu tallennustilan tilin käyttäminen** ensimmäisen AM ja valitse sitten samaa tallennustilan tilin nimeä, kun luot Lisää VMs, joka isännöi sovelluksen.</p><p>Käytettävyys joukko: Valitse **käytettävyys-joukon luominen**.</p><p>Käytettävyys määritetty nimi: Kirjoita nimi käytettävyys määrittäminen kun luot ensimmäisen AM ja valitse sitten sama nimi Lisää VMs luodessasi.</p>
    **Virtuaalikoneen määritys**  | <p>Valitse <b>Asenna AM-agentti</b> ja muut tarvitset tunnisteet.</p>

2. Kun kunkin AM on valmisteltu, kirjaudu sisään ja liittyä toimialueeseen. **Palvelimen hallinta**ja valitse **Paikallisen palvelimen** > **TYÖRYHMÄN** > **Muuta...** Valitse **toimialue** ja paikallisen toimialueen nimi. Toimialueen käyttäjän tunnistetietoja ja Käynnistä AM Viimeistele toimialueen liity.

Luomisesta VMs Windows PowerShell-toiminnolla sen sijaan, että Käyttöliittymä on artikkelissa [Azure PowerShellin Luo ja määritä Windows-pohjaisesta näennäiskoneiden valmiiksi](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

Saat lisätietoja Windows PowerShellin [Azure cmdlet-komentojen käytön aloittaminen](https://msdn.microsoft.com/library/azure/jj554332.aspx) ja [Azure Cmdlet-viittaus](https://msdn.microsoft.com/library/azure/jj554330.aspx).

## <a name="additional-resources"></a>Lisäresursseja

-  [Ohjeet Windows Azure-Virtuaalikoneissa Server Active Directory käyttöönotto](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [Olemassa olevat paikallisen Hyper-V toimialueen ohjaimet lataaminen Azure Azure PowerShell-toiminnolla](http://support.microsoft.com/kb/2904015)
-  [Asenna uusi Active Directory-metsää Azure virtual verkossa](../active-directory/active-directory-new-forest-virtual-machine.md)
-  [Azure Virtual verkossa](../virtual-network/virtual-networks-overview.md)
-  [Microsoft Azure IT Pro IaaS: (01) virtuaalikoneen perusteet](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) luominen Virtual verkkojen ja paikallisen-yhteys](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure hallinnan cmdlet-komennot](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
