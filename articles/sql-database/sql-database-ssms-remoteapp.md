<properties
    pageTitle="Yhteyden muodostaminen SQL Server Management Studiossa käyttäminen Azure RemoteApp SQL-tietokantaan | Microsoft Azure"
    description="Tässä opetusohjelmassa avulla voit opetella käyttämään SQL Server Management Studiossa Azure RemoteApp turvallisuus ja suorituskyky yhdistettäessä SQL-tietokantaan"
    services="sql-database"
    documentationCenter=""
    authors="adhurwit"
    manager="jhubbard"/>

<tags
    ms.service="sql-database"
    ms.workload="data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>Yhteyden muodostaminen SQL-tietokantaan Azure RemoteApp SQL Server Management Studiossa avulla

## <a name="introduction"></a>Johdanto  
Tässä opetusohjelmassa avulla voit muodostaa yhteyden SQL-tietokantaan Azure RemoteApp SQL Server Management Studiossa (SSMS) avulla. Se esitellään SQL Server Management Studiossa Azure RemoteApp määrittäminen, tässä artikkelissa kerrotaan, mitä etuja ja suojaustoiminnot, joiden avulla voit näyttää Azure Active Directory.

**Arvioitu kesto:** 45 minuuttia

## <a name="ssms-in-azure-remoteapp"></a>SSMS Azure RemoteApp

Azure RemoteApp on RDS-palvelu, joka tarjoaa sovellusten Azure-tietokannassa. Voit lukea lisää se tähän: [RemoteApp ominaisuudet?](../remoteapp/remoteapp-whatis.md)

Käynnissä Azure RemoteApp SSMS tutustutaan on sama kuvaus kuin suorittaminen SSMS paikallisesti.

![Näyttökuva SSMS Azure RemoteApp käynnissä][1]



## <a name="benefits"></a>Edut

On monia etuja käyttämään SSMS Azure RemoteApp, mukaan lukien:

- Porttia 1433 Azure SQL-palvelimeen ei tarvitse tarjoamia ulkoisesti (ulkopuolella Azure).
- Ei tarvitse säilyttää lisäämällä ja poistamalla IP-osoitteiden Azure SQL Server-palomuurin.
- Kaikki Azure RemoteApp yhteydet ilmetä HTTPS-protokollan välityksellä porttiin 443 käyttämisestä salattu Remote Desktop protocol
- Se on usean käyttäjän ja skaalata.
- Suorituskyky-voitto ei tarvitse SSMS samalla alueella kuin SQL-tietokanta on.
- Voit valvoa Azure RemoteApp käyttäminen Azure Active Directory on käyttäjän käyttöraporttien Premium-version mukana.
- Voit ottaa monimenetelmäisen todentamisen (MFA).
- Accessin SSMS missä tahansa, kun jotakin tuetut Azure RemoteApp-ohjelmat, joka sisältää iOS, Android, Mac, Windows Phone-ja Windows-Käyttöjärjestelmää.


## <a name="create-the-azure-remoteapp-collection"></a>Azure RemoteApp-sivustokokoelman luominen

Näin luoda Azure RemoteApp-sivustokokoelman SSMS:


### <a name="1-create-a-new-windows-vm-from-image"></a>1. Luo uusi Windows-AM kuvasta
Tee uuden AM valikoimasta "Windows Server Remote työpöydän istunnon Host Windows Server 2012 R2"-kuvan avulla.


### <a name="2-install-ssms-from-sql-express"></a>2. SSMS asentaa SQL Express

Siirry sivulle uuden AM ja lataa-sivulle: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/en-us/download/details.aspx?id=42299)

Ei voi ladata vain SSMS. Siirry kansioon asennuksen lataamisen jälkeen ja asenna SSMS suorittamalla asennusohjelma.

Sinun täytyy myös asentaa SQL Server 2014 Service Pack 1. Voit ladata sen seuraavassa: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/en-us/download/details.aspx?id=46694)

SQL Server 2014 Service Pack 1 sisältää keskeiset toimintoja Azure SQL-tietokannan käyttämisen.


### <a name="3-run-validate-script-and-sysprep"></a>3. Suorita Sysprep ja vahvista komentosarja

Työpöydän AM PowerShell-komentosarjaa kutsutaan Vahvista. Suorita tämä kaksoisnapsauttamalla. Se tarkistaa, että AM on valmis, jota käytetään sovellusten remote ylläpitäminen. Kun tarkistus on tehty, se pyytää Suorita sysprep - Valitse suorittaa.

Kun sysprep on valmis, se suljetaan AM.

Lisätietoja Azure RemoteApp kuva luomisesta on artikkelissa: [luomisesta RemoteApp-suunnittelumallin kuva Azure-tietokannassa](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)


### <a name="4-capture-image"></a>4. kuvan

Kun AM on pysähtynyt, etsimällä sen nykyisen portaalin ja tallentaa sen.

Lisätietoja sieppaus kuva on artikkelissa [Azure Windows-virtuaalikoneen, perinteinen käyttöönotto-mallin avulla luotu minkä kuvan](../virtual-machines/virtual-machines-windows-classic-capture-image.md)


### <a name="5-add-to-azure-remoteapp-template-images"></a>5. lisääminen Azure RemoteApp sivustomallien kuvia

Nykyinen portaalin Azure RemoteApp-osassa sivustomallien kuvia-välilehti ja sitten Lisää. Ponnahdusikkunoiden-ruutuun Valitse "Kuvan tuominen näennäiskoneiden kirjaston" ja valitse sitten juuri luomasi kuva.



### <a name="6-create-cloud-collection"></a>6. cloud sivustokokoelman luominen

Nykyinen portaalissa luoda uuden Azure RemoteApp Cloud kokoelman. Valitse malli, joka äsken tuomasi sisältävä kuva SSMS asennettuna.

![Luo uusi cloud kokoelma][2]


### <a name="7-publish-ssms"></a>7. SSMS julkaiseminen

Julkaisu-välilehdessä uusi cloud kokoelma, valitse Käynnistä-valikosta valinnaiseksi ja valitse luettelosta SSMS.

![Sovelluksen julkaiseminen][5]

### <a name="8-add-users"></a>8. käyttäjien lisääminen

Valitse Käyttöoikeudet-välilehdessä voit valita käyttäjät, jotka on pääsy tämä Azure RemoteApp kokoelma, joka sisältää vain SSMS.

![Käyttäjän lisääminen][6]


### <a name="9-install-the-azure-remoteapp-client-application"></a>9. Asenna Azure RemoteApp-asiakassovellukseen

Voit ladata ja Azure RemoteApp-asiakasohjelman asentaminen: [Lataa | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)



## <a name="configure-azure-sql-server"></a>Määritä Azure SQL-palvelin

Vain tarvittavat määritykset on varmistaa, että Azure-palveluja on käytössä palomuuri. Jos käytät tätä ratkaisua, sinun ei tarvitse lisätä Avaa palomuurin IP-osoitteita. Verkkoliikennettä, jolla on oikeus SQL Server on Azure muista palveluista.


![Azure salliminen][4]



## <a name="multi-factor-authentication-mfa"></a>Monimenetelmäisen todentamisen (MFA)

MFA tämän sovelluksen voidaan ottaa käyttöön erikseen. Siirry Azure Active Directory sovellukset-välilehti. Löydät Microsoft Azure RemoteApp merkinnän. Jos valitse kyseisen ohjelman ja määritä sitten, näet sivun, joissa voit ottaa tämän sovelluksen MFA.

![MFA ottaminen käyttöön][3]



## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Käyttäjän valvoa ja Azure Active Directory-Premium

Jos sinulla ei ole Azure AD Premium, sinun on ottaminen käyttöön hakemistossa käyttöoikeudet-kohdassa. Premium on otettu käyttöön, jossa voit määrittää käyttäjät Premium tasolle.

Kun käyttäjä Siirry Azure Active Directoryssa, voit siirtyä sitten Nähdäksesi kirjautumistiedot, Azure RemoteApp tehtävä-välilehti.



## <a name="next-steps"></a>Seuraavat vaiheet

Kaikkien edellä mainittujen vaiheiden suorittamisen jälkeen voi suorittaa Azure RemoteApp-asiakasohjelman ja kirjaudu sisään määritetty käyttäjä. Näyttöön tulee SSMS kanssa yhtenä sovellukset ja voi suorittaa tapaan, jos se on asennettu tietokoneeseen Azure SQL-palvelimen kanssa.

Katso lisätietoja siitä, miten voit tehdä yhteyden SQL-tietokantaan [yhteyden muodostaminen SQL-tietokantaa, jossa SQL Server Management Studiossa ja otoksen T-SQL-kyselyn](sql-database-connect-query-ssms.md).


Kaikki tiedot on nyt. Nauti!



<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png
