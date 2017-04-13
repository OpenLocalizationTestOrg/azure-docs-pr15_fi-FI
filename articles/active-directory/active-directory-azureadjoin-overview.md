<properties
    pageTitle="Cloud ominaisuuksia Windows 10-laitteisiin kautta Azure Active Directory-liittyä laajentaminen | Microsoft Azure"
    description="Tässä artikkelissa yksityiskohtaiset yleiskatsaus siitä, miten Windows 10-laitteissa voit hyödyntää Azure AD liittäminen Hae rekisteröity Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>Cloud ominaisuuksia Windows 10-laitteisiin kautta Azure Active Directory-liittyä laajentaminen

## <a name="what-is-azure-active-directory-join"></a>Mikä on Azure Active Directory-liittyä?
Azure Active Directory Join (Azure AD-liittyä) on toiminto, joka rekisteröi laite yrityksen omistamien Azure Active Directoryn keskitetyn hallinnan laitteen käyttöön. Se on mahdollista työntekijöiden ja opiskelijoiden muodostaa yrityksen pilven kautta Azure Active Directory-käyttäjille. Näin yksinkertaistettu Windows käyttöönotto ja organisaation sovellukset ja resurssien käytön Windows mistä tahansa laitteesta, sekä yrityksen omistamien ja henkilökohtaisesti omistamien (BYOD).

Azure AD-liitos on tarkoitettu yrityksille, jotka ovat ensimmäisen/cloud-vain pilvipalveluita--yleensä pieni - ja keskisuurten yritysten, joka ei ole paikallisen Windows Server Active Directory infrastruktuuri. Mainitun, Azure AD-liity voit ja näkyy myös voidaan käyttää laitteissa, jotka tekevät perinteinen toimialueen liitoksen (mobiililaitteiden, esimerkiksi) tai Office 365: ssä tai Azure AD SaaS muista sovelluksista ensisijaisesti käyttäville käyttäjille kykenemätön suurissa organisaatioissa.

Vaikka perinteinen toimialueen liity tarjoaa edelleen parhaat paikalliseen kohdata laitteissa, jotka pystyvät toimialueen liittyminen, Azure AD liity sopii laitteita, joka ei liity toimialueen. Azure AD-liity sopii myös pilveen käyttäjien hallinta. Se tekee käyttämällä mobiililaitteen hallintatoiminnot sijaan perinteinen toimialueen hallintatyökalut, kuten Ryhmäkäytäntö ja System Center Configuration Manager (SCCM).

![Yrityksen laitteet ja henkilökohtaisten laitteiden paikallisen Active Directory ja Azure AD yleiskatsaus](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)


## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Miksi yrityksille antaa Azure AD liittyä?

* **Yrityksille, jotka ovat suurelta pilvipalvelussa**: Jos olet siirtänyt tai malli, jossa haluat enemmän pilveen pienenee paikallisen vaatiman-tallennustilan ja siirtyy, Azure AD liittyä voi hyödyntää voit. Olet ehkä luonut Azure AD-tilien manuaalisesti tai avulla synkronoidaan paikallisen Active Directory. Kummassakin tapauksessa on Azure AD tili ja voit käyttää sitä Windows 10: een kirjautuminen. Käyttäjät voivat liittyä tietokoneeseensa Azure AD joko (Tervetuloa-ohjelma) ruutuun-versio tai asetukset-valikon kautta. Kun olet jo liittynyt, käyttäjien Nauti yhden kertakirjautumisportaaliin (SSO) cloud resurssien käytön Office 365: ssä, kuten selaimessa tai Office-sovelluksissa.

* **Oppilaitosten**: jokin tilanteita, joissa on kuulet usein on oppilaitosten ei ole käyttäjän kahdenlaisia: opettajille ja opiskelijoille. Henkilökunnan jäsenten pidetään kahdeksalla organisaation jäsenille, jotta paikallisen-tilien luominen niiden on. Mutta opiskelijoiden shorter-term organisaation jäsenten ja jota ei voi hallita Azure AD. Tämä tarkoittaa directory asteikko voivat sijaita pilveen tallennetaan paikallisesti sijaan. Se myös tarkoittaa, että opiskelijat voit kirjautua Windows niiden Azure AD-tilien kanssa pääset käsiksi Office 365-resursseista selaimessa tai Office-sovelluksissa.

* **Jälleenmyynti yrityksille**: toinen tilanne usein kysyttyihin tietoja asiakkaiden on haluavat hallinta kausiluonteisista työntekijöiden on helpompaa.  Tilien kahdeksalla, Täysipäiväisen työntekijöiden luodaan uudelleen, yleensä, kuin paikallisen toimialueen liittyneet tietokoneissa tilit. Mutta kausiluonteisista työntekijät ovat shorter-term jäsenillä organisaatiokaavion, joten kannattaa hallita niitä missä käyttöoikeuksia voidaan helpottaa siirtää ympärille. Näiden käyttäjätilien luominen pilvipalvelussa kanssa Office 365-käyttöoikeuksien avulla saat edut Windows ja Office-sovellusten Azure AD-tilillä kirjautuminen käyttäjät. Tänä aikana joustavammin niiden käyttöoikeudet säilyttää sen jälkeen, kun he lähtevät.
* **Muiden yritysten**: vaikka käyttäjät säilyttää paikallisen Active Directoryssa, voit helpottaa edelleen ei tarvitse olla Azure AD-liittyneet käyttäjät. Tämä johtuu siitä Azure AD tarjoaa Azure AD yksinkertaistettu liittävä kokemus, tehokas hallinta, automaattinen mobiililaitteiden hallinnan rekisteröinti ja Sign-ominaisuuksien ja paikallisten resurssit.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Mitä ominaisuuksia Azure AD liittyä tarjota?
Azure AD osallistua saat seuraavasti:

* **Yrityksen omistamien laitteiden itse valmistelu**: Windows 10-käyttäjät voivat määrittää uusi huonolaatuiset laite-ruutuun mobiilikäyttöä ilman IT osallistuminen.


* **Nykyaikainen lomakkeen tekijät tuki**: Azure AD liity toimii laitteissa, joka ei ole perinteinen toimialueen liittyä ominaisuuksia.  


* **Olemassa olevan organisaation tili tuki**: käyttäjien et enää tarvitse luoda ja ylläpitää henkilökohtainen Microsoft-tili, saat parhaan kokemuksen yrityksen myöntänyt laitteissa samalla tavalla kuin Windows 8: aan. He voivat käyttää aiemmin luotuja niiden työ-tilejä Azure AD-sijaan. Monissa organisaatioissa tämä oikeastaan tarkoittaa, että käyttäjät voivat määrittäminen ja kirjaudu Windows samoilla tunnuksilla, jota käytetään access Office 365: ssä.


* **Automaattinen mobiililaitteiden hallinnan rekisteröinti**: laitteiden voi olla automaattisesti rekisteröity mobiililaitteiden hallinta, kun Azure AD. Tämä tapahtuu Microsoft Intunella ja kumppanin mobiililaitteen ratkaisujen kanssa. Kun Laitehallinnan tehdään Intune-IT-järjestelmänvalvojat voivat näytön/Hallitse Azure AD liittyneet rinnalla SCCM hallintakonsoli toimialueeseen liittymistä, mikä.


* **Kertakirjautumisen yrityksen resurssien**: käyttäjien Nauti kertakirjautumisen Windowsin työpöydältä sovellukset ja resurssien pilvipalveluun, kuten Office 365: ssä ja business-sovelluksia, jotka käyttävät Azure Active Directory-todennusta ja [Azure AD Connect](active-directory-azureadjoin-deployment-aadjoindirect.md)tuhansia. Yrityksen omistamien laitteita, jotka ovat liittyneet Azure AD Nauti SSO myös paikallisen resursseja, kun laite on yrityksen verkossa ja missä tahansa, kun nämä resurssit ovat tarjoamia [Azure AD-sovelluksen välityspalvelimen](https://msdn.microsoft.com/library/azure/Dn768219.aspx)kautta.


* **OS tilan Roaming**: helppokäyttötoimintojen asetuksia, sivustot, Wi-Fi salasanat ja muut asetukset ovat synkronoituina yrityksen omistamien laitteiden tarvitsematta omalla Microsoft-tilillä.


* **Enterprise-sivuille Windows-kaupan**: Windows-kaupan tukee sovelluksen hankkiminen ja käyttöoikeudet Azure AD-tilien kanssa. Organisaatiot voivat volyymikäyttöoikeus sovellukset ja ne ovat käytettävissä organisaation käyttäjille.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Miten eri laitteissa toimivat Azure AD liittyä kanssa?

| Yrityksen laitteen (liitetty paikallisen toimialueen)                                                                                                                                                                                         | Yrityksen laitteen (liittyneet pilvipalveluun)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Omat laite                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Käyttäjät voivat kirjautua sisään Windows työn tunnuksilla (tavalla kuin tänään).                                                                                                                                                                        | Käyttäjät voivat kirjautua sisään Windows työn tunnistetiedot, jotka hallitaan Azure AD-kanssa. Tämä koskee yrityksen laitteiden tapauksissa, jossa on kolme: 1) organisaation ei ole Active Directory paikallinen (esimerkiksi small business). 2) organisaation ei luo kaikki käyttäjätilit Active Directory (esimerkiksi opiskelijoille, konsulteille tai kausiluonteisista työntekijöiden ovat ei luoda tiliä Active Directoryn). 3) organisaatiossa on yrityksen laitteita, joka ei voi liittää (paikallisen)-toimialueen, kuten puhelimeen tai tablettiin käynnissä Mobile SKU (esimerkiksi toissijainen laite keskimäärin factory/jälleenmyynti kerroksen). Azure AD-liity tukee sekä liitetyt hallittua organisaatioita yrityksen laitteiden liittymistä. | Käyttäjien kirjautuminen Windows niiden henkilökohtainen Microsoft-tilin tunnistetiedot (ei muutosta) avulla.                                                |
| Käyttäjillä on pääsy sijaitsevaa asetukset ja yrityksen Windows-kaupan. Näiden palvelujen toimi tilien työmäärän ja ei edellytä omalla Microsoft-tilillä. Tämä edellyttää organisaatiot voivat muodostaa yhteyden Azure AD niiden paikallisen Active Directory.                                        | Käyttäjät voivat tehdä Omatoiminen asetukset. Ne voivat mennä kautta ensimmäisen käyttökerran-ohjelmaa (FRX) niiden työpaikan vaihtoehtoisena siten, että se säännöstä laitteet, vaikka lopettamista tuetaan.                                                                                                                                                                                                                                                                                                                                                                             | Käyttäjät voivat helposti lisätä työpaikan, jota hallitaan Active Directoryssa tai Azure AD.                                                      |
| Käyttäjillä on SSO mahdollisuuden työpöydältä toimimaan sovellukset, sivustojen ja resurssien – kuten paikallisen resurssit ja cloud-sovellukset, jotka käyttävät Azure AD todennusta varten.                                                                                                            | Laitteet rekisteröidään automaattisesti yrityksen hakemistosta (Azure AD) ja automaattisesti rekisteröity mobiililaitteiden hallinta. (Azure AD-Premium-ominaisuus).                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Käyttäjillä on SSO mahdollisuuden sovellukset ja sivustojen ja resurssien työ-tilillä.                                              |
| Käyttäjät voivat lisätä niiden henkilökohtainen Microsoft-tilien käyttää omia kuviasi ja tiedostoja ilman vaikuttavat yrityksen tietoja. (Roaming asetukset toimivat edelleen niiden työ-tilien kanssa.) Microsoft-tilin avulla SSO ja ohjaa enää asetuksia roaming.  | Käyttäjät voivat tehdä käyttäjien Omatoiminen salasanan palauttaminen (Vaihtaminen)-winlogon, mikä tarkoittaa, että he voivat vaihtaa unohtuneen salasanan. (Azure AD-Premium-ominaisuus).                                                                                                                                                                                                                                                                                                                                                                                                                                   | Käyttäjät voivat käyttää yrityksen Windows-kaupan niin, että he voivat hankkia ja liiketoiminta-sovellusten käyttäminen Omat laitteensa. |                                                               |


## <a name="additional-information"></a>Lisätietoja
* [Windows 10: n yrityksen: tapoja käyttää laitteet](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud ominaisuuksia Windows 10-laitteisiin kautta Azure Active Directory-liittyä laajentaminen](active-directory-azureadjoin-user-upgrade.md)
* [Käyttäjätietojen ilman salasanan vaihtamiseen Microsoft Passport todennustapa](active-directory-azureadjoin-passport.md)
* [Lisätietoja käyttötapoja Azure AD liittyminen](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Toimialueen liittyneet laitteiden yhdistäminen Azure AD for Windows 10: ssä kokemukset](active-directory-azureadjoin-devices-group-policy.md)
* [Määritä Azure AD liittyminen](active-directory-azureadjoin-setup.md)
