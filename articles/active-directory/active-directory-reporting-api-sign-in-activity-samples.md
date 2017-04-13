<properties
    pageTitle="Azure Active Directory-kirjautuminen toimintojen raportti API näytteiden | Microsoft Azure"
    description="Miten Azure Active Directory raportoinnin Ohjelmointirajapinnan käytön aloittaminen"
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

# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory-kirjautuminen tehtävän raportin API objektit

Tässä aiheessa on kokoelma raportoinnin API Azure Active Directory-aiheisia osa.  
Azure AD-raportointi voi API, jonka avulla voit käyttää kirjautumisen tehtävän tiedot-koodin tai Aiheeseen liittyvät työkalut.  
Tämän ohjeaiheen alue on tarjota otoksen koodi **- Kirjautuminen tehtävän API**.

Lisätietoja:

- [Valvontalokien](active-directory-reporting-azure-portal.md#audit-logs) käsitteellisiä lisätietoja
- Lisätietoja raportoinnin Ohjelmointirajapinnan [Azure Active Directory raportoinnin Ohjelmointirajapinnan käytön aloittaminen](active-directory-reporting-api-getting-started.md) .

Kysymyksiä ongelmista ja palautetta, ota yhteyttä [AAD raportointi auttaa](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Edellytykset
Ennen kuin voit käyttää malleja tämän ohjeaiheen, sinun täytyy suorittaa [edellytykset käyttää raportoinnin API Azure AD](active-directory-reporting-api-prerequisites.md).  


##<a name="powershell-script"></a>PowerShell-komentosarjaa

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
    
    $i=0
    
    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {
    
        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Komentosarjan suorittaminen
Kun Lopeta muokkaaminen komentosarja, suorita se ja tarkista tarkastuksen odotettu tiedot kirjautuu raportin palautetaan.

Komentosarja palauttaa tulostus-kirjautuminen-raportin JSON-muodossa. Se luo myös `SigninActivities.json` tiedosto, jonka saman tuloksen. Voit kokeilla muokkaamalla komentosarja palauttamaan tietoja muiden raporttien ja kommentin ulos esitysmuodot, joita et tarvitse.



## <a name="next-steps"></a>Seuraavat vaiheet

- Haluatko mukauttaa tämän ohjeaiheen mallit? Tutustu [Azure Active Directory-kirjautuminen tehtävän API viittaus](active-directory-reporting-api-sign-in-activity-reference.md). 

- Jos haluat nähdä koko yleiskatsaus raportoinnin API Azure Active Directoryn avulla, katso [käytön aloittaminen Azure Active Directory raportoinnin API](active-directory-reporting-api-getting-started.md).

- Jos haluat lisätietoja Azure Active Directory raportointi on [Azure Active Directory raportoinnin Guide](active-directory-reporting-guide.md).  