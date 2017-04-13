<properties 
    pageTitle="Virheenkorjaus Pimennys Azure Java-Web-sovellus | Microsoft Azure" 
    description="Tässä opetusohjelmassa näytetään, miten virheenkorjaus Java verkkosovellukseen Azure-käyttöjärjestelmässä, Pimennys Azure-työkalujen avulla." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="debug-a-java-web-app-on-azure-in-eclipse"></a>Virheenkorjaus Pimennys Azure Java-Web-sovellus

Tässä opetusohjelmassa näytetään, miten voit korjata Azure-käyttöjärjestelmässä käyttämällä [Azure työkalujen Pimennys for]Javan verkkosovellukseen. Yksinkertaisuuden voit käyttää basic Java palvelimen sivun (JSP)-Esimerkki Tässä opetusohjelmassa, mutta vaiheet olisi Java servlet samalla, kun Azure korjattava.

Kun olet tehnyt Tässä opetusohjelmassa, sovelluksen näyttää seuraavassa kuvassa samalla, kun se on virheenkorjaus Pimennys:

![][01]
 
## <a name="prerequisites"></a>Edellytykset

* Java Developer Kit (JDK), v 1,8 tai uudempi versio.
* Pimennys IDE Java ss kehittäjille Indigo tai uudempi versio. Tämä voi ladata <http://www.eclipse.org/downloads/>.
* Java-pohjainen web-palvelimeen tai sovelluspalvelin, kuten Apache Tomcat tai jota jakauman.
* Azure tilauksen, joka on hankittu <https://azure.microsoft.com/en-us/free/> tai <http://azure.microsoft.com/pricing/purchase-options/>.
* Pimennys Azure työkalut. Lisätietoja on artikkelissa [asentamista varten Pimennys Azure-Työkalut].
* Dynaaminen Web-projekti luodaan ja ottaa käyttöön Azure App palvelu. Katso esimerkiksi [luominen Hei maailma verkkosovellukseen Pimennys Azure varten].

## <a name="to-debug-a-java-web-app-on-azure"></a>Jos haluat korjata Azure Java-Web-sovellus

Tämän osan vaiheiden suorittamisesta, voit käyttää aiemmin määritetty dynaaminen Web projekti, jonka olet jo asentanut Azure Java-Web-sovellus, Lataa [Malli dynaaminen Web-projekti] ja noudata ohjeita käyttöön Azure [Luo Hei maailma verkkosovellukseen Pimennys Azure varten] . 

1. Avaa Pimennys.

1. Määrittää virheiden poistamiseen etäyhteyden aikakatkaisu:

    1. Pimennys **Windows** -valikkoa ja valitse sitten **asetukset**.
    1. Laajenna **Java** -solmu ja valitse sitten **Korjaa**.
    1. Sekä **Virheenkorjaus aikakatkaisu (ms)** ja **Käynnistä aikakatkaisu (ms)** määrittäminen, `120000`.

        ![][02]

    1. Valitse **OK** ja sulje **asetukset** -valintaikkunassa.

1. Napsauta Pimennys 's Project Explorer-näkymässä hiiren kakkospainikkeella dynaaminen Web-projekti, jonka olet ottanut käyttöön Azure. Kun pikavalikon tulee näkyviin, valitse **Virheenkorjaus**ja valitse sitten **Azure Web Appissa**.

    ![][03]

1. Jos kyseessä on korjattava dynaaminen Web projektin ensimmäistä kertaa, **Virheenkorjaus määritykset** -valintaikkuna avautuu; Voit hyväksyä oletusarvoja, jotka on määritetty **Yhdistä** -välilehden Työkalut. **Lähde** -välilehden **Lisää**ja valitse sitten **Java projektin**, valitse **Dynaaminen Web-projekti**ja valitse sitten **OK**. Kun olet tehnyt nämä vaiheet, valitse **Korjaa**.

    ![][04]

1. Kun sinua pyydetään **käyttöön remote virheenkorjaus Web etäsovelluksen nyt?**, valitse **OK**.

1. **Web-sovellus on nyt valmis remote virheenkorjaus**pyydettäessä valitsemalla **OK**.

    ![][05]

1. Kun **Virheenkorjaus määritykset** -valintaikkuna tulee näkyviin, valitse **Korjaa**.

1. Windowsin komentokehotteen tai Unix-liittymän avautuvat ja valmisteleminen tarvittavat yhteyden virheenkorjaus; Voit joutua odottamaan remote Java-Web-sovelluksen yhteys on tarkistettu, ennen kuin jatkat. Jos käytössäsi on Windows, se näyttää samalta kuin seuraavassa kuvassa.

    ![][06]

1. Lisää sivunvaihto kohdassa JSP-sivulla ja valitse Avaa URL-Osoitteen Java Web App selaimessa:

    1. Avaa Pimennys **Azure Explorer** ylöspäin.
    1. Siirry osoitteeseen **Web Apps -sovellusten** ja haluat korjata Java Web Appissa.
    1. Web-sovelluksen napsauttamalla hiiren kakkospainikkeella ja valitse **Avaa selaimessa**.
    1. Pimennys nyt syöttää virheenkorjaus-tilaan.

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center].

Lisätietoja Azure Web Apps-sovellusten luomisesta on artikkelissa [Yleistä Web Apps].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure työkalujen Pimennys varten]: ../azure-toolkit-for-eclipse.md
[Azure-työkalut asennetaan Pimennys]: ../azure-toolkit-for-eclipse-installation.md
[Luo Hei maailma verkkosovellukseen Pimennys Azure]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Esimerkki dynaaminen Web-projekti]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web-sovellusten yleiskatsaus]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png
