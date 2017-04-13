<properties 
    pageTitle="Multivariate lineaarisen regressiosuoran | Microsoft Azure" 
    description="Multivariate muuttujan lineaarinen regressio" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="multivariate-linear-regression"></a>Multivariate muuttujan lineaarinen regressio   
 

 
Oletetaan, että olet tietojoukko ja haluat ennustaa nopeasti riippuvan muuttujan (y) kullekin henkilölle (i) riippumattoman muuttujan perusteella. Lineaarisen regressiosuoran on käytettävä näiden ennusteiden Suositut tilastollinen tekniikka. Tähän muuttujan y oletetaan on jatkuva arvo.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Yksinkertaisessa skenaariossa voi olla missä tutkija yrittää ennustaa leveyden henkilö (y) perusteella niiden korkeus (x). Monimutkaisemman tilanne voi olla missä tutkija on Lisätietoja yksittäisistä (kuten paino, sukupuoli, kilpa) ja yrittää ennustaa yksittäiset leveyden. [WWW-palvelun]( https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) sopii lineaarisen regressiosuoran mallin tiedot ja tulostaa kullekin tiedot havaintojen ennustettu arvo (y).

>Verkkosovelluksen käyttäjät – läpi mahdollisesti mobiilisovelluksessa-sivuston kautta tai paikallisesti tietokoneeseen, voi olla kulutettu esimerkiksi. Mutta WWW-palvelun tarkoituksena on myös perusarvoksi on esimerkki siitä, miten Azure koneen Learning avulla voidaan luoda verkkopalvelut R koodin päälle. Todella R koodin rivit ja napsauttaa hiiren napsautuksella sisällä Azure koneen Learning Studio kokeen voi luoda R koodilla ja julkaistaan verkkopalvelun. Web-palvelu voi julkaista Azure Marketplacesta ja kulutettu käyttäjille ja laitteille kaikkialla maailmassa ilman infrastruktuurin asetuksia WWW-palvelun tekijän mukaan.  

##<a name="consumption-of-web-service"></a>WWW-palvelun kulutus  
Web-palvelu palauttaa muuttujan riippumattoman muuttujan kaikkien huomautusten perusteella ennustetut arvot. Web-palvelu odottaa, että käyttäjä voi syötetietoja merkkijonona, jossa rivit erotetaan toisistaan pilkulla (,) ja sarakkeet on erotettu toisistaan puolipisteillä (;). Web-palvelu odottaa 1 rivi kerrallaan ja odottaa ensimmäisessä sarakkeessa on muuttujan. Esimerkki tietojoukko näyttää tältä:

![Mallitiedot][1]

On syötetty nimellä "Puuttuu" huomioita ilman riippuvan muuttujan y. Lisää yllä tietojoukko olla seuraava merkkijono tiedot: "10; 5; 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2; 1, puuttuu; 3; 4". Tulos on ennustettu arvo kunkin riippumattoman muuttujan rivit. 

>Tämän palvelun, kuten isännöimät Azure Marketplace-sivustoon, on OData-palvelu. Nämä voidaan kutsua viestin tai HAE menetelmiä kautta. 

On useita tapoja muissa palvelun automaattinen tavalla (esimerkiksi-sovellus on [tähän](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Aloitetaan C#-koodin WWW-palvelun käyttöön:

    public class Input
    {
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
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


Azure koneen Learning sisällä uusi tyhjä kokeen on luotu ja kaksi [R-komentosarjan suorittaminen] [ execute-r-script] moduulit on otettu mukaan työtilan sivulle. Verkkosovelluksen suorittaa Azure koneen Learning kokeen pohjana R-komentosarjan. On tämän kokeen 2 osaa: rakenteen määritys ja koulutus mallin + näkyvissä pistemäärä. Ensimmäisen moduulin määrittää syötteen tietojoukko, jossa ensimmäinen muuttuja on muuttujan ja jäljellä olevan muuttujat ovat itsenäisten odotettu rakenne. Toinen moduuli sovittaa syöttötiedot lineaarisen regressiosuoran yleinen malli.  
  
![Kokeen työnkulku][3]

####<a name="module-1"></a>Moduuli 1:
 
####<a name="schema-definition"></a>Rakenteen määritys  
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

####<a name="module-2"></a>Moduulin 2:
####<a name="lm-modeling"></a>LM mallinnus   
    data <- maml.mapInputPort(1) # class: data.frame  
  
    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  
 
##<a name="limitations"></a>Rajoitukset
Tämä on kuvattu yksinkertainen Esimerkki usean muuttujan lineaarinen regressio verkkopalvelun. Kun näkevät edellä olevassa esimerkissä koodista, ei ole virhe pyytämisestä ei käytössä ja palvelun oletetaan, että kaikki on jatkuva muuttuja (ei categorical ominaisuuksia sallittu), lukuarvoina-palvelua vain syötteiden verkkosovelluksen luomisen yhteydessä. Lisäksi palvelun tällä hetkellä kahvat rajoitettu arvopisteiden koko määräpäivä pyynnön ja vastauksen laatu web-Palvelukutsu ja mallin on parhaillaan Sovita aina WWW-palvelun kutsutaan kertoma. 

##<a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
Usein kysytyt kysymykset-kulutus verkkopalvelun tai julkaiseminen Azure Marketplace-kohdassa [tähän](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
