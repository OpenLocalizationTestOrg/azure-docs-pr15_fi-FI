<properties
    pageTitle="Web-sovellusten resurssin tarjoajan lisääminen Azure pinon | Microsoft Azure"
    description="Yksityiskohtaiset ohjeet Azure Pinotut Web Apps-sovellusten käyttöönotto"
    services="azure-stack"
    documentationCenter=""
    authors="ccompy, apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>

# <a name="add-a-web-apps-resource-provider-to-azure-stack"></a>Lisää verkkosovelluksissa resurssin palveluntarjoaja Azure pino

> [AZURE.NOTE] Seuraavat tiedot koskevat vain Azure pinon TP1 ominaisuuksissa.

Web Apps resurssin palveluntarjoaja lisääminen Azure pino on seitsemän vaihetta:

1.  Lataa tarvittavat osat.
2.  Luo varmenteet käytettävän Azure pinon verkkosovelluksissa.
3.  Käytä asennusohjelmaa lataaminen, vaiheen ja Azure pinon Web Apps-sovellusten asentaminen. 
4.  Vahvista Web Apps-asennuksen.
5.  DNS-tietueiden luominen edusta-ja asiakirjanhallinnan palvelimeen kuormituksen tasoitusmääritykset.
6.  Voit rekisteröidä äskettäin käyttöön verkkosovelluksissa resurssin tarjoajaan ARM.
7.  Kokeile Web Apps resurssin tarjoajan.

## <a name="download-required-components"></a>Lataa tarvittavat komponentit

1.  Lataa [Azure pinon App palvelun esikatselu asennusohjelma](http://aka.ms/azasinstaller). 
2.  Lataa [Azure pinon App palvelun käyttöönoton avustaja komentosarjoja](http://aka.ms/azashelper). 
3.  Pura tiedostot helper komentosarjojen zip-tiedoston, on oltava kolme komentosarjat:
    - Luo AppServiceCerts.ps1
    - Luo AppServiceDnsRecords.ps1
    - Rekisteröi AppServiceResourceProvider.ps1 

## <a name="create-certificates-to-be-used-by-azure-stack-web-apps"></a>Luo varmenteet Azure pinon verkkosovelluksissa käytettävä

Tätä ensimmäistä komentosarjaa toimii Azure pinon varmenteen myöntäjä luo 3 sertifikaatteja, joita tarvitaan verkkosovelluksissa. Suorita komentosarja varmistaa, että sinulla on käytössä PowerShell azurestack\administrator ClientVM:
1.  PowerShell-istunnossa **azurestack\administrator**käynnissä Suorita **Luo AppServiceCerts.ps1** -komentosarja.  Tämä luo kolme varmenteet kansioon, jossa komentosarja, joita tarvitaan verkkosovelluksissa.
2.  Kirjoita salasana suojaa pfx-tiedostot ja Merkitse muistiin sen, kun haluat kirjoittaa sen Azure pinon Web Apps-asennusohjelma.

## <a name="use-the-installer-to-download-and-install-azure-stack-web-apps"></a>Lataa ja asenna Azure pinon Web Apps-sovellusten asennusohjelma avulla

Appservice.exe asennusohjelma on:
1.  Käyttäjää pyydetään Hyväksy Microsoftin ja kolmansien osapuolten käyttöoikeussopimuksissa.
2.  Kerää Azure pinon asennustiedot.
3.  Luo määritetyn Azure pinon tallennustilan tilin blob-säilö.
4.  Lataa Azure pinon Web App-resurssin Providerin asentaminen tarvittavia tiedostoja.
5.  Valmistele Asenna Azure pino-ympäristössä Web App resurssi-palvelun käyttöön.
6.  Lataa tiedostot tallennustilan App palvelutilin.
7.  Esitä projektin Azure Resurssienhallinta-mallin tarvittavat tiedot.

Seuraavat vaiheet opastaa asennuksen vaiheet:

>[AZURE.NOTE] Suorittaa asennuksen, sinun on käytettävä laajennettuja tiliä (paikallisessa tai toimialueen järjestelmänvalvoja). Jos kirjautuminen azurestack\azurestackuser, jonka pyydetään järjestelmänvalvojan tunnistetietoja. 

1.  Suoritetaan appservice.exe **azurestack\administrator**. 
2.  Valitse **Ota käyttöön Azure resurssien hallinnan avulla**.

![Azure pinon palvelun sovelluksen teknisen ennakkoversion 1 Installer][1]

3.  Tarkista ja hyväksy Microsoft ohjelmiston ennakkoversion käyttöoikeussopimuksen ehdot ja valitse sitten **Seuraava**.
4.  Tarkista ja hyväksy kolmannen partylicense ja valitse sitten **Seuraava**.
5.  Tarkista sovelluksen pilvipalvelussa määritysten tiedot ja valitse **Seuraava**.

![Azure pinon App palvelun teknisen ennakkoversion 1 App palvelun Tunnistepilven määritykset][2]

6. Valitse **Yhdistä** (Azure pinon tilaukset-ruudun) vieressä.

![Azure pinon App palvelun teknisen ennakkoversion 1 App palvelun Cloud kokoonpanoa näytössä kaksi][3]

7.  Azure pinon todennus-ikkunassa **Azure Active Directory-palvelujen järjestelmänvalvoja-tili** ja **salasana**, ja valitse sitten **Kirjaudu sisään**.
**Huomautus:** Tämä on Azure Active Directory-tili, jonka ilmoitit Azure pinon on otettu käyttöön.
    - **Azure pinon tilaukset** vieressä olevaan ruutuun oikeassa reunassa **Alanuolta** ja valitse sitten tilauksen.

![Azure pinon App palvelun teknisen ennakkoversion 1 tilauksen valinta][5]

8.  Napsauta **Alanuolta** **Azure pinon sijainnit**vieressä olevaan ruutuun oikeassa reunassa.
    - Valitse **Paikallinen**.
9. Kirjoita **nimi** järjestelmänvalvoja.
10. Kirjoita **salasana** järjestelmänvalvoja.
11. Tarkista **SQL Server-tiedot** ja tee tarvittavat muutokset.
12. Tarkista **SysAdmin kirjautumistiliä** ja tee tarvittavat muutokset.
13. Kirjoita **SysAdmin salasana**.
14. Valitse **Seuraava**.  Tässä vaiheessa asennusohjelma nyt tarkistaa yhteystiedot SQL Server on annettu.

![Azure pinon App palvelun teknisen ennakkoversion 1 tilauksen valinta][4]    

15. **Web-sovellukset-oletus SSL sertifikaattitiedosto** vieressä **Selaa** ja siirry **webapps. AzureStack.Local** [aiemmin luotu](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)varmenne.
16. Kirjoita **varmenteen salasanaa** , jolla määritetään, kun olet luonut varmenteet.
17. Valitse **Resurssi tarjoajan SSL-varmennetiedosto** vieressä **Selaa** ja siirry **hallinta. AzureStack.Local** [aiemmin luotu](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)varmenne.
18. Kirjoita **varmenteen salasanaa** , jolla määritetään, kun olet luonut varmenteet.
19. Valitse **Resurssi tarjoajan pääkansion varmennetiedosto** vieressä **Selaa** ja siirry **hallinta. AzureStack.Local** [aiemmin luotu](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)varmenne.
20. Valitse **Seuraava** asennusohjelma tarkistaa sertifikaatin salasana.

![Azure pinon palvelun sovelluksen teknisen ennakkoversion 1 tiedot][6]

Käyttöönoton kestää noin 45 – 60 minuuttia suorittamiseen.

![Azure pinon palvelun sovelluksen teknisen ennakkoversion 1 asennuksen eteneminen][7]

21. Kun onnistuu asennusohjelma valitsemalla **Lopeta**.

## <a name="validate-web-apps-installation"></a>Vahvista Web Apps-asennus

1.  Avaa **Azure pinon Host koneen** **Hyper-V hallinta**.
2.  Etsi **CN0 AM** ja **Yhdistä** AM.
![Azure pinon App palvelun teknisen ennakkoversion 1 Hyper-V hallinta][8]
3.  Tämä AM työpöydällä Kaksoisnapsauta **Web Cloud Management Console**.
![Azure pinon palvelun sovelluksen teknisen ennakkoversion 1 hallintakonsoli][9]
4.  Siirry **hallitun palvelimiin**.
5.  Kun kaikissa tietokoneissa on **Valmis** jatka seuraavaan vaiheeseen. 
![Azure pinon App palvelun teknisen ennakkoversion 1 hallitun palvelinten tila][10]

## <a name="create-dns-records-for-the-management-server-and-front-end-load-balancers"></a>DNS-tietueiden luominen asiakirjanhallinnan palvelimeen ja edusta kuormituksen tasoitusmääritykset
1.  Avaa PowerShell-esiintymän **azurestack\administrator**.
2.  Siirry sijaintiin, komentosarjojen ladataan ja uuttaa [Vaihe](#Download-Required-Components).
3.  Suorita **Luo AppServiceDnsRecords.ps1** komentosarja, ohjelma luo DNS reititetään eteen End-palvelimiin portaalia ja web app-pyyntöjen käyttöön otettavat.  Web Apps-sovelluksista, kaksi ohjelmiston kuormituksen tasoitusmääritykset (SLBs) ARM-käyttöönoton yhteydessä on luotu resurssin verkkopalvelun. Ne osoittamalla hallinta-palvelimia ja edusta-palvelimiin. Portaalin ja ARM-pohjainen pyynnöt Azure pinon App resurssin palveluntarjoaja Siirry asiakirjanhallinnan palvelimeen.

## <a name="register-the-newly-deployed-azure-stack-web-apps-provider-with-arm"></a>Voit rekisteröidä äskettäin käyttöön Azure pinon Web Apps-palveluntarjoajan KÄDESSÄ
1.  Avaa PowerShell-esiintymän **azurestack\administrator**.
2.  Siirry sijaintiin, komentosarjojen ladataan ja uuttaa [Vaihe](#Download-Required-Components).
3.  **Rekisteröi AppServiceResourceProvider.ps1** -komentosarjan suorittaminen 

>[AZURE.NOTE] Kirjoita käyttäjänimi ja salasana **TÄSMÄLLEEN (kuten isot ja pienet kirjaimet)** , kun se on määritetty **Virtual Machine(s) järjestelmänvalvoja** - ja **salasana** -kentät, asennus- tai resurssien-palvelun rekisteröinti epäonnistuu.

## <a name="test-drive-azure-stack-web-apps"></a>Testaa asema Azure pinon Web Apps-sovelluksista

On otettu käyttöön ja verkkosovelluksissa resurssin palvelu rekisteröidyt, voit testata ja varmistaa, että alihallinnat, jotka voivat ottaa käyttöön verkkosovelluksissa.

1.  Azure pino-portaalissa uusi, valitsemalla Web + Mobile ja valitse verkkosovellus.
2.  Kirjoita nimi Web app-ruutuun Web App-sivu.
3.  Resurssiryhmä-kohdassa uusi ja kirjoita resurssiryhmä-ruutuun nimi. 
4.  Sovelluksen palvelun suunnitelma tai sijainti ja valitse sitten Luo uusi.
5.  Kirjoita nimi sovelluksen palvelun suunnitelma-ruudussa sovellus-palvelun suunnittelu-sivu.
6.  Valitse hinnoittelu taso, valitse vapaa jaettu tai jaettu jaettu, valitse, valitsemalla OK ja valitse sitten Luo.
7.  Minuutti, valitse uusi web-sovelluksen ruutu näkyy koontinäytössä. Valitse-ruutu.
8.  Valitse web app-sivu valitsemalla Selaa sovelluksen oletussivustoon.


**(Valinnainen) WordPress, DNN tai Django sivuston käyttöönotto**

1. Azure pinon-portaalissa sitten "+", siirry Azure Marketplace-sivustoon, voit ottaa käyttöön Django sivusto ja odota, kunnes onnistunut käyttöönotto. Django web-ympäristö käyttää perustuva tietokanta ja ei edellytä minkä tahansa resurssien tarjoajien, kuten SQL- tai MySQL.  

2. Jos olet asentanut myös MySQL resurssin palveluntarjoaja, voit ottaa WordPress Marketplace-sivustoon. Kun sinua pyydetään tietokannan parametrien, kirjoita käyttäjänimi kuin *User1@Server1* (sekä käyttäjänimi ja palvelimen nimi valittua).

3. Jos olet asentanut myös SQL Server-resurssi-palvelua, voit ottaa käyttöön DNN Marketplace-sivustoon. Kun sinua pyydetään tietokannan parametrien, valitse tietokanta, joka on liitetty resurssi-palvelu SQL Server-tietokoneeseen.

## <a name="next-steps"></a>Seuraavat vaiheet

Voit myös kokeilla muita [ympäristössä, kuten (PaaS) service-palvelujen](azure-stack-tools-paas-services.md), kuten [SQL Server resurssin tarjoajaan](azure-stack-sql-rp-deploy-short.md) ja [MySQL resurssin toimittaja](azure-stack-mysql-rp-deploy-short.md).

<!--Image references-->
[1]: ./media/azure-stack-webapps-deploy/AppService_exe_Start.png
[2]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep1.png
[3]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2.png
[4]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_populated.png
[5]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_SubscriptionSelection.png
[6]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep3_Certificates.png
[7]: ./media/azure-stack-webapps-deploy/AppService_exe_InstallationProgress.png
[8]: ./media/azure-stack-webapps-deploy/HyperV.png
[9]: ./media/azure-stack-webapps-deploy/MMC.png
[10]: ./media/azure-stack-webapps-deploy/ManagedServers.png


<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
