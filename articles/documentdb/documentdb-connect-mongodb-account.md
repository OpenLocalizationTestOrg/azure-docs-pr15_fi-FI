<properties 
    pageTitle="Muodostaa yhteyden DocumentDB tiliin protokolla tukee MongoDB | Microsoft Azure" 
    description="Opettele muodostamaan protokolla tukee nyt käytettävissä esikatselu MongoDB DocumentDB-tiliin. Muodosta MongoDB yhteysmerkkijonoa käyttämällä." 
    keywords="mongodb yhteysmerkkijonon"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="how-to-connect-to-a-documentdb-account-with-protocol-support-for-mongodb"></a>Voit muodostaa yhteyden DocumentDB tiliin MongoDB protokollan tuki

Opettele muodostamaan MongoDB käyttämällä vakio MongoDB yhteysmerkkijonon URI muoto tukee protokolla Azure DocumentDB-tiliin.  

## <a name="get-the-accounts-connection-string-information"></a>Hae asiakkaan yhteysmerkkijonon tiedot

1. Uudessa ikkunassa Kirjaudu [Azure-portaalissa](https://portal.azure.com).
2. Valitse tili-sivu näkyvä **Vasen siirtymispalkki** **Yhteysmerkkijonon**. Voit siirtyä **Tili-sivu**, valitse Jumpbar valitsemalla **Lisää palveluita**, **DocumentDB (NoSQL)** ja valitse protokolla tukee MongoDB DocumentDB-tili.

    ![Näyttökuva kaikki asetukset-sivu](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. **Yhteysmerkkijonon tiedot** -sivu avautuu ja siinä on kaikki ohjaimen käytön MongoDB, mukaan lukien valmiiksi rakennettava yhteysmerkkijonon tiliin yhdistämistä varten tarvittavat tiedot.

    ![Näyttökuva yhteyden merkkijono-sivu](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Yhteyden merkkijonon vaatimukset

On tärkeää muistaa, että DocumentDB tukee vakio MongoDB yhteysmerkkijonon URI muoto, tarpeita vaivattomasti: DocumentDB tilit Vaadi todennus ja suojattu tietoliikenne SSL kautta.  Näin ollen yhteysmerkkijonon muoto on:

    mongodb://username:password@host:port/[database]?ssl=true

Jos merkkijonon arvot ovat saatavissa yllä yhteysmerkkijono-sivu.

- Käyttäjänimi (pakollinen)
    - DocumentDB tilin nimi
- Salasana (pakollinen)
    - DocumentDB tilin salasana
- Host (pakollinen)
    - FQDN DocumentDB tili
- Port (pakollinen)
    - 10250
- Tietokannan (valinnainen)
    - Yhteyden käyttämän oletusarvo-tietokanta
- SSL = true (pakollinen)

Esimerkkinä tili yllä yhteysmerkkijonon tiedot näkyvät.  Kelvollinen yhteysmerkkijono on:
    
    mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## <a name="connecting-with-the-c-driver-for-mongodb"></a>Yhteyden muodostaminen MongoDB C#-ohjaimella
Kuten edellä kaikki DocumentDB tilit edellyttävät todennus- ja suojattu tietoliikenne kautta SSL. Kun MongoDB yhteysmerkkijonon URI muoto tukee SSL-protokollan = tosi kyselymerkkijonoparametrin, toimi kanssa MongoDB C# ohjaimen edellyttää käyttöä MongoClientSettings-objekti MongoClient luotaessa.  Valita yllä tilitiedot, seuraavat koodikatkelman näyttää Yhdistä tiliin ja "Tehtävät"-tietokannan kanssa.

            MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
    

## <a name="next-steps"></a>Seuraavat vaiheet


- Lue, miten DocumentDB tilillä protokollan kanssa [käyttämällä MongoChef](documentdb-mongodb-mongochef.md) tuki MongoDB.
- Tutustu DocumentDB protokollatuki MongoDB [esimerkkejä, joiden](documentdb-mongodb-samples.md)kanssa.

 
