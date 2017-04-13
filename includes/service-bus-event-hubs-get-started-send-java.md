## <a name="send-messages-to-event-hubs"></a>Tapahtuman porttiin lähettämiseen

Tapahtuman keskittimien Java asiakas-kirjasto on käytettävissä maven-testi projekteista [Maven-testi keskitettyyn säilöön](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)ja voidaan viitata käyttämällä sisällä maven-testi projektitiedoston riippuvuuden seuraava ilmoitus:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Erityyppisten muodosta ympäristöissä voi hankkia uusimmat julkaistut PURKKI-tiedostoja erikseen [Maven-testi keskitettyyn säilöön](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) tai [GitHub release jakauman-kohtaa](https://github.com/Azure/azure-event-hubs/releases).  

Yksinkertainen tapahtuman, julkaisijan tuoda tapahtuman keskittimet asiakkaan luokkien *com.microsoft.azure.eventhubs* pakkaaminen ja *com.microsoft.azure.servicebus* pakkaaminen apuohjelman luokat, kuten yleisiä poikkeuksia, jotka on jaettu Azure palvelun Bus sähköpostiviestinnän avulla. 

Seuraavassa esimerkissä Luo uuden maven-testi projektin console/shell-sovelluksen ensin tuttuja Java-kehitysympäristö. Luokan kutsutaan ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Korvaa nimitilan ja tapahtumaa-toiminnossa nimiä luotaessa tapahtumaa-toiminnossa käytettävät arvot.

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Luo yksittäinen tapahtuma sitten poistamalla merkkijonon kyselyjä sen UTF-8-tavuinen-koodausta. Olemme Luo uuden tapahtuman keskittimet asiakkaan esiintymän yhteysmerkkijonon ja Lähetä viesti.   

``` Java 
                
    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);
    
    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 
