<properties
   pageTitle="Azure Tietoturvakeskus tietojen suojauksen | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan, miten tietojen hallinnasta ja monivuotisilla Azure Tietoturvakeskuksessa."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-data-security"></a>Azure Tietoturvakeskus tietojen suojaaminen
Auttaa asiakkaita estää, haku- ja vastata uhkien, Azure Tietoturvakeskus kerää ja käsittelee tietoja Azure resursseja, mukaan lukien kokoonpanotietoja, metatietojen tapahtumalokit, kaatua vedostiedostot ja paljon muuta. Olemme Varmista vahva sitoumukset yksityisyyden ja tietosuojan näiden tietojen suojaaminen. Microsoft noudattaa tarkka yhteensopivuus ja suojauksen ohjeita –-koodaus, toimivat palvelu. 

Tässä artikkelissa kerrotaan, miten hallitaan ja Azure Tietoturvakeskuksessa monivuotisilla.

## <a name="data-sources"></a>Tietolähteet

Azure Tietoturvakeskus analysoi tietoja seuraavista lähteistä:

- Azure Services: Lukee kokoonpanotietoja Azure palvelut on otettu käyttöön toimittamalla kyseisen palvelun resurssi-palvelussa.
- Verkkoliikenteen: Lukee näyte verkon liikenne metatiedot Microsoftin infrastruktuuri, kuten lähde ja kohde-IP/portti-pakettikoko ja verkko-protokollaa.
- Kumppaniratkaisuihin: Kerää suojausvaroitusten integroitu kumppaniratkaisuihin, kuten palomuurien ja antimalware ratkaisuja. Tiedot tallennetaan Azure Tietoturvakeskus tallennus-ennenkin Yhdysvalloissa.
- Näennäiskoneiden: Azure Tietoturvakeskuksessa voit kerätä määritysten tiedot ja tietoja suojauksen tapahtumia, kuten Windowsin tapahtumalokiin ja valvonta lokit, IIS-lokit, syslog viestit ja kaatumisen vedostiedostot-että näennäiskoneiden, tietojen kerääminen tekijöiden avulla. On lisätietoja alla "Hallinta tietojen kerääminen"-osassa.  

Lisäksi tietoja suojausvaroitusten, suositukset ja suojauksen kunto tilan tallennetaan Azure Tietoturvakeskus tallennus-ennenkin Yhdysvalloissa. Nämä tiedot voivat olla liittyvät kokoonpanotietoja ja suojauksen tapahtumien koottu oman näennäiskoneiden tarvittaessa tarjota suojausvaroituksessa, suositus tai suojauksen kunto tilaa.

## <a name="data-protection"></a>Tietojen suojaaminen

**Tietoja eriytymistä**: tiedot säilytetään loogisesti erillisen palvelun koko kullekin osalle. Kaikki tiedot on merkitty organisaation kohden. Tämä tunnisteita jatkuu koko tietojen elinkaari ja kunkin tasolla palvelun pakotettu. Lisäksi että näennäiskoneiden kerättyjen tietojen tallennetaan tallennustilan tilit.

**Tietojen käyttö**: Anna suojausta koskevia suosituksia ja tutkia mahdollisia tietoturvauhkia Microsoftin henkilökunta voi käyttää tietojen kerääminen tai analysoida Azure-palveluissa, mukaan lukien kaatumisen vedostiedostot. Kaatumisen vedostiedostot ja tapahtumien luominen vahingossa voivat sisältää asiakastietoja tai oman näennäiskoneiden henkilökohtaisia tietoja. Olemme noudattaa [Microsoft Online Services-ehdot](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) ja [Tietosuojatiedot](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), minkä osavaltion, että Microsoft ei käytä asiakastietoja tai mainontaa tai samanlaisia kaupallisiin tarkoituksiin tietoja siitä. Vain Käytämme asiakastietoja tarvittaessa tarjota Azure palveluja, kuten tarkoituksiin yhteensopiva palveluja tarjoavat. Voit säilyttää oikeudet kaikkiin asiakastiedot.

**Tietojen käyttäminen**: Microsoft käyttää kuvioiden ja uhkien liiketoimintatietojen nähdä yli useita alihallinnat, jotka voit parantaa Microsoftin ehkäisemistä ja havaitsemista valmiudet, Olemme kiellä Microsoftin [Tietosuojatiedot](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx)tietosuojasitoumukset mukaisesti.

**Tietojen sijainti**: tallennustilan-tili on määritetty kunkin alueen, jossa näennäiskoneiden on käynnissä. Voit tallentaa tiedot, joista tiedot kerätään virtuaalikoneen kanssa samalla alueella. Tämä tiedot, myös kaatumisen vedostiedostot tallennetaan pysyvästi tallennustilan tilissäsi. Palvelun tallentaa tiedostoon myös suojausvaroitusten, mukaan lukien Azure Tietoturvakeskus tallennus-ennenkin Yhdysvalloissa ilmoitukset integroitu kumppaniratkaisuihin ja suositukset suojauksen kunto tilan tietoja.

## <a name="managing-data-collection-from-virtual-machines"></a>Tietojen kerääminen hallitseminen näennäiskoneiden

Kun valitset Azure Tietoturvakeskus käyttöön, tietojen kerääminen on otettu käyttöön kunkin tilauksistasi. Voit poistaa käytöstä tietojen kerääminen Azure Security Center Raporttinäkymät-ikkunan "Suojauskäytäntö"-osassa. Tietojen kerääminen on otettu käyttöön, kun Azure Tietoturvakeskus säännösten Azure seuranta agentti kaikki olemassa olevia tueta näennäiskoneiden- ja minkä tahansa uusia, jotka on luotu. Azure-suojauksen valvonta-tunniste etsii eri suojauksen liittyviä määrityksiä ja tapahtumia sen [Tapahtuman jäljitys for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (tapahtumien seuranta) jäljittää. Käyttöjärjestelmä lisäksi korottaa tapahtumaloki tapahtumien aikana käynnissä tietokoneessa. Nämä tiedot ovat: käyttöjärjestelmän tyyppi ja versio, toimivat järjestelmälokit (Windowsin tapahtumalokien,) käynnissä prosesseja, tietokoneen nimi ja IP-osoitteet, kirjautunut käyttäjä ja vuokraajan ID-tunnuksellasi. Azure seuranta-agentti lukee Tapahtumalokimerkinnät ja tapahtumien seuranta jäljittää ja kopioi ne analysis tallennustila-tiliisi. 

Kunkin alueen, jossa on käynnissä, jossa tietojen kerääminen-näennäiskoneiden siten, että sama alue on tallennettu näennäiskoneiden on määritetty tallennustilan tilin. Tämä on helppo voit säilyttää tiedot saman maantieteellisen alueen tietosuoja ja tietojen paikallisen tietosuojan varten. Voit määrittää tallennustilan tilit kunkin alueen Azure Security Center Raporttinäkymät-ikkunan "Suojauskäytäntö"-osassa.

Azure seuranta-agentti kopioi myös kaatumisen vedostiedostot tallennustilan-tiliisi.  Azure Tietoturvakeskus kaatumisen vedostiedostot tilapäiset kopiot kerää ja analysoi niitä hyödyntää yritykset ja onnistuneen kompromisseja.  Azure Tietoturvakeskus suorittaa tämän analysis tallennustila-tiliä saman maantieteellisen alueen sisällä, ja poistaa tilapäiset kopiot, kun analyysi on valmis.

Voit poistaa käytöstä tietojen kerääminen näennäiskoneiden kohteesta milloin tahansa, joka poistaa Azure Tietoturvakeskus aiemmin asentama mitään seuranta tekijöiden.


## <a name="see-also"></a>Katso myös

Tässä asiakirjassa opit, kuinka hallitaan ja Azure Tietoturvakeskuksessa monivuotisilla. Lisätietoja Azure Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Security Center suunnittelu- ja käyttöoppaan](security-center-planning-and-operations-guide.md) – tietoja suunnitteleminen ja ymmärtää tyyliseikat antaa Azure Tietoturvakeskuksessa.
- [Suojauksen kunnon seuranta Azure Tietoturvakeskus](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) – Katso, miten voit hallita ja vastata suojausvaroitusten
- [Kumppaniratkaisuihin ja Azure Tietoturvakeskus seuranta](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla
- [Azure Security-blogi](http://blogs.msdn.com/b/azuresecurity/) – Etsi tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen blogit
