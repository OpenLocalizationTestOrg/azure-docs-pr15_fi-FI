<properties
    pageTitle="Azure Active Directory-valvonta API viittaus | Microsoft Azure"
    description="Miten Azure Active Directory-valvonta-Ohjelmointirajapinnan käytön aloittaminen"
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
    ms.date="10/24/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory-valvonta API-viittaus

Tässä aiheessa on kokoelma raportoinnin API Azure Active Directory-aiheisia osa.  
Azure AD-raportointi voi API, jonka avulla voit käyttää valvontatietojen koodin tai Aiheeseen liittyvät työkalut.
Tämän ohjeaiheen alue on tarjota yksityiskohtaisia tietoja **API valvonta**.

Lisätietoja:

- [Valvontalokien](active-directory-reporting-azure-portal.md#audit-logs) käsitteellisiä lisätietoja
- Lisätietoja raportoinnin Ohjelmointirajapinnan [Azure Active Directory raportoinnin Ohjelmointirajapinnan käytön aloittaminen](active-directory-reporting-api-getting-started.md) .

Kysymyksiä ongelmista ja palautetta, ota yhteyttä [AAD raportointi auttaa](mailto:aadreportinghelp@microsoft.com).


## <a name="who-can-access-the-data"></a>Kuka voi käyttää tietoja?

- Suojauksen järjestelmänvalvojan tai suojauksen lukija-roolin käyttäjät

- Yleiset Järjestelmänvalvojat

- Sovellusta, joka on lupa käyttää Ohjelmointirajapinnan (sovelluksen luvan voi asetukset, vain perusteella yleisen järjestelmänvalvojan oikeudet)



## <a name="prerequisites"></a>Edellytykset

Jotta voit käyttää raportti – raportointi-Ohjelmointirajapinta, sinulla on oltava:

- Aiemmin [Azure Active Directory vapaa- tai paremmin edition](active-directory-editions.md)

- Valmis [käyttämään raportoinnin API Azure AD edellytykset](active-directory-reporting-api-prerequisites.md). 
 

##<a name="accessing-the-api"></a>Ohjelmointirajapinnan käyttäminen

Voit joko käyttää tätä API [Graph Explorer](https://graphexplorer2.cloudapp.net) tai ohjelmallisesti käyttämällä esimerkiksi PowerShell. PowerShell tulkita oikein käytetyn AAD Graph REST-puheluissa OData suodattimen syntaksin tilaus, sinun on käytettävä backtick (eli: gravis) merkin "pääse" $-merkki. Backtick merkki on [PowerShell's ohjausmerkkiä](https://technet.microsoft.com/library/hh847755.aspx)salliminen PowerShell literal tulkintaan $ merkin, ja välttää helpon PowerShell muuttujan nimenä (ie: $filter).

Tämän ohjeaiheen kohdistus on kaavion Resurssienhallinta. Lisätietoja PowerShell-esimerkki [PowerShell-komentosarjaa](active-directory-reporting-api-audit-samples.md#powershell-script).

 
 

## <a name="api-endpoint"></a>API päätepiste


Voit käyttää tätä Ohjelmointirajapinnan käyttäminen seuraavat URI:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Ei ole rajoitettu Azure AD-valvonta API (joko OData sivutus) palauttamien tietueiden määrän.
Säilytys raportointitiedot rajoituksia Tutustu [Reporting säilytyskäytäntöjä](active-directory-reporting-retention.md).

Puhelun palauttaa tiedot erissä. Jokainen erä on enintään 1 000 tietuetta.  
Saat tietueiden seuraava erä käyttämällä seuraavan linkin. Pyydä palautetut tietueet ensimmäisiin skiptoken tiedot. Ohita tunnuksen tulos lopussa asetetaan.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Tuetut suodattimet

Voit rajata Ohjelmointirajapinnan palautettavien tietueiden määrän soittaa lomakkeen arvoja.  
Kirjaudu sisään API liittyviä tietoja, seuraavat suodattimet tuetaan:

- **$top =\<määrä tietueita palautetaan\> ** – voit rajoittaa palautettujen tietueiden määrän. Tämä on kallista toiminnon. Älä käytä suodatin, jos haluat palata tuhansia objekteja.     
- **$filter =\<suodatin-lausekkeen\> ** – Määritä tuettu suodatinkenttien perusteella tärkeisiin tietueiden tyyppi.



## <a name="supported-filter-fields-and-operators"></a>Tuetut suodatinkenttien ja operaattorit.

Voit määrittää tietueiden tärkeisiin, voit luoda suodatin-lausekkeen, joka voi olla yksi tai yhdistelmän suodatus-kentät:

- [aktiviteetin päivämäärää](#activitydate) - määrittää päivämäärän tai päivämäärävälin
- [activityType](#activitytype) - määrittää tehtävän tyyppi
- [tehtävä](#activity) - määrittää tehtävän merkkijono  
- [toimija/nimi](#actorname) - määrittää toimija toimija nimen muoto
- [toimija tai objektitunnus](#actorobjectid) - määrittää toimija toimija tunnus-lomakkeessa   
- [toimija/upn](#actorupn) - määrittää toimija toimija tarpeen käyttäjätunnus (UPN) lomakkeessa 
- [kohdenimi /](#targetname) - määrittää kohde toimija nimen muoto
- [kohteen tai objektitunnus](#targetobjectid) - määrittää kohde lomakkeessa kohteen tunnus  
- [kohteen/upn](#targetupn) - määrittää toimija toimija tarpeen käyttäjätunnus (UPN) lomakkeessa   




----------

### <a name="activitydate"></a>Aktiviteetin päivämäärää

**Tuetut operaattorit**: eq, sivu-tiedostoon, gt, lt

**Esimerkki**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago    

**Huomautuksia**:

DATETIME pitäisi olla UTC-muodossa

----------

### <a name="activitytype"></a>activityType

**Tuetut operaattorit**: eq

**Esimerkki**:

    $filter=activityType eq 'User'  

**Huomautuksia**:

kirjainkoon huomioon

----------

### <a name="activity"></a>toiminta

**Tuetut operaattorit**: eq, sisältää, startsWith

**Esimerkki**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')   

**Huomautuksia**:

kirjainkoon huomioon

----------

### <a name="actorname"></a>toimija/nimi

**Tuetut operaattorit**: eq, sisältää, startsWith

**Esimerkki**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')  

**Huomautuksia**:

asennustavan

    

----------
### <a name="actorobjectid"></a>toimija tai objektitunnus

**Tuetut operaattorit**: eq

**Esimerkki**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    

----------
### <a name="targetname"></a>Kohdenimi /

**Tuetut operaattorit**: eq, sisältää, startsWith

**Esimerkki**:

    $filter=targets/any(t: t/name eq 'some name')   

**Huomautuksia**:

Asennustavan

----------

### <a name="targetupn"></a>kohteen/upn

**Tuetut operaattorit**: eq, startsWith

**Esimerkki**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc')) 

**Huomautuksia**:

- Asennustavan
- Sinun on lisättävä koko nimitilan, kun kysely suoritetaan Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity

----------

### <a name="targetobjectid"></a>kohteen tai objektitunnus

**Tuetut operaattorit**: eq

**Esimerkki**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

----------

### <a name="actorupn"></a>toimija/upn

**Tuetut operaattorit**: eq, startsWith

**Esimerkki**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')  

**Huomautuksia**:

- Asennustavan 
- Sinun on lisättävä koko nimitilan, kun kysely suoritetaan Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity

----------




## <a name="next-steps"></a>Seuraavat vaiheet

- Haluatko esimerkkejä on suodatettu järjestelmän toimintoja? Tutustu [Azure Active Directory valvonta API objektit](active-directory-reporting-api-audit-samples.md).

- Haluatko lisätietoja Azure AD raportoinnin API? Katso [Azure Active Directory raportoinnin Ohjelmointirajapinnan käytön aloittaminen](active-directory-reporting-api-getting-started.md).