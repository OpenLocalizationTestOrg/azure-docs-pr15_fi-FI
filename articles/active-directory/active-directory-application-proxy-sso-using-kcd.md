<properties
    pageTitle="Kertakirjautumisen kanssa sovelluksen välityspalvelimen | Microsoft Azure"
    description="Anna kertakirjautumisen Azure AD-sovelluksen välityspalvelimen käsitellään."
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
    ms.date="08/10/2016"
    ms.author="kgremban"/>


# <a name="single-sign-on-with-application-proxy"></a>Kertakirjautumisen sovelluksen välityspalvelimen kanssa

Kertakirjautumisen on välityspalvelimen Azure AD-sovelluksen osa. Se on paras käyttäjäkokemuksen seuraavasti:

1. Käyttäjä kirjautuu pilveen  
2. Kaikki suojauksen kelpoisuus tapahtua (esitarkistusta) pilveen  
3. Kun kutsu on lähetetty paikallinen-sovellukseen, sovelluksen välityspalvelimen yhdistimen tekeytyy käyttäjä. Tämä on tulossa toimialueeseen liittyneet laitteesta tavallinen käyttäjä saattavat Taustajärjestelmä sovelluksen mukaan.

![Access-kaavion peruskäyttäjän sovelluksen välityspalvelimen kautta yrityksen verkkoon](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)

Välityspalvelimen Azure AD-sovelluksen avulla voit tarjota käyttäjille kertakirjautuminen (SSO) käyttökokemuksen. Seuraavien ohjeiden avulla voit julkaista yrityksen sovellusten SSO avulla:


## <a name="sso-for-on-prem-iwa-apps-using-kcd-with-application-proxy"></a>Paikallinen IWA sovellusten KCD käyttäminen sovelluksen välityspalvelimen SSO
Voit ottaa käyttöön käyttämällä integroitu Windows todennus (IWA) tekeytyminen käyttäjille, ja Lähetä ja vastaanota tunnusten heidän puolestaan antamalla sovelluksen välityspalvelimen yhdistimet oikeudet Active Directory-sovelluksiin.


### <a name="network-diagram"></a>Verkkokaavio

Tässä kaaviossa kerrotaan kulun, kun käyttäjä yrittää käyttää IWA käyttävä paikallinen sovellus.

![Microsoft AAD todennus Tietovuokaavion Esimerkki](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. Käyttäjä lisää URL-osoite Paikallinen sovelluksen käyttämiseen sovelluksen välityspalvelimen kautta.
2. Sovelluksen välityspalvelimen ohjaa pyynnön Azure AD-todennuspalvelujen, preauthenticate. Tässä vaiheessa Azure AD koskee sovellettavan todennus- ja luvan käytäntöjä, kuten monimenetelmäisen todentamisen. Jos käyttäjä on tarkistettu, Azure AD luo tunnuksen ja lähettää sen käyttäjälle.
3. Käyttäjän välittää tunnuksen sovelluksen välityspalvelin.
4. Sovelluksen välityspalvelimen tarkistaa tunnuksen ja käyttäjän pääasiallista nimi (UPN) hakee ja lähettää pyynnön ja palvelun nimi (SPN) UPN yhdistimen dually todennetut suojatun kanavan kautta.
5. Yhdistimen suorittaa Kerberos Rajoitettu delegointi (KCD) neuvottelun kanssa paikallinen AD, tekeytyminen käyttäjä voi saada Kerberos-tunnus-sovellukseen.
6. Active Directory lähettää yhdistimen sovelluksen Kerberos-tunnus.
7. Yhdistimen lähettää alkuperäisen pyynnön sovelluspalvelin, käyttämällä sen vastaanotti AD Kerberos-tunnus.
8. Sovelluksen lähettää vastauksen yhdistin, joka palautetaan takaisin sovelluksen välityspalvelu ja lopuksi käyttäjälle.

### <a name="prerequisites"></a>Edellytykset

Ennen kuin voit aloittaa SSO välityspalvelinta, varmista, että ympäristö valmistellaan seuraavat asetukset ja määritykset:

- Sovellukset, kuten SharePoint-Web-sovellukset on määritetty käyttämään Windows-todennusta. Lisätietoja artikkelissa [Kerberos-todennus käyttöön tuki](https://technet.microsoft.com/library/dd759186.aspx)ja artikkelissa SharePoint [suunnitteleminen SharePoint 2013: n Kerberos-todentamista](https://technet.microsoft.com/library/ee806870.aspx).

- Kaikki sovellukset on palvelun lyhennys nimet.

- Yhdistimen palvelimeen ja palvelimessa, jossa sovellus on liitetty ja osa samaa toimialuetta tai luottaminen toimialueet. Lisätietoja toimialueen liitos on artikkelissa [osallistuminen tietokoneen toimialueeseen](https://technet.microsoft.com/library/dd807102.aspx).

- Yhdistimen palvelimeen käyttää Lue TokenGroupsGlobalAndUniversal käyttäjille. Tämä on oletusarvo, joka saattaa ovat ole vaikuttaneet hardening ympäristön suojausta. Saat lisää ohjeita tämän [KB2009157](https://support.microsoft.com/en-us/kb/2009157).

### <a name="active-directory-configuration"></a>Active Directory-määritys

Active Directory-määritys vaihtelee sen mukaan, onko sovelluksen välityspalvelimen-yhdistin ja julkaistun server ovat samassa toimialueessa, vai ei.

#### <a name="connector-and-published-server-in-the-same-domain"></a>Yhdistin ja samaa toimialuetta julkaistun palvelimeen

1. **Valitse Active Directory-** > **käyttäjät ja tietokoneet**.
2. Valitse yhdistin-palvelimeen.
3. Napsauta hiiren kakkospainikkeella ja valitse **Ominaisuudet** > **delegointi**.
4. Valitse **Luota tämän tietokoneen delegoinnin määritetyn palveluihin** ja **palveluja, joihin tämän tilin esittää delegoituja tunnistetietoja**, Lisää sovelluspalvelin SPN tunnus-arvo.
5. Ottaa käyttöön sovelluksen välityspalvelimen yhdistimen tekeytyminen käyttäjien AD määritetty luettelossa sovellusten vastaan.

![Yhdistimen SVR ominaisuudet-ikkunassa näyttökuva](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-published-server-in-different-domains"></a>Yhdistimen ja julkaistun palvelimen eri toimialueilla

1. Edellytykset käsittelyyn KCD toimialueiden luettelo on artikkelissa [Kerberos Rajoitettu delegointi toimialueilla](https://technet.microsoft.com/library/hh831477.aspx).
2. Windows 2012 R2: n avulla `principalsallowedtodelegateto` Connector-palvelinta, voit ottaa käyttöön sovelluksen välityspalvelin delegoida Connector-palvelinta, jossa julkaistun palvelin on ominaisuuden `sharepointserviceaccount` ja valtuutettu palvelin on `connectormachineaccount`.

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount`voi olla SPS koneen tili tai palvelutilin, jossa SPS sovellussarjan suoritetaan.


### <a name="azure-classic-portal-configuration"></a>Azure perinteinen portaalin määrittäminen

1. Julkaise sovelluksesi [Julkaise sovellusten sovelluksen välityspalvelimen](active-directory-application-proxy-publish.md)kuvattu ohjeiden mukaisesti. Varmista, että valitset **Azure Active Directory** **Preauthentication-menetelmää**.
2. Kun sovellus näkyy sovellusten luettelossa, valitse se ja valitse **Määritä**.
3. Valitse **Ominaisuudet**-arvoksi **Sisäinen todentamismenetelmän** **Windows-todennusta**.  
  ![Sovelluksen lisäasetusten määrittäminen](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  
4. Kirjoita sovelluspalvelin **Sisäinen sovelluksen SPN** . Tässä esimerkissä Microsoftin julkaistu sovellus ‑objektin on http/lob.contoso.com.  

>[AZURE.IMPORTANT] Paikallisen UPN ja Azure Active Directoryn UPN eivät ole samat, jos haluat määrittää, jotta voit käyttää esitarkistusta [valtuutetun kirjautumisen tunnistetiedot](#delegated-login-identity) .

|  |  |
| --- | --- |
| Sisäinen todentamismenetelmä | Jos käytät Azure AD esitarkistusta, voit määrittää sisäinen todentamismenetelmän käyttäjät hyötyä tämän sovelluksen kertakirjautuminen (SSO). <br><br> Valitse **Windows-todennusta** (IWA), jos sovellus käyttää IWA ja voit määrittää Kerberos Rajoitettu delegointi (KCD) käyttöön SSO tämän sovelluksen. IWA käyttävät sovellukset on oltava määritettynä KCD, muuten sovelluksen välityspalvelimen voi julkaista nämä sovellukset. <br><br> Valitse **ei mitään** , jos sovellus ei käytä IWA. |
| Sisäinen sovelluksen SPN | Tämä on paikallinen Azure AD määritetty Service-lyhennys-Name (SPN) sisäinen sovelluksen. Palvelun Päänimen käytetään sovelluksen välityspalvelimen yhdistin hakeaksesi Kerberos-sovelluksen käyttäminen KCD tunnukset. |


## <a name="sso-for-non-windows-apps"></a>-Windows sovellusten SSO
Azure AD-sovelluksen välityspalvelimen Kerberos delegointi-työnkulku käynnistyy, kun Azure AD todentaa käyttäjän pilveen. Kun kutsu on saapunut paikallisen, Azure AD-sovelluksen välityspalvelimen yhdistimen ongelmat mukaan käyttäminen paikallisen Active Directory Kerberos-lippu käyttäjän puolesta. Tätä prosessia kutsutaan nimellä Kerberos Rajoitettu delegointi (KCD). Seuraava vaihe pyyntö lähetetään Kerberos-lippu Taustajärjestelmä-sovellukseen. On useita protokollia, joka määrittää, miten voit lähettää pyynnöt. Useimmat-Windows palvelimet odottavat neuvottelu/SPNego, joka tukee nyt Azure AD-sovelluksen välityspalvelimen.

![-Windows SSO-kaavio](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### <a name="delegated-login-identity"></a>Valtuutetun kirjautumisen tunnistetiedot
Valtuutetun kirjautumisen tunnistetiedot avulla voit käsitellä kaksi eri kirjautuminen skenaariota:

- -Windows sovellusten, saat yleensä käyttäjätietoja, käyttäjänimi tai SAM tilin nimi ei sähköpostiosoite muodossa (username@domain).
- Vaihtoehtoinen kirjautuminen käyttömahdollisuudet missä UPN Azure AD- ja paikallisen Active Directoryssa UPN erot.

Sovelluksen välityspalvelimen voit valita mitkä tunnistetietojen avulla voit hankkia Kerberos-lippu. Tämä asetus on sovellusta kohden. Jotkin näistä asetuksista soveltuvat järjestelmiin, Hyväksy sähköpostin osoitemuodon, muiden käsitellään vaihtoehtoinen kirjautuminen.

![Valtuutetun kirjautumisen tunnistetiedot parametrin näyttökuva](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Jos käytössä on valtuutetun kirjautumisen tunnistetiedot, arvo välttämättä yksilöivä toimialueiden tai organisaation metsien. Voit välttää ongelman julkaisemalla nämä sovellukset, kaksi eri Connector-ryhmien käyttäminen kahdesti. Koska kunkin sovelluksen on toisen käyttäjän käyttäjäryhmän, voit liittyä sen yhdistimet toista toimialuetta käyttävään muotoon.


## <a name="working-with-sso-when-on-premises-and-cloud-identities-are-not-identical"></a>SSO käsitteleminen kun paikallisen ja cloud käyttäjätietojen eivät vastaa toisiaan
Ellei muuten määritetty, sovelluksen välityspalvelimen oletetaan, että käyttäjillä on saman tunnistetietoja cloud ja paikallisen. Voit määrittää, kunkin sovelluksen, mikä käyttäjätieto käytetään, kun kertakirjautumisen suorittamiseen.  

Tämän ominaisuuden avulla Monet organisaatiot, joissa on eri paikallinen ja cloud käyttäjätietojen on SSO pilveen paikallinen-sovellukset ilman pakollista käyttäjien eri käyttäjänimet ja salasanat. Tämä koskee organisaatioita:

- On useita toimialueita sisäisesti (joe@us.contoso.com, joe@eu.contoso.com) ja yhden toimialueen pilveen (joe@contoso.com).

- Ole-reititettävä toimialuenimeä sisäisesti (joe@contoso.usa) ja juridiset yksi pilveen.

- Älä käytä toimialuenimien sisäisesti (Esko)

- Käytä eri aliakset paikallinen ja pilveen. Esimerkiksi joe-johns@contoso.comja.joej@contoso.com  

Sen avulla sovellukset, jotka Hyväksy sähköpostiosoite, joka on hyvin käytetty vaihtoehto-Windows taustatietokannan palvelinten lomakkeen osoitteet.


### <a name="setting-sso-for-different-cloud-and-on-prem-identities"></a>Eri cloud ja paikallinen käyttäjätietojen SSO määrittäminen

1. Azure AD Connect asetusten määrittäminen, jotta tärkeimmät tunnistetiedot on sähköpostiosoite (sähköposti). Mukauta-yhteydessä tehdään muuttamalla synkronointiasetuksia **Tarpeen käyttäjänimi** -kenttään. Huomaa, että nämä asetukset määrittävät myös siitä, miten käyttäjät Kirjaudu sisään Office 365-Windows10 laitteet ja muiden sovellusten, joka käyttää Azure AD niiden identity-kaupasta.  
  ![Käyttäjien näyttökuva - käyttäjätunnus avattava tunnistamiseen](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Sovelluksen sovelluksen määritys asetuksia haluat muuttaa, valitse käytettävä **Valtuutetun kirjautumisen tunnistetiedot** :
  - Tarpeen käyttäjänimi:joe@contoso.com  
  - Vaihtoehtoiset tarpeen käyttäjänimi:joed@contoso.local  
  - Käyttäjänimi osa tarpeen käyttäjänimi: Esko  
  - Vaihtoehtoinen tarpeen käyttäjänimi käyttäjänimi osa: joed  
  - Paikallisen SAM tilin nimi: vastaavassa paikallinen toimialueen ohjauskoneen määritys

  ![Valtuutetun kirjautumisen tunnistetiedot avattavasta valikosta näyttökuva](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)  

### <a name="troubleshooting-sso-for-different-identities"></a>Eri käyttäjätiedot SSO vianmääritys
Jos ilmenee virhe SSO-prosessin se näkyy yhdistimen koneen tapahtumalokiin [vianmääritys](active-directory-application-proxy-troubleshoot.md)-artikkelissa kuvatulla tavalla.
Mutta joissakin tapauksissa pyynnön onnistuneesti lähetetään Taustajärjestelmä sovelluksen samalla, kun tämän sovelluksen vastaa eri HTTP-vastauksiin. Vianmääritys tällaisissa tapauksissa kannattaa aloittaa tutkimalla tapahtuman numeron 24029 sovelluksen välityspalvelimen istunnon tapahtumalokiin Connector-laitteeseen. Käyttäjätietojen delegointi on käytetty näkyvät "käyttäjä-kentän tapahtuman tietoja. Istunnon loki käyttöön Valitse **Näytä Analyyttisten ja virheenkorjaus lokit** tapahtuma viewer Näytä-valikko.


## <a name="see-also"></a>Katso myös

- [Sovelluksen välityspalvelimen sovellusten julkaiseminen](active-directory-application-proxy-publish.md)
- [Ilmenevien ongelmien sovelluksen välityspalvelimen ongelmien vianmääritys](active-directory-application-proxy-troubleshoot.md)
- [Saatavat huomioon sovellusten käyttäminen](active-directory-application-proxy-claims-aware-apps.md)
- [Ehdollinen käytön käyttöön ottaminen](active-directory-application-proxy-conditional-access.md)

Uusimmat uutiset ja päivitykset Tutustu [sovelluksen välityspalvelin-blogi](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg
