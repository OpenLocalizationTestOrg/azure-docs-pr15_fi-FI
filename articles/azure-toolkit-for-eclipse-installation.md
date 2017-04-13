<properties
    pageTitle="Azure-työkalut asennetaan Pimennys | Microsoft Azure"
    description="Opi asentamaan Azure-työkalujen Pimennys varten."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

# <a name="installing-the-azure-toolkit-for-eclipse"></a>Azure-työkalut asennetaan Pimennys

Azure-työkalujen Pimennys varten on malleja ja toimintoja, joiden avulla voit helposti luoda, kehittää, Testaa ja Azure sovellusten Pimennys kehitysympäristö otetaan käyttöön. Azure-työkalujen Pimennys varten on Avaa lähde-projekti, jonka lähdekoodi on käytettävissä seuraavassa URL-osoitteessa GitHub projektin sivustosta MIT-käyttöoikeudet-kohdassa:

<https://github.com/Microsoft/Azure-Tools-for-Java>

Seuraavat vaiheet Näytä asentamisesta, Pimennys Azure-työkalut.

[AZURE.INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Asenna varten Pimennys Azure-Työkalut

1. Aloita Pimennys.

1. Pimennys avatessa **Ohje** -valikossa ja valitse sitten **Asenna uusi ohjelmisto**seuraavassa kuvassa esitetyllä tavalla.

    ![Azure-työkalut asennetaan Pimennys][01]

1. Kirjoita **Käytettävissä ohjelmisto** -valintaikkunassa **käsitteleminen** tekstiruudussa **http://dl.microsoft.com/eclipse** ja sen jälkeen **Enter** -näppäintä.

1. Valitse **nimi** -ruudussa **Azure työkalujen Pimennys varten**ja poista **kaikki Päivitä sivustot aikana yhteyshenkilön asentaa löydät tarvittavat ohjelmistot**. Näytössä pitäisi näyttää seuraavankaltaiselta:

    ![Azure-työkalut asennetaan Pimennys][02]

1. Jos laajennat **Azure työkalujen Pimennys varten**, näet seuraavat kohteet:

    * **Sovelluksen tiedot-laajennuksen Java**: tämän osan avulla voit Azure's telemetriatietojen kirjaaminen ja analysis Services-palvelujen sovellusten ja server esiintymät.
    * **Azure Access ohjausobjektin suodatin**: Tämä osa tukee sovelluksen käyttäjien Azure ACS, Sign-skenaariot ottaminen käyttöön ja externalizing tunnistetietojen logiikan sovelluksesta.
    * **Azure yleisiä laajennuksen**: Tämä osa mahdollistaa työkalujen muiden osien tarvitsemia Yleiset toiminnot.
    * **Azure Explorerin Pimennys**: Tämä osa mahdollistaa työkalujen muiden osien tarvitsemia Yleiset toiminnot.
    * **Azure ‑laajennuksen Pimennys Java kanssa**: tämän osan tukee projekteissa, jotka auttavat muodosta, Testaa ja ota käyttöön Microsoft Azure pilveen Pimennys ja kautta komentoriviltä Java-sovellusten kehittämiseen.
    * **Azure Web Apps-laajennus Java kanssa**: tämän osan tukee Java verkkosovellukset Microsoft Azure Web Appin säiliöiden käyttöönotto.
    * **Microsoft JDBC ohjaimen 4.2 SQL Serverin**: tämän osan tarjoaa JDBC API Java Platform Enterprise Edition 8 SQL Server-ja Microsoft Azure SQL-tietokanta.
    * **Pakkaaminen Apache Qpid asiakkaan kirjastoja JMS**: tämän osan avulla voit ottaa käyttöön sovelluksen käyttämään AMQP messaging Microsoft Azure Apache Qpid projektista JMS asiakas-osan.
    * **Microsoft Azure-kirjastojen Java pakkaaminen**: tämän osan tarjoaa ohjelmointirajapinnan käyttäminen Microsoft Azure-palvelut, kuten tallennuskiintiön, palvelun bus, palvelun suorituksenaikainen prosessi.

1. Valitse **Seuraava**. (Jos kohtaat epätavallisia viiveitä työkalujen asennettaessa, varmista, että **kaikki Päivitä sivustot aikana yhteyshenkilön asentaa löydät tarvittavat ohjelmistot** ei ole valittuna.)

1. **Asenna tiedot** -valintaikkunassa valitsemalla **Seuraava**.

    ![Tarkista asennustiedot][03]

1. **Tarkista käyttöoikeudet** -valintaikkunassa Tarkista käyttöoikeuden sopimusten ehdot. Jos hyväksyt käyttöoikeuden sopimusten ehdot, valitse **hyväksyn käyttöoikeuden sopimusten** ja valitse sitten **Valmis**. (Jäljellä oleva vaiheissa oletetaan hyväksyt käyttöoikeuden sopimusten ehdot. Jos et hyväksy käyttöoikeuden sopimusten ehdot, Lopeta asennus.)

    ![Tarkista käyttöoikeudet][04]

    Pimennys Lataa ja asenna edeltävät paketit.

    ![Asennuksen][05]

1. Jos ohjelma pyytää käynnistämään Pimennys Viimeistele asennus, valitse **Kyllä**.

    ![Käynnistä kehote][06]

## <a name="see-also"></a>Katso myös

Lisätietoja Java IDEs Azure työkalupakettien on seuraavissa linkeissä:

- [Azure työkalujen Pimennys varten]
  - *Azure-työkalut asennetaan Pimennys (tämä artikkeli)*
  - [Luo Hei maailma verkkosovellukseen Pimennys Azure]
  - [Mitä uusia ominaisuuksia, Pimennys Azure Työkalut]
- [Azure työkalujen IntelliJ varten]
  - [Azure-työkalut asennetaan IntelliJ]
  - [Luo Hei maailma verkkosovellukseen IntelliJ Azure]
  - [Mitä uusia ominaisuuksia Azure työkalujen IntelliJ varten]

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center].

<!-- URL List -->

[Azure työkalujen Pimennys varten]: ./azure-toolkit-for-eclipse.md
[Azure työkalujen IntelliJ varten]: ./azure-toolkit-for-intellij.md
[Luo Hei maailma verkkosovellukseen Pimennys Azure]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Luo Hei maailma verkkosovellukseen IntelliJ Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Azure-työkalut asennetaan IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Mitä uusia ominaisuuksia, Pimennys Azure Työkalut]: ./azure-toolkit-for-eclipse-whats-new.md
[Mitä uusia ominaisuuksia Azure työkalujen IntelliJ varten]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

