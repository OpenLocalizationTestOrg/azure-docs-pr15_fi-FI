<properties 
    pageTitle="Varmenteen lisääminen Java Varmenteiden säilön | Microsoft Azure" 
    description="Opettele lisäämään varmenteen myöntäjä (CA) varmenteen Twilio palvelun tai Azure-palvelu Bus Java CA varmenne (cacerts)-kauppa." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Java CA varmenteiden säilön varmennetta lisääminen
Seuraavia ohjeita noudattamalla voit lisätä varmenteen myöntäjä (CA) varmenteen Myöntäjän Java varmenne (cacerts). Esimerkki, käyttää on Twilio-palvelun edellyttämät CA-sertifikaatin. Tiedot artikkelin käsitellään asentaminen CA-sertifikaatin Azure-palvelu Bus. 

Keytool avulla voit lisätä oman JDK pakkaamisesta ja lisätä se Azure projektin **approot** kansioon ennen CA-sertifikaatin tai voit suorittaa Azure käynnistys-tehtävä, joka käyttää keytool Lisää varmenne. Tässä esimerkissä oletetaan, että lisäät CA varmenteen ennen parhaillaan zip JDK. Lisäksi CA käytettävän varmenteen käytetään esimerkissä, mutta eri CA-sertifikaatin hankkiminen ja tuontitoimintoa cacerts säilöön vaiheet on samanlainen.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Varmenteen lisääminen cacerts säilö

1. Komentorivin, joka on määritetty yhteyttä JDK **jdk\jre\lib\security** kansioon Suorita asennettujen mitä varmenteita seuraavasti:

    `keytool -list -keystore cacerts`

    Sinua pyydetään store-salasana. Oletus-salasana on **changeit**. (Jos haluat vaihtaa salasanan, katso keytool documentation <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) Tässä esimerkissä oletetaan, että MD5 sertifikaattiin fingerprint 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 ei ole luettelossa ja, jonka haluat tuoda sen (Twilio API-palvelun tarvitaan tietyn sertifikaatti).
2. Hanki varmenne [GeoTrust varmenteet](http://www.geotrust.com/resources/root-certificates/)on lueteltu varmenteiden luettelo. Sarjanumero 35:DE:F4:CF sertifikaattiin linkkiä hiiren kakkospainikkeella ja tallenna se **jdk\jre\lib\security** -kansioon. Tarkoitetaan tässä esimerkissä se on tallennettu nimellä **Equifax\_suojatun\_sertifikaatin\_Authority.cer**.
3. Sertifikaattien kautta seuraava komento:

    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`

    Kun sinulta kysytään, luotetaanko tämän todistuksen Jos varmenne on MD5 tunnisteen 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, vastaa kirjoittamalla **y**.
4. Suorita seuraava komento varmistaa varmenteen Myöntäjä on tuotu onnistuneesti:

    `keytool -list -keystore cacerts`

5. Zip JDK ja lisää se Azure projektin **approot** kansioon.

Lisätietoja keytool on artikkelissa <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Azure varmenteet

Azure (kuten Azure-palvelu Bus) palveluita käyttävät sovellukset on luotettava Baltimore CyberTrust neliöjuuren. (Alkaa 15. huhtikuussa 2013 Azure alkamisen siirtyminen GTE CyberTrust yleinen pääsivustolla Baltimore CyberTrust päätasolle. Tämän siirron kulunut useita kuukautta).

Varmenteen ehkä jo asennettu cacerts-kaupan Baltimore niin muistaa suorittaa **keytool-luettelosta** komento, jotta näet, jos se on jo olemassa.

Jos haluat lisätä Baltimore CyberTrust pääkansio, se on sarjanumero 02:00:00:b9 ja SHA1 tunnisteen d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Se voi ladata <https://cacert.omniroot.com/bc2025.crt>, paikallinen tiedosto, jonka tunniste **.cer**tallennettu ja tuodaan käyttämällä **keytool** kuin yllä.

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja Azure käyttämä päämyöntäjien varmenteet [Azure päämyöntäjien varmenteen siirron](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Saat lisätietoja Java [Java Developer Center](/develop/java/).
