<properties
    pageTitle="Määritä automaattinen laitteen rekisteröinti Windows 8.1 liitetty laitteiden | Microsoft Azure"
    description=" Ohjeet Windows 8.1 toimialueeseen liittyneet laitteet automaattisesti Rekisteröi Azure AD ryhmäkäytännön määrittäminen. "
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="Markvi"/>

# <a name="configure-automatic-device-registration-for-windows-81-domain-joined-devices"></a>Automaattinen laitteen rekisteröinti Windows 8.1 toimialueen liittyneet laitteiden määrittäminen

Active Directory-ryhmäkäytännön avulla voit määrittää Windows 8.1 liitetty laitteet automaattisesti rekisteröitymään Azure AD. Määrittämiseen ryhmäkäytännön vähintään yhden toimialueen liitetyssä Windows Server 2012 R2 tai Windows 8.1 tietokoneessa on oltava asennettuna Ryhmäkäytäntöjen hallinta-toiminnon. Kun tämän ryhmäkäytännön on käytössä, kuka tahansa toimialuekäyttäjä kirjautuu-järjestelmään koneen on automaattisesti ja äänettömästi rekisteröity laitteen objektia Azure AD. Azure AD rekisteröity kaikille käyttäjille fyysinen laite on yksi laite-objekti. Muista lukea ja viimeistele edellytykset automaattisen laitteen rekisteröinnin, Azure Active Directory for Windows Domain-Joined laitteiden kanssa.

>[AZURE.NOTE]
 Uusimman ohjeita voit määrittää Automaattiset laitteen rekisteröinti-ohjeaiheissa [määrittäminen Windows-toimialueen automaattisen rekisteröinnin liittyneet laitteiden Azure Active Directory-hakemistosta](active-directory-conditional-access-automatic-device-registration-setup.md).



## <a name="configure-the-group-policy-for-your-windows-81-domain-joined-devices"></a>Windows 8.1 liitetty laitteiden ryhmäkäytännön määrittäminen

1. Avaa Serverin hallinta ja siirry **Työkalut** > **Ryhmäkäytäntöjen hallinta**.
2. Siirry Ryhmäkäytäntöjen hallinta, valitse toimialuesolmu, joka vastaa toimialueen, jossa haluat ottaa käyttöön **Automaattisen työpaikkasi liity**.
3. **Objektien ryhmittäminen käytännön** hiiren kakkospainikkeella ja valitse **Uusi**. Anna Ryhmäkäytäntö objektin nimi, esimerkiksi **Automaattinen työpaikkasi liity**. Valitse **OK**.
4. Napsauta uusi ryhmäkäytäntöobjekti hiiren kakkospainikkeella ja valitse sitten **Muokkaa**.
5. Siirry **tietokoneen määritykset** > **käytännöt** > **järjestelmänvalvojan malleja** > **Windowsin osien** > **työpaikkasi liity**.
6. Napsauta automaattisesti työpaikkasi liity asiakastietokoneiden ja valitse sitten **Muokkaa**.
7. Valitse käytössä-valintanappi ja valitse sitten Käytä. Valitse **OK**.
8. Ryhmäkäytäntöobjekti voi nyt linkittää valitsemaasi sijaintiin. Jotta käytäntö kaikille organisaatiossa liitetty Windows 8.1-laitteet, linkki Ryhmäkäytännön toimialueen.

## <a name="unregistering-your-windows-81-domain-joined-devices"></a>Windows 8.1-toimialueen rekisteröintiä liittyneet laitteet

Voit halutessasi unregister liitetty Windows 8.1-laitteiden toimimalla seuraavasti: työpaikkasi liittyä Ryhmäkäytäntö muokkausasetuksia luotu edellisessä osassa. Määritä automaattisesti työpaikkasi liity asiakkaan tietokoneiden käytäntö käytöstä. Tämä estää uudet laitteet liittymästä työpaikan automaattisesti.

Unregister aiemmin liittyneet toimialueen Windows 8.1-koneet mukaan yksi kaksi seuraavia:

* Vaihtoehto 1: **Poista Windows 8.1 toimialueen liittyneet käyttävän tietokoneen asetukset**
  1. Windows 8.1-laitteessa, siirry **Tietokoneen asetukset** > **verkon** > **työpaikkasi**
  2. Valitse **Jätä**.
Tämä toimenpide on toistettava kunkin toimialueen käyttäjälle, joka on kirjautunut tietokoneen ja se on automaattisesti työpaikkasi liittyneet.

* Vaihtoehto 2: Unregister Windows 8.1 toimialueen liitettyjen laitteen komentosarjan avulla
    1. Avaa komentokehote Windows 8.1-tietokoneen ja suorita seuraava komento:` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
Tämä komento on suoritettava kontekstissa, joka on allekirjoitettu toimialueen kunkin käyttäjän tietokoneen kyselyjä.

##<a name="event-viewer--errors-for-windows-81-domain-joined-devices"></a>Tapahtumienvalvonta ja virheet Windows 8.1 toimialueen liittyneet laitteet

Windowsin tapahtumalokiin Windows 8.1-tietokoneen näyttää laitteen rekisteröinti viestejä. Löydät viestit onnistui- ja epäonnistuneista tapahtumat. 

Tapahtumaloki löytyy, sovellusten ja palveluiden **lokit**Viewer > **Microsoft** > **Windows > työpaikkasi liittyä**.

##<a name="additional-details"></a>Lisätietoja

Ryhmäkäytännön avulla ajoitettu tehtävä järjestelmään, joka toimii käyttäjän konteksti-käyttäjien sisäänkirjautumisessa käynnistyy. Tehtävä äänettömästi rekisteröidä käyttäjään ja laitteeseen Azure AD kirjautuminen päätyttyä. Ajoitettu tehtävä löytyy tehtävien ajoituksen kirjasto-kohdassa **Microsoft**Windows 8.1, mikä > **Windows** > **Työpaikkasi liity**. Tehtävän suorittaa ja rekisteröidä kaikista Active Directory-käyttäjät, merkki-kyselyjä. 

## <a name="additional-topics"></a>Muita ohjeita
- [Azure Active Directory-laitteen rekisteröinti yleiskatsaus](active-directory-conditional-access-device-registration-overview.md)
- [Automaattinen laitteen rekisteröinti Azure Active Directory for Windows 10: ssä toimialueen liittyneet laitteiden kanssa](active-directory-conditional-access-automatic-device-registration.md)
- [Automaattinen laitteen rekisteröinti Windows 7: n toimialueen liittyneet laitteiden määrittäminen](active-directory-conditional-access-automatic-device-registration-windows7.md)

