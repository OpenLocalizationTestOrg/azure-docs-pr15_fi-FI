<properties
    pageTitle="Lisää uusi Azure pino-asiakasympäristön Azure Active Directoryn | Microsoft Azure"
    description="Kun otat Microsoft Azure pinon Käsitteiden, sinun on vähintään yksi vuokraajan käyttäjätilin luominen, niin voit tutkia vuokraajan portaalin."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Lisää uusi Azure pinon asiakasympäristön Azure Active Directory

Kun [käyttöönotto Azure pinon Käsitteiden](azure-stack-run-powershell-script.md), sinun on vuokraajan käyttäjätilin, jotta voit tutkia vuokraajan portaalin ja testata tarjouksia ja suunnitelmat. Voit luoda vuokraajan tilin [Azure-portaalissa](#create-an-azure-stack-tenant-account-using-the-azure-portal) tai [PowerShell](#create-an-azure-stack-tenant-account-using-powershell)-toiminnolla.

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Azure-portaalissa Azure pinon vuokraajan tilin luominen

Tarvitset Azure tilauksen Azure-portaalin käyttäminen.

1. [Azure](http://manage.windowsazure.com)kirjautuminen.

2.  Valitse Microsoft Azure vasemman reunan siirtymispalkissa **Active Directorysta**.

3.  Hakemisto-luettelosta kansio, jota haluat käyttää Azure pino tai luoda uuden.

4.  Valitse kansio tällä sivulla **käyttäjiä**.

5.  Valitse **Lisää käyttäjä**.

6.  Valitse **Lisää käyttäjä** ohjatun toiminnon **käyttäjän tyyppi** -luettelosta **uuden käyttäjän organisaatiossa**.

7.  Kirjoita käyttäjän nimi **käyttäjänimi** -ruutuun.

8.  Valitse **@** ruutuun ja valitse sitten oikea nimi.

9.  Napsauta Seuraava-nuolta.

10.  Kirjoita ohjatun toiminnon **käyttäjäprofiili** -sivulla **Etunimi**, **Sukunimi**ja **Näyttönimi**.

11. Valitse **rooli** -luettelosta **käyttäjä**.

12. Napsauta Seuraava-nuolta.

13. **Hae tilapäinen salasana** -sivulla Valitse **Luo**.

14. Kopioi **Uusi salasana**.

15. Kirjaudu sisään Microsoft Azure uudella tilillä. Vaihda salasana pyydettäessä.

16. Kirjaudu sisään `https://portal.azurestack.local` Nähdäksesi vuokraajan portaalin uudella tilillä.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>PowerShellin Azure pinon vuokraajan tilin luominen

Jos sinulla ei ole Azure tilauksen, voi vuokraajan käyttäjätilin lisääminen Azure portaalin avulla. Tässä tapauksessa Active Directory moduulin Windows PowerShellin Azure voit käyttää sen sijaan.

> [AZURE.NOTE] Jos käytät Microsoft Account (Live ID) Azure-pino käsitteiden, ei voi luoda asiakasympäristön AAD PowerShellin avulla. 

1.  Asenna [Microsoft Online Services-palveluiden kirjautumisavustaja IT-ammattilaisille RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).

2.  Asenna [Active Directory moduulin Windows PowerShellin Azure (64 - bittinen)](http://go.microsoft.com/fwlink/p/?linkid=236297) ja avaa se.

3.  Suorita seuraavat cmdlet-komennot:




    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack PoC
   
            $msolcred = get-credential
    
    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".
    
            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId
    
    ```

4.  Kirjaudu Microsoft Azure uudella tilillä. Vaihda salasana pyydettäessä.

5.  Kirjaudu `https://portal.azurestack.local` Nähdäksesi vuokraajan portaalin uudella tilillä.



