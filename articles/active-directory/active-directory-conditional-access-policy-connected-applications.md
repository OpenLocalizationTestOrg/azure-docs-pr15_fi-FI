<properties
    pageTitle="Määrittää laitteen perustuva ehdollisen käyttöoikeuden käytännön Azure Active Directory-yhteys sovellusten | Microsoft Azure"
    description="Laitteen perustuva ehdollisen käyttöoikeuden käytäntöjen Azure AD-yhteys-sovellusten määrittäminen."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="markvi"/>


# <a name="set-device-based-conditional-access-policy-for-azure-active-directory-connected-applications"></a>Laitteen perustuva ehdollisen käyttöoikeuden käytännön Azure Active Directory-yhteys sovellusten määrittäminen


Azure Active Directory (Azure AD) laitteen perustuva ehdollisen käyttöoikeuden auttaa suojaamaan organisaation resurssit:

- Access yrittää tuntematon tai ei-hallitun laitteilla.
- Laitteet, jotka eivät vastaa organisaation suojauskäytäntöjä.

Katso yleiskuvaus ehdollinen access- [ehdollisen Azure Active Directory-käyttöoikeuden](active-directory-conditional-access.md).

Voit määrittää laitteen perustuva ehdollisen käyttöoikeuden käytännöt suojaaminen nämä sovellukset:

- Office 365-palveluun SharePoint Online-organisaation sivustojen ja tiedostojen suojaaminen
- Office 365: n Exchange Online-organisaation sähköpostin suojaaminen
- Ohjelmiston Palvelusovellusten (SaaS), jotka ovat yhteydessä Azure AD todennusta varten
- Paikallisen sovelluksia, jotka on julkaistu Azure AD-sovelluksen välityspalvelimen-palvelujen avulla

Voit määrittää laitteen perustuva ehdollinen access-käytännön luominen Azure-portaalissa, siirry tietyn sovelluksen hakemistossa.


  ![Sovellusten Azure portaalin hakemistossa luettelo] (./media/active-directory-conditional-access-policy-connected-applications/01.png "Sovellukset")


Valitse sovellus ja valitse sitten **Määritä** -välilehti ja Määritä ehdollinen käyttöoikeuskäytäntö.  


  ![Määritä sovellus] (./media/active-directory-conditional-access-policy-connected-applications/02.png "Laitteen perusteella käyttösäännöt")




**Voit määrittää laitteen perustuva ehdollinen access-käytäntö **laitteen perusteella käyttösäännöt** -osassa **Ota käyttöön käyttösäännöt**, valitse.**

Laitteen perustuva ehdollinen käyttöoikeuskäytäntö on kolme osaa:

- **Jos haluat käyttää**. Käyttäjien käytäntöä sovelletaan aluetta.

- **Laitteen säännöt**. Ehdot laitteen on täytettävä ennen kuin sitä voidaan käyttää sovellusta.

- **Sovelluksen käyttäminen**. Asiakassovellukset (alkuperäinen ja selaimen) käytäntö koskee.

  ![Laitteen perustuva käyttöoikeuskäytäntö kolme osaa] (./media/active-directory-conditional-access-policy-connected-applications/03.png "Laitteen perusteella käyttösäännöt")


## <a name="select-the-users-the-policy-applies-to"></a>Valitse käyttäjät käytäntö

Valitse **Käytä kohteeseen** -osassa käytäntö koskee käyttäjien alue.

Kun luot access käytännön vaikutusalue käyttäjille on kaksi vaihtoehtoa:

- **Kaikki käyttäjät**. Hallintakäytäntö kaikki käyttäjät, joilla sovellus.

- **Ryhmät**. Rajoita käytännön käyttäjille, jotka ovat tietyn ryhmän jäsen.

![Käytä käytännön kaikille käyttäjille tai ryhmälle] (./media/active-directory-conditional-access-policy-connected-applications/11.png "Käytä")


 Jollet halua käytännön käyttäjän, valitse **lukuun ottamatta** -valintaruutu. Tästä on hyötyä, kun haluat myöntää pääsyn väliaikaisesti sovelluksen tietyn käyttäjän käyttöoikeuksia. Valitse tämä vaihtoehto, esimerkiksi, jos jotkin käyttäjät on laitteita, jotka eivät ole valmis ehdollisen käyttöoikeuden. Laitteet ei voi rekisteröidä vielä tai ne ovat poissa yhteensopivuus.


## <a name="select-the-conditions-that-devices-must-meet"></a>Valitse haluamasi ehdot laitteet on täytettävä

**Laitteen sääntöjen** avulla voit määrittää ehdot laitteen on täytettävä sovellusta.

Voit määrittää laitteen perustuva ehdollisen käyttöoikeuden laitteen näitä tiedostotyyppejä:

- Windows 10 vuosipäivä päivitys Windows 8.1 ja Windows 7: ssä
- Windows Server 2016, Windows Server 2012 R2, Windows Server 2012: n ja Windows Server 2008 R2
- iOS-laitteisiin (iPad, iPhone)
- Android-laitteille

Mac-tuki on tulossa.

  ![Käytä käytännön laitteet] (./media/active-directory-conditional-access-policy-connected-applications/04.png "Sovellukset")

 >[AZURE.NOTE] Lisätietoja toimialueen liittynyt ja Azure AD liittyneet laitteiden erot artikkelissa [työpaikalle käyttämällä Windows 10: ssä, mikä](active-directory-azureadjoin-windows10-devices.md).

Sinulla on kaksi vaihtoehtoa, jos laitteen säännöt:

- **Kaikki laitteet on oltava yhteensopiva**. Laitteen ympäristöissä, joka käyttää sovelluksen on oltava yhteensopiva. Laitteissa, joissa ympäristöissä, jotka eivät tue laitteen perustuva ehdollisen käyttöoikeuden ei ole käyttöoikeutta.

- **Vain valittujen laitteita on oltava yhteensopiva**. Vain tietyn laitteen ympäristöjen on oltava yhteensopiva. Muiden ympäristöjen tai muiden ympäristöjen, jotka voivat käyttää sovelluksen pääsevät.

  ![Jos laitteen säännöt vaikutusalue määritetään] (./media/active-directory-conditional-access-policy-connected-applications/05.png "Sovellukset")

Azure AD liittynyt laitteet ovat yhteensopivia, jos ne on merkitty **yhteensopiva** hakemistossa Intune tai kolmannen osapuolen mobiililaitteen hallintajärjestelmän, integroituu Azure AD.

Toimialueen liittyneet laite on yhteensopiva Jos:

- Se on rekisteröity Azure AD. Monet organisaatiot Käsittele toimialueeseen liittyneet laitteiden luotettujen laitteet.
- Se on merkitty **yhteensopiva** Azure AD System Center hallintatoiminto 2016 mukaan.

 ![Toimialueen liittyneet laitteita, jotka ovat yhteensopivia] (./media/active-directory-conditional-access-policy-connected-applications/06.png "Laitteen säännöt")

Windowsin henkilökohtaisten laitteiden ovat yhteensopivia, jos ne on merkitty **yhteensopiva** hakemistossa Intune tai kolmannen osapuolen mobiililaitteen hallintajärjestelmän, integroituu Azure AD.

-Windows laitteet ovat yhteensopivia, jos ne on merkitty kuin **yhteensopiva** hakemistossa Intune.

 >[AZURE.NOTE] Lisätietoja laitteen yhteensopivuuden Azure AD-eri järjestelmien määrittämisestä on artikkelissa [Azure Active Directory ehdollinen](active-directory-conditional-access.md).


Voit valita yhden tai usean laitteen käyttöympäristöt laitteen perustuva käyttöoikeuskäytäntö. Tämä vaihtoehto sisältää Android-, iOS-, Windows Mobile (Windows 8.1 puhelimissa ja taulutietokoneissa), ja Windows (kaikki muut Windows laitteilla, myös kaikki Windows 10-laitteissa).
Käytännön arvioinnin ilmenee vain valitun ympäristöissä. Jos laite, joka yrittää access ei ole käynnissä jokin valitun ympäristöjen, laite käyttää sovellusta, jos käyttäjä voi käyttää. Laite ei käytäntöä arvioidaan.

![Valitse laitteen säännöt ympäristöt] (./media/active-directory-conditional-access-policy-connected-applications/07.png "Laitteen säännöt")


## <a name="set-policy-evaluation-for-a-type-of-application"></a>Käytännön arvioinnin sovelluksen tyypin määrittäminen

Määritä haluamasi käytännön arvioi käyttäjän tai laitteen access-sovellusten **Sovelluksen käyttäminen** -osassa.

Käytettävissä on kaksi vaihtoehtoista sovelluksen sisältämään tyypin:

- Selain- ja alkuperäisen sovellukset
- Alkuperäisen sovellukset

![Valitse selaimessa tai alkuperäisen sovellukset] (./media/active-directory-conditional-access-policy-connected-applications/08.png "Sovellukset")

Voit pakottaa käyttöoikeuskäytäntö sovellusten valitsemalla **selaimessa ja alkuperäisen sovellukset**. Tämän jälkeen voit lisätä:

- Selaimet (esimerkiksi Microsoft Edge Windows 10) tai Safari iOS.
- Sovellukset, jotka käyttävät Active Directory käyttöoikeuksien kirjaston (ADAL) missä tahansa ympäristössä tai, jotka käyttävät WebAccountManager (WAM) API Windows 10: ssä.

>[AZURE.NOTE] Saat lisätietoja selaintuki ja muita huomioon otettavia seikkoja käyttäjä, jolla laite-pohjaisia, käyttää varmenteen myöntäjä suojatun sovelluksen [Azure Active Directory ehdollisen käyttöoikeuden](active-directory-conditional-access.md).

## <a name="help-protect-email-access-from-exchange-activesync-based-applications"></a>Suojaa sähköpostin käytön Exchange ActiveSync-sovelluksissa

Office 365: n Exchange Online-sovelluksissa voit Exchange ActiveSync-pohjainen sähköposti-sovelluksia sähköpostin käytön estäminen Exchange ActiveSync.

![Exchange ActiveSync yhteensopivuuden asetukset] (./media/active-directory-conditional-access-policy-connected-applications/09.png "Sovellukset")

![Vaadi yhteensopiva laitteen sähköpostin] (./media/active-directory-conditional-access-policy-connected-applications/10.png "Sovellukset")


## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Active Directory ehdollisen käyttöoikeuden](active-directory-conditional-access.md)
