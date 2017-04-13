<properties
    pageTitle="Azure AD Connect kunto usein kysytyt kysymykset"
    description="Nämä usein kysytyt kysymykset vastauksia kysymyksiin Azure AD yhteyden kunto. Nämä usein kysytyt kysymykset kattaa kysymyksiin palvelun, kuten laskutuksen mallin, ominaisuuksia, rajoitukset ja tuki."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Azure AD Connect kunto usein kysytyt kysymykset

Nämä usein kysytyt kysymykset vastauksia kysymyksiin Azure AD yhteyden kunto. Nämä usein kysytyt kysymykset kattaa kysymyksiin palvelun, kuten laskutuksen mallin, ominaisuuksia, rajoitukset ja tuki.

## <a name="general-questions"></a>Yleisiä kysymyksiä



**Kysymys: saan hallita useita Azure AD-kansioita. Miten vaihdan Azure Active Directory-Premium on?**

Voit siirtyä eri Azure AD-alihallinnat valitsemalla tällä hetkellä allekirjoitetun käyttäjänimi-oikeassa yläkulmassa ja valitsemalla haluamasi tili. Jos tiliä ei ole tässä luettelossa, valitse Kirjaudu ulos ja käytä sitten kansio, joka on Azure Active Directory Premium käytössä kirjautua yleisen järjestelmänvalvojan tunnistetiedot.

## <a name="installation-questions"></a>Asennuksen kysymyksiä



**K: mikä on vaikutus asentaminen Azure AD yhteyden kunto-agentti yksittäisiä palvelimissa?**

Vaikutus asentaminen Microsoft Azure AD yhteyden kunto agenttien vuoksi ADFS, Web-sovelluksen välityspalvelimet, Azure AD Connect (sycn)-palvelimet-toimialueen ohjaimet on pienin suorittimen, muistin kulutus kaistanleveys ja tallennustilaa osalta.

Alla ovat arvion.

- Suorittimen kulutus: noin 1 % suurempi
- Muistinkulutus: enintään 10 prosenttia yhteensä järjestelmän-muistia

>[AZURE.NOTE] Jos agentti ei voi toimittaa Azure-agentti tallentaa tiedot paikallisesti määritetty enimmäismäärä ylöspäin. Agentti korvaa "välimuistiin" tiedot "vähintään viimeksi määrä" välein.

- Paikallinen puskurin tallennustila Azure AD yhteyden kunto tekijöiden: noin 20 Mt
- AD FS-palvelin on suositeltavaa valmistella levytilaa Azure AD yhteyden kunto tekijöiden käyttänyt kaikki valvontatietojen, ennen kuin se korvautuu AD FS valvonta kanavan 1 024 megatavua (1 gt).

**K: vaatii uudelleenkäynnistyksen Omat Azure AD yhteyden kunto-tekijöiden asennuksen aikana?**

Ei. Niistä asennus ei edellytä palvelimen käynnistämistä uudelleen. Seuraavia asioita valmistelevat asennuksen voi kuitenkin säätää palvelimeen uudelleen.

Esimerkiksi Windows Server 2008 R2 asennuksen .net 4.5 Framework vaatii palvelimeen uudelleen.


**K: onko Azure AD yhteyden kunto Services käydä läpi läpivienti http-välityspalvelin?**

Kyllä.  Saat siirtymällä toimintoja, voit määrittää, voit lähettää lähtevä http-pyyntöjen HTTP-välityspalvelin kunto-agentti. Lisätietoja on artikkelissa [määrittäminen Azure AD yhteyden kunto agenttien vuoksi käyttämään HTTP-välityspalvelin.](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)
Jos haluat määrittää välityspalvelimen agentti rekisteröinnin yhteydessä, joudut ehkä muuttamaan Internet Explorerin välityspalvelimen etukäteen.
1. Avaa Internet Explorer-asetukset > Internet -> Asetukset -> yhteydet -> Lähiverkon asetukset.
2. Valitse Käytä välityspalvelinta lähiverkossa.
3. Valitse Lisäasetukset, jos käytössäsi on eri välityspalvelimen portit HTTP ja HTTPS/suojatun.

**K: Azure AD yhteyden kunto-palveluiden perustodentamista Http-välityspalvelimet yhdistettäessä?**

Ei. Tarkoitetut haluamaansa käyttäjänimen ja salasanan määrittäminen Perustodentamista ei tueta.


**K: mitä AD DS-versio tukevat mukaan Azure AD yhteyden kunto AD DS: ÄÄN?**

Seuranta AD DS tuetaan, kun asennettuihin OS-versiot:

- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

## <a name="operations-questions"></a>Toimintojen kysymyksiä



**K: onko taloudessani valvoa Omat AD FS sovelluksen välityspalvelimet tai Omat Web-sovelluksen välityspalvelimien?**

Ei, tarkistaminen ei tarvitse ottaa käyttöön Web-sovelluksen välityspalvelimen (WAP)-palvelimet. Ottaa sen käyttöön AD FS-palvelimiin.


**K: miten Azure AD yhteyden kunto ilmoitusten ratkaistaan?**

Azure AD yhteyden kunto ilmoitusten ratkaistaan success ehdon. Azure AD yhteyden kunto tekijöiden tunnistaminen ja raportoi onnistumisen ehdot-palveluun säännöllisin väliajoin. Muutama ilmoitusten ja on ajan perusteella. Toisin sanoen jos samassa virhetilassa ei käytetä-hälytysten 72 tunnin kuluessa, ilmoitus automaattisesti ratkennut.




**K: mitä palomuurin porttien pitääkö Avaa Azure AD yhteyden kunto agentti toimii?**

Tarvitset TCP/UDP-portit 443 ja avaa Azure AD yhteyden kunto agentti voivat pitää yhteyttä Azure AD kunto-Palvelupäätepisteet 5671.


**K: Miksi näen kahta samannimistä Azure AD yhteyden kunto-portaalissa?**

Kun poistat agentti palvelimesta, palvelimeen ei poisteta automaattisesti Azure AD yhteyden portaalista.  Jos poistettu agentti palvelimesta manuaalisesti tai poistaa palvelimen itse, sinun on poistettava Azure AD yhteyden kunto-portaalista nimipalvelin-merkintä. Jos haluat lisätietoja, siirry [palvelimeen tai palveluun poisto.](active-directory-aadconnect-health-operations.md#delete-a-server-or-service-instance)

Jos reimaged palvelimeen tai luoda uuden palvelimen saman tiedot (esimerkiksi tietokoneen nimi) ja ei poistanut jo rekisteröity server Azure AD yhteyden kunto-portaalista asennettu agentti uusi palvelimessa, näkyviin voi tulla kaksi merkintää, jolla on sama nimi.  
Tässä tapauksessa sinun tulee poistaa kuuluvat vanhat palvelimeen manuaalisesti tapahtuma. Palvelimen tiedot on vanhentunut.

## <a name="related-links"></a>Aiheeseen liittyvät linkit

* [Azure AD Connect kunto](active-directory-aadconnect-health.md)
* [Azure AD Connect kunto agentti asennus](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect kunto-toiminnot](active-directory-aadconnect-health-operations.md)
* [Käyttämällä Azure AD yhteyden kunto AD FS](active-directory-aadconnect-health-adfs.md)
* [Azure AD yhteyden kunto käytön synkronointi](active-directory-aadconnect-health-sync.md)
* [Kuntotietojen käyttämällä Azure AD yhteydessä AD DS: ÄÄN](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect kunto versiohistoria](active-directory-aadconnect-health-version-history.md)
