<properties
    pageTitle="Web-sovellusten Azure pino - tunnetut ongelmat ja vianmääritys | Microsoft Azure"
    description="Yksityiskohtaiset ohjeet Azure Pinotut Web Apps-sovellusten käyttöönotto"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Web Apps resurssin toimittaja - tunnetut ongelmat ja vianmääritys

> [AZURE.NOTE] Seuraavat tiedot koskevat vain Azure pinon TP1 ominaisuuksissa.

Jos ongelmia ilmenee käyttöönotto tai Azure pinon Web Apps-sovellusten käyttäminen Katso alla olevia ohjeita.

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure pinon Web Apps ei ole käytettävissä-portaalissa

![Azure pinon verkkosovelluksissa Luo uusi Web App][1]

Kun olet suorittanut [Azure pinon Web Apps-palvelun rekisteröinti](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm) ei voi etsiä Web + Mobile sivu Valitse toimi seuraavasti:
* **Kirjaudu ulos-portaalin** ja **Sulje selain** ja kirjoita sitten **Uusi selaimen ikkunan kirjautuminen-portaaliin** ja yritä uudelleen.

Jos et silti näe web- ja mobile sivu, toimi seuraavasti:

1.  Etsi **xRPVM** ja **Yhdistä** AM **Azure pinon Host koneen** **Hyper-V** hallinnassa.
2.  Avaa **komentokehote järjestelmänvalvojana** ja suorita **IISRESET**
3.  Palaa **ClientVM** Avaa **uudessa selainikkunassa**, **Kirjaudu sisään-portaaliin** ja yritä sitten uudelleen.

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>Käyttöönoton epäonnistui, kun uusi resurssiryhmä Web App-sovelluksen luominen

Ensimmäinen esikatselu, Web Apps-sovellusten **Kaikki verkkosovelluksissa** on luotava samaan resurssin ryhmään Web Apps resurssin tarjoajaan (**AppService paikallinen**).  Tämä on tunnettu ongelma ja korjataan tulevien esikatselu.

## <a name="other-issues"></a>Lisätietoja muista

Jos käytössä ilmenee muu Web Apps-sovellusten Azure pinon ongelmat Ota kirjaaminen [Azure pino-keskustelupalstalle] (https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) jossa tämän ryhmän jäsenet ovat seuranta viestit.


<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
