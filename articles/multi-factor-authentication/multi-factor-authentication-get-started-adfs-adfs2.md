<properties
    pageTitle="Azure MFA-palvelimen käyttäminen AD FS 2.0 | Microsoft Azure"
    description="Tämä on Azure multi-factor authentication sivu, jossa kerrotaan, miten Azure MFA ja AD FS 2.0 käytön aloittaminen."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

# <a name="secure-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-20"></a>Cloud ja paikallisten resurssien AD FS 2.0 Azure multi-factor Authentication-palvelimen käyttäminen

Tässä artikkelissa on organisaatioissa, joissa on liitetty Azure Active Directory-hakemistosta ja haluat suojata resursseille, jotka ovat paikallisen tai pilveen. Suojaa resurssien Azure multi-factor Authentication-palvelimen avulla ja määrittämällä sen toimimaan AD FS niin, että kaksivaiheinen vahvistus on käynnistetään suuren arvon päätepisteestä.

Tässä ohjeessa kerrotaan Azure multi-factor Authentication-palvelimen käyttäminen AD FS 2.0.  Lisätietoja [Securing cloud ja paikallisten](multi-factor-authentication-get-started-adfs-w2k12.md)resurssien Azure multi-factor Authentication Server Windows Server 2012 R2 AD FS avulla.


## <a name="secure-ad-fs-20-with-a-proxy"></a>AD FS 2.0 suojaaminen välityspalvelin
Voit suojata välityspalvelimeen AD FS 2.0-Azure multi-factor Authentication Server asentaminen ADFS-välityspalvelimen ja ‑palvelin.

### <a name="configure-iis-authentication"></a>Määritä IIS-todennus

1. Siirtää Azure multi-factor Authentication-palvelimen Valitse vasemmanpuoleisessa valikossa **IIS-todennus** -kuvake.
2. Valitse **Lomakepohjaisia** -välilehti.
3. Valitse **Lisää …** painike.
<center>![Asetukset](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Tunnista käyttäjänimen ja salasanan toimialueen muuttujat automaattisesti, kirjoita Login URL-osoite (esimerkiksi https://sso.contoso.com/adfs/ls) Auto-Configure Form-Based sivuston-valintaikkunassa ja valitse sitten OK.
5. Valitse **edellytä Azure Monimenetelmäisen todentamisen käyttäjän vastaa** -valintaruutu, jos kaikki käyttäjät on poistettu tai tuodaan palvelimeen ja veloittaa kaksivaiheista vahvistusta varten. Jos merkitsevän käyttäjämäärä ei ole vielä tuotu tietoja palvelimeen ja/tai on kaksivaiheinen vahvistus on Vapauta, jätä ruutu ole valittuna. Lisätietoja toiminnosta on ohjeessa.
6. Jos sivun muuttujat ei tunnisteta automaattisesti, valitse **Määritä manuaalisesti...** painike Auto-Configure Form-Based sivuston-valintaikkunassa.
7. Add Form-Based sivuston-valintaikkunassa URL-Osoitteen kirjoittaminen ADFS-kirjautumissivulle Lähetä URL-osoite-kenttään (esimerkiksi https://sso.contoso.com/adfs/ls) ja kirjoita sovelluksen nimi (valinnainen). Sovelluksen nimen Azure Monimenetelmäisen todentamisen raporteissa ja saa näkyviin SMS tai Mobile-sovelluksen käyttöoikeuksien viesteissä. Katso lisätietoja ohjetiedoston Lähetä URL-osoite.
8. Pyyntö muotoiluksi "Julkaiseminen ja HAE."
9. Kirjoita käyttäjänimi-muuttuja (ctl00$ ContentPlaceHolder1$ UsernameTextBox) ja salasana muuttujan (ctl00$ ContentPlaceHolder1$ PasswordTextBox). Jos lomakepohjaisia kirjautumissivun näyttää toimialueen tekstiruutua, kirjoita toimialueen muuttujan sekä. Voit joutua selaimessa kirjautuminen sille sivulle, sivun hiiren kakkospainikkeella ja valitse **Näytä lähdekoodi** etsiminen Kirjaudu sisään-sivulla syötteen ruutujen nimet.
10. Valitse **edellytä Azure Monimenetelmäisen todentamisen käyttäjän vastaa** -valintaruutu, jos kaikki käyttäjät on poistettu tai tuodaan palvelimeen ja veloittaa kaksivaiheista vahvistusta varten. Jos merkitsevän käyttäjämäärä ei ole vielä tuotu tietoja palvelimeen ja/tai on kaksivaiheinen vahvistus on Vapauta, jätä ruutu ole valittuna.
<center>![Asetukset](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Valitse **Advanced...** Tarkista Lisäasetukset-painiketta. Voit määrittää asetuksia, kuten mahdollisuus valita mukautetun eston sivun, tiedoston välimuistiin onnistuneen välityspalvelimia evästeiden käyttäminen sivustossa ja valitse, miten tarkistamiseen ensisijainen tunnistetiedot.
12. ADFS-välityspalvelimen ei ole liitetty toimialueeseen todennäköisesti, voit käyttää LDAP muodostaa toimialueen ohjauskoneen käyttäjän tuonti ja vanhat todennusta varten. Advanced Form-Based sivuston-valintaikkunassa valitse **Ensisijainen todennus** -välilehti ja valitse vanhat todennus todennustyyppi **LDAP sitoa** .
13. Kun olet valmis, valitse palaa Add Form-Based sivuston-valintaikkunassa **OK** -painiketta. Katso lisätietoja ohjetiedoston Lisäasetukset.
14. Sulje valintaikkuna valitsemalla **OK** .
15. Kun URL-Osoitteen ja sivun muuttujat on havainnut tai kirjoitettu-sivustotiedot näkyvät lomakepohjaisia-paneeli.
16. Valitse **Alkuperäisessä moduuli** -välilehti ja valitse palvelimeen, siinä sivustossa, jossa ADFS-välityspalvelin on käytössä-kohdassa (kuten "oletusarvoinen Web-sivusto") tai ADFS välityspalvelimen-sovelluksen (kuten "ls", "adfs-kohdassa) käyttöön haluamasi tasolla laajennuksen IIS.
17. Valitse näytön yläreunassa **käyttöön IIS-todennus** -ruutu.
18. IIS-todennus on nyt käytössä.

### <a name="configure-directory-integration"></a>Yhteystietojen integrointi määrittäminen

IIS-todennus käyttöön, mutta ennen, että Active Directory (AD) kautta LDAP-todennuksella on määritettävä toimialueen ohjauskoneen LDAP-yhteys.

1. **Yhteystietojen integrointi** -kuvaketta.
2. Valitse asetukset-välilehdessä **Käytä tiettyä LDAP-määritys** -valintanappi.
<center>![Asetukset](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
3. Valitse **muokata...** painike.
4. Muokkaa LDAP-määritys-valintaikkunassa lisätä kenttien yhdistäminen AD-toimialueen ohjauskoneen tarvittavat tiedot. Kenttien kuvaukset sisältyvät alla olevassa taulukossa. Azure multi-factor Authentication Server ohjetiedosto sisältyy myös nämä tiedot.
5. LDAP-Testiyhteys **testi** -vaihtoehdon.
<center>![Asetukset](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
6. Jos LDAP-Yhteystesti onnistui, valitse **OK** -painiketta.

### <a name="configure-company-settings"></a>Yrityksen asetusten määrittäminen

1. Seuraavaksi **Yrityksen asetukset** -kuvaketta ja valitse **Käyttäjänimi tarkkuus** -välilehti.
2. Valitse **Käytä LDAP yksilöllinen tunnus-määrite vastaavia käyttäjänimet** -valintanappi.
3. Jos käyttäjät kirjoittavat niiden käyttäjänimi "toimialue\käyttäjänimi"-muodossa, palvelin on voi poistaa käytöstä käyttäjänimi toimialueen, kun se luo LDAP-kysely. Joka voidaan toteuttaa rekisteriasetuksen avulla.
4. Avaa Rekisterieditori ja siirry HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/positiivinen verkkojen/PhoneFactor 64-bittinen palvelimessa. Jos palvelimessa 32-bittinen, kestää "Wow6432Node" polku ulos. Luo DWORD-rekisteriavaimen nimeltä "UsernameCxz_stripPrefixDomain" ja aseta arvoksi 1. Azure Monimenetelmäisen todentamisen on nyt suojaaminen ADFS-välityspalvelimen.

Varmista, että käyttäjien on tuotu Active Directory-palvelimeen. Katso [Luotetut IP -osoitteet-osassa](#trusted-ips) , jos haluat whitelist sisäisiä IP-osoitteita siten, että kaksivaiheista vahvistusta varten ei tarvita, kun kirjaudun sisään verkkosivustoon näistä sijainneista.

<center>![Asetukset](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>AD FS 2.0 suora ilman välityspalvelin

Voit suojata AD FS, kun AD FS ‑välityspalvelin ei käytetä. Asenna Azure multi-factor Authentication Server ADFS-palvelimeen ja ‑palvelin kohti seuraavasti:

1. Siirtää Azure multi-factor Authentication-palvelimen Valitse vasemmanpuoleisessa valikossa **IIS-todennus** -kuvake.
2. Valitse **HTTP** -välilehti.
3. Valitse **Lisää …** painike.
4. Kirjoita Lisää Base URL-osoite-valintaikkuna ADFS-sivustolle, jossa HTTP-tarkistus suoritetaan (esimerkiksi https://sso.domain.com/adfs/ls/auth/integrated) Base URL-osoite-kenttään URL-osoite. Kirjoita sovelluksen nimi (valinnainen). Sovelluksen nimen Azure Monimenetelmäisen todentamisen raporteissa ja saa näkyviin SMS tai Mobile-sovelluksen käyttöoikeuksien viesteissä.
5. Halutessasi voit säätää aikakatkaisun ja suurin-ruutuun istunto.
6. Valitse **edellytä Azure Monimenetelmäisen todentamisen käyttäjän vastaa** -valintaruutu, jos kaikki käyttäjät on poistettu tai tuodaan palvelimeen ja veloittaa kaksivaiheista vahvistusta varten. Jos merkitsevän käyttäjämäärä ei ole vielä tuotu tietoja palvelimeen ja/tai on kaksivaiheinen vahvistus on Vapauta, jätä ruutu ole valittuna. Lisätietoja toiminnosta on ohjeessa.
7. Valitse eväste välimuisti-valintaruutu, jos toivottuja.
<center>![Asetukset](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Napsauta **OK** -painiketta.
9. Valitse **Alkuperäisen moduuli** -välilehti ja valitse palvelin, sivuston (esimerkiksi "oletusarvoinen Web-sivusto") tai (esimerkiksi "ls" "adfs-kohdassa) ADFS-sovelluksen käyttöön haluamasi tasolla laajennuksen IIS.
10. Valitse näytön yläreunassa **käyttöön IIS-todennus** -ruutu. Azure Monimenetelmäisen todentamisen on nyt suojaaminen ADFS.

Varmista, että käyttäjien on tuotu Active Directory-palvelimeen. On luotettu IP-osoitteet-osassa, jos haluat whitelist sisäisiä IP-osoitteita siten, että kaksivaiheista vahvistusta varten ei tarvita, kun kirjaudun sisään verkkosivustoon näistä sijainneista.


## <a name="trusted-ips"></a>Luotettu IP-osoitteet
Luotettu IP-osoitteet käyttäjät voivat ohittaa Azure Monimenetelmäisen todentamisen sivuston pyyntöjen tietyn IP-osoitteet tai aliverkosta peräisin. Esimerkiksi haluat ehkä vapautettujen käyttäjille kaksivaiheinen vahvistus on peräisin kun synkronointi Office. Voit määrittää office-aliverkon Luotetut IP-osoitteet tapahtumana.

### <a name="to-configure-trusted-ips"></a>Voit määrittää Luotetut IP-osoitteet


1. Valitse IIS-todennus-osassa **Luotetut IP -osoitteet** -välilehti.
1. Valitse **Lisää …** painike.
1. Kun Lisää Luotetut IP-osoitteet-valintaikkuna tulee näkyviin, valitsemalla jokin **Yksi IP**, **IP-alueen**tai **aliverkon** valintanapit.
1. Kirjoita IP-osoite, IP-osoitteiden alue tai aliverkon, jotka kuuluvat whitelisted. Jos aliverkon, valitse haluamasi verkkopeite ja napsauta **OK** -painiketta. Luotettu IP on nyt lisätty.


<center>![Asetukset](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>
