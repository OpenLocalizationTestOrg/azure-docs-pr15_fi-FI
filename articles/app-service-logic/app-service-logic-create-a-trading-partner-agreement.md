<properties 
   pageTitle="Kaupan kumppanin sopimuksen luominen Azure App palvelun | Microsoft Azure" 
   description="Luo myynti kumppanin toimeenpano" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
    ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2016"
   ms.author="rajram"/>

# <a name="creating-a-trading-partner-agreement"></a>Kaupan kumppanin sopimuksen luominen   

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Kaupan kumppanit ovat yhdistävää B2B kohteiden viestintä (Business Business). Kun kaksi kumppanien muodostamaan yhteyden, tämä on nimitystä *sopimuksen*. Määritetty sopimuksen perustuu viestintä kahden kumppaneille haluat saada ja protokolla tai tietyn transport. Eri B2B protokollat ja Azure App palvelun tukemat tiedonsiirron ovat seuraavat:

- AS2 (puolivälissä lause 2)
- EDIFACT (Yhdistyneiden Kansakuntien/sähköiset Data Interchange hallinta, Commerce ja Transport (UN/EDIFACT))
- X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>BizTalk API-sovellukset, jotka tukevat B2B skenaariot
Seuraavat API-sovellusten käyttöön näiden ominaisuuksien avulla monipuolisen ja intuitiivinen kokemus Azure-portaalissa:


## <a name="biztalk-trading-partner-management-tpm"></a>BizTalk Kaupankäyntipäivä kumppanin Management (TPM)
- Luominen ja kumppanit, profiilit ja käyttäjätietojen hallinta
- Tallennus ja Muokkaa-mallien hallinta
- Tallennus ja hallinta (käytetään AS2 protocol) varmenteet
- Luomista ja hallintaa sopimusten AS2
- Luomista ja hallintaa sopimusten EDIFACT (mukaan lukien jonottaminen Lähetä reunassa)
- Luomisen ja hallinnan X12 sopimusten (mukaan lukien jonottaminen Lähetä reunassa)

![][1]


## <a name="as2-connector"></a>AS2 yhdistin
- Suorittaa AS2 sopimusten määritysten liittyvät TPM API App esiintymän
- Näyttömalli käyttää AS2 käsittely/seuranta tietoja vianmääritys


## <a name="biztalk-edifact"></a>BizTalk EDIFACT
- Suorittaa EDIFACT toimeenpano määritysten liittyvät TPM API App esiintymän
- Näyttömalli käyttää EDIFACT käsittely/seuranta tietoja vianmääritys
- Sisältää erissä (aloittamisesta ja lopettamisesta) hallinta valtion määritysten mukaisesti EDIFACT sopimusten aktivoinnin liittyvät TPM API App esiintymän


## <a name="biztalk-x12"></a>BizTalk X12
- Suorittaa X12 sopimusten määritysten liittyvät TPM API App esiintymän 
- Pintoja X12 käsittely/seuranta tietoja vianmääritys
- Sisältää erissä (aloittamisesta ja lopettamisesta) hallinta valtion määritysten mukaisesti X12 sopimusten aktivoinnin liittyvät TPM API App esiintymän

Aiemmin määritetyllä tavalla AS2 X 12 ja EDIFACT API-sovellukset edellyttävät TPM API-sovelluksen toimimaan odotetulla tavalla.


## <a name="getting-started"></a>Käytön aloittaminen
Voit luoda kaupan kumppanin toimeenpano seuraavasti:

1. Luo **BizTalk myynti kumppanin hallinta** yhdistimen esiintymän. Tämä edellyttää tyhjä SQL-tietokanta-funktion. Ennen aloittamista muista tyhjä tietokanta on käytettävissä ja valmis käytettäväksi.
2. Lataa rakenteita ja sopimusten vaaditulla varmenteet. Tämä tapahtuu selaaminen TPM esiintymä luodaan ja hetken 'Rakenteita' ja "Varmenteet"-osaan
3. Selaa TPM-esiintymään, joka on luotu ja vaiheen **kumppaneille** -osaan
4. Luo kumppaneille haluamallasi tavalla. Myös Muokkaa profiilia tarpeen mukaan ja lisää tarvittavat tunnistetiedot
5. Luo sopimusten **toimeenpano** -osan avulla. Kun luot sopimuksen, valitse protokolla, jota käytetään. Jäljellä oleva kokoonpanoasetusten määrittäminen perustuu protokolla, jota olet valinnut.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png
 
