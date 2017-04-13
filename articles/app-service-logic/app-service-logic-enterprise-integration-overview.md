<properties 
    pageTitle="Yleistä yrityksen integrointi | Microsoft Azure App palvelun | Microsoft Azure" 
    description="Yrityksen integrointi ominaisuuksien avulla voit ottaa käyttöön business prosessin ja integrointi skenaariot logiikan sovellusten käyttäminen" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-the-enterprise-integration-pack"></a>Yrityksen integrointi Pack yleiskatsaus

## <a name="what-is-the-enterprise-integration-pack"></a>Mikä on Enterprise integrointi pakettiin?
Yrityksen Integration Pack on Microsoftin pilvipohjainen ratkaisu käyttöönottoon saumattomasti business business (B2B) tietoliikenne. Pakettiin käyttää alan vakio protokollat esimerkiksi [AS2](./app-service-logic-enterprise-integration-as2.md) [X12](./app-service-logic-enterprise-integration-x12.md)ja [EDIFACT](./app-service-logic-enterprise-integration-edifact.md) lähettäminen liikekumppanien välillä. Viestit voidaan myös suojata salaus ja digitaalisten allekirjoitusten käyttäminen. 

Pakettiin avulla organisaatiot, joissa käytetään eri protokollat ja formats vaihtaa viestejä sähköisesti muodonmuutoksen eri muodoissa, molemmissa organisaatioissa järjestelmien voi tulkita ja toimenpiteistä muotoon. 

Jos olet tutustunut BizTalkin tai Microsoft Azure BizTalk Services, löydät sen käyttöä Enterprise-integroinnin ominaisuuksia, koska käsitteitä on samanlainen. Yksi on Enterprise integrointi käytetään integrointi tilit yksinkertaistaa tallennus- ja palvelutiedot käytetään B2B viestinnän hallinta. 

Architecturally yrityksen Integration Pack perustuu **integrointi tilit** , joihin voidaan tallentaa kaikki tiedot, jonka avulla voidaan suunnitella, ottaa käyttöön ja ylläpitää B2B sovelluksia. Integrointi-tili on lähinnä pilvipohjainen tallennuspaikka, kuten mallit, kumppaneille, varmenteet, karttoja ja toimeenpano säilön. Näitä tietoja voi käyttää sitten logiikan sovelluksissa luonnissa B2B työnkulkuja. Ennen kuin voit käyttää palvelutiedot logiikan-sovelluksessa, sinun täytyy Linkitä integrointi tilisi logiikan-sovellukseen. Logiikan sovelluksen on pääsy integrointi asiakkaan palvelutiedot jälkeen linkittämällä ne.  

## <a name="why-should-you-use-enterprise-integration"></a>Miksi yrityksen integrointi kannattaa käyttää?
- Enterprise-integroinnin olet tallentanut kaikki tiedot yhdessä paikassa, joka integrointi-tilisi on. 
- Logiikan sovellukset-ohjelma ja sen yhdistimet integroida 3 parannetulla SaaS, paikallisen sovellukset sekä mukautetut sovellukset ja muodostaa B2B työnkulkuja voidaan hyödyntää
- Voit myös hyödyntää Azure Funktiot

## <a name="how-to-get-started-with-enterprise-integration"></a>Miten yrityksen integrointi aloittamaan?
Voit luoda ja yrityksen integrointi pakettiin kautta logiikan sovellusten designer käyttäminen **Azure portal**B2B sovellusten hallinta.  

Voit myös logiikan sovellusten hallinta [PowerShellin](https://msdn.microsoft.com/library/azure/mt652195.aspx "logiikan sovellusten PowerShell-ohjeita") . 

Näin sinun on suoritettava, ennen kuin voit luoda sovelluksia Azure-portaalissa vaiheiden yleiskuvaus: ![yleistä-kuva](./media/app-service-logic-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Mitä joitakin yleisiä tilanteita, joissa on?

Yrityksen integrointi tukee seuraavia yleisesti käytettyjen:   

- Muokkaa - sähköisen Data Interchange  
- EAI - yrityssovelluksen integrointi  

## <a name="heres-what-you-need-to-get-started"></a>Seuraavassa on hyödyllisiä avulla pääset alkuun
- Azure tilauksen integrointi-tilillä
- Visual Studio 2015 voit luoda karttoja ja rakenteet
- [Microsoft Azure logiikan sovellusten yrityksen integrointi Tools for Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it"></a>Kokeile
[Kokeile sitä nyt](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) ottamaan perustoiminnot otoksen AS2 Lähetä ja vastaanota logiikan-sovellusta, joka käyttää logiikan sovellusten B2B-ominaisuuksia.

## <a name="learn-more-about"></a>Lisätietoja:
- [Toimeenpano] (./app-service-logic-enterprise-integration-agreements.md "Lue lisää yrityksen integrointi toimeenpano")
- [Liiketoiminta-Business (B2B)-skenaariot] (./app-service-logic-enterprise-integration-b2b.md "Opettele luomaan logiikan sovellusten B2B ominaisuuksilla")  
- [Varmenteet] (./app-service-logic-enterprise-integration-certificates.md "Lue lisää yrityksen integrointi varmenteet")
- [Koodaus/koodauksen tietuetiedostoon] (./app-service-logic-enterprise-integration-flatfile.md "Opettele koodata ja toistaa tietuetiedostoon sisältö")  
- [Integrointi tilit] (./app-service-logic-enterprise-integration-accounts.md "Lue lisää integrointi tilit")
- [Kartat] (./app-service-logic-enterprise-integration-maps.md "Lue lisää yrityksen integrointi määritykset")
- [Kumppaneille] (./app-service-logic-enterprise-integration-partners.md "Lue lisää yrityksen integrointi kumppanien")
- [Rakenteet] (./app-service-logic-enterprise-integration-schemas.md "Lue lisää yrityksen integrointi rakenteet")
- [XML-viestin tarkistus] (./app-service-logic-enterprise-integration-xml.md "Lue, miten voit tarkistaa XML-sanomat logiikan sovelluksilla")
- [XML-muunnos] (./app-service-logic-enterprise-integration-transform.md "Lue lisää yrityksen integrointi määritykset")
- [Yrityksen integrointi yhdistimet] (../connectors/apis-list.md "Lue lisää yrityksen integrointi pack yhdistimet")



