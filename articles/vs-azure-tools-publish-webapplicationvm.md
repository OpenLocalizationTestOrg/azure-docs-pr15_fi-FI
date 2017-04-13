<properties
   pageTitle="Julkaise WebApplicationVM | Microsoft Azure"
   description="Opi ottamaan verkkosovelluksen virtual koneeseen. Tämä komentosarja luo tarvittavat resurssit Azure-tilaukseesi, jos niitä ei ole olemassa."
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

# <a name="publish-webapplicationvm-windows-powershell-script"></a>Julkaise-WebApplicationVM (Windows PowerShell-komentosarjaa)

Ottaa käyttöön web-sovelluksen virtual koneeseen. Komentosarja luo tarvittavat resurssit Azure-tilaukseesi, jos niitä ei ole olemassa.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Määritys

JSON-kokoonpanotiedosto, joka kuvaa käyttöönoton tietoja polku.

|Tunnukset|ei mitään|
|---|---|
|Pakollinen?|TOSI|
|Sijainti|nimetty|
|Oletusarvo|ei mitään|
|Hyväksy myyntijakso syötteen?|EPÄTOSI|
|Hyväksy yleismerkkejä?|EPÄTOSI|

### <a name="subscriptionname"></a>SubscriptionName

Azure tilaus, johon haluat luoda virtuaalikoneen nimi.

|Tunnukset|ei mitään|
|---|---|
|Pakollinen?|EPÄTOSI|
|Sijainti|nimetty|
|Oletusarvo|Käyttää ensimmäisen tilauksen tilauksen-tiedostossa|
|Hyväksy myyntijakso syötteen?|EPÄTOSI|
|Hyväksy yleismerkkejä?|EPÄTOSI|

### <a name="webdeploypackage"></a>WebDeployPackage

Web-käyttöönottopaketti virtuaalikoneen julkaiseminen polku. Voit luoda tämän paketin käyttämällä ohjattua Julkaise Visual Studiossa. Katso [Toimintaohje: Web käyttöönottopaketti luominen Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

|Tunnukset|ei mitään|
|---|---|
|Pakollinen?|EPÄTOSI|
|Sijainti|nimetty|
|Oletusarvo|ei mitään|
|Hyväksy myyntijakso syötteen?|EPÄTOSI|
|Hyväksy yleismerkkejä?|EPÄTOSI|

### <a name="allowuntrusted"></a>AllowUntrusted

Jos arvo on TOSI, sallittua käyttää sertifikaatteja, joita ei ole kirjautunut luotetun varmenteiden myöntäjä.

|Tunnukset|ei mitään|
|---|---|
|Pakollinen?|EPÄTOSI|
|Sijainti|nimetty|
|Oletusarvo|EPÄTOSI|
|Hyväksy myyntijakso syötteen?|EPÄTOSI|
|Hyväksy yleismerkkejä?|EPÄTOSI|

### <a name="vmpassword"></a>VMPassword

Virtuaalikoneen tilin tunnistetiedot. Esimerkki: - VMPassword @{Name = "järjestelmänvalvojan"; Salasanan = "salasana"}

|Tunnukset|ei mitään|
|---|---|
|Pakollinen?|EPÄTOSI|
|Sijainti|nimetty|
|Oletusarvo|ei mitään|
|Hyväksy myyntijakso syötteen?|EPÄTOSI|
|Hyväksy yleismerkkejä?|EPÄTOSI|

### <a name="databaseserverpassword"></a>DatabaseServerPassword

Tunnistetietojen Azure SQL-tietokantaan. Esimerkki: - DatabaseServerPassword @{Name = "järjestelmänvalvojan"; Salasanan = "salasana"}

|Tunnukset|ei mitään|
|---|---|
|Pakollinen?|EPÄTOSI|
|Sijainti|nimetty|
|Oletusarvo|ei mitään|
|Hyväksy myyntijakso syötteen?|EPÄTOSI|
|Hyväksy yleismerkkejä?|EPÄTOSI|

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Jos arvo on TOSI, Tulosta viestit komentosarja, tulostus-muodossa.

|Tunnukset|ei mitään|
|---|---|
|Pakollinen?|EPÄTOSI|
|Sijainti|nimetty|
|Oletusarvo|EPÄTOSI|
|Hyväksy myyntijakso syötteen?|EPÄTOSI|
|Hyväksy yleismerkkejä?|EPÄTOSI|

## <a name="remarks"></a>Huomautuksia

Valmis selvitys siitä, miten komentosarjan avulla voit luoda keskihajonta ja testaa ympäristössä, katso [Windows PowerShell-komentosarjojen käyttämällä keskihajonta ja testaa ympäristöissä Julkaise](vs-azure-tools-publishing-using-powershell-scripts.md).

JSON-kokoonpanotiedosto määrittää, mikä on otettava käyttöön tietoja. Se sisältää tiedot, joka on määritetty projekti, kuten nimi, affiniteetti ryhmän, Näennäiskiintolevyn kuva ja koon virtuaalikoneen luotaessa. Se myös sisältää virtuaalikoneen valmistelu-tietokannat päätepisteet mahdollisesti ja WWW-käyttöönoton parametrit. Seuraava koodi näkyy esimerkki JSON-kokoonpanotiedosto:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Voit muokata JSON-kokoonpanotiedosto Jos haluat muuttaa, mikä on valmisteltu. Virtual machine ja pilvipalveluun ovat pakollisia, mutta tietokanta-osa on valinnainen.
