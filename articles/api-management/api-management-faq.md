<properties
    pageTitle="Azure API hallinta usein kysytyt kysymykset | Microsoft Azure"
    description="Lue vastauksia yleisiin kysymyksiin, kuvioiden ja Azure API hallinnan parhaista käytännöistä."
    services="api-management"
    documentationCenter=""
    authors="miaojiang"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="mijiang"/>

# <a name="azure-api-management-faqs"></a>Azure API hallinta usein kysyttyjä kysymyksiä

Hae vastauksia yleisiin kysymyksiin, kuvioiden ja parhaita käytäntöjä Azure API hallintaa varten.

## <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

-   [Miten voin esittää Microsoft Azure API Management-ryhmän kysymyksen?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)
-   [Mitä tarkoittaa, kun ominaisuus on esikatselu?](#what-does-it-mean-when-a-feature-is-in-preview)
-   [Kuinka voit suojata API Management Gatewayn ja palveluiden taustatietokantaan välisen yhteyden?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
-   [Miten oma API Management-palvelun esiintymän kopioiminen uuden esiintymän?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
-   [Hallitaan API hallinta-esiintymän ohjelmallisesti?](#can-i-manage-my-api-management-instance-programmatically)
-   [Kuinka lisään käyttäjän järjestelmänvalvojien ryhmään?](#how-do-i-add-a-user-to-the-administrators-group)
-   [Miksi käytännön, joka haluan lisätä käytännön-editorissa ei ole käytettävissä?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
-   [Miten API versiotietojen käyttäminen API hallinta?](#how-do-i-use-api-versioning-in-api-management)
-   [Miten useita ympäristöissä yksittäisen API-määritetään?](#how-do-i-set-up-multiple-environments-in-a-single-api)
-   [Voinko käyttää SOAP API hallinnan?](#can-i-use-soap-with-api-management)
-   [On API hallinta yhdyskäytävän IP-osoite vakio? Voit käyttää sitä palomuurisäännöt?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-   [OAuth 2.0-todennus-palvelimen määrittäminen niin, että AD FS-suojaus](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-   [Mitä reititys menetelmää API hallinta käyttää käyttöönotoissa useita maantieteellisiä sijainteja?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
-   [Luo erillisen service API hallinta Azure Resurssienhallinta-mallin avulla?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
-   [Itse allekirjoitetun SSL-varmenteen käyttää taustatietokannaksi varten](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
-   [Miksi Todennusvirhe tulee kun yritän Kloonaa GIT säilön?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
-   [Toimii API hallinta Azure ExpressRoute?](#does-api-management-work-with-azure-expressroute)
-   [Voit siirretään API hallintapalvelun yksi tilaus toiseen?](#can-i-move-an-api-management-service-from-one-subscription-to-another)


### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Miten voin esittää Microsoft Azure API Management-ryhmän kysymyksen?

Ota yhteyttä käyttämällä jotakin seuraavista vaihtoehdoista:

-   Lähettää kysymyksiä Microsoftin [API hallinta MSDN-keskustelupalsta](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
-   Lähetä sähköpostia <apimgmt@microsoft.com>.
-   Voit lähettää tukipyynnön ominaisuus [Azure palautetta keskustelupalstalle](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Mitä tarkoittaa, kun ominaisuus on esikatselu?

Kun toiminto on esikatselu, se tarkoittaa sitä, että emme ole aktiivisesti hakemista palautetta siitä, miten ominaisuus toimii puolestasi. Esikatselu-toiminto on valmis toiminnallisesti, mutta se on mahdollista, että olemme tehdä jakautumisen, muuttaa asiakaspalautteen. On suositeltavaa, että et määräytyvät ominaisuus, joka on tuotantoympäristössä esikatselussa. Jos sinulla palautetta esikatselutoiminnot, kerro meille jonkin yhteyshenkilön vaihtoehtojen avulla [Kuinka voin esittää Microsoft Azure API Management-ryhmän kysymyksen?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>Kuinka voit suojata API Management Gatewayn ja palveluiden taustatietokantaan välisen yhteyden?

Sinulla on useita vaihtoehtoja suojaamiseen API Management Gatewayn ja taustatietokantaan palvelujen välinen yhteys. Voit:

-   Käytä HTTP perustodentamista. Lisätietoja on artikkelissa [määrittäminen API asetukset](api-management-howto-create-apis.md#configure-api-settings).
- Käytä SSL-molemminpuoleista kuvatulla tavalla [suojaamisesta taustatietokantaan services Asiakasvarmenteen todennus Azure API hallinnan avulla](api-management-howto-mutual-certificates.md).
- Käytä IP-whitelisting taustatietokantaan-palvelusta. Jos sinulla on vakio- tai Premium taso-Ohjelmointirajapinnan hallinta esiintymän, yhdyskäytävän IP-osoite säilyy muuttumattomana. Voit määrittää oman whitelist Salli IP-osoitteen. Saat API hallinta esiintymän IP-osoitteen koontinäytössä Azure-portaalissa.
- Yhdistä API hallinta-esiintymän Virtual Azure-verkkoon. Lisätietoja on artikkelissa [VPN-yhteydet Azure API hallinnan määrittämisestä](api-management-howto-setup-vpn.md).

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Miten oma API Management-palvelun esiintymän kopioiminen uuden esiintymän?

Sinulla on useita vaihtoehtoja, jos haluat kopioida API hallinta esiintymä uuden esiintymän. Voit:

-   Käytä varmuuskopiointi ja palauttaminen API hallinta-funktion. Lisätietoja on artikkelissa [Toteuta palauttaminen käyttämällä palvelun varmuuskopiointi ja palauttaminen Azure API hallinnassa](api-management-howto-disaster-recovery-backup-restore.md).
-   Luoda omia varmuuskopiointi ja palauttaminen ominaisuus käyttämällä [API hallinta REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx). REST-Ohjelmointirajapinnalla avulla voit tallentaa ja palauttaa kohteita, jotka haluat palvelun esiintymän.
-   Lataa palvelun käyttämällä Git ja lataa se sitten uuden esiintymän. Lisätietoja on artikkelissa [tallentaminen ja API Management service-kokoonpanon käyttämällä Git määrittäminen](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Hallitaan API hallinta-esiintymän ohjelmallisesti?

Kyllä, voit hallita API hallinta ohjelmallisesti käyttämällä:

-   [API hallinta REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx).
-   [Microsoft Azure ApiManagement palvelun kirjaston SDK](http://aka.ms/apimsdk).
-   [Palvelun käyttöönottoa](https://msdn.microsoft.com/library/mt619282.aspx) ja [hallinnan](https://msdn.microsoft.com/library/mt613507.aspx) PowerShell-cmdlet.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Kuinka lisään käyttäjän järjestelmänvalvojien ryhmään?

Näin, miten voit lisätä käyttäjän järjestelmänvalvojien ryhmään:

1. Kirjautuminen [Azure portal](https://portal.azure.com).
2. Siirry resurssiryhmän, joka on päivitettävän API hallinnan esiintymä.
3. Määrittää API hallinta- **Ohjelmointirajapinnan hallinta osallistujan** rooli käyttäjälle.

Nyt lisätyn osallistuja käyttää Azure PowerShell- [cmdlet-komennot](https://msdn.microsoft.com/library/mt613507.aspx). Näin kirjautua sisään järjestelmänvalvojana:

1. Käytä `Login-AzureRmAccount` cmdlet-komento, kirjautua sisään.
2. Määritä yhteydessä tilaukseen, joka sisältää palvelun avulla `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Hae yhteen Sign URL käyttämällä `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. URL-Osoitteen avulla voit helposti hallintaportaalissa.


### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Miksi käytännön, joka haluan lisätä käytännön-editorissa ei ole käytettävissä?

Jos käytäntö, jonka haluat lisätä näkyy harmaana tai Varjostettu käytännön editorin, muista, että käytössä on oikean vaikutusalueen käytännön. Kunkin käytännön-lause on suunniteltu tiettyjen alueiden ja käytännön osien käyttöä. Käytännön osiot ja käytännön vaikutusalueet on ohjeaiheessa [API hallintakäytännöt](https://msdn.microsoft.com/library/azure/dn894080.aspx)käytännön käyttö-kohdassa.


### <a name="how-do-i-use-api-versioning-in-api-management"></a>Miten API versiotietojen käyttäminen API hallinta?

Sinulla on useita vaihtoehtoja API versiotietojen käyttäminen API hallinta:

-   Voit määrittää API hallinta-ohjelmointirajapinnan edustaa eri versioita. Esimerkiksi voi olla kaksi eri API, MyAPIv1 ja MyAPIv2. Kehittäjä voit valita haluamasi version kehittäjä haluaa käyttää.
-   Voit myös määrittää oman API ja palvelun URL-osoite, joka ei sisällä versio segmentin, esimerkiksi https://my.api. Määritä sitten versio-osiossa kunkin toiminnon [Uudelleen URL-Osoitteen](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) mallin. Voit esimerkiksi on toiminnon /resource ja kutsua /v1/Resource [Uuden esimerkkitekstin URL-Osoitteen](api-management-howto-add-operations.md#rewrite-url-template) mallin [URL-malli](api-management-howto-add-operations.md#url-template) . Voit muuttaa erikseen jokaiselle toiminnolle version segmentin-arvoa.
-   Jos haluat säilyttää "oletus" versio segmentin Ohjelmointirajapinnan palvelun URL-osoite, valitun toiminnoissa määrittää käytännön, joka käyttää [Taustajärjestelmä-palvelun määrittäminen](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) käytännön taustatietokantaan pyynnön liikeradan muuttaminen.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Miten useita ympäristöissä yksittäisen API-määritetään?

Voit määrittää useita ympäristöissä, esimerkiksi testiympäristössä ja tuotantoympäristössä, yhden API, sinulla on kaksi vaihtoehtoa. Voit:

-   Host eri API saman vuokraajan.
-   Ylläpitää samaa API-eri alihallinnat.

### <a name="can-i-use-soap-with-api-management"></a>Voinko käyttää SOAP API hallinnan?

Käytettävissä on nyt [SOAP läpivienti](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) tuki. Järjestelmänvalvojat voivat tuoda niiden SOAP-palvelun WSDL ja Azure API hallinta luo SOAP edusta. Kehittäjän portaalin ohjeissa, Testaa konsolin, käytännöt ja analytics ovat käytettävissä SOAP-palveluiden.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>On API hallinta yhdyskäytävän IP-osoite vakio? Voit käyttää sitä palomuurisäännöt?

Vakio- ja Premium-tasoa osoitteessa julkinen IP-osoite (VIP) API hallinta-vuokraajan on staattinen elinkaaren palveltava poikkeuksin. IP-osoitteen muuttumisen näissä tapauksissa:

-   Palvelu on poistettu ja luodaan uudelleen.
-   Palvelun tilaus on hyllytetty (esimerkiksi nonpayment) ja palauttaa sitten.
-   Voit lisätä tai poistaa Azure Virtual Network (Voit käyttää VPN vain Premium taso).

Monille käyttöönotoissa alueellisen osoite muuttuu, jos alue on vacated ja palauttaa sitten (Voit käyttää monille käyttöönoton vain Premium taso).

Premium taso alihallinnat, jotka on määritetty monille käyttöönottoa varten määritetään yksi julkiseen IP-osoite alueittain.

Saat IP-osoite (tai osoitteet, monille käyttöönoton) vuokraajan sivulla Azure-portaalissa.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>OAuth 2.0-todennus-palvelimen määrittäminen niin, että AD FS-suojaus

Opettele määrittämään OAuth 2.0-todennus palvelimen Active Directory Federation Services (AD FS)-suojausta-kohdassa [Käyttämällä ADFS API hallinta](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Mitä reititys menetelmää API hallinta käyttää käyttöönotoissa useita maantieteellisiä sijainteja?

API hallinta käyttää [suorituskyvyn liikenne reititys menetelmä](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method) käyttöönotoissa useita maantieteellisiä sijainteja. Saapuvan liikenteen reititetään lähinnä API-yhdyskäytävä. Jos jonkin alueen offline-tilassa, saapuvan liikenteen reititetään automaattisesti seuraavan lähinnä yhdyskäytävän. Lisätietoja reititys esitetyssä [liikenteen hallinta reititys tavoista](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Luo erillisen service API hallinta Azure Resurssienhallinta-mallin avulla?

Kyllä. Näet [Azure API hallintapalvelun](http://aka.ms/apimtemplate) pikaopas mallit.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Itse allekirjoitetun SSL-varmenteen käyttää taustatietokannaksi varten

Kyllä. Näin voit käyttää itse allekirjoitettua varmennetta Secure Sockets Layer (SSL) taustatietokannaksi:

1. Luo [Taustajärjestelmä](https://msdn.microsoft.com/library/azure/dn935030.aspx) kohteen API hallinnan avulla.
2. **SkipCertificateChainValidation** -ominaisuuden arvoksi **true**.
3. Jos haluat sallia itse allekirjoitettua varmenteet eivät enää, poista Taustajärjestelmä kohteen tai **skipCertificateChainValidation** -ominaisuuden arvoksi **Epätosi**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Miksi Todennusvirhe tulee kun yritän Kloonaa Git säilön?

Jos käytät Git tunnistetietojen hallinnan tai jos yrität Kloonaa Git säilön Visual Studiossa, joita saattaa ilmetä Windows-tunnistetiedot-valintaikkunan tunnettu ongelma. Valintaikkunan rajoittaa salasanan vähimmäispituus 127 merkkiä, ja se katkaisee Microsoft luotu salasana. Yritämme-peruuttamisessa sekä salasana. Nyt Käytä Git Bash Kloonaa Git säilöön.

### <a name="does-api-management-work-with-azure-expressroute"></a>Toimii API hallinta Azure ExpressRoute?

Kyllä. API hallinta toimii Azure ExpressRoute.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Voit siirretään API hallintapalvelun yksi tilaus toiseen?

Kyllä. Lisätietoja on ohjeartikkelissa [siirtää uusi resurssiryhmä tai-Tilaustani resursseja](../resource-group-move-resources.md).
