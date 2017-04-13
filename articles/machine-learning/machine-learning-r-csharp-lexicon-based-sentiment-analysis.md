<properties 
    pageTitle="Lexicon perusteella Markkinatunnelma analyysi | Microsoft Azure" 
    description="Lexicon perusteella Markkinatunnelma analyysi" 
    services="machine-learning" 
    documentationCenter="" 
    authors="pengxia" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="pengxia"/> 



#<a name="lexicon-based-sentiment-analysis"></a>Lexicon perusteella Markkinatunnelma analyysi 

Miten voit mitata käyttäjien lausuntoja ja asenteitaan kohti merkkien tai yhteisöpalveluihin online-aiheista, kuten Facebook kirjaa tweets, tarkistukset jne.? Markkinatunnelma analyysi mahdollistaa näiden kysymysten analysointiin.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Kahdella yleensä markkinatunnelma analyysia varten. Jokin käyttää valvonnan alaisena learning algoritmin ja toinen voidaan käsitellä varoitukseksi learning. Valvonnan alaisena learning algoritmin perustuu luokitus mallin yleensä suuri huomautettuja corpus. Sen tarkkuutta perustuu pääasiassa huomautuksen laadun ja yleensä koulutusprosessi kestää kauan. Lisäksi, että kun on käyttää algoritmin toisen toimialueen tulos ei yleensä ole hyvä. Verrattuna valvottava Oppiva, lexicon varoitukseksi learning käyttää markkinatunnelma oikeinkirjoitussanasto, joka ei edellytä suurien tietomäärien corpus tallentamisesta ja koulutus – joka on paljon aiempaa nopeammin koko prosessia. 

[Palvelu](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) on muodostettu MPQA Subjectivity Lexicon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), joka on yksi yleisimmin käytettyjä subjectivity sanaluettelot. MPQA on 5097 negatiiviset ja 2533 positiivinen sanoja. Ja kaikki nämä sanat on ollut kommentteja kuin vahva tai heikko polariteetti. Koko corpus on kohdassa GNU General Public License. Web-palvelu voi suojata lyhyt virkkeet, esimerkiksi tweets ja Facebook-viestit. 

>Verkkosovelluksen käyttäjät – läpi mahdollisesti mobiilisovelluksessa sivusto tai jopa paikalliseen tietokoneeseen saattaa olla kulutettu esimerkiksi. Mutta WWW-palvelun tarkoituksena on myös perusarvoksi on esimerkki siitä, miten Azure koneen Learning avulla voidaan luoda verkkopalvelut R koodin päälle. Todella R koodin rivit ja napsauttaa hiiren napsautuksella sisällä Azure koneen Learning Studio kokeen voi luoda R koodilla ja julkaistaan verkkopalvelun. Web-palvelu voi julkaista Azure Marketplacesta ja kulutettu käyttäjille ja laitteille kaikkialla maailmassa ilman infrastruktuurin asetuksia WWW-palvelun tekijän mukaan.

##<a name="consumption-of-web-service"></a>WWW-palvelun kulutus

Kenttään annettavat tiedot voivat olla tekstiä, mutta WWW-palvelun työskentelee paremmin lyhyt lauseiden. Tulos on numeerinen arvo väliltä 1 – 1. Mikä tahansa arvo 0 alla ilmaisee, että teksti markkinatunnelma on negatiivinen; Jos positiivinen yli 0. Absoluuttinen arvo, joka ilmaisee liittyvän markkinatunnelma voimakkuuden. 

>Tämän palvelun, kuten isännöimät Azure Marketplace-sivustoon, on OData-palvelu. Nämä voidaan kutsua viestin tai HAE menetelmiä kautta. 

On useita tapoja muissa palvelun automaattinen tavalla (esimerkiksi-sovellus on [tähän](http://microsoftazuremachinelearning.azurewebsites.net/)).

###<a name="starting-c-code-for-web-service-consumption"></a>Aloitetaan C#-koodin WWW-palvelun käyttöön:

    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();
    
                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



Syöte on "Tänään on hyvä päivässä." Tulos on "1", joka ilmaisee positiivinen markkinatunnelma, Kirjoita lause liittyvät. 

##<a name="creation-of-web-service"></a>WWW-palvelun luominen
>Web-palvelu on luotu käyttämällä Azure koneen Learning. Maksuttoman kokeiluversion käyttäjäksi, sekä esittelyvideot kokeissa ja [julkaisun verkkopalvelut](machine-learning-publish-a-machine-learning-web-service.md)luomisesta Katso [azure.com/ml](http://azure.com/ml). Alla on kokeilla, joka on luotu web-palvelu ja Esimerkki koodi kunkin kokeen sisällä moduulit näyttökuva.


Azure koneen Learning sisällä uusi tyhjä kokeen luotiin. Alla olevassa kuvassa lexicon perustuva markkinatunnelma analyysin koe-työnkulku. "Sent_dict.csv" tiedosto on MPQA subjectivity lexicon ja määritetään yhtä [R-komentosarjan suorittaminen]syötteiden[execute-r-script]. Toisen syöte on näytteeksi katsaus Amazon Tarkista tietojoukko testin, jossa on suorittaa valinta-sarakkeen nimen muuttaminen ja toimintojen jakaminen. Käytämme hash-paketin tallentaa subjectivity lexicon muistiin ja nopeuttaa pistemäärän laskenta-prosessin. Koko tekstin "tm" pakkaus tokenized, ja sitä markkinatunnelma sanaston word verrataan. Lopuksi pistemäärän lasketaan lisäämällä Subjektiivinen sanojen leveyden teksti. 

###<a name="experiment-flow"></a>Kokeen työnkulku:

![Kokeile työnkulku][2]


####<a name="module-1"></a>Moduuli 1:
    
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame
 
   # <a name="install-hash-package"></a>Asenna hash-paketti
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }
          
        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")
    


##<a name="limitations"></a>Rajoitukset

Algoritmin-näkökulmasta lexicon perustuva markkinatunnelma analysointi on yleinen markkinatunnelma-analyysityökalu, joka ei voi suorittaa parempi vaihtoehto tiettyjen luokitus-menetelmää. Negaatio ongelma ei sekä käsitellä. Olemme hardcode useita negaatio sanat jaetuissa kehittämisohjelmaan, mutta paremman tavan käyttämällä negaatio sanasto ja luoda sääntöjä. Web-palvelu suorittaa paremmin lyhyt ja selkeä lauseiden, kuten tweets ja viestit Facebook-kuin pitkän ja monimutkaisia lauseiden, kuten Amazon tarkistukset. 

##<a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
Usein kysytyt kysymykset-kulutus verkkopalvelun tai julkaiseminen Azure Marketplace-kohdassa [tähän](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

 
