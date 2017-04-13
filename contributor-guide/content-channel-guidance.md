<properties title="" pageTitle="Azure teknisen sisällön kanavan ohjeet" description="Tässä artikkelissa kuvataan Microsoft sisällön kanavaa, joiden työntekijät, kumppaneiden ja yhteisön osallistujien käytettävä julkaiseminen Azure teknistä sisältöä." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/06/2015" ms.author="tysonn" />

# <a name="azure-technical-content-channel-guidance"></a>Azure teknisen sisällön kanavan ohjeet

GitHub on suhteellisen helposti (kun saat Git laskumäkien päällä) luominen ja julkaiseminen teknistä sisältöä. Mutta annettava varmistaa, että sisällön pysymisen rajoitusten sisällä teknisten rajat - on muiden lähteiden muiden muunlaisten tietojen varalta.

##<a name="technical-content-that-belongs-in-the-azure-content-pr-repository"></a>Tekniset sisältöön, joka kuuluu azure-sisältö-arvo-säilöön

**Tuotteen käyttämisestä artikkeleja** kuuluvat http://azure.microsoft.com/documentation/articles julkaisun azure-sisältö ja azure-sisältö-hinta säilöjen tietoihin. Tulisi sisältää käsitteellisiä tai käsitteellisiä tietoja ja käyttää tuotetta tarvittavat tiedot. Tekniset sisällön kanavan on teknisen sisällön näyttäminen henkilöiden **miten** tekemään tiettyjä toimenpiteitä. Voit ottaa tietoja "mikä" ja "Miksi" auttaa asiakkaita ymmärtää palveluita, mutta on artikkeleissa kannattaa keskittyä kertoo henkilöiden tehtävä ja viimeistele skenaarion asiakirjan sisältöä.

##<a name="technical-content-that-does-not-belong-in-the-azure-content-pr-repository"></a>Teknisen sisällön, joka ei kuulu azure-sisältö-arvo-säilöön

**Viittaus sisällön**: hallitun viittaus, REST API, PowerShell-cmdlet-Ohje, rakenteen viittaus ja virheen viittaus kuuluu MSDN kohdistetuissa Azure-kirjastossa (http://msdn.microsoft.com/library/azure/). Solmun, Ruby ja muuta kielen viittaus sisältöä kuuluu http://azure.github.io/ käyttöön.

Seuraavia sisältö toimitetaan muiden Azure- tai Microsoft sisällön lähteiden; Joissakin tapauksissa tietyn tyyppistä sisältöä eivät ole Microsoftin sisällön strategia osa.

- **Blogimerkinnät**: blogimerkintöjen kirjoitetaan yleensä ensimmäisen henkilön puheen ja liittyvät yleensä ilmoitukset ja kampanjat. Tämä lajittelu sisällön kuuluu yleensä Azure blogista. Monitasoisissa teknisiä tai käsitteellisiä sisältöä ei siirry blogista.

- **Tapaustutkimukset asiakkaan/artikkelit**: tapaustutkimukset ja asiakkaan jutuista on hyvin tiettyjä toimituksen, käy läpi markkinointi, ne on yrityksen oma prosessi ja ohjeita ja luodaan tietyn asiakkaiden ja kumppaneiden työt. Ne on julkaistu https://customers.microsoft.com/. Älä soita jotain Esimerkkitapaus tai asiakkaan Tarinan paitsi jos se on osa muodollisen tapauksessa tutkimuksen prosessin ja Älä julkaise se tech sisällön säilöön.

- **Koodi-ja project**: koodi ja projektin näytteiden Siirry näytteiden säilöjen tietoihin ja malli-valikoiman ajankohtainen.

- **Yhteisön spotlight/yhteisöjen resurssit**: artikkeleita, jossa kerrotaan yhteisön projektit. Repo on teknistä sisältöä Microsoft persective tuotteen käyttämisestä, ei siitä, miten käyttäjät käyttävät tuotteen. Tämä on markkinointi tai mahdollisesti blogin sisältöä. Tai antaa kertoa oman Tarinan paikoissa yhteisön tykkäykset parhaiten on yhteisön!

- **Yhteensopivuus**: alan standardit ja Azure services yhteensopivuustiedot on kirjataan https://www.microsoft.com/en-us/TrustCenter/Compliance?service=Azure#Icons. Tämä sisältää sertifikaatin varten standardit, kuten ISO-Maakohtaiset ja government kaltaisilta, pankin, kunto tai muita varmenteet.

- **Ladattavia tiedostoja**: tekniset asiakirjat toimitetaan kuin artikkeleita, ei ladattavat tiedostot. Älä luo ladattavan PDF-tiedostojen sisältöä teknisen sisällön säilöstä. Downloabable muun muassa Siirry Download Centeristä.

- **Palaute - soliciting palautetta kautta sähköpostiosoitteet**: Azure sisällön hyväksytyn palautetta polut sisällyttää palautetta linkkiä, joka tulee näkyviin sivuston alatunnisteen, tyytyväisyyttä luokitus ja tarkka ohjausobjektin, Disqus kommentit, suora artikkelissa maksujen kautta GitHub erotettu pyynnöt ja UserVoice-sivustossa. Älä lisää tämä jäsentämisen kanavien pyytämällä käyttäjät voivat lähettää palautetta sähköpostitse.

- **Tulevaisuudessa tuotteen suunnitelmien**: Älä julkaise tulevien tuotteen suunnitelmien koskevista väittämistä teknisten. Tekniset tulee kuvata vain mikä on mahdollista julkaistu tuote.

- **Ehdot**: on kaikki ylöspäin Azure ehdot: https://azure.microsoft.com/en-us/support/legal/

- **Sisällön markkinoinnin**: sisältö, joka on korkean tason ominaisuus/etu kuvaus tai vain luetteloa ylätasolla palvelun ominaisuuksia on todennäköisesti sisällön markkinoinnin. Se kuuluu markkinoinnin sivuston alueille. Markkinoinnin sisällön julkaiseminen tiedoston azure.microsoft.com työn pyynnön.

- **Alla on tietoja**: Voit ottaa vaikutus tekniset vaihtoehtoja on kustannuksen Yleiset tavalla, mutta älä tietoja ei tarjouksen tietyn hinnat tiedot artikkeleja. Lisää sivun hinnoittelutiedot linkki, olet puhuminen tietoja palvelun.

- **Osoitin artikkeleita lataukset**: sijaan valitsemalla pieni sivut, jotka sisältävät ei ladata linkki vain linkittäminen lataaminen tekniset oleellisia.

- **Tietosuojatiedot**: Microsoft Online Services, joka kattaa kaikki Azure kaikki ylöspäin tietosuojakäytäntö on. Tietyn palvelun tietosuojatiedot esitetään teknistä sisältöä, ei "tietosuojatiedot". Katso https://azure.microsoft.com/en-us/support/legal/.

- **Ohjaa artikkelit**: Kun poistat artikkelin sisältö, älä jätä on artikkelissa julkaista, jossa on linkki korvaavan sisällön. Käytä reaali uudelleenohjaus.

- **Julkaisutiedot**: paitsi, jos se on SDK-artikkelin tai StorSimple-artikkelissa laitteiston päivitystä, Tämäntyyppistä tietoa vain olisi upotettu tekniset oleellisia tai palvelun päivitykset kanavan sisältyy.

- **Uudet ominaisuudet release tai palvelun**: luetteloiden tai kuvaukset mitä uusia ominaisuuksia versiossa tai palvelun Siirry Palvelupäivityksistä kanava.
