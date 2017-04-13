<properties
    pageTitle="Azure AD-toimialueen palveluista: Verrata Azure AD-toimialueen palvelujen DIY toimialueen ohjaimet | Microsoft Azure"
    description="Vertailu Azure Active Directory-toimialueen palveluista DIY toimialueen ohjaimet"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>

# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Jos Azure AD-toimialueen palvelut artikkeliin sopii oman Käyttötapaus
Azure AD-toimialueen palveluiden avulla voit ottaa Azure-infrastruktuuripalvelut oman työmääriä eikä sinun tarvitse huolehtia ylläpito identity-infrastruktuuria. Hallitun palvelua poikkeaa tyypillinen Windows Server Active Directory-käyttöönoton, joka otetaan käyttöön ja hallita Omat muistiinpanot. Palvelu on suunniteltu käyttöönoton helpottaminen, automaattinen kunnon seuranta ja korjaus ja yksinkertainen identity-infrastruktuurin pilveen. Olemme ovat jatkuvasti kehittyville yleisiä käyttöönoton tilanteita, joissa tuki Lisää palvelu.

Kannattaako käyttää Azure AD-toimialueen palveluihin tai asettamasi ja hallita omia (kodin pieniin kunnostustöihin)-Azure AD-infrastruktuuria:

- Katso luettelo [ominaisuuksia tarjoamia Azure AD-toimialueen palveluista](active-directory-ds-features.md).

- Tarkista yleiset [käyttöönoton tilanteita, joissa Azure AD-toimialueen palveluista](active-directory-ds-scenarios.md).

- Lopuksi [verrata Azure AD-toimialueen palveluista kodin pieniin kunnostustöihin AD-asetus](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).


## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Vertaa Azure AD-toimialueen palveluista Azure DIY AD-toimialueeseen
Seuraavan taulukon avulla voit valita Azure AD-toimialueen palveluiden käyttäminen ja hallinta oman AD-infrastruktuurin Azure-tietokannassa.

|**Toiminto**|**Azure AD-toimialueen palvelut**|**"Kodin pieniin kunnostustöihin" AD-Azure VMs**|
|---|:---:|:---:|
|[**Hallittujen palvelu**](active-directory-ds-comparison.md#managed-service)|**& #x 2713;**|**& #x 2715;**|
|[**Suojatut käytöt**](active-directory-ds-comparison.md#secure-deployments)|**& #x 2713;**|Järjestelmänvalvoja on suojattu käyttöönotto.|
|[**DNS-palvelin**](active-directory-ds-comparison.md#dns-server)|**& #x 2713;** (hallitun palvelu)|**& #x 2713;**|
|[**Toimialueen tai yrityksen järjestelmänvalvojan oikeudet**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges)|**& #x 2715;**|**& #x 2713;**|
|[**Toimialueen liitos**](active-directory-ds-comparison.md#domain-join)|**& #x 2713;**|**& #x 2713;**|
|[**Toimialueen todennusta NTLM ja Kerberos käyttäminen**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos)|**& #x 2713;**|**& #x 2713;**|
|[**Mukautetun OU rakenne**](active-directory-ds-comparison.md#custom-ou-structure)|**& #x 2713;**|**& #x 2713;**|
|[**Rakenteen tunnisteet**](active-directory-ds-comparison.md#schema-extensions)|**& #x 2715;**|**& #x 2713;**|
|[**AD toimialueen ja metsää luottaa**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts)|**& #x 2715;**|**& #x 2713;**|
|[**LDAP lukeminen**](active-directory-ds-comparison.md#ldap-read)|**& #x 2713;**|**& #x 2713;**|
|[**Suojatun LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap)|**& #x 2713;**|**& #x 2713;**|
|[**Kirjoita LDAP**](active-directory-ds-comparison.md#ldap-write)|**& #x 2715;**|**& #x 2713;**|
|[**Ryhmäkäytäntö**](active-directory-ds-comparison.md#group-policy)|Yksinkertainen|Koko|
|[**GEO distributed ominaisuuksissa**](active-directory-ds-comparison.md#geo-dispersed-deployments)|**& #x 2715;**|**& #x 2713;**|


#### <a name="managed-service"></a>Hallittujen palvelu
Microsoft hallitsee Azure AD-toimialueen palveluista toimialueet. Sinun ei tarvitse huolehtia korjataan päivitykset, seuranta, varmuuskopiointi- ja toimialueen käytettävyyden varmistaminen. Nämä hallintatehtäviä tarjotaan palveluna Microsoft Azure hallitun toimialueen.

#### <a name="secure-deployments"></a>Suojatut käytöt
Hallitut toimialueen on suojatusti lukittu AD-versioiden Microsoftin suojauksen parhaiden käytäntöjen mukaisesti. Näiden vinkkien runko-AD tuotetiimin vuosikymmeniä käyttökokemuksesta suunnittelu ja AD-käyttöönoton tukeminen. Kodin pieniin kunnostustöihin käyttöönotoissa haluat ryhtyä käyttöönotto Lukitse alas/suojatun käyttöönoton.

#### <a name="dns-server"></a>DNS-palvelin
Hallitut Azure AD-toimialueen palvelut-toimialueen sisältää hallittu DNS-palvelut. 'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmän jäsenet voivat hallita hallitun toimialueen DNS. Tämän ryhmän jäsenet saavat koko DNS-hallinta-oikeudet hallitun toimialueen. DNS-hallinnan voi suorittaa 'DNS hallintakonsoli' sisältyvät Remote palvelimen hallinnan työkalut (RSAT)-pakettiin.

#### <a name="domain-or-enterprise-administrator-privileges"></a>Toimialueen tai yrityksen järjestelmänvalvojan oikeudet
Nämä laajentamiseen tarjotaan hallitun AAD DS-toimialueella. Sovellukset edellyttävät näiden laajentamiseen on asennettu ja suorita ei voi suorittaa vastaan hallitun toimialueet. Pienempi alijoukon järjestelmänvalvojan oikeudet, voivat käyttää valtuutetun hallinnan nimeltä "AAD Ohjauskoneen Järjestelmänvalvojat-ryhmän jäsenet. Nämä oikeudet ovat oikeudet DNS-asetusten määrittäminen ryhmäkäytännön määrittäminen, voitto järjestelmänvalvojan oikeudet toimialueen liittyneet tietokoneissa jne.

#### <a name="domain-join"></a>Toimialueen liitos
Voit liittyä näennäiskoneiden hallitun toimialueeseen samalla tavalla kuin liityt tietokoneiden miten AD-toimialueen.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Toimialueen todennusta NTLM ja Kerberos käyttäminen
Azure AD-toimialueen palveluista, jossa voit yrityksen tunnistetiedot hallitun toimialueen todentamismenetelmä. Tunnistetiedot säilytetään Azure AD-vuokraajan kanssa. Synkronoidun alihallinnoista Azure AD Connect varmistaa, että tunnistetiedot, jotka on tehty paikallisen muutokset synkronoidaan Azure AD. DIY toimialueen asennuksessa joudut ehkä määrittää toimialueen luottamussuhde paikallisen tilin-metsää todentamismenetelmä yrityksen käyttöoikeudet käyttäjille. Vaihtoehtoisesti voit joutua määrittämään AD replikoinnin varmistaa, että käyttäjien salasanat synkronoida Azure toimialueen ohjauskoneen-näennäiskoneiden.

#### <a name="custom-ou-structure"></a>Mukautetun OU rakenne
'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmän jäsenet voivat luoda mukautettuja organisaatioyksiköiden hallitun toimialueella.

#### <a name="schema-extensions"></a>Rakenteen tunnisteet
Et voi laajentaa hallitun Azure AD-toimialueen palvelut-toimialueen perus rakenne. Vuoksi sovelluksia, jotka käyttävät tunnisteesta AD-rakennetta (esimerkiksi uusilla ominaisuuksilla kohdassa käyttäjäobjekti) ei voi palauttaa ja ajassa AAD-DS toimialueeseen.

#### <a name="ad-domain-or-forest-trusts"></a>AD-toimialueen tai metsää luottamussuhteet
Hallitut toimialueita ei voi määrittää luottamussuhteet (saapuva ja lähtevä liikenne) muiden toimialueiden määrittäminen. Tämän vuoksi skenaarioita, kuten resurssin metsää ominaisuuksissa tai tapauksissa, jos et halua lähettää Synkronoi salasanojen Azure AD ei voi käyttää Azure AD-toimialueen palveluista.

#### <a name="ldap-read"></a>LDAP-luku
Hallitut toimialueen tukee LDAP lukea toiminnoista. Tämän vuoksi käyttöönottoa sovelluksia, jotka suorittavat LDAP luku toimintojen hallittu toimialueelle.

#### <a name="secure-ldap"></a>Suojatun LDAP
Voit määrittää Azure AD-toimialueen palveluista suojatun LDAP pääsy hallitun toimialueen, kuten Internetin välityksellä.

#### <a name="ldap-write"></a>Kirjoita LDAP
Hallitut toimialueen on vain luku-käyttäjän objekteja. Sovellukset, joka suorittaa LDAP kirjoittaminen vastaan määritteet käyttäjän objektin vuoksi eivät toimi hallitun toimialueeseen. Lisäksi käyttäjien salasanat ei voi muuttaa hallittuja toimialueella. Toinen esimerkki olisi ryhmäjäsenyyksiä tai määritteet hallitun toimialueella, joka ei ole sallittua muokkaaminen. Kuitenkin käyttäjän määritteet tai salasanat Azure AD (joko PowerShell/Azure-portaalin) tehdyt muutokset tai paikalliseen AD synkronoidaan AAD DS hallitun toimialueeseen.

#### <a name="group-policy"></a>Ryhmäkäytäntö
Tyylikkään ryhmäkäytännön rakenteita ei tueta AAD DS hallitun toimialueella. Esimerkiksi ei voi luoda ja ota käyttöön jokaisen mukautetun toimialueen OU erillisessä ryhmäkäytäntöobjekteja tai käyttää WMI suodatuksen Ryhmäkäytännön kohdistamisen. Tällä valmiin Ryhmäkäytäntöobjekti kunkin "AADDC tietokoneet" ja 'AADDC käyttäjien' säilöjen, jonka voi mukauttaa ryhmäkäytännön määrittäminen.

#### <a name="geo-dispersed-deployments"></a>GEO keskiarvosta ominaisuuksissa
Azure AD-toimialueen palveluista hallitun toimialueiden ovat käytettävissä yhden virtual verkoston Azure-tietokannassa. Tilanteessa, jotka edellyttävät toimialueen ohjaimet on käytettävissä useita Azure alueilla kaikkialla maailmassa toimialueen ohjaimet-Azure IaaS VMs määrittäminen voi olla parempi vaihtoehto.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>"Kodin pieniin kunnostustöihin" (DIY) AD käyttöönottoasetukset
Saatat joutua käyttöönoton käyttö-tapauksissa tarvittaessa joitakin Windows Server AD-asennuksen tarjoamia ominaisuuksia. Muussa tapauksessa kannattaa jompikumpi seuraavista kodin pieniin kunnostustöihin (DIY):

- **Erillinen cloud toimialueen:** Voit määrittää yksittäisen cloud toimialueen, käyttämällä Azuren näennäiskoneiden, joka on määritetty toimialueen ohjaimet. Tämän infrastruktuurin ei integroida paikallisen että AD-ympäristössä. Tämä vaihtoehto edellyttää on erilaisia 'pilven tunnistetiedot' login/hallinnoimaan VMs pilveen.

- **Resurssin metsää käyttöönoton:** Voit määrittää resurssin metsää, topologian toimialue käyttämällä Azuren näennäiskoneiden on määritetty toimialueen ohjaimet. Seuraavaksi voit määrittää oman paikallista AD-luottamussuhde AD-ympäristössä. Voit toimialueen liity tietokoneiden (Azure VMs) tämän resurssin metsää pilveen. Käyttäjien todentamiseen tapahtuu päälle joko VPN/ExpressRoute yhteyden paikallisesta hakemistosta.

- **Perusteella paikallisen toimialueen Azure:** Voit muodostaa VPN/ExpressRoute-yhteyden avulla paikallisen verkon Azure virtual verkko, niin, että Azure VMs on liitetty paikallisen oman AD. Toinen vaihtoehto on edistää replikan toimialueen ohjaimet paikallisen toimialueesi nimellä AM Azure-tietokannassa. Voit sitten määritystä replikointia päälle VPN/ExpressRoute yhteyden paikallisesta hakemistosta. Käyttöönoton tässä tilassa laajenee paikallisen toimialueen tehokkaasti Azure.

> [AZURE.NOTE] Voit määrittää, että DIY vaihtoehto sopii paremmin käyttöönoton Käytä tapauksissa. Harkitse Auta meitä ominaisuudet auttavat ymmärtämään [palautteen jakaminen](active-directory-ds-contact-us.md) valitsit Azure AD-toimialueen palveluista tulevaisuudessa. Palaute auttaa meitä kehityttävä vastaamaan paremmin tarpeiden mukaan ja käytä tapauksissa palvelu.

On julkaistu [käyttöönotto Windows Server Active Directory-Azuren näennäiskoneiden ohjeet](https://msdn.microsoft.com/library/azure/jj156090.aspx) helpottavat DIY asennuksia.


## <a name="related-content"></a>Aiheeseen liittyvää sisältöä
- [Ominaisuudet - Azure AD-toimialueen palvelut](active-directory-ds-features.md)

- [Käyttöönottoskenaariot - Azure AD-toimialueen palveluista](active-directory-ds-scenarios.md)

- [Ohjeet Windows Azure-Virtuaalikoneissa Server Active Directory käyttöönotto](https://msdn.microsoft.com/library/azure/jj156090.aspx)
