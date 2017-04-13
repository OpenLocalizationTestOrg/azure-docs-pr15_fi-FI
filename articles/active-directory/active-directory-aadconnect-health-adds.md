
<properties
    pageTitle="Kuntotietojen käyttämällä Azure AD yhteydessä AD DS | Microsoft Azure"
    description="Tämä on Azure AD yhteyden kunto-sivulla, jotka käsittelevät seurannassa AD DS: ÄÄN."
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="arluca"/>

# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Kuntotietojen käyttämällä Azure AD yhteydessä AD DS: ÄÄN
Seuraavat asiakirjat on Active Directory-toimialueen palveluiden kanssa Azure AD yhteyden kunnon valvonta. AD DS: N tuetut versiot eivät: Windows Server 2008 R2, Windows Server 2012: ssa ja Windows Server 2012 R2.

Lisätietoja AD FS kanssa Azure AD yhteyden kunnon seuranta-kohdassa [Käyttämällä Azure AD yhteyden kunto AD FS](active-directory-aadconnect-health-adfs.md). Lisäksi Azure AD Connect (synkronointi) kanssa Azure AD yhteyden kunnon valvonta lisätietoja [Käyttämällä Azure AD yhteyden kunto-synkronointia varten](active-directory-aadconnect-health-sync.md).

![Azure AD Connect kunto AD DS: ÄÄN varten](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>Azure AD ilmoitusten muodostetaan kunto AD DS: ÄÄN
Azure AD yhteyden kunto, AD DS kuluessa ilmoitusten-osassa on aktiivinen ja ratkaista ilmoitusten toimialueen ohjaimet liittyvä luettelo. Aktiivinen tai ratkaistu ilmoituksen valitsemalla Avaa uusi sivu, jossa lisätietoja sekä toimintaohjeiden, ja linkittää tarvittavat asiakirjat. Kunkin laji voi olla vähintään yksi esiintymät, jotka vastaavat kunkin toimialueen ohjaimet, vaikuttaa tietyn ilmoituksen. Ilmoitusten sivu alareunassa voit kaksoisnapsauttaa tarvittavien ohjauskoneen, voit avata Lisää sivu, jossa lisätietoja ilmoitusten esiintymän.

Tämä sivu kuluessa ilmoitusten sähköposti-ilmoitusten ottaminen käyttöön ja olevan aikavälin muuttaminen näkymässä. Aikavälin laajentaminen voit nähdä edellisen ratkaistu ilmoitukset.

![Azure AD Connect synkronointivirhe](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>Toimialueen ohjaimet Raporttinäkymät-ikkunan
Tämä koontinäyttö on ympäristössäsi sekä avaimen arvoja ja terveyteen tila kaikkien valvottu toimialueen ohjaimet topologista näkymän. Tunnistaa nopeasti, minkä tahansa toimialueen ohjaimet, jotka saattavat tarvita tutkimuksen avulla esitetty arvot. Sarakkeiden alijoukko näkyy oletusarvoisesti. Kuitenkin löydät kaksoisnapsauttamalla sarakkeet-komento käytettävissä olevat sarakkeet koko sarja. Sarakkeet, jotka eniten tärkeisiin, ottaa tämän koontinäytön yksittäinen ja helposti Aseta Näytä AD DS-ympäristösi kunto valitseminen

![Toimialueen ohjaimet](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Toimialueen ohjaimet voidaan ryhmitellä vastaaviin toimialueen tai -sivusto, joka auttaa ymmärtämään ympäristön topologian perusteella. Jos kaksoisnapsautat sivu otsikon, koontinäyttö suurentaa lopuksi voit hyödyntää käytettävissä näyttö-kiinteistöjen. Tätä näkymää on hyötyä, kun useita sarakkeet ovat näkyvissä.

## <a name="replication-status-dashboard"></a>Replikoinnin tila Raporttinäkymät-ikkunan
Tämä koontinäyttö on näkymän replikoinnin tilan ja replikoinnin topologian valvottu toimialueen ohjaimet. Viimeisin replikoinnin yritys tila näkyy sekä virhe, joka löytyy kannattaa käyttää esimerkiksi seuraavissa ohjeissa. Voit kaksoisnapsauttaa virheen, voit avata uusi sivu on tietoja, kuten toimialueen ohjauskoneen: Virhe suositeltavia toimintaohjeiden sekä linkkejä vianmääritys dokumentaatio tietoja.

![Replikoinnin tila](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>Seuranta
Tämä toiminto tarjoaa graafinen trendejä suorituskyvyn eri laskureita, johon kerätään jatkuvasti kaikkien valvottu toimialue-ohjaimet. Toimialueen ohjauskoneen suorituskyky voidaan verrata helposti-että metsää kaikkien muiden valvottu toimialueen ohjainten välillä. Lisäksi voit tarkastella eri suorituskyvyn laskureita rinnakkain, joka on hyötyä, kun vianmääritys ympäristössäsi.

![Seuranta](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Oletusarvon mukaan olemme, on alustavasti valinnut vaihtoehdon neljä suorituskyvyn laskureita; Voit lisätä muiden napsauttamalla Suodata-komento ja valitseminen tai niiden valinnan poistaminen haluamasi suorituskyvyn mitään laskureita. Lisäksi voit kaksoisnapsauttaa suorituskyvyn laskuri kaavion Avaa uusi sivu, mukaan lukien arvopisteiden kunkin valvottu toimialue-ohjaimet.

## <a name="related-links"></a>Aiheeseen liittyvät linkit

* [Azure AD Connect kunto](active-directory-aadconnect-health.md)
* [Azure AD Connect kunto agentti asennus](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect kunto-toiminnot](active-directory-aadconnect-health-operations.md)
* [Käyttämällä Azure AD yhteyden kunto AD FS](active-directory-aadconnect-health-adfs.md)
* [Azure AD yhteyden kunto käytön synkronointi](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect kunto usein kysytyt kysymykset](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect kunto versiohistoria](active-directory-aadconnect-health-version-history.md)
