<properties
    pageTitle="Toimimasta Azure AD-sovelluksen välityspalvelimen yhdistimillä | Microsoft Azure"
    description="Kerrotaan, miten voit luoda ja hallita ryhmiä Azure AD-sovelluksen välityspalvelimen yhdistimiä."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Valinnaiseksi erillisten verkkojen ja sijainnit yhdistimen ryhmien käyttäminen

> [AZURE.SELECTOR]
- [Azure portal](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure perinteinen portal](active-directory-application-proxy-connectors.md)


Yhdistimen ryhmät on hyötyä eri tilanteessa, mukaan lukien.

- Useita toisiinsa liittyviä palvelinkeskusten sivustoja. Tässä tapauksessa haluat säilyttää mahdollisimman paljon liikenne sisällä sen palvelinkeskuksen mahdollisimman, koska rajat palvelinkeskuksen linkit ovat yleensä kallista ja hidasta. Yhdistimien kunkin tukemaan vain sovellukset, jotka sijaitsevat palvelinkeskukseen joten voit ottaa käyttöön. Tämän menetelmän rajat palvelinkeskuksen linkit pienennetään, ja ne sisältävät täysin läpinäkyvä käyttäjille.
- Eristettyjen verkkoja, jotka eivät kuulu tärkeimmät yritysverkon asennettuihin sovellusten hallinta Yhdistimen ryhmien avulla voit erillinen yhdistimet asentaminen erillään verkkojen eristää myös sovellusten verkkoon.
- Sovellusten asennettuihin IaaS pilvikäyttöä varten yhdistimen ryhmät voit suojata kaikki sovellukset käytön yleiset palvelun tarjoamista. Yhdistimen ryhmät ei luoda Lisää riippuvuus yrityksen verkossa tai jakaa app-versio. Yhdistimien voidaan asentaa jokaisen cloud palvelinkeskuksen ja vain sovellukset, jotka sijaitsevat verkoston yhteyshenkilönä. Voit asentaa useita yhdistimiä suuren käytettävyyden saavuttamiseksi.
- Usean metsää ympäristössä, jossa tietyn yhdistimet voit kohti metsää käyttöön ja määritetty tukemaan sovelluksia tuki.
- Yhdistimen ryhmiä voi käyttää palauttaminen-sivustoissa olevien joko automaattisesti tai varmuuskopiointi pääsivusto.
- Yhdistimen ryhmiä voidaan myös tukemaan useita yrityksiä yhden vuokraajasta.

## <a name="prerequisite-create-your-connectors"></a>Edellytyksenä: Luo yhdistimet
Jos haluat ryhmitellä yhdistimet, on Varmista että on [asennettuna useita yhdistimiä](active-directory-application-proxy-enable.md)ja olet nimi ne ja ryhmittele ne. Lopuksi sinun tarvitse määrittää niitä tiettyjen sovellukset.

## <a name="step-1-create-connector-groups"></a>Vaihe 1: Yhdistimen ryhmien luominen
Voit luoda niin monta yhdistimen ryhmiä kuin haluat. Yhdistimen ryhmän luomisen tehdään Azure perinteinen-portaalissa.

1. Valitse hakemisto ja valitse **Määritä**.  
    ![Sovelluksen välityspalvelinta, Määritä näyttökuva – valitse yhdistin-ryhmien hallinta](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)

2. Valitse sovelluksen välityspalvelimen **Connector** -ryhmien hallinta ja yhdistimen uuden ryhmän luominen antamalla ryhmän nimi.  
    ![Sovelluksen välityspalvelimen yhdistimen ryhmät näyttökuva - uuden ryhmän nimi](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Vaihe 2: Määritä ryhmien yhdistimet
Yhdistimen ryhmät luodaan, kun Siirrä yhdistimet haluamasi ryhmä.

1. Kohdassa **Sovelluksen välityspalvelimen** **Hallinta yhdistimiä**.
2. Valitse **ryhmän**jokaisen yhdistimen haluamasi ryhmä. Saattaa kestää yhdistimet enintään 10 minuutin voimaantulo uuteen ryhmään.  
    ![Näyttökuva sovelluksen välityspalvelimen yhdysviivat - käyttäjäryhmän avattavasta valikosta](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Vaihe 3: Määritä sovellusten connector-ryhmiin
Viimeinen vaihe on kunkin sovelluksen connector-ryhmään, jota käytetään sen määrittämiseen.

1. Azure perinteinen portaalissa hakemistossa Valitse Määritä ryhmään ja valitse **Määritä**sovellus.
2. Valitse **yhdistin ryhmän**sovellus käyttää haluamasi ryhmä. Tämä muutokset tulevat voimaan heti.  
    ![Sovelluksen välityspalvelimen yhdistimen ryhmän näyttökuva - käyttäjäryhmän avattavasta valikosta](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)


## <a name="see-also"></a>Katso myös

- [Ottaa käyttöön sovelluksen välityspalvelin](active-directory-application-proxy-enable.md)
- [Yksi merkki ottaminen käyttöön](active-directory-application-proxy-sso-using-kcd.md)
- [Ehdollinen käytön käyttöön ottaminen](active-directory-application-proxy-conditional-access.md)
- [Ilmenevien ongelmien sovelluksen välityspalvelimen ongelmien vianmääritys](active-directory-application-proxy-troubleshoot.md)

Uusimmat uutiset ja päivitykset Tutustu [sovelluksen välityspalvelin-blogi](http://blogs.technet.com/b/applicationproxyblog/)
