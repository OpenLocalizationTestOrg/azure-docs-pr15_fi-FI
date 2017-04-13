<properties
   pageTitle="Roolien määrittäminen Azure pilvipalvelussa Visual Studiossa | Microsoft Azure"
   description="Opettele määrittäminen Azure pilvipalveluihin roolit Visual Studion avulla."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configure-the-roles-for-an-azure-cloud-service-with-visual-studio"></a>Roolien määrittäminen Visual Studiossa Azure pilvipalvelussa

Azure pilvipalvelussa voi olla työntekijä tai web roolit. Rooleille sinun on määritettävä roolin suorittamistavan ja määritä, kuinka roolin on määritetty. Lisätietoja rooleista pilvipalveluihin, kohdassa video [Azure pilvipalveluihin esittely](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). Cloud-palvelun tiedot tallennetaan seuraavat tiedostot:

- **ServiceDefinition.csdef**

    Palvelun määritystiedoston määrittää pilvipalvelussa, mukaan lukien roolit ovat pakollisia, päätepisteet ja virtuaalikoneen koon runtime-asetukset. Ei mitään tämän tiedoston tallennettuja tietoja voi muuttaa, kun tehtäväsi on käynnissä.

- **ServiceConfiguration.cscfg**

    Service-kokoonpanotiedosto määrittää, kuinka monta esiintymää roolin määrittäminen ja roolille asetusten arvot. Tämän tiedoston tallennettuja tietoja voidaan muuttaa tehtäväsi on käynnissä.

Voi tallentaa eri arvoja näiden asetusten tehtäväsi suorittamistavan, voi olla useita palvelun määrityksiä. Voit käyttää eri palvelun määritykset kunkin käyttöönotto-ympäristössä. Voit esimerkiksi määrittää paikallisen Azure tallennustilan emulaattori paikallinen palvelu määrityksessä ja luo toisen palvelun asetukset käyttämään Azure tallennustilan pilveen tallennustilan tilin yhteysmerkkijono.

Kun luot uuden Azure pilvipalvelussa Visual Studiossa, kaksi palvelun määrityksiä luodaan oletusarvoisesti. Määritysten lisätään Azure projektin. Nimetään määritykset:

- ServiceConfiguration.Cloud.cscfg

- ServiceConfiguration.Local.cscfg

## <a name="configure-an-azure-cloud-service"></a>Määritä Azure pilvipalvelussa

Voit määrittää Azure pilvipalvelussa Explorerista ratkaisun Visual Studiossa seuraavassa kuvassa esitetyllä tavalla.

![Määritä pilvipalvelussa](./media/vs-azure-tools-configure-roles-for-cloud-service/IC713462.png)

### <a name="to-configure-an-azure-cloud-service"></a>Voit määrittää Azure pilvipalvelussa

1. Määrittämiseen rooleille Azure projektisi **Ratkaisunhallinnassa**rooli pikavalikon avaaminen Azure projekti ja valitse sitten **Ominaisuudet**.

    Roolin nimen sisältävä sivu näkyy Visual Studio-editorissa. Sivun näyttää kentät **määritys** -välilehti.

1. Valitse **Palvelun Configuration** -luettelosta nimi, jota haluat muokata palvelun määritykset.

    Jos haluat tehdä muutoksia kaikkiin tämän roolin service-määrityksiä, voit valita **Kaikki määritykset**.

    >[AZURE.IMPORTANT] Jos valitset tietyn palvelun määritykset, jotkin ominaisuudet ovat poissa käytöstä, koska ne voidaan määrittää vain kaikki määritykset. Jos haluat muokata näitä ominaisuuksia, sinun on valittava kaikki määritykset.

    Voit nyt päivittää näkymän käytössä ominaisuuksia välilehti.

## <a name="change-the-number-of-role-instances"></a>Rooli esiintymien määrän muuttaminen

Pilvipalvelussa sijaitsevaan suorituskyvyn parantamiseksi rooli esiintymät, joissa on käytössä, montako käyttäjien tai tietyn roolin odotetaan kuormituksen määrän muuttaminen. Erillinen virtual machine luodaan esiintymissä rooli pilvipalvelussa suoritettaessa Azure-tietokannassa. Tämä vaikuttaa Laskutus tämän pilvipalvelussa käyttöönottoa varten. Saat lisätietoja Laskutus [varten Microsoft Azure laskun ymmärtäminen](billing/billing-understand-your-bill.md).

### <a name="to-change-the-number-of-instances-for-a-role"></a>Jos haluat muuttaa roolin kerrat

1. Valitse **määritys** -välilehti.

1. Valitse palvelun määrityksistä, jotka haluat päivittää **Palvelun Configuration** -luettelosta.

    >[AZURE.NOTE] Voit määrittää esiintymän Laske tietyn palvelun määritykset tai kaikki palvelun määritykset.

1. Kirjoita **esiintymän määrä** -tekstiruutuun kerrat, jotka haluat aloittaa tällä roolilla.

    >[AZURE.NOTE] Jokaiselle esiintymälle suoritetaan erillisessä virtual tietokoneessa, kun julkaiset pilvipalvelussa sijaitsevaan Azure.

1. Voit tallentaa muutokset service-kokoonpanotiedosto työkalurivin **Tallenna** -painiketta.

## <a name="manage-connection-strings-for-storage-accounts"></a>Yhteyden merkkijonot tallennustilan tilien hallinta

Voit lisätä, poistaa tai muokata yhteyden merkkijonot service-määrityksiä. Voit haluta eri yhteysmerkkijonon eri määrityksiä. Esimerkiksi saatat haluta paikallinen palvelu kokoonpanossa, jonka arvo on paikallinen yhteysmerkkijonon `UseDevelopmentStorage=true`. Voit myös haluta cloud-palvelun määritykset, joka käyttää Azure-tallennustilan tilin määrittäminen.

>[AZURE.WARNING] Kun kirjoitat Azure tallennustilan avaimen tilitietojen tallennustilan tilin yhteysmerkkijonon, tiedot tallennetaan paikallisesti palvelun määritystiedostossa. Kuitenkin nämä tiedot on tallennettu tällä hetkellä ei ole salattuja tekstinä.

Käyttämällä eri arvon kunkin palvelun määritykset, sinun ei tarvitse käyttää eri yhteysmerkkijonon oman pilvipalvelussa tai muokata omassa koodissasi, kun julkaiset pilvipalvelussa sijaitsevaan Azure. Voit käyttää samaa nimeä yhteysmerkkijonon koodi ja arvo päivitetään eri-palvelun määritykset, voit valita, kun luot pilvipalvelussa sijaitsevaan tai julkaista sen perusteella.

### <a name="to-manage-connection-strings-for-storage-accounts"></a>Voit hallita yhteyden merkkijonojen tallennustilan tilit

1. Valitse **asetukset** -välilehti.

1. Valitse palvelun määrityksistä, jotka haluat päivittää **Palvelun Configuration** -luettelosta.

    >[AZURE.NOTE] Voit päivittää tietyn palvelun määritykset yhteyden merkkijonoja, mutta jos haluat lisätä tai poistaa yhteysmerkkijonon sinun on valittava kaikki määritykset.

1. Lisää yhteysmerkkijonon valitsemalla **Lisää asetus** -painiketta. Uusi tapahtuma lisätään luetteloon.

1. Kirjoita **nimi** -ruutuun nimi, jota haluat käyttää yhteysmerkkijono.

1. Valitse **tyyppi** -pudotusvalikosta **Yhteysmerkkijonon**.

1. Voit muuttaa yhteysmerkkijonon arvo valitsemalla pistettä (...). **Luo tallennustilan yhteysmerkkijono** -valintaikkuna.

1. Käyttämään paikallisen tallennustilan tilin emulaattorin valita **Microsoft Azure-tallennustilan emulaattorin** asetukset-painike ja valitse sitten **OK** -painiketta.

1. Voit käyttää tallennustilan tilin Azure- **tilauksen** asetukset-painike ja valitse haluamasi tallennustilan tilin.

1. Jos haluat käyttää mukautettuja tunnistetietoja, valitse **manuaalisesti tunnistetiedot** asetukset-painiketta. Kirjoita tallennustilan tilin nimi ja joko ensisijainen tai toinen-näppäintä. Saat tietoja siitä, miten tallennustilan tilin luominen ja kirjoita tiedot tallennustilan tilin **Luominen tallennustilan yhteysmerkkijono** -valintaikkunassa, [valmisteleminen, julkaiseminen tai Visual Studio Azure sovelluksen ottaminen käyttöön](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Yhteysmerkkijonon, valitse yhteysmerkkijono ja valitse sitten **Poista asetukset** -painiketta.

1. Valitse Tallenna muutokset service-kokoonpanotiedosto työkalurivin **Tallenna** -kuvaketta.

1. Käyttämään palvelua määritystiedostossa yhteysmerkkijonon saa arvon määritys-asetusta. Seuraava koodi näkyy esimerkki, jossa on luotu Blob-objektien tallennustilaan ja ladata yhteysmerkkijonon käyttäminen tietojen `MyConnectionString` palvelun määritys-tiedostosta, kun käyttäjä valitsee **Button1** Azure pilvipalvelussa web-roolin Default.aspx-sivulla. Lisää seuraava lauseiden Default.aspx.cs käyttämisestä:

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Avaa Default.aspx.cs rakennenäkymässä ja Lisää-painike työkaluryhmästä. Lisää seuraava koodi `Button1_Click` menetelmää. Tämä koodi käyttää `GetConfigurationSettingValue` arvon noutaminen yhteysmerkkijono palvelun määritystiedosto. Valitse blob luodaan tallennustilan tili, joka viittaa yhteysmerkkijonon `MyConnectionString` ja lopuksi ohjelma lisää tekstin blob.

    ```
    protected void Button1_Click(object sender, EventArgs e)
    {
        // Setup the connection to Azure Storage
        var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("MyConnectionString"));
        var blobClient = storageAccount.CreateCloudBlobClient();
        // Get and create the container
        var blobContainer = blobClient.GetContainerReference("quicklap");
        blobContainer.CreateIfNotExists();
        // upload a text blob
        var blob = blobContainer.GetBlockBlobReference(Guid.NewGuid().ToString());
        blob.UploadText("Hello Azure");

    }
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Lisää mukautettu asetukset käyttämään Azure pilvipalvelussa

Palvelun määritystiedostossa mukautettujen asetusten avulla voit lisätä nimen ja arvon tietyn palvelun määrittämisessä. Voit halutessasi ominaisuuden määrittäminen oman pilvipalvelussa-asetuksen arvoa ja käyttämällä tätä arvoa voit hallita koodisi logiikan tämän asetuksen avulla. Voit muuttaa palvelun määritysten arvot eikä sinun tarvitse uudelleen service-paketin tai kun pilvipalvelussa sijaitsevaan on käynnissä. Koodi tarkistaa ilmoituksissa kun asetusten muutokset. Katso [RoleEnvironment.Changing tapahtumaa](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Voit lisätä, poistaa tai muokata mukautettuja asetuksia service-määrityksiä. Voit haluta eri arvoja näiden merkkijonojen eri määrityksiä.

Käyttämällä eri arvon kunkin palvelun määritykset, sinulla ei ole eri merkkijonot oman pilvipalvelussa tai muokata omassa koodissasi, kun julkaiset pilvipalvelussa sijaitsevaan Azure. Voit käyttää samaa nimeä merkkijonon koodi ja arvo päivitetään eri-palvelun määritykset, voit valita, kun luot pilvipalvelussa sijaitsevaan tai julkaista sen perusteella.

### <a name="to-add-custom-settings-to-use-in-your-azure-cloud-service"></a>Voit lisätä mukautettuja asetuksia käyttämään Azure pilvipalvelussa

1. Valitse **asetukset** -välilehti.

1. Valitse palvelun määrityksistä, jotka haluat päivittää **Palvelun Configuration** -luettelosta.

    >[AZURE.NOTE] Voit päivittää tietyn palvelun määritykset merkkijonoja, mutta jos haluat lisätä tai poistaa merkkijonon, sinun on valittava **Kaikki määritykset**.

1. Lisää merkkijonon valitsemalla **Lisää asetus** -painiketta. Uusi tapahtuma lisätään luetteloon.

1. Kirjoita **nimi** -ruutuun nimi, jota haluat käyttää merkkijonon.

1. Valitse **tyyppi** -pudotusvalikosta **merkkijono**.

1. Voit lisätä tai muuttaa merkkijonon arvo, kirjoita uusi arvo **arvo** -tekstiruutuun.

1. Jos haluat poistaa merkkijonon, valitse ja valitse sitten **Poista asetukset** -painiketta.

1. Voit tallentaa muutokset service-kokoonpanotiedosto työkalurivin **Tallenna** -painiketta.

1. Käyttämään palvelua kokoonpanotiedosto merkkijonon saa arvon määritys-asetusta.

    Haluat, että seuraavat käyttämällä lauseet on jo lisätty Default.aspx.cs samalla tavalla kuin edellisessä.

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Lisää seuraava koodi `Button1_Click` menetelmää voit käyttää tätä merkkijonoa, jota voit käyttää yhteysmerkkijonon samalla tavalla. Koodin sitten suorittaa tiettyjä lisäkoodin määritysten tiedosto, jota käytetään asetuksia merkkijonon arvon perusteella.

    ```
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("MySetting");
    if (settingValue == “ThisValue”)
    {
    // Perform these lines of code
    }
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Rooli jokaiselle esiintymälle paikallisen-tallennustilan hallinta

Voit lisätä paikallisen tiedoston järjestelmän tallennustilaa jokaiselle esiintymälle rooli. Voit tallentaa paikalliset tiedot tähän vaikuttavat ei voi käyttää muilla ryhmän rooleilla. Tähän voidaan tallentaa tiedot, joita sinun ei tarvitse tallentaa taulukkoon, Blob-objektien tai SQL-tietokanta-tallennustilan. Voit esimerkiksi käyttää tallennustilan tämän paikallisen välimuistitiedot, jotka on ehkä voi käyttää uudelleen. Tämä tallennettuja tietoja ei voi käyttää muita esiintymiä rooli. 

Paikallinen tallennus asetukset koskevat kaikkia palvelun määrityksiä. Voit vain lisätä, poistaa tai muokata paikallista tallennustilaa kaikki palvelun määritykset.

### <a name="to-manage-local-storage-for-each-role-instance"></a>Voit hallita roolin jokaiselle esiintymälle paikallinen tallennus

1. Valitse **Paikallinen tallennus** -välilehti.

1. Valitse **Palvelun Configuration** -luettelosta **Kaikki määritykset**.

1. Lisää paikallinen tallennus tapahtuma valitsemalla **Lisää paikallinen tallennus** -painiketta. Uusi tapahtuma lisätään luetteloon.

1. Kirjoita **nimi** -ruutuun nimi, jota haluat käyttää tätä paikallinen tallennus.

1. Kirjoita koko megatavuina, tämä paikallinen tallennus varten tarvittava **koko** teksti-ruutuun.

1. Voit poistaa tämän paikallisen säilön tiedot, kun tämän roolin virtuaalikoneen on kuluessa, valitse **roolin Tyhjennä Roskakori** -valintaruutu.

1. Voit muokata olemassa olevan merkinnän paikallinen tallennus valitsemalla rivi, joka on päivitettävä. Voit muokata kentät, valitse edellisessä vaiheessa kuvatulla tavalla.

1. Voit poistaa paikallisen säilön tapahtuma, valitse tallennustilan tapahtuma-luettelosta ja valitse sitten **Poista paikallinen tallennustilaa** -painike.

1. Jos haluat tallentaa muutokset service configuration-tiedostot, valitse työkalurivillä **Tallenna** -kuvaketta.

1. Paikallinen tallennus, jotka olet lisännyt palvelun määritystiedostossa käyttämään saa paikallinen resurssi määritys-asetuksen arvoa. Seuraavat koodin rivit avulla voit käyttää tätä arvoa ja Luo tiedosto nimeltä **MyStorageTest.txt** ja kirjoittaa testitiedot tekstirivi tiedoston. Voit lisätä koodin kyselyjä `Button_Click` menetelmä, jota käytit edellisistä toiminnoista:

1. Haluat, että seuraavat käyttämällä lauseet lisätään Default.aspx.cs:

    ```
    using System.IO;
    using System.Text;
    ```

1. Lisää seuraava koodi `Button1_Click` menetelmää. Tämä luo tiedoston paikalliseen muistiin ja kirjoittaa testitiedot kyseisen tiedostoon.

    ```
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("LocalStorage1");

    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyStorageTest.txt" };
    String filePath = Path.Combine(paths);

    using (FileStream writeStream = File.Create(filePath))
    {
          Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
          writeStream.Write(textToWrite, 0, textToWrite.Length);
    }
    ```

1. (Valinnainen) Tämä tiedosto, jonka loit suorittaessasi pilvipalvelussa sijaitsevaan paikallisesti näkyviin noudattamalla seuraavia ohjeita:

  1. Suorita web rooli ja valitse **Button1** varmistaa, että sisällä koodin `Button1_Click` kutsutaan.

  1. Avaa pikavalikko Azure-kuvake ilmaisinalueella ja valitse **Näytä Laske emulaattorin-Käyttöliittymä**. **Azure Laske emulaattorin** -valintaikkuna.

  1. Web-roolin valitseminen

  1. Valitse valikkoriviltä **Työkalut**, **Avaa Paikallinen säilö**. Resurssienhallinta-ikkuna tulee näkyviin.

  1. Valikkoriviltä **MyStorageTest.txt** kirjoittaminen **hakuruutuun** ja valitse sitten Aloita haku **Enter** .

    Tiedosto näkyy hakutuloksissa.

  1. Jos haluat tarkastella tiedoston sisällön, Avaa tiedosto pikavalikon ja valitse **Avaa**.

## <a name="collect-cloud-service-diagnostics"></a>Kerää cloud palvelun vianmääritys

Voit kerätä diagnostiikka tietoja Azure pilvipalvelussa varten. Nämä tiedot lisätään tallennustilan tilin. Voit haluta eri yhteysmerkkijonon eri määrityksiä. Esimerkiksi saatat haluta tallentaa paikallisesti tilin paikallinen palvelu määrityksistä, jotka on UseDevelopmentStorage arvo TOSI. Voit myös haluta cloud-palvelun määritykset, joka käyttää Azure-tallennustilan tilin määrittäminen. Saat lisätietoja Azure diagnostiikka kirjaaminen tietojen kerääminen Azure Diagnostiikan avulla.

>[AZURE.NOTE] Paikallinen palvelu-määritys on jo määritetty käyttämään paikalliset resurssit. Jos käytät cloud palvelun määritykset julkaista Azure pilvipalvelussa, yhteysmerkkijonon, joka voi olla julkaistessasi käytetään myös diagnostiikka-yhteysmerkkijonon Jos et ole määrittänyt yhteysmerkkijonon. Jos yhteyttä pilvipalvelussa Visual Studiossa pakata, yhteysmerkkijono-palvelun ei muutu.

### <a name="to-collect-cloud-service-diagnostics"></a>Kerätä cloud palvelun vianmääritys

1. Valitse **määritys** -välilehti.

1. Valitse **Palvelun Configuration** -luettelosta, jonka haluat päivittää tai valitse **Kaikki määritykset**palvelun määritykset.

    >[AZURE.NOTE] Voit päivittää tietyn palvelun määritykset tallennustilan tiliin, mutta jos haluat ottaa käyttöön tai poistaa käytöstä diagnostiikka sinun on valittava kaikki määritykset.

1. Diagnostiikan käyttöön valitsemalla **Ota käyttöön diagnostiikka** -valintaruutu.

1. Voit muuttaa arvoa tallennustilan tilin, valitse pistettä (...).

    **Luo tallennustilan yhteysmerkkijono** -valintaikkuna.

1. Paikallinen yhteysmerkkijonon, Azure tallennustilan emulaattorin vaihtoehto ja valitse sitten **OK** -painiketta.

1. Käyttämään Azure-tilaukseen liittyvää tallennustilan tiliä **tilaus** -vaihtoehto.

1. Käytettävä paikallisen yhteysmerkkijonon tallennustilan tilin vaihtoehto **manuaalisesti tunnistetiedot** .

    Saat lisätietoja tallennustilan tilin luominen ja kirjoittaminen tietoja tallennustilan tilin **Luominen tallennustilan yhteysmerkkijono** -valintaikkunassa [valmisteleminen, julkaiseminen tai Visual Studio Azure sovelluksen ottaminen käyttöön](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Kirjoita **tilin nimi**-tallennustilan tilin valitseminen

    Jos olet manuaalisesti kirjoittamalla tallennustilan tilin tunnistetiedot, kopioi tai kirjoita perusavaimeksi **tilin avain**. Tätä näppäintä voi kopioida [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885). Jos haluat kopioida avaimeen [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885) **Tallennustilan tilit** -näkymässä seuraavasti:
    
  1. Valitse tallennustilan tili, jota haluat käyttää pilvipalvelussa sijaitsevaan.

  1. Valitse näytön alareunassa olevaa **Pikanäppäinten hallinta** -painiketta. **Pikanäppäinten hallinta** -valintaikkuna.

  1. Jos haluat kopioida pikanäppäin, valitse **Kopioi Leikepöydälle** -painike. Voit liittää avaimeen **Tiliavain tiliavain** -kenttään.

1. Jos haluat käyttää tallennustilan tili, joka saadaan, kuin yhteysmerkkijonon diagnostiikka (ja tallentamisesta välimuistiin) Kun julkaiset pilvipalvelussa sijaitsevaan Azure-työkirjassa **päivityksen kehittäminen tallennustilan yhteyden merkkijonojen diagnostiikka- ja kanssa Azuren tallennustila välimuisti tilin tunnistetietojen julkaiset Azure** -valintaruutu.

1. Voit tallentaa muutokset service-kokoonpanotiedosto työkalurivin **Tallenna** -painiketta.

## <a name="change-the-size-of-the-virtual-machine-used-for-each-role"></a>Käytetään kunkin roolin virtuaalikoneen koon muuttaminen

Voit määrittää kullekin roolille virtuaalikoneen kokoa. Voit määrittää vain tässä kaikki palvelun määritykset koon. Jos valitset tietokoneen pienemmäksi, valitse vähemmän suorittimen sydämiä, muistin ja paikalliseen levyasemaan tallennustila on varattu. Myönnetty kaistanleveys on myös pienempi. Saat lisätietoja näiden kokojen ja resurssit [koot pilvipalveluihin](cloud-services/cloud-services-sizes-specs.md).

Tarvittavat kunkin Azure virtual tämän tietokoneen resurssit vaikuttaa kustannusten käynnissä pilvipalvelussa sijaitsevaan Azure-tietokannassa. Saat lisätietoja Azure Laskutus [varten Microsoft Azure laskun ymmärtäminen](billing/billing-understand-your-bill.md).

### <a name="to-change-the-size-of-the-virtual-machine"></a>Virtuaalikoneen koon muuttaminen

1. Valitse **määritys** -välilehti.

1. Valitse **Palvelun Configuration** -luettelosta **Kaikki määritykset**.

1. Jos haluat valita tämän roolin virtuaalikoneen kokoa, valitse haluamasi koon **AM kokoa** -luettelosta.

1. Voit tallentaa muutokset service-kokoonpanotiedosto työkalurivin **Tallenna** -painiketta.

## <a name="manage-endpoints-and-certificates-for-your-roles"></a>Päätepisteet ja varmenteet oman roolien hallinta

Protokollan porttinumero, määrittämällä ja HTTPS-SSL-varmenteen tiedot määrittäminen verkko päätepisteet. Ennen kesäkuussa 2012-versioiden tuki HTTP ja HTTPS TCP. Kesäkuussa 2012-versio tukee protokollat- ja UDP. Et voi käyttää UDP Laske emulaattori syötteen päätepisteet. Voit käyttää kyseisen protokolla vain sisäinen päätepisteet varten.

Voit parantaa suojausta Azure pilvipalvelussa, voit luoda päätepisteet, jotka käyttävät HTTPS-protokollan. Esimerkiksi jos sinulla on pilvipalvelussa, jota käytetään asiakkaiden ostaa tilauksia, haluat Varmista, että niiden tiedot on suojattu SSL avulla.

Voit myös lisätä päätepisteet, joita voidaan käyttää sisäisesti- tai ulkopuolelta. Ulkoisten päätepisteiden kutsutaan syötteen päätepisteet. Syötteen päätepisteen avulla käyttäjät pilvipalvelussa sijaitsevaan toiseen tukiasema. Jos sinulla on WCF-palveluun, haluat ehkä näyttää sisäinen päätepisteen web-roolin käyttämään palvelua.

>[AZURE.IMPORTANT] Voit päivittää vain päätepisteet kaikki palvelun määritykset.

Jos lisäät HTTPS päätepisteet, sinun on käytettävä SSL-varmenteen. Voit tehdä tämän Varmenteet liittäminen tehtäväsi kaikki service-määrityksiä varten ja käyttää näitä oman päätepisteet.

>[AZURE.IMPORTANT] Nämä varmenteet on pakattu ei palveluun. Täytyy ladata sertifikaattien tarkasteleminen erikseen Azure [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)kautta.

Minkä tahansa hallinta sertifikaatteja, joita voit liittää service-määrityksiä koskevat vain, kun pilvipalvelussa sijaitsevaan suoritetaan Azure-tietokannassa. Kun paikallinen kehitysympäristö pilvipalvelussa sijaitsevaan, vakiomuotoinen varmenne, jota hallitaan Azure Laske emulaattori käytetään.

### <a name="to-add-a-certificate-to-a-role"></a>Varmenteen lisääminen rooli

1. Valitse **Varmenteet** -välilehti.

1. Valitse **Palvelun Configuration** -luettelosta **Kaikki määritykset**.

    >[AZURE.NOTE] Voit lisätä tai poistaa varmenteet, sinun on valittava kaikki määritykset. Voit päivittää nimi ja allekirjoitus tietyn palvelun määrityksen tarvittaessa.

1. Lisää tämä rooli varmennetta, voit **Lisätä varmenteen** -painiketta. Uusi tapahtuma lisätään luetteloon.

1. Kirjoita **nimi** -ruutuun nimi.

1. Valitse sijainti, johon haluat lisätä varmenteen **Säilön sijainti** -luettelossa.

1. Valitse kauppa, jota haluat käyttää sertifikaatti **Tallentaa nimi** -luettelosta.

1. Voit lisätä varmenteen, valitsemalla pistettä (...). **Windowsin suojaus** -valintaikkuna.

1. Valitse varmenne, jota haluat käyttää luettelosta ja valitse sitten **OK** -painiketta.

    >[AZURE.NOTE] Kun lisäät varmenteen säilöstä, keskitason sertifikaatteja lisätään automaattisesti puolestasi asetukset. Keskitason nämä varmenteet on myös ladattava Azure määrittämiseksi oikein palvelun SSL.

1. Varmenteen poistaminen, valitse varmenne ja valitse sitten **Poista varmenne** -painiketta.

1. Valitse työkalurivillä Tallenna muutokset service määritysten tiedostot **Tallenna** -kuvaketta.

### <a name="to-manage-endpoints-for-a-role"></a>Voit hallita päätepisteet rooli

1. Valitse **päätepisteet** -välilehti.

1. Valitse **Palvelun Configuration** -luettelosta **Kaikki määritykset**.

1. Lisää päätepisteen, voit **Lisätä päätepisteen** -painiketta. Uusi tapahtuma lisätään luetteloon.

1. Kirjoita **nimi** -ruutuun nimi, jota haluat käyttää tämän päätepisteen.

1. Valitse päätepisteen, jotka on tyyppi **tyyppi** -luettelosta.

1. Valitse protokolla, jotka tarvitset päätepisteen **protokolla** -luettelosta.

1. Jos se on syötteen päätepisteen **Julkisen portin** tekstiruutuun, kirjoita käyttämään Julkinen portti.

1. Kirjoita **Yksityinen portin** tekstiruutuun yksityinen portti käyttämään.

1. Jos päätepisteen vaatii https-protokollan, **SSL-varmenteen nimi** -luettelosta ja valitse käytettävä varmenne.

    >[AZURE.NOTE] Luettelossa on sertifikaatteja, joita olet lisännyt tämän roolin- **Varmenteet** -välilehti.

1. Voit tallentaa muutokset service configuration-tiedostot valitsemalla työkaluriviltä **Tallenna** -painiketta.

## <a name="next-steps"></a>Seuraavat vaiheet
Lue lisää Azure projektien Visual Studiossa lukemalla [Azure-projektin määrittäminen](vs-azure-tools-configuring-an-azure-project.md). Lue lisää cloud palvelun rakennetta [Rakenteiden viite](https://msdn.microsoft.com/library/azure/dd179398)lukemalla.
