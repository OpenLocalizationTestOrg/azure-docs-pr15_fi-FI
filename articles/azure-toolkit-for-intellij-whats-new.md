<properties
    pageTitle="Uudet Azure työkalujen varten IntelliJ | Microsoft Azure"
    description="Lisätietoja IntelliJ Azure-työkalujen uusimmat ominaisuudet."
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
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Mitä uusia ominaisuuksia Azure työkalujen IntelliJ varten

## <a name="azure-toolkit-for-intellij-releases"></a>Azure työkalujen IntelliJ julkaisuissa

Tässä artikkelissa on tietoja eri versioista ja Azure-työkalujen uusimmat päivitykset IntelliJ.

> [AZURE.NOTE] On myös Azure-työkalujen Pimennys IDE varten. Lisätietoja on artikkelissa [Azure työkalujen Pimennys varten].

### <a name="august-26-2016"></a>26 elokuussa 2016

Azure-Työkalut, IntelliJ - elokuussa 2016-versio sisältää seuraavat parannukset:

* **Mukautetun JDK jaot**. Azure-työkalujen IntelliJ, tukee nyt määrittäminen ja käyttöönotto Azure Web App-sovelluksen säilöön haluamaansa JDK-versio:
  - Lisäksi JDKs, Azure varten voit valita myös valituista saataville Azure-Azul järjestelmien Zulu OpenJDK versiot.
  - Voit myös määrittää omia JDK jakauman, jos lataat sen ZIP-tiedostona tallennustilan tilin.
* **Azure Resurssienhallintanäkymä parannukset**:
  - Virtuaalikoneen hallinta käyttämällä Azure's uuden Resurssienhallinta mallin tuki: Voit luettelo, luominen ja poistaminen resurssien hallinta-pohjainen näennäiskoneiden IDE poistumatta.
  - Tallennustilan blob tilinhallinta Azure's resurssien hallinnan, joka täydentää "perinteinen" tallennustilan tilit aiemmin toiminnot tuki.
* **Microsoft JDBC Driver 6.0 SQL Serveriä varten**. Tämä päivitys sisältää JDBC ohjain Microsoft SQL Server (6.0), joka on nyt sisältyvät kirjastoon, jossa voit helposti lisätä Java projektien, jolloin korvaaminen vanhemman version.

### <a name="june-29-2016"></a>29 kesäkuussa 2016

Azure-Työkalut, IntelliJ - kesäkuussa 2016-versio sisältää seuraavat parannukset:

* **Java 8 vaatimus**. Azure-Työkalut, IntelliJ edellyttää Java 8, vaikka tämä vaatimus koskee ainoastaan työkalujen - sovellusten edelleen käyttää kaikkien Java versioiden on Azure tukemat.
* **Uusimman Java-JDKs tuki**. Java-JDKs uusinta versiota tuetaan nyt Azure-työkalujen avulla IntelliJ.
* **Azure SDK v2.9.1 tuki**. Azure-SDK uusin versio on nyt pienin vanhat tarvittavat varten, IntelliJ Azure-työkalut.
* **Integroitu objektit**. Saat IntelliJ Azure-Työkalut-välilehdessä nyt otoksen useita sovelluksia sovelluskehittäjät aloittaminen.
* **Integrointi HDInsight-työkalu**. IntelliJ Azure's Hdinsightiin työkalut on nyt yhteen Azure-työkalujen avulla. Lisätietoja on artikkelissa [HDInsight Työkalut ‑laajennuksen IntelliJ].
* **Remote virheenkorjaus Java Web Apps-sovelluksista**. Azure-työkalujen IntelliJ, tukee nyt Azure App palvelun Java web Apps-sovellusten remote virheenkorjaus.

### <a name="april-12-2016"></a>2016: n huhtikuun 12

Azure-Työkalut, IntelliJ - huhtikuun 2016-versio sisältää seuraavat parannukset:

* **Azure SDK v2.9.0 tuki**. Azure-SDK uusin versio on nyt pienin vanhat tarvittavat varten, IntelliJ Azure-työkalut.
* **Muut käytettävyyttä, vastausajan ja suorituskyvyn parannuksia liittyvät Azure Web App-tukea**. Suorituskyvyn optimointi-miten työkalujen yhteydessä Azure tuloksen määrä Lisää vastaa käyttöliittymässä.
* **Mahdollisuus poistaa Azure alueella IntelliJ aiemmin verkkosovelluksen-säilö**. Saat IntelliJ Azure-työkalujen avulla voit poistaa aiemmin Azure-Web-säilön poistumatta IntelliJ nyt.

## <a name="see-also"></a>Katso myös ##

Lisätietoja Java IDEs Azure työkalupakettien on seuraavissa linkeissä:

- [Azure työkalujen Pimennys varten]
  - [Azure-työkalut asennetaan Pimennys]
  - [Luo Hei maailma verkkosovellukseen Pimennys Azure]
  - [Mitä uusia ominaisuuksia, Pimennys Azure Työkalut]
- [Azure työkalujen IntelliJ varten]
  - [Azure-työkalut asennetaan IntelliJ]
  - [Luo Hei maailma verkkosovellukseen IntelliJ Azure]
  - *Mitä uusia ominaisuuksia Azure työkalujen IntelliJ (tämä artikkeli) varten*

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center].

<!-- URL List -->

[Azure työkalujen Pimennys varten]: ./azure-toolkit-for-eclipse.md
[Azure työkalujen IntelliJ varten]: ./azure-toolkit-for-intellij.md
[Luo Hei maailma verkkosovellukseen Pimennys Azure]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Luo Hei maailma verkkosovellukseen IntelliJ Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Azure-työkalut asennetaan Pimennys]: ./azure-toolkit-for-eclipse-installation.md
[Azure-työkalut asennetaan IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Mitä uusia ominaisuuksia, Pimennys Azure Työkalut]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547

[HDInsight Työkalut ‑laajennuksen IntelliJ]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md
