<properties
   pageTitle="Raportin ja Azure palvelun kangasta ja kuntotietojen tarkistaminen | Microsoft Azure"
   description="Opettele kuntoraportit lähettäminen palvelun-koodin ja tarkista palvelun kunto kunto valvontatyökaluja, joka sisältää Azure palvelun kangasta käyttämällä."
   services="service-fabric"
   documentationCenter=".net"
   authors="toddabel"
   manager="mfussell"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/06/2016"
   ms.author="toddabel"/>

# <a name="report-and-check-service-health"></a>Raportin ja tarkista palvelun kunto
Kun palvelujen ilmenee ongelmia, vastata ja korjata tapaukset ja katkokset määräytyy esiintyvien ongelmien nopeasti järjestelmänvalvoja. Jos ilmoitat ongelmista ja virheet kangasta Azure-palvelun kunto projektipäällikölle service-koodista, voit käyttää vakio, joka tarjoaa kangasta palvelun kunto-tilan tarkistaminen Valvontatyökalut kunto.

Voit raportoida palvelun kunto kahdella tavalla:

- [Osion](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.aspx) tai [CodePackageActivationContext](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.aspx) -objektien käyttäminen.  
Voit käyttää `Partition` ja `CodePackageActivationContext` objekteja ja raportoi kuntotietojen elementtejä, jotka ovat osa nykyisessä kontekstissa. Esimerkiksi koodi, joka suoritetaan osana replikan raportoida kunto vain replika ja osio, joka kuuluu sovellus, jossa se on osa.

- Käytä `FabricClient`.   
Voit käyttää `FabricClient` , raportin kunto palvelukoodi Jos klusterin ei ole [suojattu](service-fabric-cluster-security.md) tai jos palvelu on käynnissä järjestelmänvalvojan oikeuksilla. Tämä ei ole tosi useimmissa tosielämän tilanteissa. Kanssa `FabricClient`, voit raportoida kunto yritys, joka kuuluu klusterin. Ihannetapauksessa kuitenkin palvelukoodi pitäisi lähettää vain raportteja, jotka liittyvät omassa kunto.

Tässä artikkelissa esitellään esimerkki, joka ilmoittaa koodista palvelun kunto. Esimerkki näkyvät myös siitä, miten työkalut, joka sisältää palvelun kangasta voidaan kuntotietojen tarkistaminen. Tässä artikkelissa on tarkoitettu kunnon valvonta palvelun kangasta ominaisuuksia lyhyt esittely. Lisätietoja Lue perusteellisempaa kunto artikkeleita, jotka alkavat linkki on tämän artikkelin lopussa olevassa sarjaa.

## <a name="prerequisites"></a>Edellytykset
Sinulla on oltava asennettuna seuraavat:

   * Visual Studio 2015
   * Palvelun kangasta SDK-paketissa

## <a name="to-create-a-local-secure-dev-cluster"></a>Kun haluat luoda paikallisen suojatun keskihajonta klusterin
- Avaa PowerShell-järjestelmänvalvojan oikeuksilla ja suorittamalla seuraavat komennot.

![Komennot, jotka osoittavat, miten haluat luoda suojatun keskihajonta klusterin](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Sovelluksen ottaminen käyttöön ja sen kuntotietojen tarkistaminen

1. Avaa Visual Studio järjestelmänvalvojana.

2. Luo projekti **Tilallisen Service** -mallin avulla.

    ![Palvelun kangasta-sovelluksen luominen tilallisen-palvelun kanssa](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)

3. Painamalla **F5** sovelluksen käyttämiseen virheenkorjaus-tilassa. Sovellus otetaan käyttöön paikallisen klusterin.

4. Kun sovellus on käynnissä, ilmaisinalueella paikallisen klusterin hallinta-kuvaketta hiiren kakkospainikkeella ja valitse **Hallitse paikallisen klusterin** pikavalikon Avaa palvelun kangasta Resurssienhallinta.

    ![Avaa palvelu kangasta Explorer ilmaisinalueella](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)

5. Kuten kuvassa pitäisi näkyä sovelluksen kunto. Tällä hetkellä sovelluksen on oltava kunnossa virheettömät.

    ![Kunnossa sovelluksen kangasta Explorerissa](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)

6. Voit myös tarkistaa kuntotietojen PowerShell-toiminnolla. Voit käyttää ```Get-ServiceFabricApplicationHealth``` voit tarkistaa sovelluksen terveys- ja ```Get-ServiceFabricServiceHealth``` Tarkista palvelun kunto. Saman sovelluksen PowerShellin kuntoraportti on tässä kuvassa.

    ![Kunnossa PowerShell-sovelluksen](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Mukautetun kunto tapahtumien lisääminen service-koodi
Palvelun kangasta projektimallit Visual Studiossa sisältää otoksen koodia. Seuraavissa vaiheissa kuvataan siitä, miten voit raportoida mukautetun kunto tapahtumat service-koodista. Raporttien näkyy automaattisesti kunnon valvonta, että palvelun kangasta tarjoaa, kuten palvelun kangasta Explorer, Azure portaalin kunto-näkymä ja PowerShell vakio työkaluja.

1. Avaamalla sovellus, jonka loit aiemmin Visual Studiossa, tai Luo uusi sovellus **Tilallisen palvelun** Visual Studio-mallin avulla.

2. Avaa Stateful1.cs-tiedosto ja Etsi `myDictionary.TryGetValueAsync` Soita `RunAsync` menetelmää. Näet, että tämä menetelmä palauttaa `result` , pitää nykyinen arvo laskuri, koska tämän sovelluksen tärkeimmistä logiikkaa on estää laskeminen käynnissä. Tämä on todellinen sovelluksen ja tulos puutteen edustaa epäonnistui, haluat ehkä merkki kyseisen tapahtuman.

3. Ilmoittamaan kunto-tapahtuman, kun tulos puutteen edustaa epäonnistui Lisää seuraavien ohjeiden mukaisesti.

    a. Lisää `System.Fabric.Health` nimitilan Stateful1.cs-tiedostoon.

    ```csharp
    using System.Fabric.Health;
    ```

    b. Lisää seuraava koodi jälkeen `myDictionary.TryGetValueAsync` soittaminen

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    Olemme raportoida replikan kunto, koska se raportoidaan tilallisten-palvelusta. `HealthInformation` Parametrin tallentaa kunto ongelman, joka raportoidaan tietoja.

    Jos olit luonut tilattomien palvelu, käytä seuraava koodi

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```

4. Jos palvelun on käynnissä järjestelmänvalvojan oikeuksilla tai jos klusterin ei ole [suojattu](service-fabric-cluster-security.md), voit myös käyttää `FabricClient` raportin terveyden seuraavat vaiheet esitetyllä tavalla.  

    a. Luo `FabricClient` esiintymän jälkeen `var myDictionary` ilmoitus.

    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```

    b. Lisää seuraava koodi jälkeen `myDictionary.TryGetValueAsync` Soita.

    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.ServiceInitializationParameters.PartitionId,
            this.ServiceInitializationParameters.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```

5. Oletetaan, että simuloida tämän virheen, ja se kunto valvontatyökaluja näkyvät. Voit simuloida virheen kommentteja, raportointi tunnus, jolla aiemmin lisäämäsi kunto ensimmäisellä rivillä. Kun kommentti ensimmäisen rivin pois, koodi näyttää seuraavalta.

    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
 Koodi nyt Kyselysäännön tämän kuntoraportti aina, kun `RunAsync` suorittaa. Kun olet tehnyt muutoksen, painamalla **F5** sovelluksen käyttämiseen.

6. Kun sovellus on käynnissä, avaa palvelun kangasta Explorer sovelluksen kuntotietojen tarkistus. Tällä hetkellä palvelun kangasta Explorer näkyy, että sovellus on perusasemassa. Tämä on virhe, joka ilmoitettiin koodi on lisätty aiemmin vuoksi.

    ![Perusasemassa sovelluksen kangasta Explorerissa](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)

7. Jos valitset ensisijainen se palvelun kangasta Explorerin puunäkymässä, näet, että **Kuntotietojen** osoittaa virheen, liian. Palvelun kangasta Explorer näyttää myös kunto raportin tiedot on lisätty `HealthInformation` -koodiin parametri. Voit tarkastella saman kuntoraportit PowerShell ja Azure portaalin.

    ![Replikan kunto kangasta Explorerissa](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Tämä raportti säilyy kunto hallintaan, kunnes se korvataan toisen raportin tai kunnes tämän replikan poistetaan. Koska emme ole määritetty `TimeToLive` kunto tämän raportin `HealthInformation` objekti, raporttia ei koskaan päättyy.

On suositeltavaa, että kunto raportoinut eniten hajautetun tasolla, eli tässä tapauksessa se. Voit myös raportoida kunto `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Raportin terveyden `Application`, `DeployedApplication`, ja `DeployedServicePackage`, käytä `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Seuraavat vaiheet
[Perinpohjaisesti käsittelevään artikkeliin palvelun kangasta terveyteen](service-fabric-health-introduction.md)
