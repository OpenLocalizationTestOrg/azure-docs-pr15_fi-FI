<properties 
    pageTitle="Ennusteiden: Autoregressive integroitu liukuvan keskiarvon (ARIMA) | Microsoft Azure" 
    description="Ennusteiden - Autoregressive integroitu liukuvan keskiarvon (ARIMA)" 
    services="machine-learning" 
    documentationCenter="" 
    authors="yijichen" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="yijichen"/> 

 
#<a name="forecasting---autoregressive-integrated-moving-average-arima"></a>Ennusteiden - Autoregressive integroitu liukuvan keskiarvon (ARIMA)

Tämä [palvelu]( https://datamarket.azure.com/dataset/aml_labs/arima) toteuttaa Autoregressive integroitu siirtäminen keskiarvo (ARIMA) tuottamaan ennusteiden käyttäjän määrittämä historiatiedoissa perusteella. Kasvattaa tietyn tuotteen demand tänä vuonna? Voit voin ennustaa Omat Tuotemyynti joulu Vuodenaika-kentät, niin, että voin suunnitella tehokkaasti omat varaston? Ennustaminen mallit ovat piharakennus sähköpostiosoite, esimerkiksi kysymyksiä. Valita aiempiin tietoihin, näistä malleista tarkastaa piilotetut trendejä ja kausivaihtelu ennustaa tulevia trendejä. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

>Verkkosovelluksen käyttäjät – läpi mahdollisesti mobiilisovelluksessa-sivuston kautta tai paikallisesti tietokoneeseen, voi olla kulutettu esimerkiksi. Mutta WWW-palvelun tarkoituksena on myös perusarvoksi on esimerkki siitä, miten Azure koneen Learning avulla voidaan luoda verkkopalvelut R koodin päälle. Todella R koodin rivit ja napsauttaa hiiren napsautuksella sisällä Azure koneen Learning Studio kokeen voi luoda R koodilla ja julkaistaan verkkopalvelun. Web-palvelu voi julkaista Azure Marketplacesta ja kulutettu käyttäjille ja laitteille kaikkialla maailmassa ilman infrastruktuurin asetuksia WWW-palvelun tekijän mukaan.

##<a name="consumption-of-web-service"></a>WWW-palvelun kulutus 

Tämä palvelu hyväksyy 4 argumentit ja laskee ARIMA ennusteiden.
Syötteen argumentit ovat:

* Taajuus - osoittaa, kuinka usein raaka tiedot (päivittäin tai viikoittain tai kuukausittain/vuosineljännes/vuosittain).
* Horisontti - tuleviin ennusteen ajassa.
* Päivämäärä - Lisää uusi aika sarjan tietojen kerran.
* Arvo - kohdassa uusi aika sarjojen tietoarvojen Lisää.

Palvelun tulos on laskettu ennusteen arvot. 

Esimerkki syötteen voi olla: 

* Taajuus - 12
* Horisontti - 12
* Päivämäärä - 1/15/2012; 15/2/2012; 3/15/2012; 4/15/2012; 5/15/2012; 6/15/2012; 7/15/2012; 8 / 15/2012; 9/15/2012; 10/15/2012; 11/15/2012; 12/15/2012 1/15/2013; 2/15/2013; 15/3/2013; 4/15/2013; 5/15/2013; 6/15/2013; 7/15/2013; 8 / 15/2013; 9/15/2013; 10/15/2013; 11/15/2013; 12/15/2013 1/15/2014; 2/15/2014; 3/15/2014; 4/15/2014; 5/15/2014; 6/15/2014; 7/15/2014; 8 / 15/2014; 9/15/2014
* Arvo - 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509
 
>Tämän palvelun, kuten isännöimät Azure Marketplace-sivustoon, on OData-palvelu. Nämä voidaan kutsua viestin tai HAE menetelmiä kautta. 

On useita tapoja muissa palvelun automaattinen tavalla (esimerkiksi-sovellus on [tähän](http://microsoftazuremachinelearning.azurewebsites.net/ArimaForecasting.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>Aloitetaan C#-koodin WWW-palvelun käyttöön:

    public class Input
    {
        public string frequency;
        public string horizon;
        public string date;
        public string value;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
         byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
         return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

       
    void Main()
    {
        var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };
        var json = JsonConvert.SerializeObject(input);
        var acitionUri =  "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
           
        var httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere","ChangeToAPIKey");
        httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
        var query = httpClient.PostAsync(acitionUri,new StringContent(json));
        var result = query.Result.Content;
        var scoreResult = result.ReadAsStringAsync().Result;
    }

##<a name="creation-of-web-service"></a>WWW-palvelun luominen 

>Web-palvelu on luotu käyttämällä Azure koneen Learning. Maksuttoman kokeiluversion käyttäjäksi, sekä esittelyvideot kokeissa ja [julkaisun verkkopalvelut](machine-learning-publish-a-machine-learning-web-service.md)luomisesta Katso [azure.com/ml](http://azure.com/ml). Alla on kokeilla, joka on luotu web-palvelu ja Esimerkki koodi kunkin kokeen sisällä moduulit näyttökuva.

Azure koneen Learning sisällä uusi tyhjä kokeen luotiin. Esimerkki syöttötiedot videoluettelointia ennalta määritettyjen tietojen rakennetta. Linkitetyt tiedot rakenne on [R-komentosarjan suorittaminen] [ execute-r-script] moduuli, joka luo ARIMA ennusteiden mallin R. 'auto.arima' ja 'ennuste-funktioiden avulla 

###<a name="experiment-flow"></a>Kokeen työnkulku:

![Työtilan luominen][2]

####<a name="module-1"></a>Moduuli 1:
 
    # Add in the CSV file with the data in the format shown below 
![Työtilan luominen][3]  

####<a name="module-2"></a>Moduulin 2:
    # data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)
    
    # preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]
    
    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)
    
    # fit a time-series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- auto.arima(train_ts)
    train_model <- forecast(fit1, h = data$horizon)
    plot(train_model)
    
    # produce forecasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")
    
    # data output
    maml.mapOutputPort("data.forecast");


##<a name="limitations"></a>Rajoitukset 

Tämä on kuvattu yksinkertainen Esimerkki ARIMA ennusteen. Näkevät edellä olevassa esimerkissä koodista, ei ole virhe pyytämisestä ei käytössä ja palvelun oletetaan, että kaikki muuttujat on jatkuva/positiiviset arvot ja niiden on oltava suurempi kuin 1 kokonaisluku. Päivämäärän ja vektorit pituuden on oltava sama. Päivämäärä-muuttuja noudata "mm-dd-yyyy"-muotoon.

##<a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
Usein kysytyt kysymykset-kulutus verkkopalvelun tai julkaiseminen marketplace-kohdassa [tähän](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-arima/arima-img1.png
[2]: ./media/machine-learning-r-csharp-arima/arima-img2.png
[3]: ./media/machine-learning-r-csharp-arima/arima-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
