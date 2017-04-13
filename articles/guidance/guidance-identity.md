<properties
   pageTitle="Azure käyttäjätietojen hallinta | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan ja vertaa eri tavat käytettävissä hallintaan tunnistetietojen hybrid järjestelmät, jotka ulottuvat kanssa Azure--paikallisen/cloud reunaa."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="telmosampaio"/>
   
# <a name="managing-identity-in-azure"></a>Azure jäsenyyden hallinta

Useimmat yrityksen järjestelmät Windows perusteella voit käyttää Active Directory (AD) sovellustesi hallintapalvelut Anna tunnistetiedot. AD toimii myös paikallisen ympäristön, mutta kun laajennat verkkoinfrastruktuuria pilveen sinulla on joitakin tärkeitä päätösten tekemiseen koskevat tunnistetietojen hallinta. Paikallisen toimialueen toteuttamaan VMs pilveen, laajenna? Voit Luo uusi toimialueiden pilveen ja jos näin on, miten? Olisi otat oman metsää pilveen tai voit kannattaa käyttää ja Azure Active Directory (AAD)?

Tässä artikkelissa on joitakin yleisten asetusten kokouksen tässä skenaariossa aiheuttaman haasteita ja auttaa määrittämään, mikä ratkaisu parhaiten vastaa tarpeitasi, tarpeen mukaan.

## <a name="using-azure-active-directory"></a>Azure Active Directoryn avulla

AAD avulla voit luoda AD-toimialueen pilveen ja linkittää sen paikallisen AD-toimialue. AAD avulla voit määrittää kertakirjautuminen (SSO) pilven kautta sovellukset-käyttäjille.

[! [0]][0]

AAD on selkeä tapa toteuttaa suojauksen toimialueen pilveen. Se on käyttämä monta Microsoft-sovellusten, kuten Microsoft Office 365: ssä. 

AAD eduista:

- Ei tarvitse säilyttää Active Directory-infrastruktuurin pilveen. AAD on täysin hallitaan ja Microsoftin.

- AAD on sama käyttäjätietoja, joka on saatavilla paikalliseen.

- Todennus voi esiintyä Azure-pienentämisestä ulkoisiin sovelluksiin ja käyttäjien yhteystiedot paikallisen toimialueen edellyttää.

Seikkoja huomioitavaksi AAD käytettäessä:

- Identity-palvelut on rajoitettu käyttäjät ja ryhmät. Ei voi todentaa palvelu ja tietokoneen tilit.

- Sinun on määritettävä yhteys pitämään AAD kansio synkronoidaan paikallisen toimialueen kanssa. 

- Olet vastuussa julkaisemisen sovelluksia, jotka käyttäjät voivat käyttää pilveen AAD kautta.

Lisätietoja, lue [Käyttöönoton Azure Active Directory][implementing-aad].

## <a name="using-active-directory-in-the-cloud-joined-to-an-on-premises-forest"></a>Active Directoryn avulla pilveen liitetty paikallisen metsää

Voit ylläpitää paikalliseen Active Directory-palveluista (AD DS), mutta jos sovelluksen osat sijaitsevat Azure hybrid tilanne voi olla tehokkaampaa replikointia toiminto ja AD-tietovarasto pilveen. Tämän menetelmän voit vähentää viive aiheuttaa lähettämällä todennus ja AD DS: ÄÄN käynnissä paikallisen takaisin paikalliseen pyytää pilvestä. 

[! [1]][1]

Tämän menetelmän edellyttää luominen pilvipalvelussa oma toimialue ja liittää paikallisen metsää. Voit luoda VMs isännöimiseen AD DS-palvelut.

Erillinen toimialue käytön pilveen etuja:

- Mahdollistaa todentaa käyttäjän-palvelua ja tietokoneen tilit paikallisen ja pilveen.

- Käyttää samaa käyttäjätietoja, joka on saatavilla paikalliseen avulla.

- Ei tarvitse erillistä AD-metsää; hallinta toimialueen pilveen voivat kuulua paikallisen metsää.

- Voit käyttää Ryhmäkäytäntö määrittämiä paikallisen toimialueen pilveen Ryhmäkäytäntöobjekti objektit.

Huomioitavaa erillinen toimialue pilvipalvelussa:

- Edellyttää, voit luoda ja hallita omia AD DS-palvelimia ja toimialueen pilveen.

- Voi olla joitakin synkronoinnin viive toimialueen palvelinten pilveen ja paikallisen-palvelimiin.

Lisätietoja tämän arkkitehtuuri määrittämisestä on artikkelissa [Laajentaminen Active Directory Directory Services (Lisää) ja Azure][extending-adds].

## <a name="using-active-directory-with-a-separate-forest"></a>Active Directory käyttäminen erillisessä metsää

Organisaation, joka suoritetaan, Active Directory (AD) paikallisen on ehkä metsää, joka sisältää useita eri toimialueiden. Toimialueiden avulla voit antaa toiminta-alueet, jotka on säilytettävä erillisessä mahdollisesti tietoturvasyistä välinen eristystaso, mutta voit jakaa tietoja luomalla luottamussuhteet toimialueiden välillä.

[! [2]][2]

Organisaatio, joka käyttää toimialueille voit hyödyntää Azure kunnolliselle vähintään yksi näistä toimialueen siirtäminen eri metsää pilveen. Voit myös organisaation haluta säilyttää kaikki cloud resurssit loogisesti poikkeavat näiden vastuuseen paikallisen ja Tallenna pilveen resurssien tietoja oman kansion metsää, pidetään myös pilveen osana.

Erillinen metsää käytön pilveen etuja:

- Voit toteuttaa paikallisen käyttäjätietoja ja erota vain Azure-käyttäjätietoja.

- Ei tarvitse kopioida tiloista AD metsää, Azure, vähentää verkon latenssin tehosteita.

Huomioon otettavia seikkoja:

- Paikallisen käyttäjätietojen pilveen todennus suorittaa paikallisen ylimääräisiä verkon *siirräntävälien* AD-palvelimiin.

- Edellyttää toteuttaa oman AD DS-palvelimia ja metsää pilveen ja sopiva Luottamussuhteiden puuryhmien.

Asiakirjan [luominen Active Directory Directory Services (Lisää)-resurssin metsää Azure-tietokannassa] [ adds-forest-in-azure] kerrotaan, miten voit toteuttaa tämän menetelmän tarkemmin.

## <a name="using-active-directory-federation-services-adfs-with-azure"></a>Azure kanssa käyttämällä Active Directory Federation Services (ADFS)

ADFS suorittamisen paikallisen, mutta jos sovellukset sijaitsevat Azure hybrid tilanne voi olla tehokkaampaa toteuttaa tämä toiminnallisuus pilvipalvelussa, alla kuvatulla tavalla.

[! [3]][3]

Tämä arkkitehtuuri on hyötyä erityisesti:

- Ratkaisuja, jotka käyttävät liitetyt lupa, voit näyttää web-sovellusten kumppanin organisaatioille.

- Järjestelmät, jotka tukevat selaimet käynnissä organisaation palomuurin ulkopuolella käytöltä.

- Järjestelmät, joiden avulla käyttäjät voivat käyttää web-sovellusten yhdistämällä valtuutettujen ulkoisen laitteilla, kuten etätietokoneiden, muistikirjat ja muille mobiililaitteille. 

ADFS: N käyttäminen Azure edut:

- Voit hyödyntää saatavat tukeva sovellukset.

- Se tarjoaa mahdollisuuden luotetaanko ulkopuolisten kumppaneiden todennusta varten.

- Se on suuri määrittäminen käyttöoikeuden yhteensopivuus.

ADFS: N käyttöä Azure huomioon otettavia seikkoja:

- Se vaatii toteuttavien oman Lisää ADFS ja ADFS: N Web-sovelluksen välityspalvelimen palvelinten pilveen.

- Tämä arkkitehtuuri voi olla monimutkaisia määrittämiseen.

Lisätietoja, lue [Toteuttaminen Active Directory Federation Services (ADFS) Azure-tietokannassa][adfs-in-azure].

## <a name="next-steps"></a>Seuraavat vaiheet

Resurssit alla kerrotaan tässä artikkelissa kuvattuja arkkitehtuureihin ottamisesta käyttöön.

- [Azure Active Directory toteuttaminen][implementing-aad]
- [Ulottuu Azure Active Directory-palveluihin (Lisää)][extending-adds]
- [Azure Active Directory Directory Services (Lisää)-resurssin metsää luominen][adds-forest-in-azure]
- [Azure Active Directory-liittoutumispalvelut (ADFS) toteuttaminen][adfs-in-azure]

<!-- Links -->
[0]: ./media/guidance-identity/figure1.png "Cloud tunnistetietojen arkkitehtuuri Azure Active Directoryn avulla"
[1]: ./media/guidance-identity/figure2.png "Suojattu hybrid verkoston arkkitehtuuri Active Directory-hakemistosta"
[2]: ./media/guidance-identity/figure3.png "Suojattu hybrid verkoston arkkitehtuuri ja erillinen AD-toimialueet ja metsien"
[3]: ./media/guidance-identity/figure4.png "Suojattu hybrid verkoston arkkitehtuuri ADFS: N kanssa"
[implementing-aad]: ./guidance-identity-aad.md
[extending-adds]: ./guidance-identity-adds-extend-domain.md
[adds-forest-in-azure]: ./guidance-identity-adds-resource-forest.md
[adfs-in-azure]: ./guidance-identity-adfs.md