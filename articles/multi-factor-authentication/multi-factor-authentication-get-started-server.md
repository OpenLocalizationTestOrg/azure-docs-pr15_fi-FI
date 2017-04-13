<properties 
    pageTitle="Azure multi-factor Authentication Serverin käytön aloittaminen"
    description="Tämä on Azure multi-factor authentication sivu, jossa kerrotaan, miten pääset alkuun Azure MFA-palvelimen kanssa."
    services="multi-factor-authentication"
    keywords="todennus server azure kerroin todennus sovelluksen aktivointi Monisivuinen, todennus palvelimen lataaminen"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Azure multi-factor Authentication Serverin käytön aloittaminen




<center>![Cloud](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Nyt kun onko käyttämään paikallista monimenetelmäisen todentamisen selvittänyt, aloitetaan siirtyminen. Tällä sivulla kerrotaan uuden asennuksen palvelimen ja käytön asennuksen paikallisen Active Directory-hakemistosta. Jos sinulla on jo asennettu PhoneFactor server ja päivittää on artikkelissa [Azure multi-factor palvelimeen päivittäminen](multi-factor-authentication-get-started-server-upgrade.md) näyttöä tai jos etsit tietoja asentaminen vain web-palvelu on [Azure multi-factor Authentication Server Mobile App verkkopalvelun käyttöönotto](multi-factor-authentication-get-started-server-webservice.md).


## <a name="download-the-azure-multi-factor-authentication-server"></a>Lataa Azure multi-factor Authentication-palvelin



Kahdella eri tavalla, voit ladata Azure multi-factor Authentication Server on. Molemmat tehdään Azure portaalin kautta. Ensimmäinen on hallinta multi-factor Auth palveluntarjoajan suoraan. Toinen on Palveluasetukset. Toinen vaihtoehto edellyttää multi-factor Auth palveluntarjoaja tai Azure MFA, Azure AD Premium tai yrityksen Mobility ohjelmiston käyttöoikeuden.


### <a name="to-download-the-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Voit ladata Azure Monimenetelmäisen todentamisen server Azure-portaalista
--------------------------------------------------------------------------------

1. Kirjaudu Azure-portaaliin järjestelmänvalvojana.
2. Valitse vasemmassa reunassa Active Directorysta.
3. Valitse Active Directory-sivun yläreunassa **Multi-Factor Auth tarjoajat**
4. Valitse **hallinta**
5. Tämä avaa uuden sivun.  Valitse **lataukset.** 
 ![Lataaminen](./media/multi-factor-authentication-sdk/download.png)
6. Napsauta **Luo aktivointi tunnistetiedot**yläpuolella **lataaminen.** 
 ![Lataaminen](./media/multi-factor-authentication-get-started-server/download4.png)
7. Tallenna ladattava.



### <a name="to-download-the-azure-multi-factor-authentication-server-via-the-service-settings"></a>Voit ladata Azure multi-factor Authentication palvelimen kautta Palveluasetukset


1. Kirjaudu Azure-portaaliin järjestelmänvalvojana.
2. Valitse vasemmassa reunassa Active Directorysta.
3. Kaksoisnapsauta oman Azure AD-esiintymässä.
4. Valitse yläosasta kohta **määrittäminen**
![lataaminen](./media/multi-factor-authentication-sdk/download2.png)
5. Valitse monimenetelmäisen todentamisen **palveluasetusten hallinta**
6. Valitse palvelut asetukset-sivun näytön alareunassa valitsemalla **-portaalista**.
![Lataa](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Tämä avaa uuden sivun. Valitse **lataukset.**
8. Napsauta **Luo aktivointi tunnistetiedot**yläpuolella **lataaminen.**
9. Tallenna ladattava.




## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>Azure multi-factor Authentication-palvelimen asentaminen ja määrittäminen
Nyt kun olet ladannut palvelimeen, voit asentaa ja määrittää sen.  Varmista, että palvelin, oletko asentamassa se täyttää seuraavat vaatimukset:



Azure Monimenetelmäisen todentamisen palvelinvaatimukset|Kuvaus|
:------------- | :------------- |
Laite|<li>200 Mt vapaata kiintolevytilaa</li><li>x32 tai x64 suorittimen</li><li>Vähintään 1 gt tai suurempi RAM-Muistia</li>
Ohjelmiston|<li>Windows Server 2008: n tai suurempi, jos isäntä palvelimen OS</li><li>Windows 7 tai uudempi, jos isäntä asiakkaan OS</li><li>Microsoft .NET 4.0 Framework</li><li>IIS 7.0 tai suurempi, jos käyttäjä portal tai WWW-palvelun asentaminen SDK-paketissa</li>

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Azure multi-factor Authentication palomuurin palvelinvaatimukset
--------------------------------------------------------------------------------
Kunkin MFA-palvelin edellyttää, että pysty vaihtamaan porttiin 443 lähtevä seuraavasti:

- https://pfd.phonefactor.NET
- https://pfd2.phonefactor.NET
- https://CSS.phonefactor.NET

Jos lähtevä palomuurit on rajoitettu porttiin 443, seuraavia IP-osoitealueet on voi avata:

IP-osoitteiden|Verkkopeite|IP-alue
:------------- | :------------- | :------------- |
134.170.116.0/25|255.255.255.128|134.170.116.1 – 134.170.116.126
134.170.165.0/25|255.255.255.128|134.170.165.1 – 134.170.165.126
70.37.154.128/25|255.255.255.128|70.37.154.129 – 70.37.154.254

Jos et käytä Azure multi-factor Authentication tapahtuman vahvistus ominaisuudet ja käyttäjät eivät todennustapa kanssa multi-factor Auth mobiilisovellukset laitteilla yritysverkon IP alueiden voidaan pienentää seuraavankaltaiselta:


IP-osoitteiden|Verkkopeite|IP-alue
:------------- | :------------- | :------------- |
134.170.116.72/29|255.255.255.248|134.170.116.72 – 134.170.116.79
134.170.165.72/29|255.255.255.248|134.170.165.72 – 134.170.165.79
70.37.154.200/29|255.255.255.248|70.37.154.201 – 70.37.154.206


### <a name="to-install-and-configure-the-azure-multi-factor-authentication-server"></a>Asenna ja Määritä Azure multi-factor Authentication-palvelin
--------------------------------------------------------------------------------


1. Kaksoisnapsauta suoritettavan tiedoston. Tämä aloittaa asennuksen.
2. Valitse asennuksen kansio näytössä Varmista, että kansion ovat oikein ja sitten Seuraava.
3. Kun asennus on valmis, valitse Valmis.  Tämä käynnistää ohjatun määritystoiminnon.
4. Valitse ohjatun määritystoiminnon aloitusnäyttö, laita **Ohita käytettäessä ohjattua todennus** ja valitse **Seuraava**.  Tämä Sulje ohjattu toiminto ja Käynnistä palvelin.
    ![Cloud](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Takaisin käyttöön, jotka on ladattu palvelimeen-sivu valitsemalla **Luo aktivointi tunnistetiedot** .  Kopioi nämä tiedot ruutuihin Azure MFA-palvelimeen, ja valitse **Aktivoi**.


Edellä kuvatut toimet Näytä pika-asennus ohjatun määritystoiminnon avulla.  Voit suorittaa todennusta ohjatun toiminnon uudelleen valitsemalla Työkalut-palvelimeen.



##<a name="import-users-from-active-directory"></a>Käyttäjien tuominen Active Directory

Nyt, että palvelin on asennettu ja määritetty voit nopeasti tuoda käyttäjät Azure MFA-palvelin.

### <a name="to-import-users-from-active-directory"></a>Käyttäjien tuominen Active Directorysta
--------------------------------------------------------------------------------


1. Valitse vasemmalla olevasta Azure-MFA-palvelimen **käyttäjille**.
2. Ja valitse **Tuo Active Directorysta**.
3. Nyt voit etsiä yksittäisiä käyttäjiä tai etsiä organisaatioyksiköiden Active directory-käyttäjien kanssa.  Tässä tapauksessa emme määrittää käyttäjät OU.
4. Korosta kaikki käyttäjät oikealla ja sitten **Tuo**.  Näyttöön tulee ponnahdusikkuna, joka ilmoittaa, että sinulla on onnistunut.  Sulje tuonti-ikkuna.

![Cloud](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Käyttäjien lähettää sähköpostia
Nyt kun olet tuonut käyttäjien Azure multi-factor Authentication-palvelimeen, se on suositeltavaa lähettää käyttäjille, joka ilmoittaa, että ne on rekisteröity monimenetelmäisen todentamisen sähköpostia.

Azure multi-factor Authentication-palvelimen kanssa on monia tapoja käyttämällä monimenetelmäisen todentamisen käyttäjien määrittämiseksi.  Esimerkiksi jos tiedät käyttäjien puhelinnumeroita tai pystyivät puhelinnumerot tuominen niiden yrityksen hakemistosta Azure multi-factor Authentication-palvelimeen, sähköpostin takaa käyttäjille, että ne on määritetty Azure Monimenetelmäisen todentamisen, sisältää joitakin ohjeita käyttämällä Azure Monimenetelmäisen todentamisen ja ilmoittaa puhelinnumero, he saavat niiden välityspalvelimia käyttäjälle.  

Sähköpostiviestin sisältö vaihtelevat sen mukaan, todennusmenetelmän, joka on määritetty käyttäjälle (esimerkiksi puhelu, SMS-mobiilisovelluksen).  Esimerkiksi jos käyttäjä on tarvitse käyttää PIN-tunnusta, kun ne todentavat, sähköpostin kertoo ne mitä niiden alkuperäinen PIN-koodi on määritetty.  Käyttäjien tarvitaan yleensä muuttaa niiden PIN-tunnuksen niiden ensimmäisen todennuksen aikana.

Jos käyttäjien puhelinnumerot ei ole määritetty tai tuotu Azure multi-factor Authentication-palvelimeen tai käyttäjät voivat esimääritettyjä mobile-sovelluksen käyttö todennusta varten, voit lähettää heille sähköpostiviestin, joka mahdollistaa, että ne on määritetty käyttämään Azure Monimenetelmäisen todentamisen se ohjata ne kautta Azure multi-factor Authentication käyttäjän portaalin tilin niiden rekisteröinti suorittamiseen.  Hyperlinkin sisällytetään, että käyttäjä napsauttaa avataksesi käyttäjän portaalin. Kun käyttäjä napsauttaa napsauttamalla hyperlinkkiä, selaimella Avaa ja ottaa ne yhtiön Azure multi-factor Authentication käyttäjän portaaliin.   


### <a name="configuring-email-and-email-templates"></a>Sähköpostin ja sähköpostimalleja määrittäminen

Valitsemalla vasemmalla sähköposti-kuvaketta, voit määrittää asetukset nämä sähköpostitse lähettämistä varten.  Tämä on kohtaa, johon voit kirjoittaa postipalvelin SMTP-tiedot ja sen avulla voit lähettää puitetilauksen leveä sähköpostin lähettämisen sekin lisäämällä kyseisiin käyttäjät-valintaruudun valinta.

![Sähköpostiasetukset](./media/multi-factor-authentication-get-started-server/email1.png)

Sähköpostin sisältö-välilehdessä näet kaikki eri sähköpostimalleja, jotka ovat käytettävissä valittavana.  Sen mukaan, miten olet määrittänyt multi-factor authentication-käyttäjille, voit valita mallin niin, että parhaiten voit.

![Sähköpostimalleja](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Miten Azure multi-factor Authentication Server käsittelee käyttäjätiedot

Käytettäessä multi-factor Authentication (MFA) palvelimen paikallisen käyttäjän tiedot on tallennettu paikallisen-palvelimiin. Ei ole pysyvä käyttäjätiedot tallennetaan pilveen. Kun käyttäjä suorittaa kaksiosainen todentamismenetelmä, MFA-palvelin lähettää tiedot Azure MFA pilvipalvelussa suorittaa todennusta. Kun nämä todennus-pyynnöt lähetetään pilvipalveluun, seuraavat kentät lähetetään pyyntö ja lokit niin, että ne ovat käytettävissä asiakkaan todennus-ja käyttöraportteja. Jotkin kentät ovat valinnaisia, jotta niitä voi ottaa käyttöön tai poistaa käytöstä multi-factor Authentication Serverissä. MFA-palvelimesta viestintä MFA pilvipalveluun käyttää SSL/TLS porttiin 443 lähtevä päälle. Nämä kentät ovat:

- Yksilöllinen tunnus - käyttäjänimi tai sisäinen MFA tunnus
- Ensimmäinen ja Sukunimi - valinnainen
- Sähköpostiosoite - valinnainen
- Puhelinnumero – kun tekevät äänipuhelu tai SMS-todennus
- Laitteen tunnuksen – kun tekevät mobiilisovelluksen todennus
- Todennustila
- Todennus-tulos
- MFA-palvelimen nimi
- MFA-palvelimen IP
- Asiakkaan IP – jos sellainen on käytettävissä



Edellä mainitut kentät todennus tulos (success/eston)- ja minkä tahansa denials syy on myös tallennetut todentamistiedot kanssa ja todennus-ja käyttöraportit kautta.


## <a name="advanced-azure-multi-factor-authentication-server-configurations"></a>Azure Monimenetelmäisen todentamisen palvelimen määrityksiä Lisäasetukset
Lisätietoja, valitse Lisäasetukset ja määritystietoja Käytä alla olevaa taulukkoa.

Menetelmä|Kuvaus
:------------- | :------------- |
[Käyttäjän Portal](multi-factor-authentication-get-started-portal.md)|  Tietoja määritys ja käyttöönotto sekä käyttäjien Omatoiminen käyttäjän portaalin määrittäminen.
[Active Directory Federation-palvelu](multi-factor-authentication-get-started-adfs.md)|Tietoja Azure Monimenetelmäisen todentamisen AD FS määrittämisestä.
[SÄDE-todennus](multi-factor-authentication-get-started-server-radius.md)|  Tietoja määritys ja jonka SÄDE Azure MFA-palvelimen määrittäminen.
[IIS-todennus](multi-factor-authentication-get-started-server-iis.md)|Tietoja määritys ja Azure MFA-palvelimen määrittäminen IIS: N kanssa.
[Windows-todennus](multi-factor-authentication-get-started-server-windows.md)|  Tietoja määritys ja Azure MFA-palvelimen määrittäminen Windows-todennuksen kanssa.
[LDAP-todennus](multi-factor-authentication-get-started-server-ldap.md)|Tietoja määritys ja Azure MFA-palvelimen määrittäminen LDAP-todennuksen kanssa.
[Työpöydän yhdyskäytävän ja Azure multi-factor Authentication Server RADIUS käyttäminen](multi-factor-authentication-get-started-server-rdg.md)|  Tietoja määritys ja Azure MFA-palvelimen määrittäminen Remote Desktop Gatewayn RADIUS kanssa.
[Windows Server Active Directory-synkronointiin](multi-factor-authentication-get-started-server-dirint.md)|Tietoja asetukset ja määrittämisestä Active Directory ja Azure MFA-palvelimen välisen synkronoinnin.
[Käyttöönotto Azure Monimenetelmäisen todentamisen Server Mobile sovelluksen Web-palvelu](multi-factor-authentication-get-started-server-webservice.md)|Valitse asetukset ja Azure MFA palvelimen WWW-palvelun määrittäminen tiedot.
