<properties
    pageTitle="Laske paljon Java-sovellus AM | Microsoft Azure"
    description="Opettele luomaan Azure virtual machine, joka suoritetaan, Laske paljon Java-sovellus, joka voidaan valvoa toisessa Java-sovelluksessa."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Laske paljon tehtävän suorittaminen Java-virtual tietokoneeseen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Azure voit käsitellä tehtävien suorittaminen paljon virtual machine avulla. Esimerkiksi virtual machine voit käsitellä tehtäviä ja toimittaa tulokset asiakaskoneiden tai mobile-sovellukset. Luettuasi tämän artikkelin on hallitsemista virtual machine, joka suoritetaan, Laske paljon Java-sovellus, joka voidaan valvoa toisen Java-sovelluksen luominen.

Tässä opetusohjelmassa olettaa tietää, miten voit luoda Java console-sovelluksia, tuoda kirjastojen Java-sovellukseen ja luoda Java-arkisto (JAR). Microsoft Azure ei ole kokemusta ei anneta.

Opit seuraavat asiat:

* Voit luoda virtual tietokoneen kanssa Java Development Kit (JDK) asennettuna.
* Voit kirjautua etäyhteyden virtuaalikoneen.
* Palvelun bus nimitilan luomisesta.
* Java-sovellus, joka suorittaa tehtävän suorittaminen paljon luomisesta.
* Voit luoda Java-sovelluksen, joka valvoo Laske paljon tehtävää.
* Voit suorittaa Java-sovelluksia.
* Kuinka voit lopettaa Java-sovellukset.

Tässä opetusohjelmassa käyttää Traveling myyntiyksikköönsä ongelma Laske paljon tehtävää. Seuraavassa on esimerkki Java-sovelluksen suorittaminen paljon tehtävän suorittamiseen.

![Traveling myyntiyksikköönsä ongelma Ratkaisin][solver_output]

Seuraavassa on esimerkki Java-sovelluksen suorittaminen paljon tehtävän seuranta.

![Asiakkaan Traveling myyntiyksikköönsä ongelma][client_output]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Voit luoda virtual machine

1. Kirjaudu sisään [Azure perinteinen portal](https://manage.windowsazure.com).
2. Valitse **Uusi**, valitse **Laske**, valitse **virtuaalikoneen**ja valitse sitten **-Valikoima**.
3. Valitse **virtuaalikoneen kuvan valitseminen** -valintaikkunassa **JDK 7 Windows Server 2012**.
Huomaa, että **JDK 6 Windows Server 2012: ssa** on käytettävissä siltä varalta, että sinulla on vanhoja sovelluksia, jotka eivät ole vielä valmis suorittamaan JDK 7: ssä.
4. Valitse **Seuraava**.
4. **Virtuaalikoneen määritys** -valintaikkunassa:
    1. Määritä virtuaalikoneen nimi.
    2. Määritä käytettävät virtuaalikoneen koko.
    3. Kirjoita järjestelmänvalvojan nimi **Käyttäjänimi** -kenttään. Muista tämä käyttäjänimi ja salasana, voit syöttää seuraava, voit käyttää niitä kirjautumisen etäyhteyden välityksellä, virtuaalikoneen.
    4. Kirjoita **Uusi salasana** -kenttään salasana ja kirjoita se uudelleen **Vahvista** -kenttään. Tämä on järjestelmänvalvojan tilin salasana.
    5. Valitse **Seuraava**.
5. Seuraava **virtuaalikoneen määritys** -valintaikkunassa:
    1. **Pilvipalvelussa**Käytä oletusarvoa **Luo uusi pilvipalvelussa**.
    2. **Cloud palvelun DNS-nimi** -arvo on oltava yksilöllinen cloudapp.net. Voit tarvittaessa muokata tätä arvoa niin, että Azure ilmaisee on yksilöllinen.
    2. Määritä alue, affiniteetti ryhmän tai VPN. Määrittää alueen, kuten **Länsi US**tarkoitetaan tässä opetusohjelmassa.
    2. **Tallennustilan tilin**Valitse **automaattisesti luotu tallennustilan tilin käyttäminen**.
    3. **Saatavuus**Valitse **(ei mitään)**.
    4. Valitse **Seuraava**.
5. Valitse lopullisessa **virtuaalikoneen määritys** -valintaikkunassa:
    1. Hyväksy oletusarvot päätepiste.
    2. Valitse **Valmis**.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>Voit kirjautua etäyhteyden virtuaalikoneen

1. Kirjaudu [Azure perinteinen portal](https://manage.windowsazure.com).
2. Valitse **näennäiskoneiden**.
3. Napsauta nimeä, jonka haluat kirjautua virtuaalikoneen.
4. Valitse **Muodosta yhteys**.
5. Vastaa näyttöön muodostaa virtuaalikoneen tarvittaessa. Järjestelmänvalvojan käyttäjänimeä ja salasanaa pyydettäessä Käytä yhteydessä luomasi virtuaalikoneen annettuja arvoja.

Huomaa, että Azure palvelun Bus toiminnot vaativat asennetaan osana oman JRE **cacerts** kaupan Baltimore CyberTrust pääkansio-sertifikaatti. Tämän todistuksen sisältyy automaattisesti-Java Runtime ympäristössä (JRE) tässä opetusohjelmassa käyttämä. Jos ei ole tämän todistuksen JRE **cacerts** -kaupassa, katso [Java CA varmenteen tallentaa varmenteen lisääminen] [ add_ca_cert] tietoja lisäämällä se (sekä lisätietoja tarkastelemisesta varmenteet cacerts kaupan).

## <a name="how-to-create-a-service-bus-namespace"></a>Palvelun bus nimitilan luominen

Kun haluat käyttää palvelun Bus olevien Azure, sinun on luotava palvelun nimitila. Palvelun nimitila on säilöön osoitteiden palvelun Bus resurssien sovelluksessa.

Voit luoda palvelun nimitila seuraavasti:

1.  Kirjaudu [Azure perinteinen portal](https://manage.windowsazure.com).
2.  Valitse vasemmassa siirtymisruudussa Azure perinteinen portaalin **palvelun Bus, käyttöoikeuksien hallinta ja välimuisti**.
3.  Azure perinteinen portal-vasemmassa reunassa **Palvelun Bus** -solmu ja valitse sitten **Uusi** -painiketta.  
    ![Näyttökuva palvelun Bus solmu][svc_bus_node]
4.  Kirjoita **Luo uusi palvelu Namespace** -valintaikkunassa **Namespace**ja napsauta voit varmistaa, että se on yksilöllinen, **Tarkista saatavuus** -painiketta.  
    ![Luo uusi Namespace näyttökuva][create_namespace]
5.  Kun varmistetaan, että nimitilan nimi on käytettävissä, valitse maa tai alue, jossa oman nimitilan on isännöitävä ja valitse sitten **Luo Namespace** -painiketta.  

    Nimitilan, jonka loit näkyvät Azure perinteinen portaalin ja kestää hetken aktivoimiseen. Odota, kunnes tila on **aktiivinen** ennen seuraavaan vaiheeseen jatkamista.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>Hanki hallinta tunnistetietojen oletusarvoinen nimitila

Jotta voit suorittaa hallinta, kuten luominen jono, uusi nimitilan haluat saada nimitilan tunnistetietojen hallinta.

1.  Valitse vasemmassa siirtymisruudussa **Palvelun Bus** solmu käytettävissä nimitilan luettelo.
    ![Käytettävissä olevat nimitilan näyttökuva][avail_namespaces]
2.  Valitse juuri luomasi luettelossa näkyvää nimitila.
    ![Näyttökuva Namespace luettelo][namespace_list]
3.  Oikeanpuoleinen **Ominaisuudet** -ruudussa luetellaan uusi nimitilan ominaisuudet.
    ![Näyttökuva ominaisuudet-ruutu][properties_pane]
4.  **Oletus-näppäin** on piilotettu. Napsauta Näytä suojausvaltuudet **näkymä** -painiketta.
    ![Oletusarvoinen avain näyttökuva][default_key]
5.  Pane merkille **Oletus myöntäjä** ja **Oletusarvo-näppäintä** , kun käytät tätä seuraavien tietojen suoritettava nimitilaa toimintoja.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Java-sovellus, joka suorittaa Laske paljon tehtävän luominen

1. (Joka ei tarvitse olla virtuaalikoneen, jonka loit) kehittäminen käyttämääsi laitteeseen Lataa [Azure SDK Java](https://azure.microsoft.com/develop/java/).
2. Luo Java console-sovellus, joka käyttää esimerkkikoodi tämän osan loppuun. Tässä opetusohjelmassa Luomme erityistekniikoita käyttämällä **TSPSolver.java** Java tiedoston nimeksi. Muokkaa **oman\_palvelun\_bus\_nimitilan**, **oman\_palvelun\_bus\_omistaja**, ja **oman\_palvelun\_bus\_avaimen** paikkamerkit käyttämään palvelua bus **nimitilan**, **Oletus myöntäjä** ja **Oletus avaimen** arvot, vastaavassa järjestyksessä.
3. Jälkeen coding-sovelluksen suorituskelpoiset Java-arkisto (JAR) vienti ja pakata tarvittavat kirjastojen luotu PURKKIIN kyselyjä. Tässä opetusohjelmassa luotua PURKKI nimeksi Käytämme **TSPSolver.jar** .

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>Java-sovellus, joka valvoo paljon Laske tehtävän luominen

1. Kehittäminen tietokoneessa Luo Java console-sovellus, joka käyttää esimerkkikoodi tämän osan loppuun. Tässä opetusohjelmassa Luomme erityistekniikoita käyttämällä **TSPClient.java** Java tiedoston nimeksi. Aiemmin esitetyllä muokkaaminen **oman\_palvelun\_bus\_nimitilan**, **oman\_palvelun\_bus\_omistaja**, ja **oman\_palvelun\_bus\_avaimen** paikkamerkit käyttämään palvelua bus **nimitilan**, **Oletus myöntäjä** ja **Oletus avaimen** arvot, vastaavassa järjestyksessä.
2. Sovelluksen suorituskelpoiset PURKKI vienti ja pakata tarvittavat kirjastojen luotu PURKKIIN kyselyjä. Tässä opetusohjelmassa luotua PURKKI nimeksi Käytämme **TSPClient.jar** .

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Java-sovellusten suorittaminen
Laske paljon-sovelluksen ensimmäisen kerran, jos haluat luoda jonon, sitten matkustaminen Saleseman ongelman ratkaisemiseen, joka lisää nykyisen parhaan reitin palvelun bus jonossa käyttämiseen. Kun Laske paljon-sovellus on käynnissä (tai jälkeenpäin) suorittaa asiakkaan palvelun bus jonossa tulosten esittämistä varten.

### <a name="to-run-the-compute-intensive-application"></a>Laske paljon-sovelluksen käyttämiseen

1. Kirjaudu virtuaalikoneen.
2. Luo kansio, jossa sovellus suoritetaan. Esimerkiksi **c:\TSP**.
3. Kopioi **TSPSolver.jar** **c:\TSP**
4. Luo seuraavat sisällön **c:\TSP\cities.txt** -tiedosto.

        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33

5. Komentokehotteeseen muuta kansioita c:\TSP.
6. Varmista, JRE bin-kansio on PATH-ympäristömuuttuja.
7. Tarvitset palvelun bus jonossa ennen kuin suoritat TSP Ratkaisimen permutaatioiden luomiseen. Suorita seuraava komento palvelun bus jonossa luomiseen.

        java -jar TSPSolver.jar createqueue

8. Jonossa on luotu, voit suorittaa TSP Ratkaisimen permutaatioiden. Esimerkiksi suorittamalla seuraavan komennon toimimaan 8 kaupunkien Ratkaisimen.

        java -jar TSPSolver.jar 8

 Jos et määritä luvuksi, se suoritetaan 10 kaupunkien. Ratkaisimen löytää nykyisen lyhyimmän tiet, kun se lisää ne jonossa.

> [AZURE.NOTE]
> Mitä suurempi luku, voit määrittää enää Ratkaisimen suoritetaan. Käynnissä esimerkiksi 14 kaupunkien voi kestää useita minuutteja ja 15 kaupunkien suorittaminen voi kestää useita tunteja. Lisääntyvien 16 tai useampia kaupunkeja saattaa johtaa päivinä Runtimen (myöhemmin viikkoa, kuukautta ja vuotta). Tämä on nopea lisäys tulkinnut Ratkaisimen kaupunkien kasvaa lukumääränä permutaatioiden määrä.

### <a name="how-to-run-the-monitoring-client-application"></a>Seurannan asiakassovellus suorittaminen
1. Kirjaudu tietokoneesta, jossa suoritat asiakassovellus. Tämä ei tarvitse olla samassa tietokoneessa, jossa **TSPSolver** -sovellusta, vaikka se voi olla.
2. Luo kansio, jossa sovellus suoritetaan. Esimerkiksi **c:\TSP**.
3. Kopioi **TSPClient.jar** **c:\TSP**
4. Varmista, JRE bin-kansio on PATH-ympäristömuuttuja.
5. Komentokehotteeseen muuta kansioita c:\TSP.
6. Suorita seuraava komento.

        java -jar TSPClient.jar

    Voit myös määrittää lepotilaan välissä jonon tarkistamisessa komentorivin argumentit kulkee minuuttimäärä. Nukkumaanmenoaikojen oletusaika tarkistuksen jonossa on 3 minuuttia, jota käytetään, jos komentorivin argumenttia siirretään **TSPClient**. Jos haluat käyttää eri arvon nukkumaanmenoaikojen aikaväli, kuten minuutin, suorita seuraava komento.

        java -jar TSPClient.jar 1

    Asiakkaan ovat käytössä, kunnes se näkee jonon sanoman "Valmis". Huomaa, että jos suoritat Ratkaisimen useita kertoja ilman asiakasohjelmaa, joudut ehkä suorittaa asiakkaan tyhjentää kokonaan jonossa useita kertoja. Vaihtoehtoisesti voit poistaa jonossa ja luo uudelleen. Poista jono varten suorittamalla seuraavan komennon **TSPSolver** (ei **TSPClient**).

        java -jar TSPSolver.jar deletequeue

    Ratkaisimen ovat käytössä, kunnes kaikki tiet käsittelystä on valmis.

## <a name="how-to-stop-the-java-applications"></a>Kuinka voit lopettaa Java-sovellukset
Sekä Ratkaisimen että asiakassovellukset voit painaa **Ctrl + C** Lopeta, jos haluat lopettaa ennen valmistumista.


[solver_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../java-add-certificate-ca-store.md
