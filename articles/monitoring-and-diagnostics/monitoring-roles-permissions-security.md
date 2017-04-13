<properties
    pageTitle="Aloita roolit ja käyttöoikeudet arvopaperille, jonka Azure näytön | Microsoft Azure"
    description="Opettele käyttämään käytön rajoittaminen resurssien valvominen Azure-näytössä sisäinen rooleista ja käyttöoikeuksista."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Roolit ja käyttöoikeudet arvopaperille, jonka Azure näytön käytön aloittaminen

Useat työryhmät on ehdottoman sääteleviä käyttöoikeuksien valvonta tiedot ja asetukset. Esimerkiksi jos sinulla työryhmän jäseniä, jotka yksinomaan työstää seuranta (tukihenkilöt, devops engineers) tai jos käytät hallitun palveluntarjoaja, haluat ehkä antaa heille access-tietojen valvominen vain aikana rajoittaminen mahdollisuus luoda, muokata tai poista resursseja. Tämän artikkelin avulla voit nopeasti käyttää valmiita seurannan RBAC-roolin Azure-käyttäjän tai muodosta oma mukautettu rooli seurantaa rajoitetut käyttöoikeudet tarvitsevalle käyttäjälle. Tässä artikkelissa sitten Azure näytön liittyvät resurssit ja kuinka voit rajoittaa ne sisältävät tietoihin pääsy suojausta koskevat asiat.

## <a name="built-in-monitoring-roles"></a>Valmiin seurantaa roolit

Azure näytön valmiit roolit on suunniteltu rajoittaa resurssien otettaessa ne valvonnasta infrastruktuurin hankkiminen ja määrittäminen tarvitsemansa tiedot edelleen tilauksen käyttöoikeuksia. Azure valvonta sisältää kaksi ulos,-valmiilla rooleja: A seuranta Reader ja seuranta osallistujan.

### <a name="monitoring-reader"></a>Lukija seuranta

Jolle seuranta lukija voi tarkastella kaikkia seurantatiedot-tilauksen, mutta ei voi muokata mitä tahansa resurssia tai muokata resurssien valvominen liittyviä asetuksia. Tämä rooli ei sovi organisaation, kuten tuki tai toimintojen insinöörien, voivat käyttäville käyttäjille:

- Tarkastella seuranta-portaalissa ja luoda omia yksityinen seurantaa raporttinäkymiä.
- Kyselyn [Azure näytön REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx)sekä [PowerShell cmdlet-komentojen](insights-powershell-samples.md)ja [Office kaikissa ympäristöissä CLI](insights-cli-samples.md)arvot.
- Kyselyn portal, Azure näytön REST API, PowerShellin cmdlet-komennot tai Office kaikissa ympäristöissä CLI tehtävän loki.
- Näytä resurssin [Diagnostiikka-asetusten muuttaminen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) .
- Tilauksen [loki profiilin](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) tarkasteleminen.
- Näytä Automaattinen skaalaus-asetukset.
- Näytä ilmoitus toiminta ja asetukset.
- Hakemuksen tiedot tietoja ja tarkastella tietoja AI Analytics.
- Etsi lokin Analytics (OMS) työtilatiedot myös työtilan käyttötiedot.
- Lokitiedoston Analytics (OMS) hallinta ryhmien tarkasteleminen.
- Noutaa Log Analytics (OMS) hakurakenteen.
- Luettelon Log Analytics (OMS) Liiketoimintatieto-paketit.
- Hae ja suorita Log Analytics (OMS) tallennetut haut.
- Noutaa Log Analytics (OMS) tallennustilan kokoonpanon.

> [AZURE.NOTE] Tämä rooli ei anna lukuoikeudet tietoja, jotka on virtauttaa tapahtumaa-toiminnossa tai tallennettu tallennustilan tilin. [Noudata seuraavia ohjeita](#security-considerations-for-monitoring-data) näiden resurssien käyttöoikeuksien määrittämisestä.

### <a name="monitoring-contributor"></a>Osallistuja seuranta

Henkilöt, jotka on määritetty seuranta osallistujan rooli tarkastella kaikkien seurantatiedot tilauksen ja luoda tai muokata valvonta-asetusten, mutta ei voi muokata muita resursseja. Tällä roolilla on osa seuranta lukija ja vastaava jäsenet organisaation seurannan ryhmän tai hallitun palveluntarjoajien käyttäville, edellä käyttöoikeudet lisäksi myös voivat:

- Julkaise seurantaa raporttinäkymien jaetun Raporttinäkymät-ikkunan.
- Resource.* [Diagnostiikka-asetusten muuttaminen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings)
- Määritä subscription.* [lokin profiili](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles)
- Voit määrittää ilmoitusten tehtävän ja asetukset.
- Hakemuksen tiedot web testit ja -osien luominen
- Luettelon Log Analytics (OMS) Jaettu työtila-näppäimiä.
- Ota käyttöön tai poistaa käytöstä Log Analytics (OMS) liiketoimintatietojen Pack-paketit.
- Luominen, poistaminen ja suorita Log Analytics (OMS) tallennetut haut.
- Luoda ja poistaa lokin Analytics (OMS) tallennustilan kokoonpanon.

* käyttäjä on myös erikseen myönnettävä ListKeys oikeudet kohde resurssi (tallennustilan tilin tai tapahtuman keskittimeen nimitilan) lokin profiilin tai diagnostiikan asetus.

> [AZURE.NOTE] Tämä rooli ei anna lukuoikeudet tietoja, jotka on virtauttaa tapahtumaa-toiminnossa tai tallennettu tallennustilan tilin. [Noudata seuraavia ohjeita](#security-considerations-for-monitoring-data) näiden resurssien käyttöoikeuksien määrittämisestä.

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Käyttöoikeudet ja mukautettuja RBAC rooleja seuranta
Yllä valmiit roolit eivät sovellu ryhmän tarkka tarpeisiin, voit [luoda mukautettuja RBAC roolin](../active-directory/role-based-access-control-custom-roles.md) eritellympiä käyttöoikeuksilla. Seuraavassa on yleisiä Azure näytön RBAC toimintoja, joiden kuvaukset.

| Toiminto                                                   | Kuvaus                                                                                                                                               |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Microsoft.Insights/AlertRules/[Read, kirjoita, poista]         | Luku, kirjoittaminen ja poistaminen ilmoitusten säännöt.                                                                                                                            |
| Microsoft.Insights/AlertRules/Incidents/Read                | Luettele tapaukset (ilmoitusten sääntö, joka on saatu historiatiedot) ilmoitusten säännöt. Tämä koskee vain portaalin.                                              |
| Microsoft.Insights/AutoscaleSettings/[Read, kirjoita, poista]  | Luku, kirjoittaminen ja poistaminen Automaattinen skaalaus-asetukset.                                                                                                                     |
| Microsoft.Insights/DiagnosticSettings/[Read, kirjoita, poista] | Luku, kirjoittaminen ja poistaminen diagnostiikan asetuksia.                                                                                                                    |
| Microsoft.Insights/eventtypes/digestevents/Read             | Nämä oikeudet tarvitaan käyttäjät, jotka tarvitsevat lokeja portaalin kautta.                                                                   |
| Microsoft.Insights/eventtypes/values/Read                   | Luettele tilauksen tapahtumat tehtävän Log (management tapahtumien). Nämä käyttöoikeudet on käytettävissä sekä ohjelmallisesti ja portaalin käytön tehtävän lokiin. |
| Microsoft.Insights/LogDefinitions/Read                      | Nämä oikeudet tarvitaan käyttäjät, jotka tarvitsevat lokeja portaalin kautta.                                                                   |
| Microsoft.Insights/MetricDefinitions/Read                   | Lue metrisillä määritelmät (käytettävissä metrisillä tyypit resurssin luettelo).                                                                                  |
| Microsoft.Insights/Metrics/Read                             | Lue resurssin mittaukset.                                                                                                                              |

> [AZURE.NOTE] Käyttää ilmoituksia, diagnostiikka-asetusten muuttaminen ja arvot resurssin edellyttää, että käyttäjällä on lukuoikeus resurssilaji ja resurssin aluetta. Luominen ("kirjoitus") diagnostiikan asetus tai loki profiilia, joka arkistoidaan tallennustilan tilin tai tapahtuman porttiin virtaa edellyttää, että käyttäjä on myös kohde resurssin ListKeys käyttöoikeus.

Esimerkiksi käyttämällä yllä olevaa taulukkoa voi luoda mukautettu RBAC rooli "tehtävän Log Reader" tältä:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Tietojen valvominen liittyviä suojaustietoja
Tietojen valvominen – etenkin lokitiedostot, voi olla luottamuksellisia tietoja, kuten IP-osoitteet tai käyttäjänimet. Azure-tietojen valvominen on kolme peruslomakkeet:
1. Tehtävän lokiin, jossa kuvataan kaikki ohjausobjektin tasossa toiminnot Azure-tilauksessa.
2. Vianmäärityslokit, joka on resurssin lähettämän lokit.
3. Arvot, jotka ovat lähettämän resurssit.

Kaikissa kolmessa näitä tietotyyppejä voidaan tallennustilan tilin tallennetuista tai virtauttaa molemmat on yleinen Azure resursseja tapahtumaa-toiminnossa. Nämä ovat Yleinen resursseja, koska luominen, poistaminen ja käyttää niitä on yleensä varattu järjestelmänvalvoja sellaisten toiminto. Suosittelemme, että käytät seuranta liittyvät resurssit seuraavat käytännöt estämiseksi väärin:

- Yksittäisen, varatun tallennustilan tilin käyttöä varten-tietojen valvominen. Jos haluat erottaa useaan tallennustilan useita tilejä seurantatiedot, Älä jaa käyttö tallennustilan tilin välillä seuranta ja -seuranta tietoja, koska tämä vahingossa voi antaa käyttäjille, joiden tarvitsee vain käyttöoikeuksien valvonta tiedot (esimerkiksi. kolmannen osapuolen SIEM) access seurantaan tiedot.
- Käyttää yksittäinen, erillinen palvelun Bus tai tapahtumaa-toiminnossa nimitilan kaikkia diagnostiikan asetuksia kuin yllä Narrator.
- Rajoittaa käyttöä seuranta-ryhmän tallennustila asiakkaat- tai tapahtuman keskittimet säilyttämällä erillinen resurssiryhmä ja [käyttää laajuus](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) seurantaa roolit vain kyseisen resurssiryhmä käyttörajoitukset.
- Tapahtuman keskittimet tilauksen alueessa, kun käyttäjä on vain access-tietojen valvominen tai ListKeys käyttöoikeuksien joko tallennustilan-tileissä. Anna sen sijaan näiden oikeuksien resurssi tai resurssiryhmä käyttäjä (Jos sinulla on oma seurantaa resurssiryhmä) aluetta.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Seuranta-ryhmän tallennustila tilit pääsyä
Kun käyttäjä tai sovellus on pääsy tallennustilan tilillä-tietojen valvominen, kannattaa tallennustilan tilin, joka sisältää seurantatiedot palvelun vain luku-käyttöoikeustaso-blob-säiliö ja [Luo tili-Suojaussidokset](https://msdn.microsoft.com/library/azure/mt584140.aspx) . PowerShellin Tämä voi näyttää tältä:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Voit tarkastella tunnuksen kohteeseen sitten, että tarvitsee lukea kyseisen tallennustilan tilin, se luettelo ja lukea kaikki Blob-objektien tallennustilaan kyseisessä tilissä.

Voit myös, jos haluat määrittää nämä oikeudet RBAC kanssa, voit myöntää kyseisen kohteen tietyn tallennustilan tilin Microsoft.Storage/storageAccounts/listkeys/action käyttöoikeudet. Tämä on tarpeen käyttäjille, joilla on voi määrittää diagnostiikan asetus tai arkistoida profiilin kirjautumaan tallennustilan tilin. Voit luoda esimerkiksi seuraavan mukautetun RBAC-roolin, käyttäjän tai sovellus, jossa on vain luku tallennustilan-tililtä:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [AZURE.WARNING] ListKeys käyttöoikeuksia käyttäjä luettelon ensisijainen ja toissijainen tallennustilan tilin näppäimet. Painettavat näppäimet Myönnä käyttäjälle kaikki kirjautunut käyttöoikeudet (luku, kirjoitus, Luo BLOB-objektit, poista BLOB jne.) kaikissa kirjautunut tallennustilan tilin palvelut (blob, jono, taulukkoon, tiedosto). Suosittelemme, että tilin-Suojaussidokset, kun se on mahdollista yllä olevien ohjeiden avulla.

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Seuranta-ryhmän tapahtuman porttiin käytön rajoittamisesta
Vastaavat kuvion voi seurata tapahtuman keskittimet kanssa, mutta ensin on luotava erillinen kuunnella käyttöoikeuksien sääntöä. Jos haluat myöntää oikeuden sovellus, joka on vain kuuntele valvontaan liittyviä tapahtuman porttiin, toimi seuraavasti:

1. Luo jaettu käyttöoikeuskäytäntö tapahtuman USB-ohjaimen alla, joka on luotu streaming vain kuunnella vaatimusten seurantatiedot. Tämä on mahdollista portaalissa. Esimerkiksi ehkä kutsun yhteydessä "monitoringReadOnly." Jos mahdollista haluat antaa avaimen suoraan kuluttajille ja siirry seuraavaan vaiheeseen.
2. Jos haluat sallia avaimen itsenäisen kuluttaja, myönnä käyttäjälle, tapahtuma-toiminnossa ListKeys toiminto. Tämä on myös käyttäjille, joilla voi määrittää diagnostiikan asetus tai Kirjaudu profiilin stream tapahtuman porttiin on tarpeen. Voit esimerkiksi luoda RBAC säännön:

   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```


## <a name="next-steps"></a>Seuraavat vaiheet
- [Lisätietoja RBAC ja käyttöoikeudet resurssien hallinta](../active-directory/role-based-access-control-what-is.md)
- [Lue yleiskatsaus Azure seuranta](monitoring-overview.md)
