<properties
    pageTitle="Azure AD Connect: Ottaminen käyttöön laitteen takaisinkirjoituksen | Microsoft Azure"
    description="Tämän asiakirjan tiedot-laitteen takaisinkirjoituksen käyttämällä Azure AD Connect ottaminen käyttöön"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Ottaminen käyttöön laitteen takaisinkirjoituksen

>[AZURE.NOTE] Azure AD Premium-tilauksen vaaditaan laitteen takaisinkirjoituksen.

Seuraavat asiakirjat on tietoja siitä, miten voit ottaa käyttöön Azure AD Connect laitteen takaisinkirjoituksen-ominaisuuden. Laitteen takaisinkirjoituksen käytetään seuraavissa tilanteissa:

- Ehdollinen käyttöönottaminen ADFS-laitteiden perusteella (2012 R2: n tai sitä uudemmassa versiossa) suojattu sovellusten (varmenteen käyttäjän osapuolen luottamussuhteet).

Tämä on lisäsuojauksen ja varmistaa, että access-sovellukset on myönnetty vain luotettavien laitteet. Lisätietoja ehdollisen käyttöoikeuden on artikkeleissa [Ehdollisen käyttöoikeuden hallinta riskin](active-directory-conditional-access.md) ja [paikallisen ehdollinen käytön Azure Active Directory laitteen rekisteröinti](https://msdn.microsoft.com/library/azure/dn788908.aspx).

>[AZURE.IMPORTANT]
<li>Laitteet on sijaittava saman metsää käyttäjiksi. Laitteet on kirjoitettava takaisin yhteen metsää, koska tämä toiminto ei tue ympäristö, jonka usean käyttäjän metsien tällä hetkellä.</li>
<li>Paikallisen Active Directory-metsää voidaan lisätä vain yhden laitteen rekisteröinti kokoonpano-objekti. Tämä toiminto ei ole yhteensopiva topologian, jossa paikallisen Active Directory on synkronoitu useita Azure AD-kansioita.</li>

## <a name="part-1-install-azure-ad-connect"></a>Osa 1: Asenna Azure AD yhdistäminen
1. Asenna Azure AD Connect käyttämällä mukautettu tai Expressin asetukset. Microsoft suosittelee aluksi kaikki käyttäjät ja ryhmät synkronoitu ennen kuin otat laitteen takaisinkirjoituksen.

## <a name="part-2-prepare-active-directory"></a>Osa 2: Valmistelemisesta Active Directory
Seuraavien vaiheiden avulla voit laitteen takaisinkirjoituksen käytön valmisteleminen.

1.  Tietokoneesta, johon on asennettu Azure AD Connect Käynnistä PowerShell laajennettuja-tilassa.

2.  Jos PowerShellin Active Directory-moduulia ei ole asennettu, asenna se käyttämällä seuraava komento:

    `Install-WindowsFeature –Name AD-Domain-Services –IncludeManagementTools`

3. Jos PowerShellin Azure Active Directory-moduulia ei ole asennettu, lataa ja asenna se [Active Directory moduulin Windows PowerShellin Azure (64-bittinen versio)](http://go.microsoft.com/fwlink/p/?linkid=236297). Tämän osan riippuvuuden kirjautumisavustaja, johon on asennettu Azure AD Connect.

4.  Suorittamalla seuraavat komennot ja sulje sitten PowerShell yrityksen järjestelmänvalvojan tunnistetietoja.

    `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`

    `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

Yrityksen järjestelmänvalvojan tunnistetietoja ei tarvita, sillä muutoksia määritysten nimitilan tarvitaan. Toimialueen järjestelmänvalvoja ei ole tarpeeksi käyttöoikeuksia.

![PowerShell laitteen takaisinkirjoituksen ottaminen käyttöön](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

Kuvaus:

- Jos ne eivät ole jo, Luo ja määrittää uuden säilöjen ja objektien CN = laitteen rekisteröinti Configuration, CN = palveluja, CN = Configuration, [dn metsää].
- Jos ne eivät ole jo, Luo ja määrittää uuden säilöjen ja objektien CN = RegisteredDevices, [toimialueen dn]. Laitteen objektit luodaan tähän säilöön.
- Tarvittavat käyttöoikeudet määrittää Azure AD Connector-tilin Active Directory-laitteiden hallinta.
- Vain täytyy suorittaa yhden metsää, vaikka Azure AD Connect asennetaan useita metsiin.

Parametrit:

- Toimialue: Active Directory-toimialueen mihin laitteen objektit luodaan. Huomautus: Kaikki tietyn Active Directory-metsää laitteiden luodaan yhden toimialueen.
- AdConnectorAccount: Active Directory tili, jota käytetään Azure AD Connect hallitsemaan objekteja hakemistossa. Tämä on tili, jota käytetään Azure AD Connect synkronointi AD yhdistäminen. Jos olet asentanut käyttämällä pika-asetuksia, se on etuliite MSOL_ tili.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Osa 3: Ota käyttöön laitteen takaisinkirjoituksen Azure AD Connect:
Noudattamalla seuraavia vaiheita laitteen takaisinkirjoituksen Azure AD Connect-käyttöön.

1.  Suorita ohjattu asennus uudelleen. Valitse **Mukauta synkronointiasetusten** tehtävät-sivulta ja valitse **Seuraava**.
![Mukautetun asennuksen synkronoinnin asetusten mukauttaminen](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2.  Valinnaiset ominaisuudet-sivun laitteen takaisinkirjoitusta ei enää on harmaa. Ota Huomautus, että jos Azure AD Connect prep vaiheet eivät ole valmis laitteen takaisinkirjoituksen ovat harmaat pois valinnaisia ominaisuuksia-sivulla. Laitteen takaisinkirjoituksen valintaruutu ja valitse **Seuraava**. Jos valintaruutu on edelleen poistettu käytöstä, on artikkelissa [osan vianmääritys](#the-writeback-checkbox-is-still-disabled).
![Mukautettu asennus laitteen takaisinkirjoituksen valinnaisia ominaisuuksia](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3.  Takaisinkirjoituksen sivulla näet annettu toimialueen oletusarvon laitteen takaisinkirjoituksen metsää nimellä.
![Mukautetun asennuksen laitteen takaisinkirjoituksen kohde metsää](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4.  Viimeistelee ohjatun toiminnon lisämäärityksiä ilman muutoksia. Tarvittaessa viitata [Mukautettu asennus Azure AD Connect.](./connect/active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Ehdollinen käytön käyttöön ottaminen
Yksityiskohtaiset ohjeet, jotta tämä skenaario ovat käytettävissä [paikallisen ehdollinen käytön Azure Active Directory laitteen rekisteröinti on määritetty](https://msdn.microsoft.com/library/azure/dn788908.aspx).

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Tarkista laitteiden synkronoidaan Active Directoryyn
Laitteen takaisinkirjoituksen pitäisi nyt toimi oikein. Huomaa, että voi kestää jopa 3 tuntia laitteen objektit voidaan kirjoittaa takaisin AD.  Voit varmistaa, että laitteet on synkronoitavan oikein, toimi seuraavasti synkronointi sääntöjen jälkeen:

1.  Käynnistä Active Directory-hallintakeskus.
2.  Laajenna RegisteredDevices toimialueella, joka on liitetty.
![Active Directory-hallintakeskukseen rekisteröityjen laitteet](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3.  Nykyinen rekisteröity laitteet näkyvät siellä.
![Active Directory-hallintakeskukseen rekisteröityjen laitteet-luettelosta](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Vianmääritys

### <a name="the-writeback-checkbox-is-still-disabled"></a>Takaisinkirjoituksen-valintaruutu on edelleen poissa käytöstä
Jos laitteen takaisinkirjoituksen valintaruudun valinta ei ole käytössä, vaikka olet noudattanut edellä kuvatut vaiheet kaikkia, seuraamalla näitä ohjeita opastaa sinua ohjattu asennus tarkistaa ennen ruutu on käytössä.

Ensin ensimmäisen:

- Varmista, että vähintään yksi metsää on Windows Server 2012R2. Laitteen objektilaji on sijaittava.
- Jos ohjattu asennus on käynnissä, valitse muutoksia ei havaita. Tässä tapauksessa Suorita ohjattu asennus ja suorita se uudelleen.
- Varmista, että saadaan alustus komentosarja tili on todella Active Directory Connectorin käyttämä oikea käyttäjä. Voit tarkistaa tämän toimimalla seuraavasti:
    - Avaa Käynnistä-valikosta **Synkronointipalvelu**.
    - Avaa **Yhdysviivat** -välilehti.
    - Hakea yhdistimen tyyppi Active Directory-toimialueen palveluista ja valitse se.
    - Valitse **Toiminnot**-kohdassa **Ominaisuudet**.
    - Siirry **yhteyden muodostaminen Active Directory metsää**. Tarkista, että toimialue ja käyttäjän määritetty tämän näytön vastine-komentosarjan tilille.
![Connector-tilin synkronoinnin palvelun hallinta](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Tarkista Active Directoryn määritykset:
- Varmista, että laitteen rekisteröintipalvelu sijaitsee alla sijainti (CN = DeviceRegistrationService, CN = laitteen rekisteröinti palvelujen CN = laitteen rekisteröinti Configuration CN = palveluja, CN = Configuration) määritysten nimeämiskäytäntö kontekstissa.

![Vianmääritys DeviceRegistrationService määritysten nimitila](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

- Varmista hakemalla määritysten nimitila on vain yksi kokoonpano-objekti. Jos näkyvissä on enemmän kuin yksi, poista Monista.

![Vianmääritys, etsiä päällekkäisiä kohteita](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

- Varmista laitteen rekisteröintipalvelu kohdetta, koska-DeviceLocation-määritettä ei sisällä tietoja ja arvo. Hakukentän sijaintia ja varmista, että se ei sisällä tietoja objektilaji koska-DeviceContainer kanssa.

![Vianmääritys, koska DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Vianmääritys RegisteredDevices objektiluokka](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

- Tarkista käyttämällä Active Directory Connectorin tilillä on tarvittavat käyttöoikeudet edellisessä vaiheessa löytämä rekisteröity laitteiden säilö. Tämä on tässä säilössä odotettu käyttöoikeuksia:

![Liittyviä ongelmia, Tarkista käyttöoikeudet säilö](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

- Tarkista Active Directory-tili on CN = laitteen rekisteröinti Configuration CN = palveluja, CN = kokoonpano-objekti.

![Liittyviä ongelmia, tarkista laitteen rekisteröinti määritysten käyttöoikeudet](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Lisätietoja
- [Riskin ehdollisen käyttöoikeuden hallinta](active-directory-conditional-access.md)
- [Paikallisen ehdollinen käytön Azure Active Directory laitteen rekisteröinti määrittäminen](https://msdn.microsoft.com/library/azure/dn788908.aspx)

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
