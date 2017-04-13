<properties
   pageTitle="SQL Azure Azure RemoteApp | Microsoft Azure"
   description="Opettele käyttämään SQL Azure Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="ericorman"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="sql-azure-with-azure-remoteapp"></a>SQL Azure Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Usein, kun asiakkaat haluavat isännöidä verkkosovelluksistaan Windows Azure RemoteApp pilveen he haluavat myös niiden tiedot, kuten SQL Server-palvelinten siirtäminen pilveen koko cloud käyttöönottoa varten. Näin isännöidään koko cloud-ratkaisun, jotka voivat käyttää milloin tahansa laitteeseen käyttämällä jotakin Azure RemoteApp. Alla on linkkejä ja viittaukset sekä ohjeet auttavat prosessia.  

## <a name="migrate-your-sql-data"></a>SQL-tietojen siirtäminen

Aloita siirtämisen [Azure SQL-tietokantaan SQL Server-tietokantaan](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Määritä Azure RemoteApp
Ylläpitää Windows Azure RemoteApp sovelluksen. Alla on erittäin korkea on vaiheittaiset ohjeet:

1.     Luo [malli Azure RemoteApp AM](remoteapp-imageoptions.md). 
2.     Asenna tarvittavat sovelluksen AM.
3.     Määritä sovellus, jotta se muodostaa yhteyden SQL-Tietokantaan ja varmista, että se toimii.
4.     Sysprep ja Sammuta AM. Tallenna kuvana Azure käytettäväksi. **Huomautus:** Haluat varmistaa, että sovellus pystyy säilyttämään DB yhteyden tiedot sysprep vaiheet. Jos sovellus ei voi säilyttää DB-yhteystietoja, haluat ehkä osallistua sovelluksen, voit tarkistaa, kuinka Microsoft voit määrittää yhteysmerkkijonon toimittaja.
5.     Tuo mukautettu kuva Azure RemoteApp-kirjastossa valitsemalla SQL Azure-käyttöönoton sijaitsee ERISNIMI maantieteellinen sijainti. 
6.     Ota RemoteApp-sivustokokoelman saman data Centerissä SQL Azure käyttöönoton yllä mallin avulla ja julkaista sovellus. Käyttöönotto Azure RemoteApp saman data Centerissä SQL Azure käyttöönoton avulla varmistetaan nopein yhteysnopeus ja vähentää viive. 

## <a name="app-and-sql-configuration-considerations"></a>Sovelluksen ja SQL määritysten huomioon otettavia seikkoja:
On muutamia seikkoja huomioitavaksi käytettäessä RemoteApp Azure SQL:

Opettele [määrittämään Azure SQL-tietokantaan palomuurin](../sql-database/sql-database-firewall-configure.md). Ote artikkelissa, tiloista "aluksi kaikki access Azure SQL-tietokanta-palvelimeen on estänyt palomuurin. Jotta voit aloittaa oman Azure SQL-tietokantapalvelinta perinteinen-portaalista ja määritä vähintään yksi palvelintason palomuurisäännöt, jotka mahdollistavat Azure SQL-tietokanta-palvelimen. Palomuurisäännöt Määritä käyttämällä mitä IP-osoitealueet Internetistä sallitaan ja onko Azure sovellukset voivat yrittää Azure SQL-tietokanta-palvelimeen."

Lisäksi, kun tietokoneen yrittää muodostaa Internet-tietokantapalvelin, palomuurin tarkistaa pyynnön vastaan täydellisen luettelon palvelintason alkuperäiseen IP-osoite ja (tarvittaessa) tietokannan tason palomuurisäännöt. "Jos pyynnön IP-osoite on yksi palvelintason palomuurisäännöt määritettyjä alueita, yhteys on myönnetty Azure SQL-tietokanta-palvelimellesi." Näin ollen voimme tuoda käyttäminen IP-alueita, eikä vain yksittäisiä lähde-IP-osoitteet.

Noudata vaiheittainen [Toimintaohje: Azure-portaalissa SQL-tietokantaan palomuurin asetusten määrittäminen](../sql-database/sql-database-configure-firewall-settings.md) Määritä IP-alueen. Kun olet määrittämässä SQL-palomuurin säännöt, anna aliverkon, joka on määritetty IP-alueen Azure RemoteApp-kokoelmaan. Tämä kannattaa antaa muodostaa yhteyden SQL-Tietokantaan, vaikka heillä on dynaamisesti-määritetään IP-osoitteiden ARA-palvelimiin.

## <a name="troubleshooting"></a>Vianmääritys
Jos asiakassovellus, jossa käytetään käyttäjäkokemuksen hallinnoida Azure RemoteApp, joka muodostaa yhteyden SQL-tietokannan Jos isännöimät Azure tai paikallisen on hidas voi olla useita syitä miksi.  

- Verkon latenssin mobiililaitteen Azure on riittävä. Siirrä parhaiten ja nopeimmin verkkoyhteys voit varmistaa parhaan suorituskyvyn. Määritä [azurespeed.com](http://azurespeed.com/) Yleiset työkalu Testaa laitteet-viive Azure tietojen käyttöoikeudet.  
- Azure RemoteApp sijaitseva asiakas-sovellus on kohdassa kuormitus. Valitseminen eri laskutuksen suunnitelma, kuten Premium Laskutus parantaa suorituskykyä. Toisen huijata on valvoa sovellusta muissa resurssit: aktiivisen istunnon aikana suorittaa ctrl + alt + end näppäinyhdistelmä, joka käynnistää SAS näytön, valitse Tehtävienhallinta ja noudata resurssien käyttöön, kun sovellus.
- SQL server on kohdassa kuormitus tai ei ole optimoitu. Noudata SQL vianmäärityksen lisäohjeita. 

