<properties 
    pageTitle="Verkon # Neural verkkojen määrityksen kielen opas | Microsoft Azure" 
    description="Syntaksi verkon # neural verkot määrityksen kieli-ja esimerkkejä neural verkon mukautetun mallin luominen Microsoft Azure ML verkon käyttäminen#" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jeannt" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="jeannt"/>



# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning"></a>Opas verkon # neural verkon määritys kielen Azure koneen Learning varten

## <a name="overview"></a>Yleiskatsaus
Verkon # on kieli, jota käytetään ehkä määrittää neural verkon arkkitehtuureihin neural verkon moduuleissa Microsoft Azure koneen Learning Microsoftin kehittämä. Tässä artikkelissa kerrotaan:  

-   Neural verkkoihin liittyvät peruskäsitteet
-   Neural Verkkovaatimukset ja pääosaa määrittäminen
-   Syntaksi ja verkon #-määrityksen kielen avainsanat
-   Esimerkkejä mukautettujen neural verkkojen luotu verkon# 
    
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

## <a name="neural-network-basics"></a>Neural verkoston perusteet
Neural verkon rakenteen koostuu ***solmut*** , jotka on järjestetty ***Kerrokset***ja painotettu ***yhteydet*** (tai ***Reunat***) solmujen välissä. Yhteydet on suunta ja jokaista on ***lähde*** -solmu ja ***kohde*** -solmu.  

Kunkin ***trainable kerroksen*** (piilotettu tai tulosteen layer) on vähintään yksi ***yhteyden nippujen***. Yhteys-pikaoppaista koostuu lähde kerroksen ja määrityksen lähde kerroksen yhteydet. Kaikki tietyn pikaoppaista yhteydet jakaa saman ***taso*** ja sama ***kohde kerroksen***. Verkon # yhteyden pikaoppaista pidetään kuuluvat pikaoppaista kohde kerrokseen.  
 
Verkon # tukee monenlaisia yhteyden nippujen, jonka avulla voit mukauttaa tapa syötteiden yhdistetty piilotetut kerrokset ja yhdistetty hyväksyttäväksi.   

Oletus- tai standard pikaoppaista on **koko pikaoppaista**, jossa kukin solmu taso on yhdistetty kunkin kohteen kerros solmun.  

Lisäksi verkon # tukee yhteyden Lisäasetukset nippujen neljä seuraavanlaisia:  

-   **Suodatettu nippujen**. Käyttäjän määrittää lause käyttämällä lähde kerros-solmu ja kohde kerros-solmu. Solmut on yhdistetty aina, kun lause on TOSI.
-   **Convolutional nippujen**. Käyttäjä voi määrittää pieni asuintaloissa solmujen taso. Kukin kohde kerros solmu on yhdistetty yksi ympäristö solmujen tietolähteen tasolla.
-   **Yhteysvarannon nippujen** ja **vastaus normalisointi nippujen**. Nämä ovat samalla convolutional niput siten, että käyttäjä määrittelee pieni asuintaloissa solmujen taso. Ero on se, että nämä paaleina reunoja leveydet eivät ole trainable. Sen sijaan ennalta määritettyä funktiota käytetään solmu lähdearvot määrittää kohde solmuarvon.  

Verkon # määrittämään neural verkon rakenne auttaa voi määrittää monimutkaisia rakenteet, kuten kattavaa neural verkkojen tai haluamaansa dimensiot, joita kutsutaan parantamiseksi learning tietoja, kuten kuvaa, ääntä tai videota convolutions.  

## <a name="supported-customizations"></a>Tuetut mukautukset
Neural verkko-mallit, jonka luot Azure koneen Learning arkkitehtuuri voi mukauttaa yleisesti käyttämällä verkon #. Voit:  

-   Luo piilotetut kerrokset ja hallita kerroksen näkyvien solmujen määrän.
-   Määritä, miten kerrokset ovat yhteydessä toisiinsa.
-   Määritä määräten connectivity rakenteiden, kuten convolutions ja jakaminen nippujen leveys.
-   Määritä eri aktivointi-funktiot.  

Lisätietoja määrityksen kielen syntaksi on artikkelissa [Rakenteen määritys](#Structure-specifications).  
 
[Esimerkkejä neural verkkojen joitakin yleisiä konepohjaisten oppimistekniikoiden tehtäviin, kuten simplex monimutkaisia, voit määrittää esimerkkejä.](#Examples-of-Net#-usage)  

## <a name="general-requirements"></a>Yleiset vaatimukset
-   On oltava täsmälleen yksi tulos kerroksen, vähintään yhden syötteen kerroksen ja nolla tai piilotettuja kerroksia. 
-   Kerroksen on solmuissa käsitteellisesti järjestetty suorakulmainen haluamaansa dimensiot-matriisissa on kiinteä määrä. 
-   Syötteen kerrokset ei ole liitetty koulutetun parametrit on, ja ne edustavat jossa esiintymän tietoihin tulee verkkoon. 
-   Trainable kerrokset (piilotettu- ja kerrokset) on liitetty koulutetun parametreja, paksuuksia ja biases nimeltään. 
-   Lähde- ja solmut on oltava eri tasot. 
-   Yhteyksien on oltava asykliset; Toisin sanoen ei voi olla komentoketju, jonka yhteydet, jotka takaisin alkuperäinen lähdesolmu.
-   Tulosteen kerros ei voi olla yhteyden pikaoppaista tasona lähde.  

## <a name="structure-specifications"></a>Rakenne-määritykset
Neural verkon rakenteen määritys koostuu kolme osaa: **vakion määrittelyssä**, **kerroksen ilmoituksen** **yhteyden ilmoitus**. On myös Valinnainen **ilmoitus jakaminen** -osassa. Osien voidaan määrittää järjestyksessä.  

## <a name="constant-declaration"></a>Vakion ilmoitus 
Vakio-määritys on valinnainen. Sen avulla voit määrittää käytettävät muualla neural verkon määritys arvot. Ilmoitus-lauseen koostuu tunnisteen perässä on oltava yhtäläisyysmerkki ja arvolauseke.   

Esimerkiksi seuraava lause määrittää vakion **x**:  


    Const X = 28;  

Voit määrittää kahden tai useamman vakioita samanaikaisesti, kirjoita tunniste nimet ja arvot aaltosulkeet ja erottaa ne toisistaan puolipisteillä. Esimerkki:  

    Const { X = 28; Y = 4; }  

Oikean reunan kunkin tehtävän lauseke voi olla kokonaisluku, reaaliluku, totuusarvon (tosi tai EPÄTOSI) tai matemaattisia. Esimerkki:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Kerroksen ilmoitus
Kerroksen määritys vaaditaan. Se määrittää koko ja lähde kerroksen, mukaan lukien sen yhteyden nippujen ja määritteet. Ilmoitus-lause alkaa (input, piilotetut tai tulosteen) kerroksen nimi mitat kerroksen (positiivisen kokonaisluvun monikon) perään. Esimerkki:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

-   Mittojen tulo lasketaan kerros näkyvien solmujen määrän. Tässä esimerkissä on kaksi mitat [5,20], mikä tarkoittaa, että on 100 solmujen kerros.
-   Kerrokset voi määrittää järjestyksessä vain yhtä poikkeusta lukuun ottamatta: Jos useamman kuin yhden syötteen kerroksen on määritetty, siinä järjestyksessä, jossa ne on määritetty vastattava syöttötiedot ominaisuuksien järjestystä.  


Voit määrittää, että kerroksen näkyvien solmujen määrän määritetään automaattisesti, käytä **Automaattinen** avainsanaa. **Automaattinen** avainsana on erilaisia tehosteita, tason mukaan:  

-   Syötteen kerroksen koskeva ilmoitus näkyvien solmujen määrän on useita suojausominaisuuksia, KESKIPOIKKEAMA.
-   Piilotetut kerroksen ilmoitukseen näkyvien solmujen määrän on luku, joka määrittämän parametriarvo **piilotetut solmujen määrän**. 
-   Tulosteen kerroksen koskeva ilmoitus näkyvien solmujen määrän on kaksi luokan luokitus-1 regression ja multiclass luokituksen tulosteen solmujen määrän, joka vastaa 2.   

Esimerkiksi seuraava verkko-määritys avulla määritetään automaattisesti kaikki tasot kokoa:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


Kerroksen ilmoitus trainable kerroksen (piilotettu- tai kerrokset) voit halutessasi sisällyttää tulosteen funktion (kutsutaan myös aktivointi-funktio), joka tulee oletusarvon mukaan **sigmoid** luokitus-mallit ja **lineaarisen** regressiosuoran malleille. (Vaikka käyttäisit oletusarvo-voit voit erikseen ilmoitettava aktivointi-funktio, selkeyttä halutessasi.)

Tukee seuraavia tulosteen toimintoja:  

-   sigmoid
-   Lineaarinen
-   softmax
-   rlinear
-   neliö
-   neliöjuuri
-   srlinear
-   itseisarvo
-   TANH 
-   brlinear  

Esimerkiksi **softmax** -funktiota käytetään seuraava ilmoitus:  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Yhteys-ilmoitus
Heti, kun määrität trainable kerroksen, on määritellä yhteydet leviävät kerrokset on määritetty. Yhteyden pikaoppaista ilmoituksen käynnistyy ja avainsana **-**nimi pikaoppaista taso ja luoda yhteyden pikaoppaista tyyppi perään.   

Tällä hetkellä tueta yhteyden nippujen viidenlaisia:  

-   **Koko** nippujen-avainsanan **kaikki** merkitty
-   **Suodatettu** nippujen, merkitty-avainsanan **missä**predikaatin lausekkeen perään
-   **Convolutional** nippujen, joka on merkitty-avainsanan **convolve**, seuraa poimutus määritteet
-   **Pooling** nippujen, joka on merkitty avainsanat **varannon** tai **tarkoittaa resurssivarantoon**
-   **Vastauksen normalisointi** nippujen, merkitty avainsana **vastauksen Normaali**      

## <a name="full-bundles"></a>Koko niput  

Koko yhteyden pikaoppaista sisältää kunkin solmun yhteyden kukin solmu kohde kerros kerroksen lähde. Tämä on oletusarvo verkon yhteyden tyyppi.  

## <a name="filtered-bundles"></a>Suodatettu niput
Suodatettu yhteyden pikaoppaista määritys sisältyy predikaatin, syntaksi, ilmaistuna paljon, kuten C#-lambda lauseke. Seuraava esimerkki määrittää kahden suodatetut nippujen:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

-   **S** on indeksiä vastaavan solmujen syötteen kerroksen _kuvapistettä_, suorakulmainen taulukon yhdeksi parametrin ja **d** on indeksiä vastaavan huomioon matriisin solmujen piilotetut kerroksen _ByRow_parametrin _ByRow_, lause. **S** ja **d** tyyppi on monikon kokonaislukujen pituinen kaksi. Käsitteellisesti **s** alueiden päälle kaikki parit kokonaislukujen kanssa _0 < = [0] s < 10_ ja _0 < = s[1] < 20_, ja **d** alueet kokonaisluvut, kaikki paria päälle ja _0 < = [0] d < 10_ ja _0 < = d[1] < 12_. 
-   Predikaatin lausekkeen oikeassa reunassa on ehto. Tässä esimerkissä **s** ja **d** kaikki arvot, jos ehto on TOSI, joka on viiston reunan lähteen kerroksen solmun kohde kerros-solmu. Tässä suodatin-lausekkeen siis tarkoittaa, että töihin sisältää **d** s [0] on yhtä suuri kuin [0] d kaikissa tapauksissa määrittämiä solmu **s** määrittämiä solmu yhteyden.  

Vaihtoehtoisesti voit määrittää joukon suodatetut pikaoppaista leveydet. **Leveydet** -määritteen arvon on oltava monikon irrallisen pisteen arvojen pituinen, joka vastaa töihin määrittämiä yhteyksien määrä. Oletusarvon mukaan leveydet luodaan satunnaisesti.  

Paino arvot ryhmitellään kohde solmu indeksin mukaan. Jos ensimmäinen Kohdesolmu on yhdistetty K lähde solmujen, **leveydet** monikon ensimmäisen _K_ osista ovat leveydet ensimmäinen kohde solmun tietolähteen indeksi järjestyksessä. Sama koskee jäljellä olevan kohteen solmujen.  

Se on mahdollista määrittää leveydet suoraan vakion arvoina. Esimerkiksi jos oppimiasi paksuuksia aiemmin, voit määrittää ne vakiot käyttämällä seuraavaa syntaksia:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Convolutional niput
Kun koulutus tiedot yhtenäisinä rakenne, convolutional yhteydet käytetään yleensä lisätietoja tiedoista korkean tason ominaisuuksia. Esimerkiksi kuva-, ääntä tai videota tietoja, paikkatietojen tai ajallinen dimensionaalisuus voi olla varsin yhtenäisen.  

Convolutional nippujen käyttää suorakulmainen **ytimet** , dimensiot – slid. Kunkin ydin määrittää leveydet paikallisen asuintaloissa käytetty **ydin sovellusten**jäljempänä. Ydin kunkin sovelluksen vastaa tietolähteen tasolla, joka on tarkoitettu **keskitetyn solmu**solmu. Ydin leveydet jaetaan yhteyksien kesken. Kunkin ydin on suorakulmainen convolutional töihin, ja kaikki ydin sovellukset on yhtä suuri.  

Convolutional nippujen tukevat määritteet:

**InputShape** määrittää taso dimensionaalisuus tarkoitetaan tässä convolutional pikaoppaista. Arvon on oltava monikon positiivisen kokonaisluvun. Kokonaisluvut tuote on yhtä suuri kuin taso näkyvien solmujen määrän, mutta, se ei tarvitse vastaa tietueeksi lähde kerroksen dimensionaalisuus. Tämän monikon pituuden tulee convolutional töihin **arity** -arvoa. (Yleensä paikkaluku viittaa argumentit tai operandeja, funktio voi kestää.)  

Voit määrittää muodon ja sijainti ytimet, käytä määritteiden **KernelShape**, **Stride** **Täyttö**, **LowerPad**tai **UpperPad**:   

-   **KernelShape**: (pakollinen) määrittää kunkin ydin varten convolutional pikaoppaista dimensionaalisuus. Arvon on oltava monikon positiivisen kokonaisluvun, joka vastaa arvoa töihin paikkaluku pituinen. Tämän monikon jokaisen osan on oltava ei ole suurempi kuin **InputShape**vastaava osa. 
-   **STRIDE**: (valinnainen) määrittää liukuva vaihe koot poimutus (vaihe koon kunkin dimension), joka on keskitetty solmujen välistä etäisyyttä. Arvon on oltava monikon positiivisen kokonaisluvun, joka on töihin paikkaluku pituinen. Tämän monikon jokaisen osan on oltava ei ole suurempi kuin **KernelShape**vastaava osa. Oletusarvo on monikon kaikki komponentit yhtä kanssa. 
-   **Jakaminen**: (valinnainen) määrittää kunkin poimutus dimension jakaminen leveys. Arvo voi olla yksittäinen totuusarvo tai monikon totuusarvoja, joka on töihin paikkaluku pituinen. Yksittäisen totuusarvo on laajennettu on monikon oikea pituus ja kaikki komponentit yhtä suuri kuin määritetty arvo. Oletusarvo on monikon, joka sisältää kaikki True arvot. 
-   **MapCount**: (valinnainen) määrittää ominaisuus määrän määritykset convolutional töihin. Arvo voi olla yksi positiivinen kokonaisluku tai monikon positiivisen kokonaisluvun, joka on töihin paikkaluku pituinen. Yksittäisen kokonaislukuarvo on laajennettu on yhtä suuri kuin määritetty arvo ensimmäisen rakenneosien oikea pituinen monikon ja kaikki jäljellä olevat osat suuri kuin yksi. Oletusarvo on yksi. Ominaisuuden yhdistämismääritykset kokonaismäärän tuotetta monikon osat. Tämä kokonaismäärä käyttötilejä eri osien määrittää kuinka ominaisuus kartan arvot ryhmitellään kohde-solmujen. 
-   **Leveydet**: (valinnainen) määrittää ensimmäisen leveydet töihin varten. Arvon on oltava monikon irrallisia pituus, kuinka monta kertaa ytimet on kohdassa arvojen leveydet määrän kohti ydin, tämän artikkelin määritysten mukaisesti. Oletus-leveydet satunnaisesti luodaan.  

On kaksi joukkoa ominaisuuksia, jotka täyttö, että toisensa poissulkevia ominaisuudet:

-   **Täyttäminen**: (valinnainen) määrittää onko syötteen numerojärjestykseen käyttämällä **oletusarvoista täyttäminen värimallin**. Arvo voi olla yksittäinen totuusarvo tai se voi olla monikon totuusarvoja, joka on töihin paikkaluku pituinen. Yksittäisen totuusarvo on laajennettu on monikon oikea pituinen kanssa kaikkien osien yhtä suuri kuin määritetty arvo. Jos dimension arvo on TOSI, lähde on loogisesti numerojärjestykseen kyseisen dimension nolla-arvon solujen muita ydin sovellusten siten, että ensimmäinen ja viimeinen ytimet kyseisen dimension keskitetyn solmut ovat ensimmäisen ja viimeisen solmujen dimension tietolähteen tasolla. Näin ollen kunkin dimension "tyhjä" näkyvien solmujen määrän määritetään automaattisesti sopivaksi täsmälleen _(InputShape [d] - 1) / Stride [d] + 1_ ytimet kyselyjä pehmustettu taso. Jos dimension arvo on EPÄTOSI, ytimet on määritetty siten, että kummallakin puolella, pois jätetyt solmujen määrän on sama (enintään 1 ero). Tämän määritteen oletusarvo on monikon kanssa kaikki komponentit arvoksi False.
-   **UpperPad** ja **LowerPad**: (valinnainen) Anna tehostaa hallintaa täyttö, jos haluat käyttää. **Tärkeä:** Seuraavanlaisia määritteitä voidaan määrittää vain, jos edellä **Täyttö** -ominaisuus ei ***ole*** määritetty. Arvojen on oltava taulukkomuotoinen monikot kanssa, jotka ovat töihin paikkaluku pituudet. Kun nämä määritteet määritetään, "tyhjä" solmujen lisätään kunkin dimension syötteen kerroksen ja alarajan päät. Ylä- ja alarajan päät jokaisesta lisätään solmujen määrän määräytyy **LowerPad**[i] ja **UpperPad**[i] tarpeen mukaan. Voit varmistaa, että ytimet vastaavat vain solmujen "reaali- ja ei"tyhjä"solmujen, seuraavat ehdot täyttyvät:
    -   Kunkin osan **LowerPad** on oltava ehdottoman alle KernelShape [d] / 2. 
    -   Kunkin osan **UpperPad** on oltava ei ole suurempi kuin KernelShape [d] / 2. 
    -   Nämä määritteet oletusarvo on monikon kanssa kaikki komponentit yhtä suuri kuin 0. 

**Täyttö-** asetukseksi = tosi sallii mahdollisimman paljon täyttöä pitämään "keskelle" ydin "real" sisällä syötteen tarpeen mukaan. Muutos matemaattiset hieman tietojenkäsittely lähtevä varten. Yleensä näyttökoko _D_ lasketaan _D = (I – K) / S-näppäinyhdistelmää + 1_, jossa _I_ on syötteen koko _K_ ydin kokoiseksi, _S_ on askel, ja _/_ on kokonaisluvun jako (PYÖRISTÄ nollaa kohti). Jos määrität UpperPad = [1, 1] syötteen koko _minulla_ on tehokas 29 ja siten _D = (29-5) / 2 + 1 = 13_. Kuitenkin, kun **täyttäminen** = TOSI, olennaisesti _voin_ saa bumped _K - 1_; Näin ollen _D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14_. Määrittämällä arvot **UpperPad** ja **LowerPad** saat paljon tarkemmin kuin jos vain Määritä **Täyttö** = true täyttö.

Lisätietoja convolutional verkkoja ja niiden sovelluksia on seuraavissa artikkeleissa:  

-   [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html )
-   [http://Research.microsoft.com/Pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
-   [http://People.csail.mit.edu/jvb/Papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Yhteysvarannon niput
**Yhteysvarannon pikaoppaista** koskee geometria samalla convolutional yhteys, mutta se käyttää ennalta määritetyt toiminnot solmu lähdearvot saada kohde solmun arvo. Näin ollen yhteysvarannon nippujen on ei ole trainable tila (leveydet tai biases). Yhteysvarannon nippujen tukevat kaikki paitsi **jakaminen**, **MapCount**ja **leveydet**convolutional määritteet.  

Yleensä yhteenveto vierekkäisten yhteysvarannon yksiköt ytimet mene päällekkäin. Jos askel [d] on yhtä kuin kunkin dimension KernelShape [d], saadaan kerros on perinteinen paikallisen yhteysvarannon kerroksen, joka työskentelee yleisesti convolutional neural verkkoja. Kunkin kohteen solmun laskee suurimman tai sen ydin tietolähteen tasolla toimintoja keskiarvon.  

Seuraavassa esimerkissä havainnollistetaan yhteysvarannon pikaoppaista: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

-   Töihin paikkaluku on 3 (monikot **InputShape**, **KernelShape**ja **Stride**pituuden). 
-   Lähde-kerroksen näkyvien solmujen määrän on _5 *24* 24 = 2880_. 
-   Tämä on perinteinen paikallisen yhteysvarannon kerroksen, koska **KernelShape** ja **Stride** ovat samat. 
-   Kohde-kerroksen näkyvien solmujen määrän on _5 *12* 12 = 1 440_.  
    
Lisätietoja yhteysvarannon kerrokset on seuraavissa artikkeleissa:  

-   [http://www.cs.Toronto.edu/~hinton/absps/imagenet.pdf](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Osan 3.4)
-   [http://CS.nyu.edu/~koray/publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
-   [http://CS.nyu.edu/~koray/publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)
    
## <a name="response-normalization-bundles"></a>Vastauksen normalisointi niput
**Vastauksen normalisointi** on paikallinen normalisointi malli, jossa otettiin Geoffrey Hinton mukaan et al, valitse paperin [ImageNet Classiﬁcation laaja Convolutional Neural verkkojen kanssa](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Vastauksen normalisointi käytetään neural verkkoja Yleistys tukeen. Kun yksi neuron on käynnistettäessä erittäin suuri aktivointi tasolla, paikallinen vastauksen normalisointi kerroksen vaimentaa ympäröivän neurons aktivointi taso. Tämä on valmis käyttämällä kolme parametrit (***α***, ***β***ja ***k***) ja convolutional rakenne (tai ympäristö muodon). Kohde-kerroksen ***y*** jokaisen neuron vastaa neuron ***x*** -taso. ***Y*** aktivointi taso saadaan seuraava kaava, jossa ***f*** on neuron aktivointi taso ja ***Nx*** ydin (tai joukko, joka sisältää neurons-ympäristö ***x***), seuraava convolutional rakenne määrittämä:  

![][1]  

Vastauksen normalisointi nippujen tukevat convolutional määritteet paitsi **jakaminen**, **MapCount**ja **leveydet**.  
 
-   Jos ydin on sama kartta ***x***neurons, normalisointi värimallin kutsutaan **saman kartan normalisointi**. Saman kartan normalisointi määrittämään **InputShape** ensimmäisen koordinaatin on oltava arvon 1.
-   Jos ydin sisältää neurons paikkatietojen sijainti sama kuin ***x***, mutta neurons on muita karttoja, normalisointi värimallin kutsutaan **kartat normalisointi yli**. Tämäntyyppisen vastauksen normalisointi toteuttaa lomakkeeseen radan viereen varattu mikrobien inspired luominen kilpailua suuri aktivointi tasoissa neuron tulostaa eri kartat lasketun muun muassa reaali neurons löytyvät tyypin mukaan. Määrittämään yli kartat normalisointi ensimmäisen koordinaatin on oltava kokonaisluku enemmän kuin yksi ja ei ole suurempi kuin karttojen ja muiden koordinaatteja on oltava arvon 1.  

Koska vastaus normalisointi nippujen käyttää ennalta määritettyä funktiota määrittää kohde solmuarvon solmu lähdearvot, heillä ei ole trainable tila (leveydet tai biases).   

**Ilmoitusten**: kohde kerros solmujen vastaavat neurons, jotka ovat ytimet keskitetyn solmut. Jos KernelShape [d] on pariton, valitse esimerkiksi _KernelShape [d] / 2_ vastaa keskitetyn ydin-solmu. Jos _KernelShape [d]_ on parillinen, keskitetty solmu on _KernelShape [d] / 2-1_. Sen vuoksi, jos **täyttäminen**[d] on EPÄTOSI, ensimmäinen ja viimeinen _KernelShape [d] / 2_ solmujen ei ole vastaavaa solmujen kohde kerros. Voit välttää tämän tilanteen, Määritä kuin **täyttäminen** [arvo on TOSI, TOSI,...,-true].  

Määritteet neljä edempänä vastaus normalisointi nippujen tukevat määritteet:  

-   **Alfa**: (pakollinen) määrittää perus arvo, joka vastaa ***α*** edelliseen kaavaan. 
-   **Beta**: (pakollinen) määrittää perus arvo, joka vastaa ***β*** edelliseen kaavaan. 
-   **Siirtymä**: (valinnainen) määrittää perus arvo, joka vastaa ***k*** edelliseen kaavaan. Oletusarvo on 1.  

Seuraava esimerkki määrittää vastauksen normalisointi pikaoppaista-käyttämällä seuraavanlaisia määritteitä:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

-   Taso sisältää viisi kartat, joissa aof dimension 12 x 12-1 440 solmujen summat. 
-   **KernelShape** arvo tarkoittaa, että kyseessä on sama kartta normalisointi kerroksen, jossa ympäristö on 3 x 3 suorakulmio. 
-   **Täyttö** oletusarvo on EPÄTOSI, näin kohde-kerros sisältää vain 10 solmujen kunkin dimension. Sisällytettävät kohde kerroksen, joka vastaa kunkin solmun taso yhden solmun lisätä täytön = TOSI, TOSI, TOSI; ja 5, 12, 12 RN1 koon muuttaminen.  

## <a name="share-declaration"></a>Jaa-ilmoitus 
Verkon # valinnaisesti tukee useita nippujen kanssa jaettujen leveydet määrittäminen. Jos niiden rakenteet ovat samat, voit jakaa minkä tahansa kahden nippujen leveydet. Seuraavaa syntaksia määrittää nippujen jaetun leveydet kanssa:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }
    
    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name
    
    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec
    
    bundle-spec:
       layer-name    =>     layer-name
    
    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec
    
    bias-spec:
        1    =>    layer-name
    
    layer-name:
        identifier  

Esimerkiksi seuraava Jaa-ilmoitus määrittää kerroksen nimet, joka ilmoittaa, että leveydet ja biases jaettu.  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

-   Syöttötavan toiminnot on osioitu kahteen yhtä samankokoista syötteen kerrokseen. 
-   Piilotetut kerrokset Laske syötteen kerrosten suurempi tason ominaisuudet. 
-   Jaa-ilmoitus osoittaa, että _H1_ ja _H2_ täytyy olla laskettu niiden vastaaviin syötteiden samalla tavalla.  
 
Vaihtoehtoisesti tämä saattaa määritettävä kaksi erillistä Jaa-ilmoitukset seuraavasti:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

Voit käyttää lyhyessä muodossa vain silloin, kun kerrokset sisältävät yhden pikaoppaista. Yleensä jakaminen on mahdollista vain, kun asiaa rakenne on sama kuin, mikä tarkoittaa, että ne on yhtä suuri, saman convolutional geometria ja niin edelleen.  

## <a name="examples-of-net-usage"></a>Esimerkkejä käyttö verkkoa #
Tässä osassa on esimerkkejä siitä, miten voit käyttää verkon # piilotetut kerrokset ja määritä tapaa, jolla piilotetut kerrokset käsitellä muut kerrokset ja luoda convolutional verkkoja.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Määritä yksinkertaisen mukautetun neural verkon: "Hei-maailman"-Esimerkki
Yksinkertainen tässä esimerkissä näytetään neural verkko-malli, jossa on yksi piilotetut kerroksen luominen.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

Esimerkissä on kuvattu joitakin peruskomennot seuraavasti:  

-   Ensimmäisen rivin määrittää syötteen kerroksen (nimeltään _tiedot_). Kun käytössä on **Automaattinen** -avainsana, neural verkon sisältää automaattisesti kaikki ominaisuus sarakkeet syötteen esimerkkejä. 
-   Toiselta riviltä Luo piilotettuun kerrokseen. Piilotetut kerrokseen, joka on 200 solmujen määritetään nimi _H_ . Kerroksen on täysin yhdistetty syötteen kerrokseen.
-   Kolmas rivi määrittää tulosteen kerroksen (nimeltään _O_), jossa on 10 tulosteen solmujen. Jos neural verkon käytetään luokittelu, on yksi tulos solmu luokan kohden. Avainsana **sigmoid** osoittaa, että tulostus-funktio on otettu käyttöön tulosteen kerrokseen.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Määritä piilotetut kerrosta: tietokoneen näkö Esimerkki
Seuraavassa esimerkissä kerrotaan, miten voit määrittää monimutkaisempia hieman neural verkko, mukautetun piilotetut kerrosta kanssa.  

    // Define the input layers 
    input Pixels [10, 20];
    input MetaData [7];
    
    // Define the first two hidden layers, using data only from the Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
    
    // Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }
    
    // Define the output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

Tässä esimerkissä on kuvattu neural verkkojen määrityksen kielen useita ominaisuuksia:  

-   Rakenne on kaksi syötteen kerrosta, _kuvapistettä_ ja _metatiedot_.
-   _Kuvapisteiden_ kerros on lähdetaso kaksi yhteyden nippujen kohde tasot, _ByRow_ ja _ByCol_kanssa.
-   Kerrosten _keräämään_ ja _tulos_ on useita yhteyden paaleina kerrokset kohde.
-   Tulosteen kerros, _tulos_on on kaksi yhteyden nippujen; kohde kerrokseen yhden, jossa toisen tason piilotetut (tarvittavan) kohde kerros ja toinen kohde kerros syötteen kerroksen (metatiedot) kanssa.
-   Piilotetut tasot, _ByRow_ ja _ByCol_, Määritä suodatetut connectivity predikaatin lausekkeiden avulla. Tarkemmin-solmu _ByRow_ [x y] on yhdistetty _kuvapistettä_ , joissa on sama kuin solmun ensimmäisen koordinaatin ensimmäisen indeksin koordinaatin solmujen x. Vastaavasti _ByCol [x y]-solmu on yhdistetty solmut _kuvapisteinä_ , joiden toisen indeksin järjestämiseen johonkin solmun toisen koordinaatin y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Määrittää convolutional verkon multiclass luokituksen: numeron tunnistuksen Esimerkki
Seuraavat verkon määritys on suunniteltu tunnistamaan numerot ja se on kuvattu joitakin Lisäasetukset tekniikoita neural verkon mukauttamiseen.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


-   Rakenne on yhden syötteen kerros- _Kuva_.
-   Avainsana **convolve** osoittaa, että _Conv1_ ja _Conv2_ kerrokset ovat convolutional kerrokset. Kunkin tason näiden ilmoitusten perässä poimutus määritteet luettelo.
-   Verkon on kolmannen piilottaa kerroksen, _Hid3_, joka on liitetty täysin toisen piilotetut kerroksen _Conv2_.
-   Tulosteen kerroksen, _numero_, on yhdistetty vain kolmannen piilotetut kerroksen _Hid3_. Avainsanojen **kaikki** osoittaa, että tulostus kerros on täysin yhteydessä _Hid3_.
-   Poimutus paikkaluku on kolme (monikot **InputShape**, **KernelShape**, **Stride**ja **jakaminen**pituus). 
-   Leveydet kohti ydin määrä on _1 + **KernelShape**\[0] * * *KernelShape**\[1]* **KernelShape**\[2] = 1 + 1 *5* 5 = 26. Tai 26 * 50 = 1300_.
-   Piilotetut kerroksen solmujen lasketaan seuraavasti:
    -   **NodeCount**\[0] = (5 - 1) tai 1 + 1 = 5.
    -   **NodeCount**\[1] = (13 – 5) / 2 + 1 = 5. 
    -   **NodeCount**\[2] = (13 – 5) / 2 + 1 = 5. 
-   Solmujen kokonaismäärän voidaan laskea käyttämällä ilmoitettu dimensionaalisuus kerroksen, [50, 5, 5] seuraavasti: _ **MapCount** * * *NodeCount**\[0]* **NodeCount**\[1] * * *NodeCount**\[2] = 10* 5 *5* 5_
-   Koska **jakaminen**[d] on EPÄTOSI vain _d == 0_, ytimet määrä on _ **MapCount** * * *NodeCount**\[0] = 10* 5 = 50_. 


## <a name="acknowledgements"></a>Kuittaussanomien

Mukauta neural verkostojen arkkitehtuuri verkon #-kielen kehittämä Microsoftin Shon Katzenberger (Architect, tietokoneen Learning) ja Alexey Kamenev (ohjelmiston lähdekoodiksi, Microsoft Researchin). Sitä käytetään sisäisesti konepohjaisten oppimistekniikoiden projektien ja väliltä kuva tunnistus tekstin analytics sovelluksia varten. Lisätietoja on artikkelissa [Neural verkkoja Azure millilitroina - verkon # esittely](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)


[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif
 
