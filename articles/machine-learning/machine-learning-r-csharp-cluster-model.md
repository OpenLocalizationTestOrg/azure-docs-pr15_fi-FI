<properties 
    pageTitle="Klusterin mallin | Microsoft Azure" 
    description="Klusterin malli" 
    services="machine-learning" 
    documentationCenter="" 
    authors="FrancescaLazzeri" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="lazzeri"/> 


#<a name="cluster-model"></a>Klusterin malli    

Miten Microsoft ennustaa luottotietojen cardholders toiminnan ryhmät jotta luottokortin myöntäjistä maksu käytöstä riskien? Miten on Määritä ryhmät joka piirteitä työntekijöiden, jotta voit parantaa suorituskykyä käytössä? Miten voit lääkärien luokitella potilaille ryhmiin, niiden palstassa ominaisuuksien perusteella? Valitse tarpeen kaikki näihin kysymyksiin voida vastata klusterin analyysin.   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
Klusterin analyysi luokittelee huomioita ryhmiin vähintään kaksi toisensa poissulkevia Tuntematon perustuvan muuttujien joukko. Klusterin analyysi tarkoituksena on saat tietää, järjestelmä huomioita, yleensä henkilöiden tai niiden ominaisuuksien järjestäminen ryhmiin, jossa ryhmissä jakaa ominaisuudet yhteiset. Tämä [palvelu](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) käyttää K tarkoittaa kehitysmenetelmistä, yleisesti käytettyjen klusteroinnin tekniikka klusteri satunnaisia tietoja ryhmiin. Verkkosovelluksen tulee tiedot ja k määrän klusterit syötteenä ja tuottaa ennusteiden, jonka k-ryhmät, joihin kunkin huomioita kuuluu. 

>Verkkosovelluksen käyttäjät – läpi mahdollisesti mobiilisovelluksessa-sivuston kautta tai paikallisesti tietokoneeseen, voi olla kulutettu esimerkiksi. Mutta WWW-palvelun tarkoituksena on myös perusarvoksi on esimerkki siitä, miten Azure koneen Learning avulla voidaan luoda verkkopalvelut R koodin päälle. Todella R koodin rivit ja napsauttaa hiiren napsautuksella sisällä Azure koneen Learning Studio kokeen voi luoda R koodilla ja julkaistaan verkkopalvelun. Web-palvelu voi julkaista Azure Marketplacesta ja kulutettu käyttäjille ja laitteille kaikkialla maailmassa ilman infrastruktuurin asetuksia WWW-palvelun tekijän mukaan.  

##<a name="consumption-of-web-service"></a>WWW-palvelun kulutus   
Verkkosovelluksen ryhmittelee tiedot k ryhmäjoukossa kyselyjä ja tulostaa ryhmämäärityksen kullekin riville. Web-palvelu odottaa, että käyttäjä voi syötetietoja merkkijonona, jossa rivit erotetaan toisistaan pilkulla (,) ja sarakkeet on erotettu toisistaan puolipisteillä (;). Web-palvelu odottaa 1 rivi kerrallaan. Esimerkki tietojoukko näyttää tältä:

![Mallitiedot][1]

Oletetaan, että käyttäjän määrittää erottelemiseen tiedoista 3 toisensa poissulkevia ryhmiin. Kirjoita yllä tietojoukko olla seuraavat tiedot: arvo = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". Tulos on budjetoidut Ryhmäjäsenyyden kunkin rivit.

>Tämän palvelun, kuten isännöimät Azure Marketplace-sivustoon, on OData-palvelu. Nämä voidaan kutsua viestin tai HAE menetelmiä kautta. 

On useita tapoja muissa palvelun automaattinen tavalla (esimerkiksi-sovellus on [tähän](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Aloitetaan C#-koodin WWW-palvelun käyttöön:

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




##<a name="creation-of-web-service"></a>WWW-palvelun luominen  
>Web-palvelu on luotu käyttämällä Azure koneen Learning. Maksuttoman kokeiluversion käyttäjäksi, sekä esittelyvideot kokeissa ja [julkaisun verkkopalvelut](machine-learning-publish-a-machine-learning-web-service.md)luomisesta Katso [azure.com/ml](http://azure.com/ml). Alla on kokeilla, joka on luotu web-palvelu ja Esimerkki koodi kunkin kokeen sisällä moduulit näyttökuva.

Azure koneen Learning sisällä uusi tyhjä kokeen on luotu ja kaksi [R-komentosarjan suorittaminen] [ execute-r-script] moduulit vedetään työtilan sivulle. Tietojen rakenne on luotu yksinkertaista [R-komentosarjan suorittaminen][execute-r-script]. Valitse sitten tietomallin linkitetty klusterin malli-osassa, luoda uudelleen, [R-komentosarjan suorittaminen][execute-r-script]. [Suorita R komentosarjan] [ execute-r-script] käytetään klusterin mallin, WWW-palvelun sitten käyttämällä "k tarkoittaa"-funktiota, joka on valmiiksi kyselyjä [R-komentosarjan suorittaminen] [ execute-r-script] Azure koneen opiskelun.    
   

     
![Kokeen työnkulku][3]

####<a name="module-1"></a>Moduuli 1: 
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)
    
    maml.mapOutputPort("mydata");     
    

####<a name="module-2"></a>Moduulin 2:
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##<a name="limitations"></a>Rajoitukset
Tämä on kuvattu yksinkertainen Esimerkki klusteroinnin verkkopalvelun. Kun näkevät edellä olevassa esimerkissä koodista, ei ole virhe pyytämisestä ei käytössä ja palvelun oletetaan, että kaikki on jatkuva muuttuja (ei categorical ominaisuuksia sallittu), lukuarvoina-palvelua vain syötteiden verkkosovelluksen luomisen yhteydessä. Lisäksi palvelun tällä hetkellä kahvat rajoitettu arvopisteiden koko määräpäivä pyynnön ja vastauksen laatu web-Palvelukutsu ja mallin on parhaillaan Sovita aina WWW-palvelun kutsutaan kertoma. 

##<a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
Usein kysytyt kysymykset-kulutus verkkopalvelun tai julkaiseminen Azure Marketplace-kohdassa [tähän](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
