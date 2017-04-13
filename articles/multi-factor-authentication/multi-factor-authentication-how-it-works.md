<properties 
    pageTitle="Azure multi-factor Authentication - toiminta"
    description="Azure Monimenetelmäisen todentamisen auttaa suojaavat käyttämään tietoja ja sovelluksia käyttäjän tarve-kirjautuminen yksinkertaista kokouksen aikana. Se tarjoaa lisäsuojauksen toisen lomakkeen todennusta edellyttävän ja toimittaa vahva todennus helposti vahvistus asetukset solualueen kautta."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

#<a name="how-azure-multi-factor-authentication-works"></a>Azure Monimenetelmäisen todentamisen toiminta

Monimenetelmäisen todentamisen suojaus on sen kerroksittain vaiheelta. Merkittäviä hankala vaarantavat useita todennus tekijät näkyy hyökkääjät varten. Vaikka onnistuisi Lisätietoja käyttäjän salasanan, se on hyödytön ilman, että myös luotettujen laitteen käytettävissään. Olisi käyttäjä menettää laitteen, henkilö, joka vastaa sitä voi käyttää sitä, jos hän tietää myös käyttäjän salasana.

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)



Azure Monimenetelmäisen todentamisen auttaa suojaavat käyttämään tietoja ja sovelluksia käyttäjän tarve-kirjautuminen yksinkertaista kokouksen aikana.  Se tarjoaa lisäsuojauksen toisen lomakkeen todennusta edellyttävän ja toimittaa vahva todennus helposti vahvistus asetukset solualueen kautta:

- Puhelun soittaminen
- tekstiviestin
- mobiilisovelluksen ilmoitus, jonka avulla käyttäjät voivat valita menetelmä haluamansa
- mobiilisovelluksen tarkistuskoodi
- 3 osapuolen sijasta tunnusten

Lisätietoja muista kuinka se toimii on seuraavassa videossa.

>[AZURE.VIDEO multi-factor-authentication-deep-dive-securing-access-on-premises]

##<a name="methods-available-for-multi-factor-authentication"></a>Menetelmiä, joiden monimenetelmäisen todentamisen
Käyttäjän kirjautuessa sisään muita vahvistus lähetetään käyttäjälle.  Seuraavassa on luettelo tavoista, joita voi käyttää tätä toinen yhteydenottotapa.

Tarkistustapa  | Kuvaus
------------- | ------------- |
Puhelun soittaminen | Puhelun sijoitetaan käyttäjän älypuhelimella pyydät heitä tarkistamaan, että ne kirjautumassa painamalla #-merkki.  Tämä viimeistelee vahvistus.  Tämä asetus on määritettävä ja voidaan muuttaa tunnus, jolla voit määrittää.
Tekstiviestin | Tekstiviestin lähetetään käyttäjän älypuhelimella 6 numeroinen koodi.  Anna tämän koodin Suorita vahvistus.
Mobiilisovelluksen ilmoitus | Tarkistus-pyyntö lähetetään käyttäjän älypuhelimella pyydät heitä valmis vahvistus valitsemalla Tarkista mobiilisovelluksessa. Tilanne ilmenee, jos valitsit sovelluksen ilmoituksella ensisijainen tarkistustavaksi.  Jos saamiinsa tämä ne ole kirjautumisessa käyttäjät voivat valita ilmoittaminen ilmoituksen.
Tarkistuskoodi Mobile-sovelluksessa | Tarkistuskoodi lähetetään mobile-sovellus, jossa on käynnissä käyttäjän älypuhelimella.  Tilanne ilmenee, jos valitsit vahvistus-koodin ensisijainen tarkistustavaksi.


##<a name="available-versions-of-azure-multi-factor-authentication"></a>Käytettävissä olevat Azure multi-factor Authentication-versiot
Azure Monimenetelmäisen todentamisen sisältyy kolme eri versioita.  Seuraavassa taulukossa kuvataan kaikkien näiden tarkemmin.

Versio  | Kuvaus
------------- | ------------- |
Monimenetelmäisen todentamisen Office 365: lle | Tämä versio toimii ainoastaan Office 365-sovellusten ja hallinnasta Office 365-portaalista. Jotta järjestelmänvalvojat voivat nyt auttaa suojatun käyttämällä monimenetelmäisen todentamisen resursseille Office 365: ssä.  Tämä versio sisältyy Office 365-tilauksen.
Monimenetelmäisen todentamisen Azure järjestelmänvalvojille | Office 365: n ominaisuuksia Monimenetelmäisen todentamisen saman alijoukko on käytettävissä kaikkien Azure järjestelmänvalvojien ilmaiseksi. Jokaisen järjestelmänvalvojan tilin Azure tilauksen nyt avata muita suojaus ottamalla tämän monimenetelmäisen todentamisen perustoiminnot. Jotta tallennustilan hallinta, joka haluaa käyttää AM-web-sivuston luominen Azure-portaalissa järjestelmänvalvoja, mobile-palvelujen tai Azure-palvelu voi lisätä monimenetelmäisen todentamisen hänen järjestelmänvalvojan tilille.
Azure Monimenetelmäisen todentamisen | Azure Monimenetelmäisen todentamisen on richest joukko ominaisuuksia. <br><br>Se on lisäasetuksia, esimerkiksi Azure hallinta-portaalin Lisäasetukset raportointi ja tuen kautta paikallisen solualueen ja cloud sovellukset. Azure Monimenetelmäisen todentamisen voit ostaa erillinen käyttöoikeuden ja Azure Active Directory-Premium ja yrityksen Mobility Suite yhteen. <br><br>Se myös voi ostaa kulutus välein luomalla Azure multi-factor Authentication tarjoajan Azure tilauksen.
##<a name="feature-comparison-of-versions"></a>Versioiden ominaisuuksien vertailu
Seuraavassa taulukossa on luettelo ominaisuuksia, jotka ovat käytettävissä Azure Monimenetelmäisen todentamisen eri versioissa.


Toiminto  | Monimenetelmäisen todentamisen Office 365: ssä (sisältyy Office 365-tuotteissa)|Monimenetelmäisen todentamisen Azure järjestelmänvalvojille (Azure-tilaukseen kuuluva) | Azure Monimenetelmäisen todentamisen (sisältyy Azure AD Premium ja yrityksen Mobility Suite)
------------- | :-------------: |:-------------: |:-------------: |
Järjestelmänvalvojat voivat suojata tili ja MFA| * | * (Käytettävissä vain Azure Järjestelmänvalvojatileille)|*
Mobiilisovelluksen kuin toinen tekijä|* | * | *
Puhelun kuin toinen tekijä|* | * | *
SMS kuin toinen tekijä|* | * | *
Asiakkaat, jotka eivät tue MFA ohjelmasalasanat|* | * | *
Järjestelmänvalvoja määrittää todennustavat| *| *| *
PIN-tilassa| | | *
Ilmoituksen ilmoitus| | | *
MFA-raportit| | | *
Ohita kerran| | | *
Mukautetut tervehdykset puhelut| | | *
Soittajatunnus puhelut mukauttaminen| | | *
Tapahtuman vahvistus| | | *
Luotettu IP-osoitteet| | | *
MFA keskeyttää tallennettujen laitteiden (Public Preview)| | | *
MFA SDK-PAKETISSA| | | *
MFA paikallisen sovellusten MFA-palvelimen käyttäminen| | | *


##<a name="how-to-get-azure-multi-factor-authentication"></a>Azure Monimenetelmäisen todentamisen hankkiminen

Jos haluat täysi toiminnallisuus tarjoamia Azure Monimenetelmäisen todentamisen sen sijaan, että vain ne Office 365-käyttäjille ja Azure Järjestelmänvalvojat, useilla eri tavoilla hankkiminen:

1.  Osta Azure Monimenetelmäisen todentamisen käyttöoikeuksia ja määrittää niitä käyttäjille.
2.  Osta käyttöoikeuksia, joiden Azure Monimenetelmäisen todentamisen yhteen sisällä esimerkiksi Azure Active Directory-Premium tai yrityksen Mobility Suite ja määrittää niitä käyttäjille.
3.  Luo Azure multi-factor Authentication tarjoajan Azure-tilauksen piiriin kuuluvien. Jos sinulla ei vielä ole Azure tilaus, voit rekisteröityä Azure kokeiluversiotilauksen osalta. Kokeilutilaukset on tavallisen tilaukset ennen kokeilujakson muunnetaan.

Käyttämällä Azure multi-factor Authentication-palvelu ole kaksi käyttö-mallit, ovat laskuttaa Azure tilauksen kautta:


- **Käyttäjää kohden**. Yleensä yrityksille, jotka haluat ottaa käyttöön monimenetelmäisen todentamisen on kiinteä määrä työntekijät, jotka tarvitsevat säännöllisesti todennusta varten.
- **Todennus kohden**. Yleensä yrityksille, jotka haluat ottaa käyttöön monimenetelmäisen todentamisen ulkoisten käyttäjien käyttöoikeuksien harvoin käyttäville suurelle ryhmälle.

Katso hinnat tiedot [Azure MFA hinnat.](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)

Valitse kirja tai, joka sopii parhaiten organisaatiosi kulutus-malli.   Valitse, jos haluat saada aloittaminen Katso [Käytön aloittaminen](multi-factor-authentication-get-started.md)
