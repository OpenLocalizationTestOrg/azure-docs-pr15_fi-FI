<properties
    pageTitle="Luo ei ole vuorovaikutteinen käyttöoikeuksien Hdinsightiin .NET saaneilta | Microsoft Azure"
    description="Opettele luomaan ei ole vuorovaikutteinen todennus .NET HDInsight-sovellukset."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Ei ole vuorovaikutteinen todennus .NET HDInsight-sovellusten luominen

Voit suorittaa .NET Azure HDInsight-sovellus, valitse sovelluksen omat tunnistetiedot (ei ole vuorovaikutteinen) tai (vuorovaikutteinen)-sovelluksen kirjautunut sisään käyttäjän tunnistetiedot-kohdassa. Katso vuorovaikutteinen sovelluksen otoksen [Lähetä rakenne/Possu/Sqoop työt HDInsight .NET SDK: N avulla](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). Tässä artikkelissa näytetään, miten voit luoda ei ole vuorovaikutteinen todennus .NET-sovelluksen yhteyden muodostaminen Azure Hdinsightiin ja lähetät rakenteen työn.

.NET-ohjelmasta tarvitset:

- Azure tilauksen vuokraajan-tunnus
- Azure-hakemisto-sovelluksen Asiakastunnus
- Azure Directory sovelluksen salausavaimen.  

Tärkeimmät sisältää seuraavat toimet:

2. Azure-hakemisto-sovelluksen luominen.
2. Määrittää roolit AD-sovellukseen.
3. Kehittää asiakassovellus.


##<a name="prerequisites"></a>Edellytykset

- HDInsight-klusterin. Voit luoda sen löytyvät [Aloitusopas](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster)annettujen ohjeiden mukaisesti. 




## <a name="create-azure-directory-application"></a>Azure-hakemisto-sovelluksen luominen 
Kun luot Active Directory-sovelluksen, todella luomilleen sovelluksen ja palvelun pääasiallista. Voit suorittaa sovelluksen sovelluksen käyttäjätiedot-kohdassa.

Tällä hetkellä sinun on käytettävä Azure perinteinen portaalin voit luoda uuden Active Directory-sovelluksen. Tämän ominaisuuden lisätään myöhemmällä Azure-portaaliin. Voit suorittaa nämä vaiheet PowerShellin Azure tai Azure CLI. Saat lisätietoja käyttämällä PowerShell tai CLI principal-palveluun, [Tarkista palvelun pääasiallista Azure resurssien hallinta](../resource-group-authenticate-service-principal.md).

**Azure-hakemisto-sovelluksen luominen**

1.  Kirjautuminen [Azure perinteinen portal]( https://manage.windowsazure.com/).
2.  Valitse vasemmanpuoleisessa ruudussa **Active Directorysta** .

    ![Azure perinteinen portaalin active Directoryyn](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\active-directory.png)
    
3.  Valitse kansio, johon haluat luoda uuden sovelluksen. Sen on oltava nykyisen tiedoston.
4.  Valitse **sovellusten** aiemmin sovellusten luettelon yläpuolella.
5.  Valitse **Lisää** Lisää uusi sovellus.
6.  Kirjoita **nimi**, valitse **Web-sovelluksen ja/tai verkko-Ohjelmointirajapinnan**ja valitse sitten **Seuraava**.

    ![Uusi azure active directory-sovellus](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application.png)

7.  Kirjoita **Sign URL** ja **Sovelluksen tunnus URI**. **KIRJAUDU edelleen URL-osoite**on web-sivusto, joka kuvaa sovelluksen URI. Web-sivuston olemassaolo ei ole vahvistettu. Säätää, joka määrittää sovelluksen URI Sovelluksen tunnus URI. Ja valitse sitten **Valmis**.
Kestää jonkin aikaa, voit luoda sovelluksen.  Kun sovellus on luotu, portaalin näyttää nopeasti Glace sivun uuden sovelluksen. Älä sulje portaalin. 

    ![azure active directory-sovelluksen uudet ominaisuudet](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application-properties.png)

**Saat sovelluksen asiakasohjelman tunnuksen ja salausavaimen**

1.  Valitse juuri luomasi AD sovelluksen sivun yläreunan valikosta **määrittäminen** .
2.  Tallenna kopio **Asiakastunnus**. Tarvitset sitä .NET-sovelluksessa.
3.  **Näppäimet** **Valitse kesto** avattava valikko ja valitse **1 vuosi** tai **2 vuoden**. Avainarvon ei näytetä, ennen kuin tallennat määritykset.
4.  Valitse sivun alareunassa **Tallenna** . Salausavaimen näkyessä avaimen kopion. Tarvitset sitä .NET-sovelluksessa.

##<a name="assign-ad-application-to-role"></a>AD-sovelluksen roolin määrittäminen

Määritä [rooli](../active-directory/role-based-access-built-in-roles.md) -sovelluksen avulla myöntää käyttöoikeuksia toimintojen tekemistä varten. Voit määrittää alueen tasolla, tilauksen, resurssiryhmä tai resurssi. Käyttöoikeudet periytyvät alemman tason vaikutusalueen (esimerkiksi lisäämällä sovelluksen resurssiryhmä lukija-rooliin tarkoittaa, että se voi lukea resurssiryhmän ja se sisältää resursseja). Tässä opetusohjelmassa vaikutusalue määritetään resurssin ryhmän tasolla.  Azure perinteinen portaalin tue resurssiryhmät, tässä osassa on suoritettava Azure-portaalista. 

**Voit lisätä omistajan rooli AD-sovellukseen**

1.  Kirjautuminen [Azure portal](https://portal.azure.com).
2.  Valitse vasemmanpuoleisessa ruudussa **Resurssiryhmä** .
3.  Valitse resurssiryhmä, joka sisältää missä rakenteen kyselyn suoritat jäljempänä tässä opetusohjelmassa-HDInsight-klusterin. Jos määritettynä on liian monta resurssin ryhmää, voit käyttää suodatinta.
4.  Valitse **Access** -klusterin-sivu.

    ![cloud ja thunderbolt kuvaketta = pikaopas](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5.  Valitse **Lisää** **käyttäjiä** -sivu.
6.  Noudattamalla **omistajan** rooli lisääminen AD-sovellukseen edellisen prosessin vaiheessa luomasi. Kun suoritat se onnistuu, näet on lueteltu käyttäjät-sivu, jonka rooli on omistaja sovelluksen.


##<a name="develop-hdinsight-client-application"></a>Kehittää HDInsight-asiakassovellukseen.

Luo C# .net console-sovellus, joka [lähettää Hadoop työt HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk)-kohdan ohjeiden mukaisesti. Korvaa GetTokenCloudCredentials menetelmä seuraavasti:

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

Voit hakea vuokraajan tunnuksen PowerShellin kautta:

    Get-AzureRmSubscription

Voit myös Azure CLI:

    azure account show --json

      
## <a name="see-also"></a>Katso myös

- [Lähetä Hadoop töiden Hdinsightiin](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Luoda Active Directory-sovelluksen ja palvelun lyhennys-portaalissa](../resource-group-create-service-principal-portal.md)
- [Todentaa palvelun lyhennys Azure resurssien hallinta](../resource-group-authenticate-service-principal.md)
- [Azure Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md)
