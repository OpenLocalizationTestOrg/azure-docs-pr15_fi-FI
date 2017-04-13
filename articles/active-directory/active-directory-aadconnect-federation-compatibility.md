<properties
    pageTitle="Azure AD federation yhteensopivuuden luettelo"
    description="Tällä sivulla on-Microsoft tunnistetietojen toimittajat, joilla voidaan ottaa käyttöön kertakirjautumisen."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="billmath"/>

# <a name="azure-ad-federation-compatibility-list"></a>Azure AD federation yhteensopivuuden luettelo
Azure Active Directory tarjoaa single sign ja parannetun sovelluksen access-suojauksen Office 365: ssä ja muihin Microsoft Online services hybrid ja vain pilvipalveluita käyttöotot tarvitsematta minkä tahansa-Microsoft ratkaisun. Office 365-useimmat Microsoftin Online-palveluihin, kuten on integroitu Azure Active Directory hakemistopalvelujen, todennus- ja varten. Azure Active Directory sisältää myös kertakirjauksen tuhansia SaaS sovellukset käyttöön ja paikallisen verkkosovelluksia. Katso tuetut SaaS sovellusten Azure Active Directory-sovelluksen valikoima.

Organisaatioissa, joissa on sijoitettu muiden kuin Microsoft federation ratkaisuja tässä artikkelissa on ohjeita määrittäminen kertakirjautumisen niiden Windows Server Active Directory-käyttäjien Microsoft Online Services-palvelun avulla-Microsoft tunnistetietojen toimittajat "Azure Active Directory federation yhteensopivuuden alla olevasta luettelosta". 


![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Oxford tietokoneen ryhmän](http://oxfordcomputergroup.com/), kolmannelta osapuolelta, Microsoftin puolesta testannut yhden Sign-on nyt käyttämällä-Microsoft tunnistetietojen toimittajat vastaan Käytä tapauksissa yleisiä Azure Active Directory-hakemistosta.

Tietoja siitä, miten voit saada kolmannen osapuolen palveluntarjoajan tässä luettelossa, ota yhteyttä Oxford tietokoneen ryhmän [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com).

>[AZURE.IMPORTANT] Oxford tietokoneen ryhmän testata vain yhden Sign tilanteisiin federation-toimintoja. Oxford tietokoneen ryhmän ei suorittanut kaikki testit synkronoinnin, kaksiosainen todentamismenetelmä, jne. komponentit yksittäisen Sign tilanteisiin.

>Kirjautuminen vaihtoehtoisen täydellisen Käyttäjätunnuksen tunnuksen mukaan käyttö on myös ei testannut ohjelma.



- [Azure Active Directory](#azure-active-directory)
- [Paras mahdollinen IDM Virtual tunnistetietojen Server-liittoutumispalvelut](#optimal-idm-virtual-identity-server-federation-services) 
- [PingFederate 6.11](#pingfederate-611) 
- [PingFederate 7.2](#pingfederate-72) 
- [PingFederate 8.x](#pingfederate-8x)
- [Centrify](#centrify) 
- [IBM Tivoli liitetty käyttäjätietojen hallinta 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
- [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
- [CA SiteMinder 12,52](#ca-siteminder-1252-sp1-cumulative-release-4) 
- [RadiantOne CFS-pelin 3.0](#radiantone-cfs-30) 
- [Okta](#okta) 
- [OneLogin](#onelogin) 
- [NetIQ Access hallinta 4.0.1](#netiq-access-manager-401) 
- [Access-käytännön hallinta ISO-IP-versio x – 11.6 x 11.3 kanssa ISO-IP](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x) 
- [VMware työtilan Portal versio 2.1](#vmware-workspace-portal-version-21) 
- [Kirjaudu ja siirry 5.3](#signampgo-53) 
- [IceWall Federation 3.0-versiota](#icewall-federation-version-30) 
- [CA suojatun pilveen](#ca-secure-cloud) 
- [Dell yksi käyttäjätietojen Cloud käytön hallinta v7.1](#dell-one-identity-cloud-access-manager-v71) 
- [AuthAnvil Single Sign-4.5](#authavil-single-sign-on-45) 

>[AZURE.IMPORTANT] Koska nämä ovat kolmannen osapuolen tuotteita, Microsoft ei tarjoa tukea käyttöönoton, määritys, vianmääritys, parhaat käytännöt, jne ongelmat ja nämä tunnistetietojen toimittajat koskevia kysymyksiä. Tuki-ja käyttäjätiedot näistä palveluista koskevia kysymyksiä Ota yhteyttä tuetut kolmansille osapuolille.

>Kolmannen osapuolen näistä palveluista on testannut yhteentoimivuus WS Federation ja WS luotettavaksi protokollat vain Microsoftin pilvipalveluihin. Testauksessa eivät kuulu SAML-protokollan avulla.

## <a name="azure-active-directory"></a>Azure Active Directory 
Azure Active Directory voi todentaa käyttäjät mukaan sisällytetyistä paikallisen Active Directory kanssa tai ilman paikallisen-liittoutumispalvelimen käyttämällä salasanan synkronointi. 

Seuraavassa on tämän Sign takaamiseksi skenaarion tuki-matriisin: 


| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Ei mitään|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Ei mitään|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|
|Nykyaikainen sovellusten ADAL, kuten Office 2016| Tuettu|Ei mitään|

Saat lisätietoja Azure Active Directoryn avulla AD FS [Active Directory Federation Services (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

Saat lisätietoja Azure Active Directory käyttämisestä salasanan synkronointi [Azure AD Connect](active-directory-aadconnect.md).


## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Paras mahdollinen IDM Virtual tunnistetietojen Server-liittoutumispalvelut 
Paras mahdollinen IDM Virtual tunnistetietojen palvelimen liittoutumispalvelut voi todentaa käyttäjät, jotka sijaitsevat asiakkaiden paikallisen Active kansioita.

Seuraavassa on skenaarion tukevat matriisin tämän yksittäisen Sign-versio:


| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Ei mitään|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Integroitu Windows-todennus|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Katso lisätietoja client access-käytäntöjä [rajoittaminen käyttää Office 365: n palveluiden perustuu asiakkaan sijaintiin.](https://technet.microsoft.com/library/hh526961.aspx)|



## <a name="pingfederate-611"></a>PingFederate 6.11 

PingFederate 6.11 toteuttaa yleisesti käytetty WS Federation identity-standardin antamaan kertakirjautumisen ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin:


| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Ei mitään|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Ei mitään (aiempien versioiden on päivitettävä 6.11|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

PingFederate ohjeita siitä, miten voit määrittää tämän STS ja tarjota Sign-käyttökokemuksen Active Directory-käyttäjien ladata PDF-tiedosto [tähän.](http://go.microsoft.com/fwlink/?LinkID=266321)

## <a name="pingfederate-72"></a>PingFederate 7.2 
PingFederate 7.2 toteuttaa yleisesti käytetty WS Federation/WS-salli tunnistetietojen standardiksi antamaan kertakirjautumisen ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin:


| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Ei mitään|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Ei mitään|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

PingFederate ohjeita siitä, miten voit määrittää tämän STS ja tarjota Sign-käyttökokemuksen Active Directory-käyttäjiä, katso [tähän.](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)

## <a name="pingfederate-8x"></a>PingFederate 8.x 
PingFederate 8.x toteuttaa yleisesti käytetty WS Federation/WS-luota identity-standardin antamaan kertakirjautumisen ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin:


| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Ei mitään|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Ei mitään|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

PingFederate ohjeita siitä, miten voit määrittää tämän STS ja tarjota Sign-käyttökokemuksen Active Directory-käyttäjiä, katso [tähän.](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="centrify"></a>Centrify 
Centrify auttaa tarjota liitetyt yksittäisen Sign-käyttökokemuksen Office 365: n ilman, että sekä isännöinnin paikallisen-liittoutumispalvelimen.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin:


| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Ei mitään|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Ei mitään|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Client Access ei tueta 

Saat lisätietoja Centrify [tähän.](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli liitetty käyttäjätietojen hallinta 6.2.2 
IBM Tivoli liitetty Identity Managerin 6.2.2 kanssa IBM suojauksen Access, Microsoft-sovellusten 1.4 toteuttaa yleisesti käytetty WS Federation/WS-luota identity-standardin antamaan kertakirjautumisen ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin: 

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Ei mitään|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Ei mitään|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

Katso lisätietoja tietoja IBM Tivoli liitetty käyttäjätietojen hallinta [IBM Security Access Manager for Microsoft Applications.](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0 
SecureAuth IdP 7.2.0 toteuttaa yleisesti käytetty WS Federation/WS-luota identity-standardin antamaan Sign-toiminto ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin: 

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Ei mitään|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Ei mitään|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

Saat lisätietoja SecureAuth [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>CA SiteMinder 12,52 SP1 kumulatiivinen versio 4
CA SiteMinder Federation 12,52 toteuttaa yleisesti käytetty WS Federation/WS-salli tunnistetietojen standardiksi antamaan kertakirjautumisen ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin: 

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Ei mitään|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Ei mitään|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

Saat lisätietoja CA SiteMinder [CA SiteMinder Federation.](http://www.ca.com/us/products/ca-single-sign-on.html) 

## <a name="radiantone-cfs-30"></a>RadiantOne CFS-pelin 3.0 
RadiantOne Cloud Federation Service (CFS) 3.0 toteuttaa yleisesti käytetty WS Federation/WS-salli tunnistetietojen standardiksi antamaan kertakirjautumisen ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin: 

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Ei mitään|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Integroitu Windows-todennus|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

Saat lisätietoja RadiantOne CFS-pelin [RadiantOne CFS-pelin.](http://www.radiantlogic.com/products/radiantone-cfs/)


## <a name="okta"></a>Okta 
Okta toteuttaa yleisesti käytetty WS Federation/WS-salli tunnistetietojen standardiksi antamaan kertakirjautumisen ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin: 


| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Windows-todennusta edellyttää muita verkkopalvelin ja Okta sovelluksen asetukset.|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Integroitu Windows-todennus|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

Saat lisätietoja Okta [Okta.](https://www.okta.com/)
 
## <a name="onelogin"></a>OneLogin 
OneLogin kuin testannut toukokuu 2014 toteuttaa yleisesti käytetty WS Federation/WS-luota identity-standardin antamaan kertakirjautumisen ja määritettä exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin: 

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Integroitu Windows-todennus|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Integroitu Windows-todennus|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

Saat lisätietoja OneLogin [OneLogin.](https://www.onelogin.com/)

## <a name="netiq-access-manager-401"></a>NetIQ Access hallinta 4.0.1 
NetIQ Access hallinta 4.0.1 toteuttaa yleisesti käytetty WS Federation/WS-luota identity-standardin antamaan kertakirjautumisen ja määritettä exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin:

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |* Kerberos sopimuksia tuettu|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Windows-todennusta ei tueta|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

* NetIQ tuetaan Kerberos-todentamista Kerberos-sopimus määrittäminen kautta.  Apua määritysten Ota NetIQ tai tarkastella palvelupaketin määritysopas. Katso lisätietoja NetIQ Access Managerin [NetIQ Access Managerin.](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html)

## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>Access-käytännön hallinta ISO-IP-versio ja ISO-IP 11.3 x – 11.6 x 
ISO-IP Access käytännön hallinnan avulla (APM) ISO-IP-versio 11.3 x – 11.6 x toteuttaa yleisesti käytetty SAML tunnistetietojen standardiksi antamaan Sign-toiminto ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin: 

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Ei mitään|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Ei tueta |Ei tueta|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

Katso lisätietoja tietoja ISO IP Access käytännön hallinta [ISO IP Access käytännön Managerin.](https://f5.com/products/modules/access-policy-manager) 

ISO-IP Access käytännön hallinta ohjeita siitä, miten voit määrittää tämän STS ja tarjota Sign-käyttökokemuksen Active Directory-käyttäjien ladata PDF-tiedosto [tähän.](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)

## <a name="vmware--workspace-portal-version-21"></a>VMware työtilan Portal versio 2.1 
VMware työtilan Portal versio 2.1 toteuttaa yleisesti käytetty WS Federation/WS-luota identity-standardin antamaan kertakirjautumisen ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin:

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Windows-todennusta ei tueta|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM | Tuettu |Windows-todennusta ei tueta|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

Lisätietoja VMware työtilan Portal versio 2.1 Lataa PDF-tiedosto [tähän.](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf)

## <a name="signgo-53"></a>Kirjaudu ja siirry 5.3 
Kirjaudu ja siirry 5.3 toteuttaa yleisesti käytetty WS Federation/WS-luota käyttäjätietoja kertakirjautumisen sekä määrite exchange framework on vakio.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin:

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Kerberos-sopimuksia tuettu |
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM | Tuettu |Ei mitään|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|


Kirjaudu ja siirry 5.3 tukee Kerberos-todennus Kerberos-sopimus määrittäminen kautta.  Tässä määrityksessä apua, ota yhteyttä Ilex tai Näytä asetukset-opas [tähän.](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)


## <a name="icewall-federation-version-30"></a>IceWall Federation 3.0-versiota 
IceWall Federation versiota 3.0 toteuttaa yleisesti käytetty WS Federation/WS-luota identity-standardin antamaan kertakirjautumisen ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin:

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Windows-todennusta ei tueta|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM | Tuettu |Windows-todennusta ei tueta|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

Saat lisätietoja IceWall Federation [tähän](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) ja [tähän.](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html)

## <a name="ca-secure-cloud"></a>CA suojatun pilveen 

CA suojatun Cloud toteuttaa yleisesti käytetty WS Federation/WS-salli tunnistetietojen standardiksi antamaan kertakirjautumisen ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin:

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Windows-todennusta ei tueta|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM | Tuettu |Windows-todennusta ei tueta|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

Saat lisätietoja CA suojatun Cloud [CA suojatun Cloud.](http://www.ca.com/us/products/security-as-a-service.aspx)

## <a name="dell-one-identity-cloud-access-manager-v71"></a>Dell yksi käyttäjätietojen Cloud käytön hallinta v7.1 
Dell yhden käyttäjätietojen Cloud käytön hallinta toteuttaa yleisesti käytetty WS Federation/WS-luota identity-standardin antamaan kertakirjautumisen ja määritettä exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin:

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Ei mitään|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM |  Tuettu |Ei mitään|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|

Katso lisätietoja Dell yhden tunnistetietojen Cloud Access Managerin [Dell yhden käyttäjätietojen Cloud käytön hallinta.](http://software.dell.com/products/cloud-access-manager)

 Kertakirjauksen kokemus tarjota Office 365-käyttäjille siitä, miten voit määrittää tämän STS ohjeet [määrittäminen Office 365-käyttäjille.](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365) 

## <a name="authanvil-single-sign-on-45"></a>AuthAnvil Single Sign-4.5 
AuthAnvil yksittäinen merkki, 4.5 toteuttaa yleisesti käytetty WS Federation/WS-salli tunnistetietojen standardiksi antamaan kertakirjautumisen ja määritteen exchange framework.

Seuraavassa on tämän yksittäisen Sign takaamiseksi skenaarion tuki-matriisin:

| Asiakkaan |Tuki  |Poikkeukset|
| --------- | --------- |--------- |
| WWW-asiakkaille, kuten Exchange Web Accessin ja SharePoint Online | Tuettu |Windows-todennusta ei tueta|
| Rich client-sovelluksista, kuten Lync, Office-tilaus, CRM | Tuettu |Windows-todennusta ei tueta|
| Sähköpostin RTF-asiakkaille, kuten Outlook ja ActiveSync |  Tuettu |Ei mitään|


Lisätietoja on artikkelissa [AuthAnvil kertakirjautuminen.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-)
