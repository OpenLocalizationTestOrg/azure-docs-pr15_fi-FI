<properties 
    pageTitle="Ero mittasuhteet testin | Microsoft Azure" 
    description="Mittasuhteet testi ero" 
    services="machine-learning" 
    documentationCenter="" 
    authors="aniedea" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="aniedea"/> 


#<a name="difference-in-proportions-test"></a>Mittasuhteet testi ero


On kaksi mittasuhteet tilastollinen eri? Oletetaan, että käyttäjä haluaa vertaa kahden elokuvia selvittää, onko yhden elokuvan merkittävästi korkeampi osuus "tykkäykset" milloin verrattuna toiseen. Suuri otoksen kanssa voi olla mittasuhteet 0,50 ja 0,51 tilastollinen merkittäviä erotuksen. Pieni näyte, jossa ei ole ehkä riittävästi tietoja sen selvittämisestä, jos nämä mittasuhteet erilaisella. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

[Web-palvelu]( https://datamarket.azure.com/dataset/aml_labs/prop_test) suorittaa hypoteesitesti ero kaksi suhteissa syöttötapa onnistuneiden yritysten määrä ja yritykset 2 vertailu ryhmien kokonaismäärän perusteella. Yksi mahdollinen tilanne verkkosovelluksen nimi on alueella elokuvan vertailu-sovelluksen, kertoo käyttäjälle, onko jokin elokuvia on todella 'tykännyt' useammin kuin muut-filmin luokitusten perusteella.

>Verkkosovelluksen käyttäjät – läpi mahdollisesti mobiilisovelluksessa-sivuston kautta tai paikallisesti tietokoneeseen, voi olla kulutettu esimerkiksi. Mutta WWW-palvelun tarkoituksena on myös perusarvoksi on esimerkki siitä, miten Azure koneen Learning avulla voidaan luoda verkkopalvelut R koodin päälle. Todella R koodin rivit ja napsauttaa hiiren napsautuksella sisällä Azure koneen Learning Studio kokeen voi luoda R koodilla ja julkaistaan verkkopalvelun. Web-palvelu voi julkaista Azure Marketplacesta ja kulutettu käyttäjille ja laitteille kaikkialla maailmassa ilman infrastruktuurin asetuksia WWW-palvelun tekijän mukaan.


##<a name="consumption-of-web-service"></a>WWW-palvelun kulutus

Tämä palvelu hyväksyy 4 argumentit ja hypoteesien testaaminen mittasuhteet.

Syötteen argumentit ovat:

* Successes1 - esimerkissä 1 success tapahtumien määrä.
* Successes2 - malli 2 success tapahtumien määrä.
* Total1 - 1 otoksen suuruus.
* Total2 - 2 otoksen suuruus.

Palvelun tulosteen syynä oletusta chi-square tilasto-df\ssreg, p-arvon sekä testata ja otoksen 1/2 ja LUOTTAMUSVÄLI rajojen omistusosuuden.

>Tämän palvelun, kuten isännöimät Azure Marketplace-sivustoon, on OData-palvelu. Nämä voidaan kutsua viestin tai HAE menetelmiä kautta. 

On useita tapoja muissa palvelun automaattinen tavalla (esimerkiksi-sovellus on [tähän](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Aloitetaan C#-koodin WWW-palvelun käyttöön:

    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
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

Azure koneen Learning sisällä uusi tyhjä kokeen luotiin kaksi [R-komentosarjan suorittaminen] [ execute-r-script] moduulit. Ensimmäisen moduulin tietojen rakenne on määritetty, kun toinen moduuli käyttää R prop.test-komennosta 2 mittasuhteet hypoteesien testin suorittamiseen. 


###<a name="experiment-flow"></a>Kokeen työnkulku:

![Kokeen työnkulku][2]


####<a name="module-1"></a>Moduuli 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port
    

####<a name="module-2"></a>Moduulin 2:

    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port
    

##<a name="limitations"></a>Rajoitukset 

Tämä on kuvattu yksinkertainen Esimerkki testin 2 mittasuhteet eron. Kun näkevät edellä olevassa esimerkissä koodista, ei ole virhe pyytämisestä ei käytössä ja palvelun oletetaan, että kaikki muuttujat on jatkuva.

##<a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
Usein kysytyt kysymykset-kulutus verkkopalvelun tai julkaiseminen Azure Marketplace-kohdassa [tähän](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
