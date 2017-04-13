<properties
    pageTitle="Oletuskäytäntö- ja Mobiililaitteiden ryhmäasetusten | Microsoft Azure"
    description="Tietoja Ryhmäkäytäntö ja mobiililaite management (MDM)-asetukset, jota käytetään yrityksen omistamien laitteissa. Käytännöt käytetään käyttäjän koko laite."
    services="active-directory"
    keywords="Mitä ryhmän käytännön ja yrityksen tilan Roaming, yrityksen tilan Roaming, Windowsin cloud MDM asetuksia"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="group-policy-and-mdm-settings"></a>Ryhmäkäytäntö ja Mobiililaitteiden asetukset

Käytä näitä Ryhmäkäytäntö sekä mobiililaitteiden hallinnan (MDM) asetukset vain laitteissa yrityksen omistamien koska käytännöt otetaan käyttöön käyttäjän koko laite. Käyttämällä MDM-käytäntö oman synkronointi asetusten poistaminen käytöstä käyttäjän laitteen heikentävät laitteen käyttö. Lisäksi käyttäjätilejä laitteeseen vaikuttaa myös käytännön.

Yrityksille, jotka haluat hallita roaming Omat (ei-hallitun) laitteille käyttää Azure portaalin käyttöön ottaminen tai käytöstä roaming sijaan Ryhmäkäytäntö tai mobiililaitteiden avulla
Seuraavassa taulukossa kuvataan käytettävissä käytäntöasetukset.

## <a name="mdm-settings"></a>Mobiililaitteiden asetukset
MDM-käytäntöasetukset koskevat Windows 10: ssä ja Windows 10 Mobile.  Windows 10 Mobile tuki on vain Microsoft-tili mukaan roaming käyttäjän OneDrive-tilin kautta.  Lue lisätietoja mitkä laitteet tuetaan mukaan Azure AD-synkronointi "Laitteet ja päätepisteet"-osaan.

| Nimi                               | Kuvaus                                                          |
|------------------------------------|----------------------------------------------------------------------|
| Salli Microsoft-tili | Käyttäjä voi todentaa käyttämällä Microsoft-tiliä laitteeseen |
| Salli synkronoinnin asetukset             | Käyttäjät voivat vierailla Windows-asetukset ja sovelluksen tiedot. Käytäntö poistetaan käytöstä synkronointi sekä mobiililaitteiden varmuuskopioiden                  |

## <a name="group-policy-settings"></a>Ryhmäkäytäntöasetukset
Ryhmäkäytäntöasetukset käyttää Windows 10-laitteisiin, jotka ovat liittyneet Active Directory-toimialueen. Taulukko sisältää myös aiempien versioiden asetukset, joka näyttää synkronointiasetusten hallitseminen, mutta, jotka eivät toimi yrityksen tilan Roaming for Windows 10, joka on merkitty kanssa-Do not use-kuvaus.

| Nimi                                | Kuvaus |
|-------------------------------------|-------------|
| Tilit: Estä Microsoft-tilit  |Tämän asetuksen voit estää käyttäjiä lisätään uusi Microsoft-tili tässä tietokoneessa|
| Ei synkronoida                         |Voit estää käyttäjiä siirtymisen Windows-asetukset ja sovelluksen tiedot|
| Ei synkronoida mukauttaminen             |Poistaa käytöstä synkronoinnin teemat-ryhmä|
| Synkronoi selainasetukset        |Poistaa käytöstä synkronoinnin Internet Explorer-ryhmä|
| Salasanojen ei synkronoida               |Poistaa käytöstä synkronoinnin salasanat-ryhmä|
| Synkronointi Windowsin muut asetukset  |Poistaa käytöstä synkronoinnin muut Windows-asetukset-ryhmä|
| Synkronoi työpöydän mukauttaminen |Älä käytä; ei vaikuta|
| Käytön mukaan laskutettavan yhteyden ei synkronoida  |Poistaa käytöstä, valitse roaming käytön mukaan laskutettavat yhteydet, kuten matkapuhelinverkon 3 G|
| Sovellusten ei synkronoida                    |Älä käytä; ei vaikuta|
|Synkronoi app-asetukset             |Poistaa käytöstä roaming sovelluksen tiedot|
|Käynnistä asetusten ei synkronoida           |Älä käytä; ei vaikuta|


## <a name="related-topics"></a>Aiheeseen liittyviä ohjeita
- [Yrityksen tilan Roaming yleiskatsaus](active-directory-windows-enterprise-state-roaming-overview.md)
- [Roaming Azure Active Directoryn enterprise-tila käyttöön](active-directory-windows-enterprise-state-roaming-enable.md)
- [Asetukset ja tiedot roaming usein kysytyt kysymykset](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Windows 10: ssä sijaitsevan asetukset viittaus](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
