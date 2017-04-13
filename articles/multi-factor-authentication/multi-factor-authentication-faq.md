<properties
    pageTitle="Azure Monimenetelmäisen todentamisen usein kysytyt kysymykset"
    description="On usein kysyttyjä kysymyksiä ja vastauksia Azure Monimenetelmäisen todentamisen liittyvä luettelo. Monimenetelmäisen todentamisen on menetelmä käyttäjän tunnistetiedot, joka edellyttää enemmän kuin käyttäjänimi ja salasana. Se sisältää kerroksen arvopaperin käyttäjien sisäänkirjautumisessa ja tapahtumia."
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
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="azure-multi-factor-authentication-faq"></a>Azure Monimenetelmäisen todentamisen usein kysytyt kysymykset


Nämä usein kysytyt kysymykset vastauksia yleisiin kysymyksiin Azure Monimenetelmäisen todentamisen ja multi-factor Authentication-palveluun, mukaan lukien laskutuksen mallin ja käytettävyydelle koskeviin kysymyksiin.

## <a name="general"></a>Yleiset

**K: miten Azure multi-factor Authentication Server Käsittele käyttäjätietoja?**

Käyttäjätiedot on tallennettu vain paikallisen palvelimissa multi-factor Authentication-palvelimen kanssa. Ei ole pysyvä käyttäjätiedot tallennetaan pilveen. Kun käyttäjä suorittaa kaksivaiheista vahvistusta varten, Multi-factor Authentication Server lähettää tiedot Azure Monimenetelmäisen todentamisen pilvipalvelussa todennusta varten. Multi-factor Authentication Server ja Monimenetelmäisen todentamisen pilvipalvelussa välisen käyttää Secure Sockets Layer (SSL) tai Transport Layer Security (TLS) porttiin 443 lähtevä päälle.

Kun todennusta pyynnöt lähetetään pilvipalveluun, tiedot kerätään todennus ja käyttö raportit. Tietojen kentät kaksivaiheista vahvistusta lokit ovat seuraavat:

- **Yksilöllinen tunnus** (joko nimi tai paikalliseen multi-factor Authentication Server Käyttäjätunnus)
- **Etunimi ja Sukunimi** (valinnainen)
- **Sähköpostiosoite** (valinnainen)
- **Puhelinnumero** (kun äänipuhelu tai SMS-todennuksen avulla)
- **Laitteen tunnus** (käytettäessä mobiilisovelluksen authentication)
- **Todennustila**
- **Todennus-tulos**
- **Monimenetelmäisen todentamisen palvelimen nimi**
- **Monimenetelmäisen todentamisen palvelimen IP**
- **Asiakkaan IP** (jos saatavilla)

Multi-factor Authentication Server määrittää valinnaisia kenttiä.

Vahvistus tulos (success tai eston) ja syy, jos se on estetty, tallennetaan todentamistiedot ja on käytettävissä todennus-ja käyttö.


## <a name="billing"></a>Laskutus

Useimmat laskutuksen kysymyksiin voi vastata viittaamalla [multi-factor Authentication hinnat sivun](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

**K: perittävän puheluita tai viestejä organisaation käytetään todentamiseen aiemmin käyttäjiä?**

Yksittäisten puheluja kohta tai teksti ei perittävän organisaatiot käyttäjille Azure Monimenetelmäisen todentamisen kautta lähetettyihin viesteihin. Puhelimen omistajat voivat voidaan veloittaa puhelut tai tekstiviestejä, he saavat niiden Omat puhelin mukaan.

**K: käytettäessä käyttäjäkohtainen laskutuksen mallin kulu on määritetty käyttämään Monimenetelmäisen todentamisen käyttäjien lukumäärä tai todentamiseen käyttäjien lukumäärä perusteella?**

Laskutus perustuu käyttämään multi-factor Authentication-käyttäjien lukumäärä.

**K: miten Monimenetelmäisen todentamisen Laskutus toimii?**

Kun käytät "käyttäjäkohtainen" tai "kohti todennus-malli, Azure MFA on kulutus perustuva resurssi. Kulut ovat laskuttaa organisaation Azure tilaukseen tavoin näennäiskoneiden, sivustot, jne.

Kun käyttöoikeus-mallin, Azure Monimenetelmäisen todentamisen käyttöoikeudet ostetaan ja määritetään käyttäjille, kuten Office 365: ssä ja tilauksen muihin ohjelmiin.

**K: onko ilmaisen Azure multi-factor Authentication-version järjestelmänvalvojille?**

Joissakin tapauksissa Kyllä. Monimenetelmäisen todentamisen Azure-järjestelmänvalvojille on alijoukkoa Azure MFA ominaisuuksia ilmaiseksi. Tarjous koskee Azure Active Directory-esiintymät, jotka eivät ole linkitetty kulutus perustuva Azure Monimenetelmäisen todentamisen palveluntarjoaja Azure Yleiset Järjestelmänvalvojat-ryhmän jäsenet. Multi-factor Authentication-palvelun avulla päivittää kaikki järjestelmänvalvojat ja hakemiston käyttäjät, joilla on määritetty käyttämään Monimenetelmäisen todentamisen Azure Monimenetelmäisen todentamisen täydellisen version.

**K: onko ilmaisen Azure multi-factor Authentication-version Office 365-käyttäjille?**

Joissakin tapauksissa Kyllä. Monimenetelmäisen todentamisen Office 365: lle tarjoaa alijoukkoa Azure MFA ominaisuuksia ilmaiseksi. Tarjous koskee käyttäjät, joilla on määritetty, kun kulutus perustuva Azure multi-factor Authentication-palvelu ei ole linkitetty vastaavan Azure Active Directory-esiintymä Office 365-käyttöoikeus. Multi-factor Authentication-palvelun avulla päivittää kaikki järjestelmänvalvojat ja hakemiston käyttäjät, joilla on määritetty käyttämään Monimenetelmäisen todentamisen Azure Monimenetelmäisen todentamisen täydellisen version.

**K: Voinko käyttäjäkohtainen ja todennusta kohti kulutus laskutuksen mallien milloin tahansa vaihtaa oman organisaation?**

Organisaation valitsee laskutuksen mallin, kun se luo resurssin. Et voi muuttaa laskutuksen mallin, kun resurssi on valmisteltu. Voit kuitenkin luoda toisen Monimenetelmäisen todentamisen resurssin korvaa alkuperäinen. Käyttäjäasetusten ja -asetuksia ei voi siirtää uusi resurssi.

**K: organisaation välillä voi siirtyä kulutus laskutus- ja käyttöoikeusmalli milloin tahansa?**

Kun käyttöoikeuksia on lisätty kansioon, jossa on jo Azure Monimenetelmäisen todentamisen käyttäjäkohtainen toimittaja, kulutus perustuva laskutus pienenee omistaa käyttöoikeuksien määrän mukaan. Kaikki käyttäjät, jotka on määritetty käyttämään Monimenetelmäisen todentamisen on määritetty käyttöoikeuksia, jos järjestelmänvalvoja poistaa Azure multi-factor Authentication-palvelu.

Todennus kulutus Laskutus käyttöoikeus-mallia ei voi yhdistellä keskenään. Kun todennus multi-factor Authentication-palvelu on linkitetty kansioon, organisaation laskuttaa kaikki Monimenetelmäisen todentamisen vahvistus pyynnöt, omistaa käyttöoikeuksista huolimatta.

**K: organisaation on ja synkronoida käyttäjätietojen käyttämään Azure Monimenetelmäisen todentamisen?**

Kun organisaatiossa käytetään kulutus perustuva laskutuksen mallin, Azure Active Directory ei tarvita. Monimenetelmäisen todentamisen palveluntarjoaja linkittäminen kansio on valinnainen. Jos organisaatiosi ei ole linkitetty kansioon, voit ottaa Azure multi-factor Authentication Server tai Azure multi-factor Authentication SDK paikalliseen.

Azure Active Directory tarvitaan käyttöoikeusmalli koska käyttöoikeuksia lisätään hakemiston, kun ostat ja määrittää niitä käyttäjille hakemistossa.


## <a name="usability"></a>Käytettävyys

**K: mikä käyttäjä? jos niitä ei saa vastausta niiden puhelimessa tai jos puhelimen ei ole käyttäjän käytettävissä**

Jos käyttäjä on määritetty varmuuskopion puhelimella, ne uudelleen ja valitsemalla Kirjaudu sisään-sivulla pyydettäessä puhelimen. Jos käyttäjällä ei ole määritetty toista menetelmää, organisaation järjestelmänvalvoja voi päivittää käyttäjän Ensisijainen puhelin määritetty numero.


**K: mitä järjestelmänvalvojan tekee Jos käyttäjän yhteystietojen järjestelmänvalvojan tilille, jolla käyttäjä voi enää käyttää tietoja?**

Järjestelmänvalvoja palauttaa käyttäjän tilin mukaan pyyntö menee läpi rekisteröinnin loppuun uudelleen. Lisätietoja [käyttäjä-ja Laiteasetukset Azure Monimenetelmäisen todentamisen pilveen kanssa](multi-factor-authentication-manage-users-and-devices.md).

**K: mikä järjestelmänvalvoja? Jos käyttäjän matkapuhelimen, joka käyttää sovelluksen salasanat katoaa tai varastamiselta**

Järjestelmänvalvoja voi poistaa kaikki käyttäjän sovelluksen salasanat luvattoman käytön estämiseksi. Käyttäjällä on korvaava-laite, kun käyttäjä luo salasanat. Lisätietoja [käyttäjä-ja Laiteasetukset Azure Monimenetelmäisen todentamisen pilveen kanssa](multi-factor-authentication-manage-users-and-devices.md).

**K: mitä jos käyttäjän kirjautuminen ei onnistu selaimen sovelluksia?**

Käyttäjä, joka on määritetty käyttämään Monimenetelmäisen todentamisen edellyttää sovelluksen salasanan kirjautumaan jotkin selaimen sovellukset. Käyttäjän täytyy poistaa (Poista)-kirjautuminen tiedot, Käynnistä sovellus uudelleen ja kirjaudu sisään käyttämällä niiden käyttäjän nimi ja sovelluksen salasana.

Saat lisätietoja sovelluksen salasanat ja muut [apua sovelluksen salasanat](multi-factor-authentication-end-user-app-passwords.md)luomisesta.


>[AZURE.NOTE] Nykyaikainen todentaminen Office 2013-asiakkaat
>
> Office 2013: n asiakkaiden (kuten Outlookin) tue uusi todennusprotokollia. Voit määrittää Monimenetelmäisen todentamisen tuki Office 2013: ssa. Kun olet määrittänyt Office 2013-sovelluksen salasanat eivät ole pakollisia Office 2013-asiakkaille. Lisätietoja on artikkelissa [Office 2013: n Nykyaikainen todentaminen public preview-version ilmoitus](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**K: mikä käyttäjä? Jos käyttäjä saa tekstiviestin tai käyttäjän vastaukset kaksisuuntainen tekstiviestin mutta vahvistus aikakatkaistaan**

Toimita tekstin viestien ja vastausten määrästä kaksisuuntainen SMS vastaanotto ei välttämättä koska valvomattomassa eri tekijät, jotka voivat vaikuttaa palvelun luotettavuutta. Näitä tekijöitä ovat kohdemaa, matkapuhelinoperaattorin tietoliikennesopimus ja signaali.

Käyttäjät, joilla on vaikeuksia luotettavasti vastaanottaa tekstiviestejä Valitse mobile sovelluksen tai puhelun menetelmä sen sijaan. Mobile-sovellus voi vastaanottaa ilmoituksia sekä Matkapuhelin ja Wi-Fi-yhteyden kautta. Lisäksi mobile-sovellus voi luoda vahvistus koodit, myös silloin, kun laite on ei lainkaan. Microsoft Authenticator-sovellus on käytettävissä [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071)-, [Android](http://go.microsoft.com/fwlink/?Linkid=825072)- ja [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Jos sinun on käytettävä tekstiviestejä, on suositeltavaa käyttää yksisuuntainen SMS kaksisuuntainen SMS, kun se on mahdollista sijaan. Yksisuuntainen SMS on luotettava ja se estää käyttäjiä lisäkustannuksia yleinen SMS-vastattaessa viestiin tekstiä, joka on lähetetty toisesta maasta.


**K: Voinko käyttää laitteiston tunnusten Azure multi-factor Authentication-palvelimen kanssa?**

Jos käytössäsi on Azure multi-factor Authentication Server, voit tuoda kolmannen osapuolen Avaa todennus (sijasta) aikapohjaisten ja erikseen salasanan (TOTP) tunnusten ja niiden käyttöä kaksivaiheista vahvistusta varten.

Voit käyttää ActiveIdentity tunnuksia, jotka ovat sijasta TOTP tunnuksia, jos Siirrä salaisen avaimen tiedosto CSV-tiedostoon ja tuoda Azure multi-factor Authentication palvelimeen. Voit käyttää kanssa Active Directory Federation Services (ADFS), Remote puhelinyhteyden käyttäjän palvelun RADIUS (Authentication) kun asiakasjärjestelmän voit käsitellä access todennus vastaukset ja Internet Information Server (IIS) Lomakepohjainen todentaminen sijasta tunnukset.

Voit tuoda kolmannen osapuolen sijasta TOTP tunnusten kanssa seuraavissa muodoissa:  
- Kannettava symmetrisen avaimen säilön (PSKC)  
- CSV-tiedostossa on järjestysluvun salausavaimen Base 32-muodossa ja aikavälin  

**K: Voinko käyttää Azure multi-factor Authentication Server suojaamiseen päätepalvelut?**

Kyllä, mutta jos käyttämällä Windows Server 2012 R2 tai myöhemmin, ainoastaan käyttämällä etätyöpöydän yhdyskäytävää (RD yhdyskäytävä).

Suojauksen muutokset Windows Server 2012 R2 on muutettu siten, että Azure multi-factor Authentication Server muodostaa yhteyden Windows Server 2012: ssa ja aiemmissa versioissa paikallisen suojaustoiminnon (LSA) suojauspaketti. Päätepalvelut Windows Server 2012: ssa tai aiempi versio voit [suojata sovelluksen Windows-todennuksen kanssa](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Jos käytössäsi on Windows Server 2012 R2, sinun on Etätyöpöydän yhdyskäytävä.

**K: Miksi käyttäjän vastaanotat Monimenetelmäisen todentamisen puhelun Anonyymi soittaja Soittajatunnus määrittämisen jälkeen?**

Kun Monimenetelmäisen todentamisen puhelut sijoitetaan yleisen puhelinverkon kautta, joskus ne reititetään kautta carrier, joka ei tue soittajan tunnus. Tästä syystä Soittajatunnus ei välttämättä, vaikka Monimenetelmäisen todentamisen järjestelmä lähettää sen aina.


## <a name="errors"></a>Virheet

**K: mitä käyttäjät tehdä, jos käyttäjät näkevät "todennus pyyntö ei ole aktivoitu tilin"-virhesanoma, kun mobiilisovelluksen ilmoitusten avulla?**

Kerro ohjeita noudattamalla voit poistaa tilin mobiilisovelluksessa ja lisää se sitten uudelleen:

1. Siirry [Azure portaalin profiilisi](https://account.activedirectory.windowsazure.com/profile/) ja kirjaudu sisään organisaation tiliä.
2. Valitse **lisäsuojauksen vahvistus**.
4. Poista aiemmin luotu tili mobiilisovelluksessa.
5. Valitse **Määritä**ja noudata sitten ohjeita määrittämään mobile-sovellus.


**K: mitä käyttäjät tehdä, jos käyttäjät näkevät 0x800434D4L virhesanoma, kun selain sovellukseen kirjautumisessa?**

Tällä hetkellä käyttäjä käyttää lisäsuojauksen vahvistus vain sovelluksia ja palveluita käyttäjä voi käyttää selaimen kautta. Muu selain (tunnetaan myös nimellä *rich client-sovellukset*) asennetut sovellukset paikalliseen tietokoneeseen, kuten Windows PowerShellin ei toimi tilien, jotka edellyttävät lisäsuojauksen vahvistus. Tässä tapauksessa käyttäjä voi tulla aiheuttaa virheen 0x800434D4L sovelluksen.

Vaihtoehtoinen menetelmä tämä on on erillinen käyttäjän tilit järjestelmänvalvojan liittyvät ja järjestelmänvalvoja toimintoja. Voit linkittää postilaatikoiden järjestelmänvalvojatilin ja Järjestelmänvalvoja tilin myöhemmin siten, että voit kirjautua sisään Outlookiin Järjestelmänvalvoja tilin avulla. Lue lisätietoja siitä, miten voi [antaa järjestelmänvalvoja voi avata ja katsella käyttäjän postilaatikon sisällön](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Seuraavat vaiheet

Jos kysymykseesi ei ole vastattu tässä, jätä se kommenttien sivun alareunassa. Voit myös tässä on joitakin lisäasetuksia saaminen:


**K: Miten voin hankkia apua Azure Monimenetelmäisen todentamisen?**

- Etsi [Microsoft-tuen tietämyskannan](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) ratkaisuja yleisiin teknisiä ongelmia.

- Hae ja Selaa tekniset kysymykset ja vastaukset-yhteisöltä tai oman kysymyksen [Azure Active Directory-keskustelupalstoilla](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

- Jos olet aiempien versioiden PhoneFactor asiakas ja sinulla kysyttävää tai Tarvitsetko apua salasanan nollaamista, voit avata tukipyynnön [salasanan](mailto:phonefactorsupport@microsoft.com) -linkin avulla.

- Ota yhteys tuotetukeen tukemalla [Azure multi-factor Authentication Server (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947). Yhteystiedot, kun se on hyödyllinen, jos lisäät mahdollisimman paljon tietoja tietoja mahdollisimman ongelmaa. Voit antaa tiedot sisältävät sivu, jossa näyttöön tuli virheen, virhekoodi tietyn istunnon tunnus tai käyttäjä, joka tuli virheen tunnus.
