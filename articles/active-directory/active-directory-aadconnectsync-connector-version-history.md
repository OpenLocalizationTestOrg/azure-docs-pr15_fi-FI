<properties
   pageTitle="Yhdistimen versiohistoria Release | Microsoft Azure"
   description="Tässä artikkelissa on lueteltu kaikki versiot yhdistimet Forefront käyttäjätietojen hallinta (FIM) ja Microsoft käyttäjätietojen hallinta (MIM)"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="billmath"/>

# <a name="connector-version-release-history"></a>Yhdistimen Release versiohistoria
Yhdistimien Forefront käyttäjätietojen hallinta (FIM) ja Microsoft käyttäjätietojen hallinta (MIM) päivitetään säännöllisesti.

>[AZURE.NOTE]
Tässä aiheessa on vain FIM ja MIM. Nämä yhdistimet ei tue Azure AD Connect.

Tässä ohjeaiheessa Luettele kaikki versiot yhdistimiä, jotka on julkaistu.

Aiheeseen liittyvät linkit:

- [Lataa uusimmat yhdistimet](http://go.microsoft.com/fwlink/?LinkId=717495)
- Oppaat [Yleisiä LDAP-yhdistin](active-directory-aadconnectsync-connector-genericldap.md)
- [Yleinen SQL-yhdistin](active-directory-aadconnectsync-connector-genericsql.md) oppaat
- [Web Services-yhdistin](http://go.microsoft.com/fwlink/?LinkID=226245) oppaat
- [PowerShell-yhdistin](active-directory-aadconnectsync-connector-powershell.md) oppaat
- [Lotus Domino yhdistimen](active-directory-aadconnectsync-connector-domino.md) oppaat

## <a name="111170"></a>1.1.117.0
Julkaistu: 2016 maaliskuussa

**Uusi yhdistin**  
[Yleinen SQL-yhdistin](active-directory-aadconnectsync-connector-genericsql.md)alkuperäisessä versiossa.

**Uudet ominaisuudet:**

- Yleisiä LDAP-yhdistin:
    - Tukee nyt delta-tuonti Isode.
- Web Services-yhdistin
    - Päivittää csEntryChangeResult tehtävään ja setImportErrorCode tehtävän sallimaan objektin tason virheitä palautetaan takaisin synkronointi-ohjelma.
    - Päivitetty uusien objektin tason virhe-toimintoa voi käyttää SAP6 ja SAP6User-mallit.
- Lotus Domino yhdistin:
    - Vie sinun on yksi certifier kohti osoitteisto. Voit nyt kaikki certifiers saman salasanan helpottaa hallintaa.

**Kiinteä ongelmat:**

- Yleisiä LDAP-yhdistin:
    - Saat IBM Tivoli DS viittaus joitakin määritteitä ei havaittu oikein.
    - Avaa LDAP delta tuonnin aikana välilyönnit alussa ja lopussa merkkijonot on katkaistu.
    - Novell ja NetIQ Vie, joka siirtää objektin organisaatioyksiköiden tai säilöjä ja samanaikaisesti nimetä uudelleen objektiin epäonnistui.
- Web Services-yhdistin
    - Jos web-palvelu käyttää useita merkitsemisperusteet saman sidontaa varten, valitse yhdistin ei oikein Tutustu nämä merkitsemisperusteet.
- Lotus Domino yhdistin:
    - Posti-tietokannan nimi-määritteen vienti ei ollut.
    - Vie joka sekä lisätä ja poistaa ryhmän jäsenen viedä vain lisätyt jäsenet.
    - Jos muistiinpanot tiedosto on virheellinen (määritteen isValid arvo on EPÄTOSI) ja valitse yhdistin epäonnistuu.

## <a name="older-releases"></a>Vanhemmat versiot
Maaliskuussa 2016 ennen yhdistimet julkaistu Tukiartikkelit.

**Yleisiä LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, 2015 syyskuussa
- [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, maaliskuu 2015
- [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, tammikuussa 2015
- [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, syyskuussa 2014
- [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, maaliskuussa 2014

**Verkkopalvelujen**

- [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, syyskuussa 2014

**PowerShellin**

- [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, syyskuussa 2014

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, 2015 syyskuussa
- [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, maaliskuu 2015
- [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, elokuussa 2014
- [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, helmikuussa 2014  
- [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, lokakuussa 2013
- [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, 2013 elokuussa

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [Azure AD Connect synkronointi](active-directory-aadconnectsync-whatis.md) -määritys.

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
