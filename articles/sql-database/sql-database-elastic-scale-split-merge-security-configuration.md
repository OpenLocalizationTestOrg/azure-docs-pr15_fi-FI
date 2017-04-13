<properties 
    pageTitle="Jaa ja yhdistäminen suojausasetukset | Microsoft Azure" 
    description="Määritä x409 salauksen varmenteet" 
    metaKeywords="Elastic Database certificates security" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng" />


# <a name="split-merge-security-configuration"></a>Jaa ja yhdistäminen suojausasetukset  

Jaa ja Yhdistä-palvelun käyttöä varten on määritettävä oikein suojaus. Palvelu on osa Microsoft Azure SQL-tietokanta joustavasti asteikko-ominaisuus. Lisätietoja on artikkelissa [joustavasti asteikko ja yhdistää palvelun opetusohjelma](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Varmenteiden määrittäminen

Varmenteet on määritetty kahdella tavalla. 

1. [SSL-varmenteen määrittäminen](#To-Configure-the-SSL#Certificate)
2. [Voit määrittää asiakkaan varmenteet](#To-Configure-Client-Certificates) 

## <a name="to-obtain-certificates"></a>Voit hankkia varmenteet

Varmenteiden saadaan julkisen varmenteiden myöntäjistä (CA) tai [Windows-sertifikaattipalvelu](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx). Nämä ovat ensisijainen tapoja hankkia varmenteet.

Jos näistä asetuksista eivät ole käytettävissä, voit luoda **itse allekirjoitettua varmenteet**.
 
## <a name="tools-to-generate-certificates"></a>Luo varmenteet Työkalut

* [Makecert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>Jos haluat suorittaa Työkalut

* -Developer komentorivi Visual Studios varten, katso [Visual Studio komentorivi-ikkuna](http://msdn.microsoft.com/library/ms229859.aspx) 

    Jos tietokoneeseen on asennettu, siirry:

        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 

* Hae-WDK [Windows 8. 1: lataa paketit ja työkalut](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>SSL-varmenteen määrittäminen
Salaa tietoliikenne ja todennetaan palvelimen edellyttää SSL-varmenne. Valitse alla kolmella eri sovellettavan eniten ja suorittaa sen vaiheet:

### <a name="create-a-new-self-signed-certificate"></a>Uuden itse allekirjoitetun varmenteen luominen

1.    [Itse allekirjoitetun varmenteen luominen](#Create-a-Self-Signed-Certificate)
2.    [Luo PFX Self-Signed SSL-varmenne](#Create-PFX-file-for-Self-Signed-SSL-Certificate)
3.    [SSL-varmenteen cloud palvelun lataaminen](#Upload-SSL-Certificate-to-Cloud-Service)
4.    [Päivitä SSL-varmenteen palvelun kokoonpanotiedosto](#Update-SSL-Certificate-in-Service-Configuration-File)
5.    [Tuo SSL-varmenteen myöntäjältä](#Import-SSL-Certification-Authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Jos haluat käyttää varmenne säilöstä
1. [SSL-varmenteen vieminen sertifikaatin](#Export-SSL-Certificate-From-Certificate-Store)
2. [SSL-varmenteen cloud palvelun lataaminen](#Upload-SSL-Certificate-to-Cloud-Service)
3. [Päivitä SSL-varmenteen palvelun kokoonpanotiedosto](#Update-SSL-Certificate-in-Service-Configuration-File)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>Käytettävä varmenne PFX-tiedosto

1. [SSL-varmenteen cloud palvelun lataaminen](#Upload-SSL-Certificate-to-Cloud-Service)
2. [Päivitä SSL-varmenteen palvelun kokoonpanotiedosto](#Update-SSL-Certificate-in-Service-Configuration-File)

## <a name="to-configure-client-certificates"></a>Voit määrittää asiakkaan varmenteet
Asiakkaan varmenteet ovat vaatii todennusta palveluun. Valitse alla kolmella eri sovellettavan eniten ja suorittaa sen vaiheet:

### <a name="turn-off-client-certificates"></a>Asiakkaan varmenteet poistaminen käytöstä
1.    [Asiakkaan Varmenteen todentaminen poistaminen käytöstä](#Turn-Off-Client-Certificate-Based-Authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Anna uuden itse allekirjoitettua asiakkaan todistuksia
1.    [Luo itse allekirjoitetun varmenteen myöntäjältä](#Create-a-Self-Signed-Certification-Authority)
2.    [Lataa CA-sertifikaatin palvelun pilveen](#Upload-CA-Certificate-to-Cloud-Service)
3.    [Päivitä CA-sertifikaatin palvelun kokoonpanotiedosto](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Asiakkaan todistuksia](#Issue-Client-Certificates)
5.    [Luo PFX tiedostot asiakkaan varmenteet](#Create-PFX-files-for-Client-Certificates)
6.    [Tuo Asiakasvarmenne](#Import-Client-Certificate)
7.    [Kopioi asiakkaan varmenteen Thumbprints](#Copy-Client-Certificate-Thumbprints)
8.    [Palvelun määritystiedostossa sallittu asiakkaiden määrittäminen](#Configure-Allowed-Clients-in-the-Service-Configuration-File)

### <a name="use-existing-client-certificates"></a>Olemassa olevan asiakkaan varmenteiden käytöstä
1.    [Etsi CA julkinen avain](#Find-CA-Public Key)
2.    [Lataa CA-sertifikaatin palvelun pilveen](#Upload-CA-certificate-to-cloud-service)
3.    [Päivitä CA-sertifikaatin palvelun kokoonpanotiedosto](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Kopioi asiakkaan varmenteen Thumbprints](#Copy-Client-Certificate-Thumbprints)
5.    [Palvelun määritystiedostossa sallittu asiakkaiden määrittäminen](#Configure-Allowed-Clients-in-the-Service-Configuration File)
6.    [Määritä asiakasohjelman varmenteen kumottujen varmenteiden tarkistaminen](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Sallittujen IP-osoitteet

Palvelupäätepisteet pääsy voidaan rajoittaa tiettyjä alueita IP-osoitteet.

## <a name="to-configure-encryption-for-the-store"></a>Säilön salauksen määrittäminen

Varmenteen tarvitaan salaa metatietosäilön tallennettuja tunnistetietoja. Valitse alla kolmella eri sovellettavan eniten ja suorittaa sen vaiheet:

### <a name="use-a-new-self-signed-certificate"></a>Käytä uutta itse allekirjoitettua varmennetta.

1.     [Itse allekirjoitetun varmenteen luominen](#Create-a-Self-Signed-Certificate)
2.     [Luo PFX Self-Signed salausvarmenne](#Create-PFX-file-for-Self-Signed-Encryption-Certificate)
3.     [Lataa salausvarmenne palvelun pilveen](#Upload-Encryption-Certificate-to-Cloud-Service)
4.     [Päivitä salausvarmenne palvelun määritystiedostossa](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Käytä varmenne säilöstä

1.     [Vie salausvarmenne varmenne-kaupasta](#Export-Encryption-Certificate-From-Certificate-Store)
2.     [Lataa salausvarmenne palvelun pilveen](#Upload-Encryption-Certificate-to-Cloud-Service)
3.     [Päivitä salausvarmenne palvelun määritystiedostossa](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Käytä aiemmin luotu sertifikaatti PFX-tiedosto

1.     [Lataa salausvarmenne palvelun pilveen](#Upload-Encryption-Certificate-to-Cloud-Service)
2.     [Päivitä salausvarmenne palvelun määritystiedostossa](#Update-Encryption-Certificate-in-Service-Configuration-File)

## <a name="the-default-configuration"></a>Oletusarvon määrittäminen

Oletusmääritys estää kaikki HTTP-päätepisteen käytön. Tämä on suositeltua asetusta, koska nämä päätepisteet pyynnöt voivat suorittaa luottamuksellisia tietoja, kuten tietokannan tunnistetietoja.
Oletus-asetusten avulla kaikki access HTTPS päätepisteelle. Tämä asetus saattaa olla rajoitettu edelleen.

### <a name="changing-the-configuration"></a>Kokoonpanon muuttaminen

Ohjausobjektin käyttösäännöt, jotka koskevat ja päätepisteen ryhmä on määritetty **<EndpointAcls>** - **palvelun määritystiedosto**-osaan.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

Access-hallinta-ryhmässä säännöt on määritetty <AccessControl name=""> palvelun määritystiedosto-osassa. 

Muoto on selitetty verkon käytön hallinta luettelo ohjeista.
Esimerkiksi sallimaan vain IP-osoitteet alueen 100.100.0.0 100.100.255.255 käyttämään HTTPS-päätepisteen sääntöjä näyttää tältä:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>Palvelun estäminen ja eston

On kaksi eri järjestelmiä tunnistaa ja estää palvelun esto kalastelu tueta:

*    Samanaikaiset pyynnöt kohden etäpalvelin määrän rajoittaminen (oletusarvoisesti poissa käytöstä)
*    Rajoita access kohti etäpalvelin korko (Valitse oletusarvon mukaan)

Nämä perustuvat edelleen kuvattu dynaaminen IP-suojauksen IIS-ominaisuuksia. Kun muuttaminen määritysten varo seuraavat seikat:

* Välityspalvelimet ja NAT-laitteiden etäpalvelin tietojen päälle toimintaa
* Mihin tahansa resurssiin-web-roolin sivupyynnön pidetään (kuten ladataan komentosarjoja, kuvia ja niin edelleen)

## <a name="restricting-number-of-concurrent-accesses"></a>Samanaikainen sisäänkäyntien määrän rajoittaminen

Asetukset, joka määrittää tämän asetuksen ovat seuraavat:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Muuta DynamicIpRestrictionDenyByConcurrentRequests tosi käyttöön suojaus.

## <a name="restricting-rate-of-access"></a>Korkokannan käytön rajoittaminen

Asetukset, joka määrittää tämän asetuksen ovat seuraavat:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a>Käyttö estetty pyyntö vastauksen määrittäminen

Seuraava asetus määrittää estetty pyynnön vastaus:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Tarkista muut tuetut arvot dynaaminen IP-suojauksen IIS-ohjeista.

## <a name="operations-for-configuring-service-certificates"></a>Toimintojen service todistusten määrittämistä varten
Tämä artikkeli koskee vain viittaus. Toimi kuvatut määritysvaiheet:

* SSL-varmenteen määrittäminen
* Määritä asiakkaan varmenteet

## <a name="create-a-self-signed-certificate"></a>Itse allekirjoitetun varmenteen luominen
Suorita:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Voit mukauttaa seuraavasti:

*    -n ja palvelun URL-osoite. Yleismerkkejä ("CN = * .cloudapp .net") ja vaihtoehtoiset nimet ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") ovat tuettuja.
*    e - sertifikaatin vanhentumispäivä, vahvan salasanan luominen ja määritä se pyydettäessä.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>Itse allekirjoitetun SSL-varmenteen PFX-tiedoston luominen

Suorita:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Kirjoita salasana ja vie tiedot sitten varmenteen vaihtoehdoista:
* Kyllä, vie yksityinen avain
* Vie kaikki laajennetut ominaisuudet

## <a name="export-ssl-certificate-from-certificate-store"></a>SSL-varmenteen vieminen sertifikaatin

* Etsi varmenne
* Valitse Toiminnot -> kaikki tehtävät -> Vie...
* Voit viedä varmenteen tuominen. PFX-tiedosto, jonka vaihtoehdoista:
    * Kyllä, vie yksityinen avain
    * Jos mahdollista sisällyttää kaikki varmenteet varmenteiden polku * Vie kaikki laajennetut ominaisuudet

## <a name="upload-ssl-certificate-to-cloud-service"></a>SSL-varmenteen lataaminen pilvipalvelussa

Lataa varmenteen kanssa nykyisen tai luoda. SSL-avaimen pari PFX tiedoston:

* Kirjoita yksityisen avaimen tietojen suojaaminen salasanalla

## <a name="update-ssl-certificate-in-service-configuration-file"></a>Päivitä SSL-varmenteen palvelun kokoonpanotiedosto

Päivitä seuraava asetus palvelun määritystiedostossa allekirjoitusarvo on ladattu pilvipalveluun varmenteen allekirjoitus:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>Tuo SSL-varmenteen myöntäjältä

Kaikki tilin/machine, jotka ovat yhteydessä palvelu-seuraavasti:

* Kaksoisnapsauta. CER-tiedoston kohdalla Resurssienhallinnassa
* Valitse varmenne-valintaikkuna Asenna sertifikaatti...
* Sertifikaattien luotettujen päämyöntäjien säilöön

## <a name="turn-off-client-certificate-based-authentication"></a>Asiakkaan Varmenteen todentaminen poistaminen käytöstä

Vain asiakkaan Varmenteen todentaminen tueta, eikä se käytöstä ansiosta julkisen käyttämään, Palvelupäätepisteet, ellei muita järjestelmiä ovat paikoillaan (kuten Microsoft Azure Virtual Network).

Voit muuttaa näitä asetuksia FALSE palvelun määritystiedostossa ominaisuus käytöstä:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Kopioi sitten sama allekirjoitus kuin SSL-varmenteen Myöntäjän varmenne-asetusta:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Luo itse allekirjoitetun varmenteen myöntäjältä
Suorita seuraavat toimet voit luoda itse allekirjoitetun varmenteen myöntäjältä edustajana:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Voit mukauttaa sen

*    e - sertifikaatin vanhenemispäivämäärä kanssa


## <a name="find-ca-public-key"></a>Etsi CA julkinen avain

Kaikki asiakas-varmenteet on myöntänyt palvelu luottaa varmenteiden myöntäjä. Etsi myöntäjä, joka on myöntänyt asiakkaan sertifikaatteja, joita voidaan käyttää todennustavaksi lataaminen pilvipalvelussa sijaitsevaan siirtymällä julkinen avain.

Jos julkisella avaimella tiedosto ei ole käytettävissä, voit viedä ne säilöstä:

* Etsi varmenne
    * Hae asiakkaan todistus samaan varmenteiden myöntäjä
* Kaksoisnapsauta varmennetta.
* Valitse varmenne-valintaikkuna varmenteiden polku-välilehti.
* Kaksoisnapsauta CA polun.
* Varmenteen ominaisuuksien muistiinpanoja.
* Sulje **varmenne** -valintaikkuna.
* Etsi varmenne
    * Etsi ole mainittu edellä varmenteiden Myöntäjä.
* Valitse Toiminnot -> kaikki tehtävät -> Vie...
* Voit viedä varmenteen tuominen. CER seuraavat vaihtoehdot:
    * **Ei, älä vie yksityinen avain**
    * Sisällytä kaikki varmenteet varmenteiden polku Jos mahdollista.
    * Vie kaikki laajennetut ominaisuudet.

## <a name="upload-ca-certificate-to-cloud-service"></a>Lataa varmenne pilvipalveluun

Lataa varmenteen kanssa nykyisen tai luoda. CER-tiedoston CA julkisella avaimella.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Päivitä CA varmenteen palvelun kokoonpanotiedosto

Päivitä seuraava asetus palvelun määritystiedostossa allekirjoitusarvo on ladattu pilvipalveluun varmenteen allekirjoitus:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

Päivitä seuraavat asetuksen arvoa saman allekirjoitus:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Anna asiakkaan todistuksia

Kunkin henkilön oikeus käyttää palvelua pitäisi olla asiakas todistus his/hers yksityisesti, käytä ja valitse his/hers omista vahva salasana suojaa sen yksityinen avain. 

Seuraavat vaiheet on suoritettava samaan tietokoneeseen, johon itse allekirjoitetun varmenteen Myöntäjän luotu ja tallennettu:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Mukauttaminen:

* n - tunnuksen, joka todennetaan tällä varmenteella asiakkaan kanssa
* e - sertifikaatin vanhentumispäivä kanssa
* MyID.pvk ja MyID.cer tämän Asiakasvarmenne yksilöllinen tiedostonimet kanssa

Tämä komento pyytää salasanan voi luoda ja käyttää sitten kerran. Käytä vahva salasana.

## <a name="create-pfx-files-for-client-certificates"></a>Luo PFX-tiedostot-asiakasohjelman varmenteet

Kunkin luodun Asiakasvarmenne suorittaa:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Mukauttaminen:

    MyID.pvk and MyID.cer with the filename for the client certificate

Kirjoita salasana ja vie tiedot sitten varmenteen vaihtoehdoista:

* Kyllä, vie yksityinen avain
* Vie kaikki laajennetut ominaisuudet
* Henkilö, jolle tämä todistus annetaan kannattaa valita Vie-salasana

## <a name="import-client-certificate"></a>Tuo Asiakasvarmenne

Jokainen henkilö, jolle on myönnetty Asiakasvarmenne tuo hän käyttää pitää yhteyttä palvelun koneet avaimen paria:

* Kaksoisnapsauta. PFX-tiedoston kohdalla Resurssienhallinnassa
* Tuo varmenteen tuominen Omat kansiot-tiedostosta tallentaa vähintään asetus:
    * Sisällytä kaikki laajennetut ominaisuudet, jotka on valittuna

## <a name="copy-client-certificate-thumbprints"></a>Kopioi asiakkaan varmenteen thumbprints
Jokainen henkilö, jolle on myönnetty Asiakasvarmenne on noudattamalla seuraavia ohjeita, jotta voit noutaa his/hers allekirjoitus varmenne, joka lisätään service-kokoonpanotiedosto:
* Suorita certmgr.exe
* Valitse henkilökohtainen-välilehti
* Kaksoisnapsauta Asiakasvarmenne käytettävän todennusta varten
* Varmenne-valintaikkuna, joka avautuu Valitse tiedot-välilehti
* Varmista, että Näytä näyttää kaikki
* Valitse luettelosta allekirjoitus-kenttä
* Kopioi arvo allekirjoitus **ei näy Unicode-merkkejä eteen ensimmäisen numeron poistaminen** poistaa kaikki välilyönnit

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Määritä sallitut asiakkaat palvelun määritystiedostossa

Päivitä seuraava asetus palvelun määritystiedostossa arvo palvelun käyttöoikeus asiakkaan varmenteet thumbprints CSV-luettelo:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Määritä asiakasohjelman varmenteen kumottujen varmenteiden tarkistaminen

Oletusasetus ei tarkista asiakkaan varmenteen voimassaolon tilasta myöntäjä. Jos haluat ottaa tarkistukset, joka on antanut asiakkaan varmenteiden myöntäjä tukeeko tarkastusten, muuta seuraava asetus mainitun X509RevocationMode luettelointi määritetyt arvot:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>Itse allekirjoitetun salausvarmenteita PFX-tiedoston luominen

Salaus-varmenteelle Suorita:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Mukauttaminen:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Kirjoita salasana ja vie tiedot sitten varmenteen vaihtoehdoista:
*    Kyllä, vie yksityinen avain
*    Vie kaikki laajennetut ominaisuudet
*    Tarvitset salasanan ladattaessa varmenne pilvipalveluun.

## <a name="export-encryption-certificate-from-certificate-store"></a>Vie salausvarmenne varmenne-kaupasta

*    Etsi varmenne
*    Valitse Toiminnot -> kaikki tehtävät -> Vie...
*    Voit viedä varmenteen tuominen. PFX-tiedosto, jonka vaihtoehdoista: 
  *    Kyllä, vie yksityinen avain
  *    Jos mahdollista sisällyttää kaikki varmenteet varmenteiden polku 
*    Vie kaikki laajennetut ominaisuudet

## <a name="upload-encryption-certificate-to-cloud-service"></a>Lataa salausvarmenne pilvipalveluun

Lataa varmenteen kanssa nykyisen tai luoda. Salauksen avaimen pari PFX tiedoston:

* Kirjoita yksityisen avaimen tietojen suojaaminen salasanalla

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Päivitä salausvarmenne palvelun määritystiedostossa

Päivitä pilvipalveluun ladattu varmenteen allekirjoitus allekirjoitusarvo palvelun määritystiedostossa seuraavista asetuksista:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Varmenteen Yleiset toiminnot

* SSL-varmenteen määrittäminen
* Määritä asiakkaan varmenteet

## <a name="find-certificate"></a>Etsi varmenne

Noudattamalla seuraavia ohjeita:

1. Suorita mmc.exe.
2. Tiedosto -> Lisää tai poista laajennuksen...
3. Valitse **Varmenteet**.
4. Valitse **Lisää**.
5. Valitse varmenne tallennuspaikan.
6. Valitse **Valmis**.
7. Valitse **OK**.
8. Laajenna **Varmenteet**.
9. Laajenna varmenteen store-solmu.
10. Laajenna varmenteen alisolmu.
11. Valitse varmenne-luettelossa.

## <a name="export-certificate"></a>Varmenteen vieminen
**Ohjattu vientitoiminto varmenteen**:

1. Valitse **Seuraava**.
2. Valitse **Kyllä**, **Vie yksityinen avain**.
3. Valitse **Seuraava**.
4. Valitse haluttu tulos-tiedostomuodossa.
5. Valitse haluamasi asetukset.
6. Valitse **salasana**.
7. Kirjoita vahva salasana ja vahvista se.
8. Valitse **Seuraava**.
9. Tai Selaa tiedostonimi tallennuspaikan varmenne (käytetään. PFX-tunniste).
10. Valitse **Seuraava**.
11. Valitse **Valmis**.
12. Valitse **OK**.

## <a name="import-certificate"></a>Varmenteen tuominen

Ohjatun varmenteen tuominen:

1. Valitse säilön sijainti.

    * Valitse **Nykyisen käyttäjän** , jos vain nykyisen käyttäjän suoritettavat prosessit käyttävät palvelu
    * Valitse **Paikallinen kone** , jos muista prosesseista tämä tietokone käyttää palvelua
2. Valitse **Seuraava**.
3. Jos tuot tiedostosta, Vahvista tiedostopolku.
4. Jos tuomisesta. PFX-tiedosto:
    1.     Kirjoita salasana suojaa yksityinen avain
    2.     Tuontiasetusten valitseminen
5.     Valitse "Sijainti" varmenteet seuraavaan säilöön
6.     Valitse **Selaa**.
7.     Valitse haluamasi säilö.
8.     Valitse **Valmis**.
       
    * Jos Luotetut varmenteiden päämyöntäjän säilö on valittu, valitse **Kyllä**.
9.     Valitse valintaikkunan ikkunoiden **OK** .

## <a name="upload-certificate"></a>Lataa sertifikaatti

[Azure Portal](https://portal.azure.com/)

1. Valitse **pilvipalveluihin**.
2. Valitse pilvipalvelussa.
3. Valitse yläreunan **Varmenteet**.
4. Alapalkissa Valitse **Lähetä**.
5. Valitse varmennetiedosto.
6. Jos kyseessä. PFX tiedosto, kirjoita salasana yksityinen avain.
7. Kun suoritettu, kopioi varmenteen allekirjoitus-luettelossa uusi tapahtuma.

## <a name="other-security-considerations"></a>Muut suojausasiat
 
SSL-asetukset, joka on kuvattu tämän asiakirjan salaa palvelu ja sen asiakkaiden välisen HTTPS-päätepisteen käytettäessä. Tämä on tärkeää tunnistetietojen tietokannan käytön jälkeen ja mahdollisesti muita luottamuksellisia tietoja sisältyvät tietoliikenne. Huomaa kuitenkin, että palvelun jatkuu sisäinen tila-tunnistetiedot, mukaan lukien sen sisäinen taulukoiden Microsoft Azure SQL-tietokantaan, joka on tarkoitettu metatiedon Microsoft Azure-tilaukseesi. Tietokannan on määritetty osana palvelun kokoonpanotiedosto seuraava asetus (. CSCFG-tiedosto): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

Salataan tietokannan tallennettuja tunnistetietoja. Kuitenkin paras käytäntö on varmistettava, että palvelun käyttöönoton Internetin kautta tai työntekijä roolit ovat ajan tasalla ja turvallinen kuin ne molemmat voivat käyttää metatietojen tietokantaa ja varmennetta käytetään salausta ja salauksen tallennettuja tunnistetietoja. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
