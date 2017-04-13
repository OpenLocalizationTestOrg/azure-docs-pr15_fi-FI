<properties
    pageTitle="Päivityksen Dirsync-työkalun ja Azure AD-synkronointi | Microsoft Azure"
    description="Tässä artikkelissa käsitellään päivittää Azure AD Connect Dirsync-työkalun ja Azure AD-synkronointi."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>


# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>Päivitä Windows Azure Active Directory-synkronoinnin ("DirSync") ja Azure Active Directory-synkronoinnin ("Azure AD-synkronointi")
Azure AD Connect on paras tapa yhdistää paikallisesta hakemistosta Azure AD- ja Office 365: ssä. Tämä on hyvä päivittäminen Windows Azure Active Directory-synkronointi (DirSync) tai Azure AD-synkronointi Azure AD Connect työkaluja nyt poistettu ja saavuttaa 13 huhtikuun 2017 tuen loppuun.

Kaksi tunnistetietojen synkronointi-työkaluja, jotka on poistettu oli tarjoaa yhden metsää asiakkaiden (DirSync) ja usean metsää ja muiden advanced asiakkaat (Azure AD-synkronointi). Yksittäisen ratkaisu, joka on käytettävissä kaikissa tilanteissa on korvattu vanhoja työkaluja: Azure AD Connect. Se tarjoaa uusia toimintoja, toiminnon parannukset ja uudet skenaariot tuki. Siirry Azure AD-ympäristöön tunnistetietojen tiedot on synkronoitava voivat ja Office 365: ssä, on suositeltavaa, että päivität Azure AD Connect.

Heinäkuussa 2014 julkaistiin viimeisen DirSync-versiossa ja Azure AD-synkronointi viimeisin versio julkaistiin toukokuu 2015.

## <a name="what-is-azure-ad-connect"></a>Mikä on Azure AD Connect
Azure AD Connect on seuraaja Dirsync-työkalun ja Azure AD-synkronointi. Seuraavat kaksi tuettu yhdistää kaikissa tilanteissa. Voit lukea lisää tietoja sen [integroimalla paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Heikentämisestä aikataulu

Päivämäärä | Kommentti
 --- | ---
2016: n huhtikuun 13 | Windows Azure Active Directory-synkronointi ("DirSync") ja Microsoft Azure Active Directory-synkronointi ("Azure AD-synkronointi") ilmoitettiin kuin poistettu.
13 huhtikuussa 2017 | Tuki päättyy. Asiakkaiden ei enää voi avata tukipyynnön päivittämättä Azure AD Connect ensin.

## <a name="how-to-transition-to-azure-ad-connect"></a>Siirtymisestä Azure AD Connect
Jos käytössäsi on DirSync, voit päivittää kahdella tavalla: käytönaikainen päivitys- ja rinnakkaisvaiheista käyttöönotto. Paikallaan tapahtuva päivitys on suositeltavaa Useimmat asiakkaat ja että sinulla viimeisimmät käyttöjärjestelmän ja pienempi kuin 50 000 objekteja. Muissa tapauksissa kannattaa tehdä rinnakkaisia käyttöönoton kohtaa, johon DirSync-määritys on siirretty uuteen Azure AD Connect-palvelimeen.

Jos käytät Azure AD-synkronointi paikallaan tapahtuva päivitys-versiota. Jos haluat, on mahdollista asentaa uuden Azure AD Connect palvelimen rinnakkain ja käyttää kääntymiskulma siirtoa Azure AD-synkronointi-palvelimesta Azure AD Connect.

Ratkaisu | Skenaario
----- | -----
[Dirsync-työkalun päivittäminen](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Jos sinulla on aiemmin DirSync-palvelin, jo käynnissä.</li>
[Azure AD-synkronointi päivittäminen](active-directory-aadconnect-upgrade-previous-version.md)| <li>Jos siirrät Azure AD-synkronointi.</li>

Jos haluat nähdä suorittamiseen paikallaan tapahtuva päivitys DirSync Azure AD Connect, sitten, katso kanavan 9 Tässä videossa:

> [AZURE.VIDEO azure-active-directory-connect-in-place-upgrade-from-legacy-tools]

## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
**Kysymys: saan poistaa Azure-ryhmän ja/tai Office 365-viestikeskus osoitteesta sähköposti-ilmoituksen, mutta käytän Yhdistä.**  
Azure AD Connect käyttäminen muodosta luvun 1.0 asiakkaille myös lähettää ilmoituksen. \*.0 (joko pre 1.1-julkaisu). Microsoft suosittelee asiakkaita pysyä ajan tasalla Azure AD Connect versioista. [Automaattinen päivitys](active-directory-aadconnect-feature-automatic-upgrade.md) -toiminto tekee 1.1 todella helppoa on aina uutta versiota Azure AD Connect asennettu.

**K: tulee DirSync/Azure AD-synkronointi Lopeta 13 huhtikuun 2017 parissa?**  
Ei. Kun nämä ei enää voi pitää yhteyttä Azure AD päivämäärä ilmoitettava myöhemmin. Osaat tiedot löytyvät artikkelista, kun niitä on saatavilla.

**K: mitä DirSync-versioissa voit päivittää?**  
Se on tuettu mitä tahansa käytössä DirSync-version päivittäminen.

**K: Entä FIM/MIM Azure AD-Connector?**  
Azure AD-Connector for FIM/MIM ei **ole** ollut ilmoitettiin kuin poistettu. Se on **ominaisuus kiinnittäminen**; lisätään uusi-toiminto ei toimi ja se saa ei virheenkorjauksia. Microsoft suosittelee asiakkaat, jotka käyttävät sen Siirry Azure AD Connect suunnitteleminen. On erittäin suositeltavaa ei käynnisty kaikki uudet ominaisuuksissa sitä käytännössä. Tämä yhdistin ilmoitettava poistettu tulevaisuudessa.

## <a name="additional-resources"></a>Lisäresursseja

* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)
