<properties 
    pageTitle="BINOMIJAKAUMA-ohjelmistopaketin | Microsoft Azure" 
    description="BINOMIJAKAUMA Suite" 
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


#<a name="binomial-distribution-suite"></a>BINOMIJAKAUMA Suite




BINOMIJAKAUMA-ohjelmistopaketin on esimerkki verkkopalvelut ([Binomijakauman luontitoiminnon](https://datamarket.azure.com/dataset/aml_labs/bdg5), [Todennäköisyys Laskimen]( https://datamarket.azure.com/dataset/aml_labs/bdp4) [Kvantiili Laskimen]( https://datamarket.azure.com/dataset/aml_labs/bdq5)), joiden avulla luodaan ja binomijakauman jaot käsittely. Palvelujen Salli pituus, laskeminen quantiles binomijakauman sarja luodaan poissa annettu annetun kvantiili ja laskemiseen: in. Palvelut tietokoneesta kuuluu äänimerkki eri tulostaa valitun palvelun perusteella (Katso alla kuvaus). BINOMIJAKAUMA-ohjelmistopaketin perustuu R Funktiot qbinom, rbinom ja pbinom, jotka sisältyvät R tilasto-paketti. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Nämä verkkopalvelut voi olla kulutettu käyttäjät – mahdollisesti suoraan marketplace kautta mobiilisovelluksessa-sivuston kautta tai paikallisesti tietokoneeseen, esimerkiksi. Mutta WWW-palvelun tarkoituksena on myös perusarvoksi on esimerkki siitä, miten Azure koneen Learning avulla voidaan luoda verkkopalvelut R koodin päälle. Todella R koodin rivit ja napsauttaa hiiren napsautuksella sisällä Azure koneen Learning Studio kokeen voi luoda R koodilla ja julkaistaan verkkopalvelun. WWW-palvelun sitten voidaan julkaista Azure Marketplacesta ja kulutettu käyttäjille ja laitteille kaikkialla maailmassa – infrastruktuurin määritystoimia tekijä web-palvelu ei ole pakko.

##<a name="consumption-of-web-service"></a>WWW-palvelun kulutus
BINOMIJAKAUMA-Suite sisältää seuraavat 3 palvelut.

###<a name="binomial-distribution-quantile-calculator"></a>BINOMIJAKAUMA kvantiili laskuri
Tämä palvelu hyväksyy normaalijakauman 4 argumenttia ja laskee liittyvät kvantiili.
Syötteen argumentit ovat:

- p - yksittäinen koostetun todennäköisyys useita satunnaiskokeiden lukumäärä.  
- koon - kokeiden määrä.
- todennäköisyys - kokeiluversion onnistumisen todennäköisyys.
- Sivu - jakauman jakauman ylälaidassa U alareunassa L. 

Palvelun tulos on laskettu kvantiili, joka on liitetty annetun todennäköisyys.

###<a name="binomial-distribution-probability-calculator"></a>BINOMIJAKAUMA todennäköisyys laskuri
Tämä palvelu hyväksyy 4 argumenttia binomijakaumaa ja laskee liittyvä todennäköisyys.
Syötteen argumentit ovat:

- q-yksittäisen kvantiili tapahtuman, jossa binomijakauman kertymäfunktion arvo. 
- koon - kokeiden määrä.
- todennäköisyys - kokeiluversion onnistumisen todennäköisyys.
- sivu - jakauman jakelua tai E, joka on yhtä kuin yhden onnistuneiden yritysten määrä ylälaidassa U alareunassa L.

Palvelun tulos on laskettu todennäköisyyden sille, että annetun kvantiili liittyy.

###<a name="binomial-distribution-generator"></a>BINOMIJAKAUMA luontitoiminto
Tämä palvelu hyväksyy 3 argumenttia binomijakaumaa ja luo numeroita, jotka binomially jaetaan sattumanvaraisia. Seuraavat argumentit on annettava siihen kuluessa pyynnön:

- n - havaintojen määrä. 
- koon - satunnaiskokeiden lukumäärä.
- todennäköisyys - onnistumisen todennäköisyys.

Palvelun tulos on sarja, pituus n, koko ja todennäköisyys argumentteihin perustuva binomijakaumaa.

>Tämän palvelun, kuten isännöimät Azure Marketplace-sivustoon, on OData-palvelu. Nämä voidaan kutsua viestin tai HAE menetelmiä kautta. 

On useita tapoja muissa palvelun automaattinen tavalla (esimerkiksi-sovellukset ovat: [luontitoiminnon](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [Todennäköisyys Laskimen](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx) [Kvantiili Laskimen](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)). 

###<a name="starting-c-code-for-web-service-consumption"></a>Aloitetaan C#-koodin WWW-palvelun käyttöön:

###<a name="binomial-distribution-quantile-calculator"></a>BINOMIJAKAUMA kvantiili laskuri
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="binomial-distribution-probability-calculator"></a>BINOMIJAKAUMA todennäköisyys laskuri
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="binomial-distribution-generator"></a>BINOMIJAKAUMA luontitoiminto
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
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

###<a name="binomial-distribution-quantile-calculator"></a>BINOMIJAKAUMA kvantiili laskuri

![Työtilan luominen][4]

####<a name="module-1"></a>Moduuli 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
####<a name="module-2"></a>Moduulin 2:

    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


###<a name="binomial-distribution-probability-calculator"></a>BINOMIJAKAUMA todennäköisyys laskuri

![Työtilan luominen][5]

####<a name="module-1"></a>Moduuli 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


####<a name="module-2"></a>Moduulin 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

###<a name="binomial-distribution-generator"></a>BINOMIJAKAUMA luontitoiminto

![Työtilan luominen][6]

####<a name="module-1"></a>Moduuli 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

####<a name="module-2"></a>Moduulin 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Rajoitukset 
Nämä ovat erittäin yksinkertaisia esimerkkejä ympärillä binomijakauman. Kun näkevät edellä olevassa esimerkissä koodista, pieni virhe pyytämisestä on toteutettu.

##<a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
Usein kysytyt kysymykset-kulutus verkkopalvelun tai julkaiseminen Azure Marketplace-kohdassa [tähän](machine-learning-marketplace-faq.md).


[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png
 
