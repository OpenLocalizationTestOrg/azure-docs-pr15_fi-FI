<properties
   pageTitle="Julkaise-WebApplicationWebSite (Windows PowerShell-komentosarjaa) | Microsoft Azure"
   description="Opettele web projektin julkaiseminen Azure verkkosivustosta. Tämä komentosarja luo tarvittavat resurssit Azure-tilaukseesi, jos niitä ei ole olemassa."
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

# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Julkaise-WebApplicationWebSite (Windows PowerShell-komentosarjaa)

##<a name="syntax"></a>Syntaksi

Web-projekti julkaisee Azure sivustoon. Komentosarja luo tarvittavat resurssit Azure-tilaukseesi, jos niitä ei ole olemassa.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Määritys

JSON-kokoonpanotiedosto, joka kuvaa käyttöönoton tietoja polku.

|Parametri|Oletusarvo|
|---|---|
|Tunnukset|ei mitään|
|Pakollinen?|TOSI|
|Sijainti|nimetty|
|Oletusarvo|ei mitään|
|Hyväksy myyntijakso syötteen?|EPÄTOSI|
|Hyväksy yleismerkkejä?|EPÄTOSI|

## <a name="subscriptionname"></a>SubscriptionName

Azure tilaus, jonka haluat luoda sivustosi nimi.

|Parametri|Oletusarvo|
|---|---|
|Tunnukset|ei mitään|
|Pakollinen?|EPÄTOSI|
|Sijainti|nimetty|
|Oletusarvo|ei mitään|
|Hyväksy myyntijakso syötteen?|EPÄTOSI|
|Hyväksy yleismerkkejä?|EPÄTOSI|

## <a name="webdeploypackage"></a>WebDeployPackage

Sivuston julkaiseminen web-käyttöönottopaketti polku. Voit luoda tämän paketin käyttämällä ohjattua Julkaise Visual Studiossa. Lisätietoja on artikkelissa [Azure pilvipalveluihin ja ASP.NET käytön aloittaminen](http://go.microsoft.com/fwlink/p/?LinkID=623089).

|Parametri|Oletusarvo|
|---|---|
|Tunnukset|ei mitään|
|Pakollinen?|EPÄTOSI|
|Sijainti|nimetty|
|Oletusarvo|ei mitään|
|Hyväksy myyntijakso syötteen?|EPÄTOSI|
|Hyväksy yleismerkkejä?|EPÄTOSI|

## <a name="databaseserverpassword"></a>DatabaseServerPassword

Käyttäjänimi ja salasana Azure SQL-tietokantaan.

|Parametri|Oletusarvo|
|---|---|
|Tunnukset|ei mitään|
|Pakollinen?|EPÄTOSI|
|Sijainti|nimetty|
|Oletusarvo|ei mitään|
|Hyväksy myyntijakso syötteen?|EPÄTOSI|
|Hyväksy yleismerkkejä?|EPÄTOSI|

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Jos arvo on TOSI, Tulosta viestit komentosarja, tulostus-muodossa.

|Parametri|Oletusarvo|
|---|---|
|Tunnukset|ei mitään|
|Pakollinen?|EPÄTOSI|
|Sijainti|nimetty|
|Oletusarvo|EPÄTOSI|
|Hyväksy myyntijakso syötteen?|EPÄTOSI|
|Hyväksy yleismerkkejä?|EPÄTOSI|

## <a name="remarks"></a>Huomautuksia

Valmis selvitys siitä, miten komentosarjan avulla voit luoda keskihajonta ja testaa ympäristössä, katso [Windows PowerShell-komentosarjojen käyttämällä keskihajonta ja testaa ympäristöissä Julkaise](vs-azure-tools-publishing-using-powershell-scripts.md).

JSON-kokoonpanotiedosto määrittää, mikä on otettava käyttöön tietoja. Se sisältää tiedot, että olet määrittänyt luotaessa projektin, kuten nimi ja verkkosivuston käyttäjänimi. Se sisältää tietokannan valmistelu, myös mahdolliset. Seuraava koodi näkyy esimerkki JSON-kokoonpanotiedosto:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Voit muokata JSON-kokoonpanotiedosto Jos haluat muuttaa, mikä on otettu käyttöön. Sivuston osan tarvitaan, mutta tietokanta-osa on valinnainen.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on kohdassa [Julkaise-WebApplicationVM (Windows PowerShell-komentosarjaa)](vs-azure-tools-publish-webapplicationvm.md)
