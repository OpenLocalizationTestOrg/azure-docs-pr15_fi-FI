<properties 
   pageTitle="B2B prosessin luominen Azure App palvelun | Microsoft Azure" 
   description="Opit luomaan Business Business-prosessi" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>

# <a name="creating-a-b2b-process"></a>B2B prosessin luominen

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


## <a name="business-scenario"></a>Liiketoiminta-skenaario 
Contoson ja Northwind on kaksi liikekumppanien. Contoson (jälleenmyyjältä) lähettää osto Northwind (toimittaja) alan tason transport esimerkiksi AS2 päälle. Northwind tallentaa kaikki saapuvat tilaukset sen pilvitallennustilaa. Osta tilauksia ovat XML-viestien nämä kaksi kumppanit välillä. Kun viesti on tallennettu Northwind's pilvitallennustilaa Northwind's sisäiset prosessit Käsittele kyseiseen pisteeseen tilaus.
 
Tässä opetusohjelmassa tavoitteena on muodostaa miten Northwind muodostaa liiketoimintaprosessien kautta, johon se vastaanottaa viestejä (ostaa tilauksia XML) sen kumppanilta Contoso AS2 päälle ja voimassa sen pilvitallennustilaa.


## <a name="capabilities-demonstrated"></a>Osoittaa ominaisuudet 
Tässä opetusohjelmassa auttaa korostaa seuraavia ominaisuuksia: 

- **Viestin kuljetus**: jälleenmyyjältä ja toimittaja voi olla eri ympäristöissä, mutta ne edelleen tavoitteet välinen tiedonsiirto. Tässä opetusohjelmassa ne ovat yhteydessä AS2 päälle (puolivälissä lauseen 2). AS2 on suosittu tapa transport tietoja kaupan kumppaneita on lueteltu business business communications välillä.
- **Tietojen pysyvyyttä**: Kun viesti on vastaanotettu AS2 päälle, valitse Northwind haluaa jatkuvat ennen lisäkäsittelyä. Se sen pilvitallennustilaa viesteistä jatkuvat yhdistimen avulla. Tässä opetusohjelmassa Azure-BLOB on parhaillaan hyödyntää nimellä pilvitallennustilaa Northwind varten.
- **Liiketoimintaprosessien luominen**: työnkulussa, useita API-sovellukset voi olla stitched yhdessä saavuttamiseksi business tulos osoittanut tähän.


## <a name="before-you-begin"></a>Ennen aloittamista
Tässä opetusohjelmassa oletetaan, että perusteet Azure App palveluista, tietää, miten voit luoda API-sovellukset ja lankanidontaa vuo yhdessä.


## <a name="steps-to-achieve-the-business-scenario"></a>Tavoitteet business-skenaarion vaiheet
**Luo ja määritä tarvittavat API-sovellukset**

1. Luo erillisen **Azure tallennustilan Blob yhdistin**. Tämä edellyttää tunnistetietoja Azuren tallennustilaan-tiliin. Varmista, että se on ole valmis ennen kuin alat luoda tämä.
2. Luo erillisen **BizTalk myynti kumppanin hallinta**. Tämä edellyttää tyhjä SQL-tietokanta-funktion. Tarkista, että se on valmis ennen kuin alat luoda tämä.
3. Luo erillisen **AS2 yhdistin**. Tämä edellyttää myös tyhjä SQL-tietokanta-funktion. Tarkista, että se on valmis ennen kuin alat luoda tämä. Lisäksi voit halutessasi viestien arkistoiminen osana AS2 käsittelyn voit antaa tunnistetiedot Azure-Blob, sen luomisen aikana.
4. Määritä TPM (myynti kumppanin hallinta)-palvelun, joka on luotu:  
    1. Selaa sitten edellä kuvatut toimet osana on luotu TPM palvelun esiintymän.
    2. Käytä **kumppaneille** -vaihtoehtoa, **Lisää** uusi **Contoso** -kumppani ja sen profiilin *osat* Lisää AS2 tarvittavat tunnistetiedot.
    3. Käytä **kumppaneille** -vaihtoehtoa, **Lisää** uusi **Northwind** -kumppani ja sen profiilin *osat* Lisää AS2 tarvittavat tunnistetiedot.
    4. Käytä **sopimuksia** asetuksen *osat* -kohdassa **Lisää** uusi AS2 Northwind ja Contoso sopimus. Northwind on isännöityä kumppanin ja Contoso päivitetään Vieras-kumppani. Tapauksen määrittäminen allekirjoitus, salaus, pakkaaminen ja kuittaussanomien tämän sopimuksen luonnin aikana. Siltä varalta, että varmenteita on käytettävä, hän voi ladata kautta **Varmenteet** -vaihtoehto selattaessa TPM-palvelun, joka on luotu.


## <a name="create-a-flow--business-process"></a>Luo vuo / business käsittely
1. Luo uusi työnkulku, jonka ensimmäiseksi on AS2. Vedä ja pudota **AS2 yhdistin** ei luotu esiintymä. Valitse käynnistin toimintoja:  
    ![][1]  
2. Vedä seuraavaksi ja pudota **Azure tallennustilan Blob yhdistin** ei luotu esiintymä. Toimintoa, toiminnot ja kyseisessä, valitse **Lataa Blob** haluamasi toiminnot. Määritä tarvittaessa.
3. Nyt luoda ja ottaa kulun.


## <a name="message-processing--troubleshooting"></a>Viestin käsittely- ja vianmääritys
1. Kannattaa testata työnkulku, että olet ottanut käyttöön. Lähettää XML viestejä rivitetty-AS2 (kuten edellä luotu AS2 sopimuksen) mukaan, jonka loit AS2Connector esiintymän esiin AS2 päätepisteelle. Voit joutua määrittämään päätepisteen todennus niin, että se on kaikkien käytettävissä.
2. Suorittamisen tietoja on esiin kulun selaamalla ja valitse hetken työnkulun esiintymää, jolla käytössä suorittaa kyselyjä
3. AS2 käsittely lisätietoja yhdistävää AS2Connector esiintymän selaamalla ja noudata sitten avulla seuranta-osaan. Voit käyttää yhdistävää rajoittaa näkymän tietoihin, jotka on haluamasi suodattimet.

![][2]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-b2b-process/Flow.png
[2]: ./media/app-service-logic-create-a-b2b-process/Tracking.png
 
