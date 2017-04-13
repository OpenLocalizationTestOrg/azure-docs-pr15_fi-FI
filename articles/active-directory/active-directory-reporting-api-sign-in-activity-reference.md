<properties
    pageTitle="Azure Active Directory-kirjautuminen toimintojen raportti API viittaus | Microsoft Azure"
    description="Azure Active Directory-kirjautuminen tehtävän raportin API-viittaus"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory-kirjautuminen toimintojen raportti API-viittaus


Tässä aiheessa on kokoelma raportoinnin API Azure Active Directory-aiheisia osa.  
Azure AD-raportointi voi API, jonka avulla voit käyttää kirjautumisen tehtävän tiedot-koodin tai Aiheeseen liittyvät työkalut.
Tässä ohjeaiheessa on antaa sinulle lisätietoja **Kirjautuminen tehtävän raportin API**.

Lisätietoja:

- [Kirjaudu sisään toimintoja](active-directory-reporting-azure-portal.md#sign-in-activities) käsitteellisiä lisätietoja
- Lisätietoja raportoinnin Ohjelmointirajapinnan [Azure Active Directory raportoinnin Ohjelmointirajapinnan käytön aloittaminen](active-directory-reporting-api-getting-started.md) .

Kysymyksiä ongelmista ja palautetta, ota yhteyttä [AAD raportointi auttaa](mailto:aadreportinghelp@microsoft.com).



## <a name="who-can-access-the-api-data"></a>Kuka voi käyttää API tiedot?

- Suojauksen järjestelmänvalvojan tai suojauksen lukija-roolin käyttäjät

- Yleiset Järjestelmänvalvojat

- Sovellusta, joka on lupa käyttää Ohjelmointirajapinnan (sovelluksen luvan voi asetukset, vain perusteella yleisen järjestelmänvalvojan oikeudet)



## <a name="prerequisites"></a>Edellytykset

Käyttämään tämän raportin kautta raportoinnin Ohjelmointirajapinnan on oltava:

- Aiemmin [Azure Active Directory Premium P1 tai P2 edition](active-directory-editions.md)

- Valmis [käyttämään raportoinnin API Azure AD edellytykset](active-directory-reporting-api-prerequisites.md). 


##<a name="accessing-the-api"></a>Ohjelmointirajapinnan käyttäminen

Voit joko käyttää tätä API [Graph Explorer](https://graphexplorer2.cloudapp.net) tai ohjelmallisesti käyttämällä esimerkiksi PowerShell. PowerShell tulkita oikein käytetyn AAD Graph REST-puheluissa OData suodattimen syntaksin tilaus, sinun on käytettävä backtick (eli: gravis) merkin "pääse" $-merkki. Backtick merkki on [PowerShell's ohjausmerkkiä](https://technet.microsoft.com/library/hh847755.aspx)salliminen PowerShell tekemään literal tulkitseminen $ merkin ja välttää helpon PowerShell muuttujan nimenä (ie: $filter).

Tämän ohjeaiheen kohdistus on kaavion Resurssienhallinta. Lisätietoja PowerShell-esimerkki [PowerShell-komentosarjaa](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).


## <a name="api-endpoint"></a>API päätepiste

Voit käyttää tätä Ohjelmointirajapinnan käyttäminen seuraavat perus-URI:  
    
    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Tietoja on paljon vuoksi tämä API palautetut tietueet miljoonan enimmäismäärä on. 

Puhelun palauttaa tiedot erissä. Jokainen erä on enintään 1 000 tietuetta.  
Saat tietueiden seuraava erä käyttämällä seuraavan linkin. Pyydä palautetut tietueet ensimmäisiin [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) tiedot. Ohita tunnuksen tulos lopussa asetetaan.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Tuetut suodattimet

Voit rajata Ohjelmointirajapinnan palautettavien tietueiden määrän soittaa lomakkeen arvoja.  
Kirjaudu sisään API liittyviä tietoja, seuraavat suodattimet tuetaan:

- **$top =\<määrä tietueita palautetaan\> ** – voit rajoittaa palautettujen tietueiden määrän. Tämä on kallista toiminnon. Älä käytä suodatin, jos haluat palata tuhansia objekteja.  
- **$filter =\<suodatin-lausekkeen\> ** – Määritä tuettu suodatinkenttien perusteella tärkeisiin tietueiden tyyppi.



## <a name="supported-filter-fields-and-operators"></a>Tuetut suodatinkenttien ja operaattorit.

Voit määrittää tietueiden tärkeisiin, voit luoda suodatin-lausekkeen, joka voi olla yksi tai yhdistelmän suodatus-kentät:

- [signinDateTime](#signindatetime) - määrittää päivämäärän tai päivämäärävälin

- [käyttäjätunnus](#userid) - määrittää tietyn käyttäjän perusteella käyttäjätunnusta.

- [userPrincipalName](#userprincipalname) - määrittää tietyn käyttäjän perusteella käyttäjän täydellinen käyttäjätunnus (UPN)

- [sovelluksen tunnus](#appid) - määrittää tietyn sovelluksen perusteella sovelluksen tunnus

- [appDisplayName](#appdisplayname) - määrittää tietyn sovelluksen perusteella sovelluksen näyttönimi

- [loginStatus](#loginStatus) - määrittää kirjautumiset tilan (success / virhe)


> [AZURE.NOTE] Kun Graph Resurssienhallinnan avulla, sinun ei tarvitse käyttää kirjainkoko jokaiseen viestiin on suodatus-kentät.


Rajataksesi laajuus palautetut tiedot voit luoda tuetut suodattimet ja suodatinkenttien yhdistelmät. Esimerkiksi seuraava lause palauttaa Ylin 10 tietuetta 1st heinäkuussa 2016 ja 6 heinäkuussa 2016:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


----------

### <a name="signindatetime"></a>signinDateTime

**Tuetut operaattorit**: eq, sivu-tiedostoon, gt, lt

**Esimerkki**:

Tietyn päivämäärän käyttäminen

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z  



Päivämääräalueen avulla    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Huomautuksia**:

Datetime-parametri on oltava UTC-muodossa 


----------

### <a name="userid"></a>Käyttäjätunnus

**Tuetut operaattorit**: eq

**Esimerkki**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Huomautuksia**:

Käyttäjätunnus arvo on merkkijono



----------

### <a name="userprincipalname"></a>userPrincipalName

**Tuetut operaattorit**: eq

**Esimerkki**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Huomautuksia**:

UserPrincipalName arvo on merkkijono

----------

### <a name="appid"></a>sovelluksen tunnus

**Tuetut operaattorit**: eq

**Esimerkki**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Huomautuksia**:

Sovelluksen tunnus arvo on merkkijono

----------


### <a name="appdisplayname"></a>appDisplayName

**Tuetut operaattorit**: eq

**Esimerkki**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Huomautuksia**:

AppDisplayName arvo on merkkijono

----------

### <a name="loginstatus"></a>loginStatus

**Tuetut operaattorit**: eq

**Esimerkki**:

    $filter=loginStatus+eq+'1'  


**Huomautuksia**:

Kahdella eri tavalla, loginStatus: 0 - success, 1 - virhe

----------



## <a name="next-steps"></a>Seuraavat vaiheet

- Haluatko esimerkkejä on suodatettu kirjautumisen toimintoja? Tutustu [Azure Active Directory-kirjautuminen tehtävän raportin API objektit](active-directory-reporting-api-sign-in-activity-samples.md).

- Haluatko lisätietoja Azure AD raportoinnin API? Katso [Azure Active Directory raportoinnin Ohjelmointirajapinnan käytön aloittaminen](active-directory-reporting-api-getting-started.md).