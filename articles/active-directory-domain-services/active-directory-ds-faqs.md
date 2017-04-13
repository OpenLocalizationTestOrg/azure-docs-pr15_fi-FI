<properties
    pageTitle="Usein kysyttyjä kysymyksiä - Azure Active Directory-toimialuepalveluista | Microsoft Azure"
    description="Usein kysyttyjä kysymyksiä Azure Active Directory-toimialueen palveluista"
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
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory-toimialuepalveluista: Usein kysyttyjä kysymyksiä

Tällä sivulla on vastauksia usein kysyttyihin kysymyksiin Azure Active Directory ‑toimialueen palveluihin. Säilytä tarkistaa päivitykset.

### <a name="troubleshooting-guide"></a>Vianmääritysoppaan käyttäminen
Lisätietoja Microsoftin [vianmääritys opas](active-directory-ds-troubleshooting.md) yleisimpien ongelmatilanteiden ratkaisuista muodostettaessa määrittäminen tai Azure AD-toimialueen palveluiden hallintaan.


### <a name="configuration"></a>Määritys

#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Voit luoda useita toimialueita Single Azure Active directory?
Ei. Voit luoda vain yhden toimialueen Azure AD-toimialueen palveluista Single palvelee Azure AD-kansio.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Azure AD-toimialueen palveluista virtual Azure Resurssienhallinta-verkossa käyttöön
Ei. Azure AD-toimialueen palveluihin voi ottaa käyttöön vain perinteinen Azure virtual verkossa. Voit muodostaa perinteinen virtual verkon Resurssienhallinta virtual verkkoon käyttämällä virtual verkon peering käyttää hallitun toimialuetta Resurssienhallinta virtual verkossa.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Voinko soittaa Azure AD-toimialueen palveluista käytettävissä useiden virtual verkostojen kuluessa tilauksen?
Palvelun itse ei tue suoraan Tämä skenaario. Azure AD-toimialueen palveluista on käytettävissä vain yksi virtual verkon kerrallaan. Voi kuitenkin määrittää useita virtual verkkoja voit näyttää Azure AD-toimialueen palvelut virtual muiden verkkojen väliset yhteydet. Tässä artikkelissa kuvataan, kuinka voit [muodostaa Azure virtual verkot](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>PowerShellin Azure AD-toimialueen palvelut käyttöön
Azure AD-toimialueen palveluiden PowerShell/automaattinen käyttöönotto ei ole tällä hetkellä käytettävissä.

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal"></a>Azure AD-toimialueen palveluista käytettävissä uudessa Azure-portaalissa on?
Ei. Azure AD-toimialueen palveluista voidaan määrittää vain [Azure perinteinen portal](https://manage.windowsazure.com). Odotamme laajentaa [Azure portal](https://portal.azure.com) tuesta myöhemmin.

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Toimialueen ohjaimet Lisää Azure AD-toimialueen palveluista hallitun toimialueeseen
Ei. Azure AD-toimialueen palveluista myöntämä toimialue on hallitun toimialueeseen. Sinun ei tarvitse valmistella, määrittäminen ja hallinta muussa toimialueen ohjaimet toimialueessa - nämä hallintatoimintoihin toimitetaan palveluna Microsoft. Tämän vuoksi et voi lisätä hallitun toimialueen lisätoimialuetta ohjaimet (luku-ja kirjoitusoikeudet tai vain luku).

### <a name="administration-and-operations"></a>Hallinta ja toiminnot

#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Voin muodostaa yhteyden toimialueen ohjauskoneen etätyöpöydän hallitun toimialueen varten?
Ei. Ei ole oikeutta muodostaa toimialueen ohjaimet hallitun toimialueen etätyöpöydän kautta. 'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmän jäsenet voivat hallita hallitun toimialueen AD hallintatyökalujen, kuten Active Directoryn hallinta Center (ADAC) tai AD PowerShellin avulla. Nämä työkalut asennetaan hallitun toimialueen Windows Serverissä 'Remote Server-hallintatyökalut'-ominaisuudella.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Minulla on otettu Azure AD-toimialueen palveluista. Mitä käyttäjätilin avulla toimialueen liity koneet toimialueessa?
Olet lisännyt hallintaryhmän (esimerkiksi "AAD Ohjauskoneen Järjestelmänvalvojat") käyttäjät voivat toimialueen liity koneet. Lisäksi tämän ryhmän käyttäjät myönnetään koneet, joka on liitetty toimialueeseen työpöydän etäkäyttö.

#### <a name="can-i-wield-domain-administrator-privileges-for-the-domain-provided-by-azure-ad-domain-services"></a>Toimialueen järjestelmänvalvojan oikeudet Azure AD-toimialueen palveluista myöntämä toimialueen wield
Ei. Ei ole myönnetty järjestelmänvalvojan oikeudet hallitun toimialueella. Toimialueen järjestelmänvalvoja ja yrityksen järjestelmänvalvoja-oikeudet eivät ole käytettävissä, voit käyttää toimialueessa. Aiemmin toimialueen järjestelmänvalvoja tai yrityksen järjestelmänvalvoja-ryhmien Azure AD-kansiossa ei myös ole myönnetty toimialueen tai yrityksen järjestelmänvalvojan oikeudet-toimialueella.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-domains-provided-by-azure-ad-domain-services"></a>Voit muokata ryhmäjäsenyyksiä LDAP tai muita AD Valvontatyökalut käyttäminen toimialueiden Azure AD-toimialueen palveluista myöntämä?
Ei. Toimialueiden palvelee Azure AD-toimialueen palveluista ei voi muokata ryhmäjäsenyyksiä. Sama koskee käyttäjän määritteet. Voi kuitenkin muuttaa ryhmäjäsenyyksiä tai käyttäjän määritteitä Azure AD- tai paikallisen toimialueen. Nämä muutokset synkronoidaan automaattisesti Azure AD-toimialueen palveluihin.

#### <a name="can-i-extend-the-schema-of-the-domain-provided-by-azure-ad-domain-services"></a>Voit laajentaa Azure AD-toimialueen palveluista toimialueen rakennetta?
Ei. Rakenteen hallitaan Microsoft hallitun toimialueen. Azure AD-toimialueen palveluista ei tue rakenteen tunnisteet.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Voit muokata tai hallitun toimialueen DNS-tietueiden lisääminen?
Kyllä. 'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmään kuuluvia käyttäjiä myönnetään DNS järjestelmänvalvoja oikeudet, voit muokata hallittujen toimialueen DNS-tietueet. Nämä käyttäjät voivat käyttää DNS-hallinta-konsolin koneessa, joka on käytössä Windows Server hallitun toimialueen DNS-hallinta. Voit käyttää DNS-hallinta-konsolin asentamalla 'DNS Server työkalut', jotka on osa 'Remote Server-hallintatyökalut' valinnainen ominaisuus palvelimessa. Saat lisätietoja [apuohjelmien hallinta, seurantaa ja vianmääritys DNS](https://technet.microsoft.com/library/cc753579.aspx) on käytettävissä TechNet-sivustossa.


### <a name="billing-and-availability"></a>Laskutus- ja käytettävyys

#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Azure AD-toimialueen palvelut maksullisen palvelun ominaisuudet
Kyllä. Lisätietoja on artikkelissa [hinnoittelua sivun](https://azure.microsoft.com/pricing/details/active-directory-ds/).

#### <a name="is-there-a-free-trial-for-the-service"></a>Onko maksuttoman kokeiluversion palvelulle?
Tämä palvelu sisältyy maksuttoman kokeiluversion Azure varten. Voit rekisteröidä [Azure yhden kuukauden maksuton](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems"></a>Saan Azure AD-toimialueen palveluista osana yrityksen Mobility Suite (EMS)?
#### <a name="do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Pitääkö Azure AD-Premium käyttämään Azure AD-toimialueen palveluista?
Ei. Azure AD-toimialueen palveluista on ryhmävakuutussopimukset Azure-palvelu ja ei kuulu EMS. Azure AD-toimialuepalvelut ovat käytettävissä kaikissa Azure AD-versioissa (maksuton, Basic, ja Premium) ja tunnissa käytön mukaan laskutettu.

#### <a name="what-azure-regions-is-the-service-available-in"></a>Mitä Azure alueiden on palvelu käytössä?
Lisätietoja [Azure palvelujen alue](https://azure.microsoft.com/regions/#services/) -sivulle nähdäkseen Azure alueet, joissa Azure AD-toimialueen palveluista on käytettävissä luettelo.
