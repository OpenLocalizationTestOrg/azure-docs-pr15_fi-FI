<properties 
    pageTitle="Normaalijakaumaa WWW-palvelun Suite | Microsoft Azure" 
    description="Normaalijakaumaa WWW-palvelun Suite" 
    services="machine-learning" 
    documentationCenter="" 
    authors="ireiter" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="ireiter"/> 

#<a name="normal-distribution-suite"></a>Normaalijakaumaa Suite



Normaalijakaumaa Suite on esimerkki verkkopalvelut ([luontitoiminto]( https://datamarket.azure.com/dataset/aml_labs/ndg7), [Kvantiili Laskimen]( https://datamarket.azure.com/dataset/aml_labs/ndq5) [Todennäköisyys Laskimen]( https://datamarket.azure.com/dataset/aml_labs/ndp5)), joiden avulla luodaan ja käsittelemisen Normaali jaot joukko. Palvelujen Salli luodaan minkä tahansa pituus quantiles annetun todennäköisyys-laskeminen ja laskeminen todennäköisyys annetun kvantiili-normaalijakaumaa järjestyksessä. Palvelut tietokoneesta kuuluu äänimerkki eri tulostaa valitun palvelun perusteella (Katso alla kuvaus). Normaalijakaumaa Suite perustuu R Funktiot qnorm, rnorm ja pnorm, jotka sisältyvät R tilasto paketti.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Verkkosovelluksen käyttäjät – läpi mahdollisesti mobiilisovelluksessa-sivuston kautta tai paikallisesti tietokoneeseen, voi olla kulutettu esimerkiksi. Mutta WWW-palvelun tarkoituksena on myös perusarvoksi on esimerkki siitä, miten Azure koneen Learning avulla voidaan luoda verkkopalvelut R koodin päälle. Todella R koodin rivit ja napsauttaa hiiren napsautuksella sisällä Azure koneen Learning Studio kokeen voi luoda R koodilla ja julkaistaan verkkopalvelun. Web-palvelu voi julkaista Azure Marketplacesta ja kulutettu käyttäjille ja laitteille kaikkialla maailmassa ilman infrastruktuurin asetuksia WWW-palvelun tekijän mukaan.  
 

##<a name="consumption-of-web-service"></a>WWW-palvelun kulutus
Normaalijakaumaa Suite sisältää seuraavat palvelut 3.

###<a name="normal-distribution-quantile-calculator"></a>Normaalijakaumaa kvantiili laskuri
Tämä palvelu hyväksyy normaalijakauman 4 argumenttia ja laskee liittyvät kvantiili.

Syötteen argumentit ovat:

* p tapahtuman normaalijakaumaan-yksittäisen todennäköisyys. 
* Keskiarvo - normaalijakaumaa keskiarvosta.
* SD - normaalijakaumaa keskihajonta. 
* Sivu - jakauman alareunassa L ja jakauman ylälaidassa U.

Palvelun tulos on laskettu kvantiili, joka on liitetty annetun todennäköisyys.

###<a name="normal-distribution-probability-calculator"></a>Normaalijakauman todennäköisyys laskuri
Tämä palvelu hyväksyy normaalijakauman 4 argumenttia ja laskee liittyvä todennäköisyys.

Syötteen argumentit ovat:

* q tapahtuman normaalijakaumaan-yksittäisen kvantiili. 
* Keskiarvo - normaalijakaumaa keskiarvosta.
* SD - normaalijakaumaa keskihajonta. 
* Sivu - jakauman alareunassa L ja jakauman ylälaidassa U.

Palvelun tulos on laskettu todennäköisyyden sille, että annetun kvantiili liittyy.

###<a name="normal-distribution-generator"></a>Normaalijakaumaa luontitoiminto
Tämä palvelu hyväksyy normaalijakauman 3 argumenttia ja luo numerot, jotka ovat jakautunut sattumanvaraisia. Seuraavat argumentit on annettava siihen kuluessa pyynnön:

* n - havaintojen määrä. 
* keskiarvo - normaalijakaumaa keskiarvosta.
* SD - normaalijakaumaa keskihajonnan. 

Palvelun tulos on pituus n keskiarvo ja sd argumentteihin perustuva normaalijakauman sarja.

>Tämän palvelun, kuten isännöimät Azure Marketplace-sivustoon, on OData-palvelu. Nämä voidaan kutsua viestin tai HAE menetelmiä kautta. 

On useita tapoja muissa palvelun automaattinen tavalla (esimerkiksi-sovellukset ovat: [luontitoiminnon](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [Todennäköisyys Laskimen](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx) [Kvantiili Laskimen](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>Aloitetaan C#-koodin WWW-palvelun käyttöön:

###<a name="normal-distribution-quantile-calculator"></a>Normaalijakaumaa kvantiili laskuri
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="normal-distribution-probability-calculator"></a>Normaalijakauman todennäköisyys laskuri
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="normal-distribution-generator"></a>Normaalijakaumaa luontitoiminto
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
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
>Web-palvelu on luotu käyttämällä Azure koneen Learning. Maksuttoman kokeiluversion käyttäjäksi, sekä esittelyvideot kokeissa ja [julkaisun verkkopalvelut](machine-learning-publish-a-machine-learning-web-service.md)luomisesta Katso [azure.com/ml](http://azure.com/ml). 

Alla on kokeilla, joka on luotu web-palvelu ja Esimerkki koodi kunkin kokeen sisällä moduulit näyttökuva.

###<a name="normal-distribution-quantile-calculator"></a>Normaalijakaumaa kvantiili laskuri

Kokeen työnkulku:

![Kokeen työnkulku][2]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-probability-calculator"></a>Normaalijakauman todennäköisyys laskuri
Kokeen työnkulku:

![Kokeen työnkulku][3]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-generator"></a>Normaalijakaumaa luontitoiminto
Kokeen työnkulku:

![Kokeen työnkulku][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Rajoitukset 

Nämä ovat erittäin yksinkertaisia esimerkkejä normaalijakaumaa ympärillä. Kun näkevät edellä olevassa esimerkissä koodista, pieni virhe pyytämisestä on toteutettu.

##<a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
Usein kysytyt kysymykset-kulutus verkkopalvelun tai julkaiseminen Azure Marketplace-kohdassa [tähän](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png
 
