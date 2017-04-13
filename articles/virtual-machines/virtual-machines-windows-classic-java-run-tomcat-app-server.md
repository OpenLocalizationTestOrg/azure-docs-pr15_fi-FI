<properties
    pageTitle="Tomcat virtual koneeseen | Microsoft Azure"
    description="Tässä opetusohjelmassa käytetään resursseja perinteinen käyttöönotto-mallin avulla luotu ja näyttää, miten voit luoda Windows Virtual machine ja määrittää sen toimimaan Apache Tomcat sovelluspalvelin."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.workload="web"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Java-sovelluspalvelin suorittaminen perinteisen käyttöönotto-mallin avulla luotu virtual tietokoneeseen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Azure, jossa voit virtual machine antamaan palvelimen ominaisuuksia. Esimerkkinä Azure-käyttöjärjestelmässä virtual koneen voi määrittää isännöimiseen Java sovelluspalvelin, kuten Apache Tomcat. Kun olet suorittanut tämän oppaan, on tietoja siitä, miten voit luoda Azure-käyttöjärjestelmässä virtual koneen ja määrittää sen toimimaan Java sovelluspalvelin.

Opit seuraavat asiat:

* Miten voit luoda virtual tietokoneeseen, jossa on Java Development Kit (JDK) asennettuna.
* Voit kirjautua etäyhteyden virtuaalikoneen.
* Miten voit asentaa Java sovelluspalvelin virtuaalikoneen.
* Voit luoda virtuaalikoneen päätepiste.
* Miten voit avata palomuurin sovelluksen palvelimen portti.

Tässä opetusohjelmassa Apache Tomcat-sovelluspalvelin asennetaan virtual koneeseen. Valmiit asennus aiheuttaa Tomcat asennuksessa, kuten jokin seuraavista.

![Virtual machine Apache Tomcat käynnissä][virtual_machine_tomcat]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Voit luoda virtual machine

1. Kirjautuminen [Azure perinteinen portal](https://manage.windowsazure.com).
2. Valitse **Uusi**, valitse **Laske**, valitse **virtuaalikoneen**ja valitse sitten **-Valikoima**.
3. Valitse **virtuaalikoneen kuvan valitseminen** -valintaikkunassa **JDK 7 Windows Server 2012**.
Huomaa, että **JDK 6 Windows Server 2012: ssa** on käytettävissä, jos sinulla on vanhoja sovelluksia, jotka eivät ole valmis suorittamaan JDK 7: ssä.
4. Valitse **Seuraava**.
5. **Virtuaalikoneen määritys** -valintaikkunassa:
    1. Määritä virtuaalikoneen nimi.
    2. Määritä käytettävät virtuaalikoneen koko.
    3. Kirjoita järjestelmänvalvojan nimi **Käyttäjänimi** -kenttään. Muista tämä käyttäjänimi ja salasana, voit syöttää seuraava, voit käyttää ne kun etäyhteyden virtuaalikoneen kirjautumisen.
    4. Kirjoita **Uusi salasana** -kenttään salasana ja kirjoita se uudelleen **Vahvista** -kenttään. Tämä on järjestelmänvalvojan tilin salasana.
    5. Valitse **Seuraava**.
6. Seuraava **virtuaalikoneen määritys** -valintaikkunassa:
    1. **Pilvipalvelussa**Käytä oletusarvoa **Luo uusi pilvipalvelussa**.
    2. **Cloud palvelun DNS-nimi** -arvo on oltava yksilöllinen cloudapp.net. Voit tarvittaessa muokata tätä arvoa niin, että Azure ilmaisee on yksilöllinen.
    2. Määritä alue, affiniteetti ryhmän tai VPN. Tässä opetusohjelmassa yhdistämiskyselyssä Määritä alue, kuten **Länsi US**.
    2. **Tallennustilan tilin**Valitse **automaattisesti luotu tallennustilan tilin käyttäminen**.
    3. **Saatavuus**Valitse **(ei mitään)**.
    4. Valitse **Seuraava**.
7. Valitse lopullisessa **virtuaalikoneen määritys** -valintaikkunassa:
    1. Hyväksy oletusarvot päätepiste.
    2. Valitse **Valmis**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>Voit kirjautua etäyhteyden virtuaalikoneen

1. Kirjaudu [Azure perinteinen portal](https://manage.windowsazure.com).
2. Valitse **näennäiskoneiden**.
3. Napsauta nimeä, jonka haluat kirjautua virtuaalikoneen.
4. Kun virtuaalikoneen on jo alkanut-Ponnahdusvalikossa sivun alareunassa sallii yhteydet.
5. Valitse **Muodosta yhteys**.
6. Vastaa näyttöön muodostaa virtuaalikoneen tarvittaessa. Tämä on aiheuttaa tallentaminen tai avaaminen .rdp-tiedosto, joka sisältää yhteystiedot. Voit joutua kopioi URL-osoite: portti .rdp-tiedoston ensimmäisen rivin viimeinen osana ja liitä se remote-kirjautuminen-sovelluksessa.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>Jos haluat asentaa Java sovelluspalvelin virtuaalikoneen

Voit kopioida Java sovelluspalvelin virtuaalikoneen tai voit asentaa Java-sovelluspalvelin asennusohjelmaa kautta.

Tässä opetusohjelmassa tarkoitetaan Tomcat asennetaan.

1. Kun olet kirjautuneena virtuaalikoneen, Avaa [Apache Tomcat](http://tomcat.apache.org/download-70.cgi)selaimen istunto.
2. Kaksoisnapsauta **32-bittinen ja 64-bittinen Windows-palvelun Installer**linkkiä. Käyttämällä tätä tapaa Tomcat asennetaan Windows-palvelun.
3. Valitse pyydettäessä Suorita asennusohjelma.
4. Sisällä **Apache Tomcat** ohjatun kehotteiden Tomcat asentamiseksi. Tässä opetusohjelmassa yhdistämiskyselyssä hyväksy oletusarvot on kunnossa. Kun pääset **ohjatun Apache Tomcat ohjattu asennus** -valintaikkunassa, voit tarkistaa halutessasi **Suorittaa Apache Tomcat** on Tomcat Aloita nyt. Valitse Viimeistele Tomcat asennus **Valmis** .

## <a name="to-start-tomcat"></a>Aloita Tomcat
Jos et valinnut Suorita Tomcat **ohjatun Apache Tomcat ohjattu asennus** -valintaikkunassa, voit käynnistää sen avaamista komentokehote-virtuaalikoneen ja suorittamalla **verkon Käynnistä Tomcat7**.

Jos suoritat virtual machine selain ja Avaa <8080>Tomcat pitäisi tulla näkyviin.

Saat Tomcat suorittaminen ulkoisen koneet, sinun on päätepisteen luominen ja portin avaaminen.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>Voit luoda päätepiste virtuaalikoneen
1. Kirjautuminen [Azure perinteinen portal](https://manage.windowsazure.com).
2. Valitse **näennäiskoneiden**.
3. Napsauta virtuaalikoneen Java-sovelluksen Serveriä suorittava nimeä.
4. Valitse **päätepisteet**.
5. Valitse **Lisää**.
6. Varmista **Lisää päätepisteen** -valintaikkunan **Lisää erillinen päätepiste** on valittuna, ja valitse sitten **Seuraava**.
7. **Uusi päätepisteen tiedot** -valintaikkunassa:
    1. Päätepisteen; nimi esimerkiksi **HttpIn**.
    2. Määritä **TCP** -protokolla.
    3. Määritä julkinen portin **80** .
    4. Voit määrittää yksityinen portin **8080** .
    5. Sulje valintaikkuna valitsemalla **Valmis** . Päätepiste nyt luodaan.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>Voit avata portin virtuaalikoneen palomuurissa
1. Kirjaudu virtuaalikoneen.
2. Napsauta **Windowsin Käynnistä-painiketta**.
3. Valitse **Ohjauspaneeli**.
4. Valitse **Järjestelmä ja suojaus**, valitse **Windowsin palomuuri**ja valitse sitten **Lisäasetukset**.
5. Valitse **Saapuvan liikenteen säännöt**ja valitse sitten **Uusi sääntö**.
 ![Uusi saapuvan liikenteen sääntö][NewIBRule]
6. **Sääntötyyppi**Valitse **portti**ja valitse sitten **Seuraava**.
 ![Uusi saapuvan liikenteen sääntö portti][NewRulePort]
7. **Protokolla ja portit** näytössä Valitse **TCP**, Määritä **8080** **tietyn paikallisen portin**ja valitse sitten **Seuraava**.
 ![Uusi saapuvan liikenteen sääntö][NewRuleProtocol]
8. **Toiminnon** näytössä Valitse **Salli yhteys**ja valitse sitten **Seuraava**.
 ![Uusi saapuvan liikenteen sääntö-toiminto][NewRuleAction]
9. **Profiilin** näytössä Varmista, että **toimialue**, **Yksityinen**ja **julkinen** ovat valittuina, ja valitse sitten **Seuraava**.
 ![Saapuvan liikenteen säännön uusi profiili][NewRuleProfile]
10. Valitse **nimi** -näytössä sääntö **HttpIn** (säännön nimi ei tarvitse vastaa päätepisteen nimi, mutta), kuten nimi ja valitse sitten **Valmis**.  
 ![Uusi saapuvan liikenteen säännön nimi][NewRuleName]

Tässä vaiheessa Tomcat sivuston olisi voi tarkastella ulkoisia selaimelta lomakkeen URL-Osoitteen kautta * *http://*oman\_DNS\_nimi*. cloudapp.net**jossa ** *oman\_DNS\_nimi*** on määritetty luotaessa virtuaalikoneen DNS-nimi.

## <a name="application-lifecycle-considerations"></a>Sovelluksen elinkaari huomioon otettavia seikkoja
* Voit luoda oman web-sovelluksen arkisto (SODAN) ja lisääminen **webapps** -kansioon. Esimerkiksi basic Java palvelun sivun (JSP) dynaaminen web projektin luominen ja vieminen SOTA-tiedostona, kopioi SODAN virtuaalikoneen, suorita selaimessa Apache Tomcat **webapps** -kansioon.
* Kun Tomcat-palvelu on asennettu, se määritetään oletusarvoisesti voit käynnistää manuaalisesti. Voit siirtyä se käynnistymään automaattisesti palveluiden avulla. Aloita palvelut-laajennus valitsemalla **Käynnistä**, **Valvontatyökalut**ja sitten **palvelut**. Kaksoisnapsauta **Apache Tomcat** palvelua ja Aseta **käynnistystavaksi** **Automaattinen**.

    ![Palvelun automaattisen käynnistyksen määrittäminen][service_automatic_startup]

    Ottaa Tomcat Käynnistä automaattisesti etuna on se, että se käynnistyy, jos virtuaalikoneen käynnistetään (esimerkiksi ohjelmistopäivitykset, jotka edellyttävät uudelleenkäynnistyksen asentamisen jälkeen).

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja muiden palvelujen (kuten Azure-tallennustilan, palvelun bus ja SQL-tietokanta), voivat haluat ottaa Java-sovellusten tarkastelemalla [Java Developer Center](https://azure.microsoft.com/develop/java/)käytettävissä olevat tiedot.

[virtual_machine_tomcat]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProfile.png
