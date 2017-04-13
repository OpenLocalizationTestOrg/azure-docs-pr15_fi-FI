<properties 
    pageTitle="Azure koneen Learning eloonjääneisyys analysointi | Microsoft Azure" 
    description="Eloonjääneisyys analyysi tapahtuman esiintymä todennäköisyys" 
    services="machine-learning" 
    documentationCenter="" 
    authors="zhangya" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="zhangya"/> 


#<a name="survival-analysis"></a>Eloonjääneisyys analyysi 

Valitse skenaarioita kohdassa arviointi tärkeimmät lopputulos on, jolloin tapahtuma, jonka korko. Toisin sanoen kysymys "kun tapahtuman tapahtuu?" pyydetään. Esimerkkejä, harkitse tilanteita, joissa tiedot kuvataan kulunut aika (päiviä, vuoden, matka jne.) kunnes halutut (taudista relapse, PhotoDraw'n asteen vastaanotetut jarrun pad-virhe) tapahtuma tapahtuu. Jokaiselle esiintymälle tietojen edustaa tietyn objektin (potilaalla opiskelija Auto, jne.).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

[Web-palvelu]( https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) on vastauksia "mikä on halutut tapahtuma tapahtuu aika n varten objektin x? todennäköisyys" kysymys Antamalla eloonjääneisyys analyysi mallin tämä WWW-palvelun avulla käyttäjä voi antavat tietoja mallin kouluttaminen ja testaa se. Kokeen tärkeimmät teema on kulunut aika malliin, kunnes halutut tapahtuman ilmetessä. 

>Verkkosovelluksen käyttäjät – läpi mahdollisesti mobiilisovelluksessa-sivuston kautta tai paikallisesti tietokoneeseen, voi olla kulutettu esimerkiksi. Mutta WWW-palvelun tarkoituksena on myös perusarvoksi on esimerkki siitä, miten Azure koneen Learning avulla voidaan luoda verkkopalvelut R koodin päälle. Todella R koodin rivit ja napsauttaa hiiren napsautuksella sisällä Azure koneen Learning Studio kokeen voi luoda R koodilla ja julkaistaan verkkopalvelun. Web-palvelu voi julkaista Azure Marketplacesta ja kulutettu käyttäjille ja laitteille kaikkialla maailmassa ilman infrastruktuurin asetuksia WWW-palvelun tekijän mukaan.  

##<a name="consumption-of-web-service"></a>WWW-palvelun kulutus

Seuraavassa taulukossa on näkyvissä WWW-palvelun syöttötiedot rakenne. Kuusi kappaletta tietojen tarvitaan syötteenä: tietojen koulutus, tietojen testaus, korko-hakemisto "aika" aika-dimensio, "tapahtuma-dimensio ja muuttujien tyypit indeksi (jatkuva tai kerroin). Koulutus tiedot esitetään merkkijonolla, jossa rivit on erotettu toisistaan pilkulla ja sarakkeet on erotettu toisistaan puolipisteillä. Useita suojausominaisuuksia, tiedot on joustava. Syötteen merkkijonon elementit on oltava numeerinen. Koulutus, tietojen "aikadimension" ilmaisee aikayksiköiden (päiviä, vuoden, matka jne.) määrä kulunut aloituskohdan tutkimuksen (potilaan vastaanottaminen niiden käsittelyä ohjelmia, student aloitus PhotoDraw'n tutkimus, auton käynnistäminen määräydy, jne.) ennen tapahtuman halutut (potilaan niiden käyttö hankkiminen PhotoDraw'n aste, ottaa jarrun pad virheen auton käänteisen palaaminen jne.) tapahtuu. "Tapahtuma"-dimensio ilmaisee, onko halutut tapahtuman ilmetessä tutkimuksen lopussa. Arvo on "tapahtuman = 1" tarkoittaa, että halutut tapahtuma esiintyy "aika"; dimension merkitty aikaa "tapahtuman = 0" tarkoittaa, että halutut tapahtuma on tapahtunut ei ole merkitty "aikadimension" ajan mukaan.

- trainingdata - merkkijono. Rivit erotetaan toisistaan pilkulla ja sarakkeet on erotettu toisistaan puolipisteillä. Kullakin rivillä on "aikadimension", "tapahtuma-dimensio ja predictor muuttujat.
- testingdata - yksi rivi, joka sisältää tietyn objektin muuttujat predictor.
- time_of_interest - edun n aikarajan.
- index_time - sarakkeen indeksiä "" aikadimension (1 lähtien).
- index_event - sarakkeen indeksiä "tapahtuma" dimension (1 lähtien).
- variable_types - merkkijonon toisistaan puolipisteillä ei erottimiksi. 0 edustaa jatkuva muuttujat ja 1 kerroin muuttujat.


Tulos on tapahtuman määrä tiettyyn aikaan. 

>Tämän palvelun, kuten isännöimät Azure Marketplace-sivustoon, on OData-palvelu. Nämä voidaan kutsua viestin tai HAE menetelmiä kautta. 

On useita tapoja muissa palvelun automaattinen tavalla (esimerkiksi-sovellus on [tähän](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

###<a name="starting-c-code-for-web-service-consumption"></a>Aloitetaan C#-koodin WWW-palvelun käyttöön:
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




Tämän testin tulkinnassa on seuraavasti. Oletetaan, että tiedot tavoitteen on malli kuluneen ajan ennen niiden käyttö palaa toimittamista potilaille, jotka ovat saaneet kaksi käsittely-ohjelmissa. Web-palvelu lukee tulos: potilaille on 35 vuoden vanha, ottaa edellisen huumausaineiden käsittely 2 kerrotaan luvulla kestää kauan kiinteistön käsittely-ohjelman ja kanssa heroin ja cocaine käyttö, TODENNÄKÖISYYS palauttaa niiden käyttö on 95.64 % päivä 500.

##<a name="creation-of-web-service"></a>WWW-palvelun luominen

>Web-palvelu on luotu käyttämällä Azure koneen Learning. Maksuttoman kokeiluversion käyttäjäksi, sekä esittelyvideot kokeissa ja [julkaisun verkkopalvelut](machine-learning-publish-a-machine-learning-web-service.md)luomisesta Katso [azure.com/ml](http://azure.com/ml). Alla on kokeilla, joka on luotu web-palvelu ja Esimerkki koodi kunkin kokeen sisällä moduulit näyttökuva.

Azure koneen Learning sisällä uusi tyhjä kokeen on luotu ja kaksi [R-komentosarjan suorittaminen] [ execute-r-script] moduulit on otettu mukaan työtilan sivulle. Tietojen rakenne on luotu yksinkertaista [R-komentosarjan suorittaminen][execute-r-script], joka määrittää WWW-palvelun syöttötiedot rakenne. Tämä moduuli linkitetään sitten toinen [R-komentosarjan suorittaminen] [ execute-r-script] moduuli, jossa pää työmäärä. Tämä moduuli ei tietojen esikäsittely, mallin luominen ja ennusteiden. Esikäsittelyn tiedot-vaiheessa kenttään annettavat tiedot, joita edustaa pitkä merkkijono on muunnettava ja tietojen kehykset muunnetaan. Mallin rakennuksen vaiheessa ulkoisen R-paketin "survival_2.37 7.zip" ensin asennetaan toteuttaa eloonjääneisyys analyysi. Valitse "coxph"-toimintoa suoritetaan jälkeen sarjan tietojen käsittely-tehtävät. Tietoja eloonjääneisyys analyysi "coxph"-funktio voidaan lukea R-ohjeista. Ennakoiva tekstinsyöttö vaiheessa testauksen esiintymän toimitetaan koulutetun malliin "surfit"-funktiolla ja säilymiseen käyrän testauksen tässä esiintymässä on valmistettu "käyrän" muuttujana. Lopuksi saadaan, kun korko todennäköisyyden. 

###<a name="experiment-flow"></a>Kokeen työnkulku:

![Kokeile työnkulku][1]

####<a name="module-1"></a>Moduuli 1:

    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"
    
    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port
    
####<a name="module-2"></a>Moduulin 2:

    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




##<a name="limitations"></a>Rajoitukset

Verkkosovelluksen voi kestää vain numeroarvot kuin ominaisuus muuttujia (sarakkeet). "Tapahtuma"-sarakkeen voi kestää vain arvo on 0 tai 1. "Aika"-sarakkeessa on oltava positiivinen kokonaisluku.

##<a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
Usein kysytyt kysymykset-kulutus verkkopalvelun tai julkaiseminen Azure Marketplace-kohdassa [tähän](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
