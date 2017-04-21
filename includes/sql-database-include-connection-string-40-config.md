
<!--
includes/sql-database-include-connection-string-40-config.md

Latest Freshness check:  2015-09-04 , GeneMi.

## Connection string
-->


### <a name="example-config-file-for-connection-string-security"></a>Esimerkki määritystiedosto yhteyden merkkijonon arvopaperille.


On unsound Aseta yhteysmerkkijonon literaaleina C#-koodin. On parempi sijoittaa yhteysmerkkijonon määritystiedostossa. Voit muokata merkkijonon tahansa ilman, että Käännä.

Oletetaan käännetty C# ohjelma nimeltä **ConsoleApplication1.exe**ja että tätä .exe sijaitsee **bin\debug\* * hakemisto.

Tässä esimerkissä yhteysmerkkijono useimmat osista on tallennettu nimeltä täsmälleen **ConsoleApplication1.exe.config**määritystiedostossa. Tämä määritystiedosto on oltava asennettuna **bin\debug\**.

Näet seuraavat määritystiedoston XML nimeltä **ConnectionString4NoUserIDNoPassword**yhteysmerkkijonon. C#-koodin etsii merkkijonon.

Muokkaa reaali nimet paikkamerkit:

- {your_serverName_here}
- {your_databaseName_here}



        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
        
            <connectionStrings>
                <clear />
                <add name="ConnectionString4NoUserIDNoPassword"
                providerName="System.Data.ProviderName"
        
                connectionString=
                "Server=tcp:{your_serverName_here}.database.windows.net,1433;
                Database={your_databaseName_here};
                Connection Timeout=30;
                Encrypt=True;
                TrustServerCertificate=False;" />
            </connectionStrings>
        </configuration>



Tämä kuva Microsoft valitsi pois kaksi parametria:

- Käyttäjätunnus = {your_userName_here};
- Salasanan = {your_password_here};


Voit sisällyttää ne, mutta joskus kannattaa on ohjelmasi Hae näppäimistöön syötetyistä käyttäjän kyseiset arvot. Se on riippuvainen.



<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
