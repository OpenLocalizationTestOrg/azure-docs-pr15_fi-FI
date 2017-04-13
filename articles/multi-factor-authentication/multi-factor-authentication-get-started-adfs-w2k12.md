<properties
    pageTitle="MFA-palvelimeen, Windows Server 2012 R2 AD FS | Microsoft Azure"
    description="Tässä artikkelissa käsitellään Azure Monimenetelmäisen todentamisen ja AD FS Windows Server 2012 R2: n käytön aloittaminen."
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


# <a name="secure-your-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-in-windows-server-2012-r2"></a>Suojattu käyttäen Azure multi-factor Authentication Server AD FS Windows Server 2012 R2 cloud ja paikallisten resurssien

Jos käytät Active Directory Federation Services (AD FS) ja suojattu cloud tai paikallisen resursseja, voit määrittää Azure multi-factor Authentication Server AD FS-käyttöä varten. Tässä määrityksessä käynnistää kaksivaiheinen vahvistus on suuri arvo päätepisteiden.

Tässä artikkelissa käsiteltävät aiheet Azure multi-factor Authentication Server käyttäminen AD FS Windows Server 2012 R2. Lisätietoja Lue, kuinka [suojatun cloud ja paikallisten](multi-factor-authentication-get-started-adfs-adfs2.md)resurssien Azure multi-factor Authentication Serverin AD FS 2.0 avulla.

## <a name="secure-windows-server-2012-r2-ad-fs-with-azure-multi-factor-authentication-server"></a>Suojattu Windows Server 2012 R2 AD FS Azure multi-factor Authentication-palvelimen kanssa

Kun olet asentanut Azure multi-factor Authentication Server, sinulla on seuraavat vaihtoehdot:

- Asentaminen samaan palvelimeen, jossa AD FS Azure multi-factor Authentication Server paikallisesti
- Asenna Azure Monimenetelmäisen todentamisen paikallisesti AD FS ‑palvelin ja asenna sitten toiseen tietokoneeseen multi-factor Authentication Server

Ennen kuin aloitat, ota huomioon seuraavat tiedot:

- Sinun ei tarvitse Azure multi-factor Authentication Server asentaminen ADFS-palvelimeen. Sinun on asennettava multi-factor Authentication-sovittimen AD FS Windows Server 2012 R2-järjestelmästä, jossa on käytössä AD FS. Voit asentaa palvelimen toiseen tietokoneeseen, jos se on tuettu versio ja asennat AD FS-sovittimen erikseen AD FS-liittoutumispalvelinten palvelimeen. Katso lisätietoja sovittimen erikseen noudattamalla seuraavia ohjeita.

- MFA-palvelimen AD FS-sovittimen on suunniteltu, kun se on odotettavissa, että AD FS voi välittää nimi varmenteen käyttäjän osapuolen sovittimen. Varmenteen käyttäjän osapuolen nimen voinut sitten käytettävä sovelluksen nimi. Kuitenkin tämä käytössä ei voi olla. Jos organisaatiossasi on käytössä, tekstiviesti- ja mobiilisovelluksen vahvistus kautta, yrityksen asetuksissa määritetyn mukaisesti merkkijonot sisältää paikkamerkin, <$*sovelluksen_nimi*$>. Tämän paikkamerkin ei automaattisesti korvata AD FS-sovittimen käytettäessä. On suositeltavaa, että poistat paikkamerkin tarvittavat merkkijonot Kun suojaat AD FS.

- Tili, jolla kirjaudut on oltava käyttöoikeusryhmät luominen Active Directory-palvelun käyttöoikeudet.

- Monimenetelmäisen todentamisen AD FS sovittimen ohjattu asennus luo käyttöoikeusryhmän kutsutaan PhoneFactor järjestelmänvalvojat Active Directory-esiintymässä. Se Lisää AD FS-palvelutilin federation palvelun tässä ryhmässä. On suositeltavaa, että tarkistat toimialueen ohjauskoneen, että PhoneFactor Järjestelmänvalvojat-ryhmän varmasti luodaan ja AD FS palvelun tili on ryhmän jäsen. Tarvittaessa lisätä AD FS-palvelutilin manuaalisesti toimialueen ohjauskoneen PhoneFactor Järjestelmänvalvojat-ryhmään.

- Lue lisätietoja WWW-palvelun SDK-asennuksen käyttäjän portaalin [käyttöönotto käyttäjän portaalin Azure multi-factor Authentication-palvelin.](multi-factor-authentication-get-started-portal.md)


### <a name="install-azure-multi-factor-authentication-server-locally-on-the-ad-fs-server"></a>Asenna Azure multi-factor Authentication Server paikallisesti AD FS-palvelimeen

1. Lataa ja asenna Azure multi-factor Authentication Server ADFS-palvelimeen. Asennusten tiedot lisätietoja [Azure multi-factor Authentication Serverin käytön aloittaminen](multi-factor-authentication-get-started-server.md).
2. Azure multi-factor Authentication Server management-konsolin **AD FS** -kuvaketta ja valitse sitten **Salli käyttäjien rekisteröinti** ja **Valitse Salli käyttäjien**asetukset.
3. Valitse muut asetukset haluat määrittää omalle organisaatiollesi.
4. Valitse **Asenna AD FS sovittimen**.
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Jos Active Directory-ikkunassa on näkyvissä, siis kaksi asiaa. Tietokone on liitetty toimialueeseen ja AD FS-sovittimen ja multi-factor Authentication-palvelun välisen suojaamiseen Active Directory-määritys ei ole täydellinen. Valitse **Seuraava** automaattisesti Viimeistele määritysten tai valitse **Ohita automaattinen Active Directory-määritys ja asetusten määrittäminen manuaalisesti** valintaruutu ja valitse sitten **Seuraava**.
6. Jos näyttöön tulee paikallisen windows, siis kaksi asiaa. Tietokoneeseen ei ole liitetty toimialue ja suojaaminen AD FS-sovittimen ja multi-factor Authentication-palvelun välisen paikallisen ryhmän määritystä ei ole täydellinen. Valitse **Seuraava** automaattisesti Viimeistele määritysten tai valitse **Ohita automaattinen paikallinen ryhmä-määritys ja asetusten määrittäminen manuaalisesti** valintaruutu ja valitse sitten **Seuraava**.
7. Valitse ohjatun asennuksen valitsemalla **Seuraava**. Azure multi-factor Authentication Server luo PhoneFactor Järjestelmänvalvojat-ryhmän ja Lisää AD FS-palvelutilin PhoneFactor Järjestelmänvalvojat-ryhmään.
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. Valitse **Käynnistä Installer** -sivulla **Seuraava**.
9. Monimenetelmäisen todentamisen AD FS sovittimen asennusohjelman Valitse **Seuraava**.
10. Kun asennus on valmis, valitse **Sulje** .
11. Kun sovittimen on asennettu, sinun on rekisteröitävä AD FS. Avaa Windows PowerShellin ja suorittamalla seuraavan komennon:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
   <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Käyttämään juuri rekisteröity sovittimen Muokkaa AD FS yleinen todennus-käytäntö. Siirry AD FS-hallintakonsoli **Todennus käytännöt** -solmun. Valitse **Multi-factor Authentication** -osassa kohta **Yleiset asetukset** -kohdan vieressä olevaa **Muokkaa** -linkkiä. **Käytännön yleinen käyttöoikeuksien muokkaaminen** -ikkunassa Valitse **Monimenetelmäisen todentamisen** muita todentamismenetelmän ja valitse sitten **OK**. Sovittimen on rekisteröity WindowsAzureMultiFactorAuthentication. Käynnistä voimaan rekisteröinnin AD FS-palvelu uudelleen.

<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

Tässä vaiheessa multi-factor Authentication Server on määritetty on muita käyttöoikeustarkistuspalvelun AD FS käytettäväksi.

## <a name="install-a-standalone-instance-of-the-ad-fs-adapter-by-using-the-web-service-sdk"></a>Asentaa AD FS-sovittimen yksittäisen esiintymän SDK WWW-palvelun avulla
1. Web-palvelu SDK asentaminen multi-factor Authentication Serveriä suorittava palvelin.
2. Kopioi sitten seuraavat tiedostot \Program Files\Multi-palvelimeen, johon haluat asentaa AD FS-sovittimen kerroin todennus Server-hakemisto:
  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Rekisteröi MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config
3. MultiFactorAuthenticationAdfsAdapterSetup64.msi asennustiedoston suorittaminen
4. Monimenetelmäisen todentamisen AD FS-sovittimen installer Valitse **Seuraava** Käynnistä asennus.
5. Kun asennus on valmis, valitse **Sulje** .

## <a name="edit-the-multifactorauthenticationadfsadapterconfig-file"></a>MultiFactorAuthenticationAdfsAdapter.config-tiedoston muokkaaminen

Muokkaa MultiFactorAuthenticationAdfsAdapter.config tiedostoa seuraavasti:

1. Aseta **UseWebServiceSdk** solmun **True**.  
2. Voit määrittää arvon **WebServiceSdkUrl** multi-factor Authentication Web palvelun SDK URL-osoitteeseen. Esimerkki: *https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx*, jossa certificatename on sertifikaatin nimi.  
3. Muokkaa rekisteriä MultiFactorAuthenticationAdfsAdapter.ps1 komentosarjaa lisäämällä *- ConfigurationFilePath &lt;polku&gt; * loppuun `Register-AdfsAuthenticationProvider` -komento, jossa * &lt;polku&gt; * on MultiFactorAuthenticationAdfsAdapter.config tiedoston koko polku.

### <a name="configure-the-web-service-sdk-with-a-username-and-password"></a>Määrittää WWW-palvelun SDK käyttäjänimellä ja salasanalla

On kaksi vaihtoehtoa SDK WWW-palvelun määrittämistä varten. Ensimmäinen on käyttäjänimellä ja salasanalla, toisen asiakkaan sertifikaatilla. Näiden ohjeiden ensimmäinen vaihtoehto tai Ohita toisen.  

1. Voit määrittää arvon **WebServiceSdkUsername** tilille, joka on PhoneFactor järjestelmänvalvojat-käyttöoikeusryhmän jäsen. Käytä &lt;toimialueen&gt;& #92; &lt;käyttäjänimi&gt; muodossa.  
2. Voit määrittää arvon **WebServiceSdkPassword** tilin salasana.

### <a name="configure-the-web-service-sdk-with-a-client-certificate"></a>Määrittää WWW-palvelun SDK Asiakasvarmenne

Jos et halua käyttää käyttäjänimi ja salasana, määrittää WWW-palvelun SDK Asiakasvarmenne seuraavasti.

1. Asiakkaan sertifikaatin hankkiminen varmenteen myöntäjältä, jossa on käytössä Web-palvelu SDK palvelimen. Lisätietoja [asiakkaan varmenteet](https://technet.microsoft.com/library/cc770328.aspx)hankkiminen.  
2. Tuo sertifikaattia paikallisen tietokoneen-lukulaitteeseen palvelimessa, jossa on käytössä Web-palvelu SDK. Tarkista, että varmenteen myöntäjä julkisen varmenne on Luotetut varmenteet varmenteen Storessa.  
3. Julkiset ja yksityiset avaimet sertifikaatin vieminen .pfx-tiedosto.  
4. Julkinen avain Base64 muodossa vieminen .cer-tiedosto.  
5. Palvelimen hallinnan varmistaa, että Web Server (IIS) \Web Server\Security\IIS Asiakasvarmenteen yhdistäminen todennus-ominaisuus on asennettu. Jos sitä ei ole asennettu, valitse **Lisää rooleja ja toiminnot** Lisää tätä ominaisuutta.  
6. IIS Manager kaksoisnapsauttamalla **Configuration Editoria** , joka sisältää Web-palvelu SDK näennäiskansio-sivustossa. On tärkeää tehdä tämän sivuston tasolla ja ei näennäiskansio tasolla.  
7. Siirry kohtaan **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** .  
8. Aseta arvoksi **true**käytössä.  
9. Määritä oneToOneCertificateMappingsEnabled **Tosi**.  
10. Valitse **...** oneToOneMappings vieressä ja valitse sitten **Lisää** linkki.  
11. Avaa aiemmin luotu Base64 .cer-tiedosto. Poista *---ALKAA VARMENTEEN---*, *---lopettaa VARMENTEEN---*ja kaikki rivinvaihdot. Kopioi tuloksena saatava merkkijono.  
12. Merkkijono, joka on kopioitu edellisessä vaiheessa määrittäminen varmenne.  
13. Aseta arvoksi **true**käytössä.  
14. Määritä käyttäjänimi tilille, jolla on PhoneFactor järjestelmänvalvojat-käyttöoikeusryhmän jäsen. Käytä &lt;toimialueen&gt;& #92; &lt;käyttäjänimi&gt; muodossa.  
15. Määrittää salasanan asianmukainen tilin salasanan ja sulje sitten Configuration Editoria.  
16. Valitse **Käytä** -linkki.  
17. Kaksoisnapsauta Web-palvelu SDK näennäiskansio **todennusta**.  
18. Varmista, että ASP.NET tekeytyminen ja Perustodentamista on valittu **käytössä**ja muut kohteet on määritetty **ei käytössä**.  
19. Kaksoisnapsauta Web-palvelu SDK näennäiskansio **SSL-asetukset**.  
20. Asiakkaan varmenteet asetukseksi **Hyväksy**ja valitse sitten **Käytä**.  
21. Kopioi luotu aiemmin palvelimeen, jossa on käytössä AD FS-sovittimen .pfx-tiedosto.  
22. .Pfx-tiedoston tuominen paikallisen tietokoneen lukulaitteeseen.  
23. Napsauta hiiren kakkospainikkeella ja valitse **Hallitse yksityiset avaimet**ja myöntää tilin, voit käyttää AD FS-palveluun kirjautuminen lukuoikeudet.  
24. Avaa sertifikaattia ja kopioi allekirjoitus **tiedot** -välilehti.  
25. Määritä MultiFactorAuthenticationAdfsAdapter.config-tiedoston **WebServiceSdkCertificateThumbprint** edellisessä vaiheessa kopioimasi merkkijono.  


Lopuksi voit rekisteröidä sovittimen, suorita \Program Files\Multi-PowerShell-komentosarja kerroin todennus Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1. Sovittimen on rekisteröity WindowsAzureMultiFactorAuthentication. Käynnistä voimaan rekisteröinnin AD FS-palvelu uudelleen.

## <a name="related-topics"></a>Aiheeseen liittyviä ohjeita

Ohjeen vianmääritys-artikkelissa [Azure multi-factor Authentication usein kysyttyjä kysymyksiä](multi-factor-authentication-faq.md)
