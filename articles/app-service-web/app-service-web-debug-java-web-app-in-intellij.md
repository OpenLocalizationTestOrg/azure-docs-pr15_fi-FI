<properties 
    pageTitle="Korjaa Azure IntelliJ Java-Web-sovellus | Microsoft Azure" 
    description="Tässä opetusohjelmassa näytetään, miten virheenkorjaus Java verkkosovellukseen Azure-käyttöjärjestelmässä, IntelliJ Azure-työkalujen avulla." 
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

# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>Korjaa Azure IntelliJ Java-Web-sovellus

Tässä opetusohjelmassa näytetään, miten voit korjata käytössä Azure käyttämällä [Azure työkalujen for IntelliJ]Javan verkkosovellukseen. Yksinkertaisuuden voit käyttää basic Java palvelimen sivun (JSP)-Esimerkki Tässä opetusohjelmassa, mutta vaiheet olisi Java servlet samalla, kun Azure korjattava.

Kun olet tehnyt Tässä opetusohjelmassa, sovelluksen näyttää seuraavassa kuvassa samalla, kun se on virheenkorjaus IntelliJ:

![][01]
 
## <a name="prerequisites"></a>Edellytykset

* Java Developer Kit (JDK), v 1,8 tai uudempi versio.
* IntelliJ VERRATA Ultimate Edition. Tämä voi ladata <https://www.jetbrains.com/idea/download/index.html>.
* Java-pohjainen web-palvelimeen tai sovelluspalvelin, kuten Apache Tomcat tai jota jakauman.
* Azure tilauksen, joka on hankittu <https://azure.microsoft.com/en-us/free/> tai <http://azure.microsoft.com/pricing/purchase-options/>.
* IntelliJ Azure työkalut. Lisätietoja on artikkelissa [asentamista varten IntelliJ Azure-Työkalut].
* Dynaaminen Web-projekti luodaan ja ottaa käyttöön Azure App palvelu. Katso esimerkiksi [luominen Hei maailma verkkosovellukseen Azure IntelliJ varten].

## <a name="to-debug-a-java-web-app-on-azure"></a>Jos haluat korjata Azure Java-Web-sovellus

Tämän osan vaiheiden suorittamisesta, voit käyttää aiemmin määritetty dynaaminen Web projekti, jonka olet jo asentanut Azure Java-Web-sovellus, Lataa [Malli dynaaminen Web-projekti] ja noudata ohjeita käyttöön Azure [Luo Hei maailma verkkosovellukseen Azure IntelliJ varten] . 

1. Avaa Java Web App projekti, jonka voit ottaa käyttöön Azure IntelliJ.

1. **Suorita** -valikkoa ja valitse sitten **Muokkaa määrityksiä...**.

    ![][02]

1. Kun **Suorita/virheenkorjaus määritykset** -valintaikkuna avautuu: 

    1. Valitse **Azure Web Appissa**.
    1. Valitse **+** Lisää uudet asetukset.
    1. Anna **nimi** kokoonpanoa varten.
    1. Hyväksy jäljellä oletusarvoja, jotka ovat ehdottamia Azure-työkalut ja valitse sitten **OK**.

        ![][03]

1. Valitse juuri luomasi valikon oikeassa yläkulmassa Azure Web App-virheenkorjaus määritystä, ja valitse **korjaaminen**

    ![][04]

1. Kun sinua pyydetään **käyttöön remote virheenkorjaus Web etäsovelluksen nyt?**, valitse **OK**.

1. **Web-sovellus on nyt valmis remote virheenkorjaus**pyydettäessä valitsemalla **OK**.

    ![][05]

1. Valitse juuri luomasi valikon oikeassa yläkulmassa Azure Web App-virheenkorjaus määritystä, ja valitse sitten Valitse **Korjaa**.

1. Windowsin komentokehotteen tai Unix-liittymän avautuvat ja valmisteleminen tarvittavat yhteyden virheenkorjaus; Voit joutua odottamaan remote Java-Web-sovelluksen yhteys on tarkistettu, ennen kuin jatkat. Jos käytössäsi on Windows, se näyttää seuraavalta:

    ![][06]

1. Lisää sivunvaihto kohdassa JSP-sivulla ja valitse Avaa URL-Osoitteen Java Web App selaimessa:

    1. Avaa IntelliJ **Azure Explorer** ylöspäin.
    1. Siirry osoitteeseen **Web Apps -sovellusten** ja haluat korjata Java Web Appissa.
    1. Web-sovelluksen napsauttamalla hiiren kakkospainikkeella ja valitse **Avaa selaimessa**.
    1. IntelliJ nyt syöttää virheenkorjaus-tilaan.

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center].

Lisätietoja Azure Web Apps-sovellusten luomisesta on artikkelissa [Yleistä Web Apps].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure työkalujen IntelliJ varten]: ../azure-toolkit-for-intellij.md
[Azure-työkalut asennetaan IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Luo Hei maailma verkkosovellukseen IntelliJ Azure]: ./app-service-web-intellij-create-hello-world-web-app.md
[Esimerkki dynaaminen Web-projekti]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web-sovellusten yleiskatsaus]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png
