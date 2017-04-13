<properties 
    pageTitle="Cloud Services ja hallinnan varmenteet | Microsoft Azure" 
    description="Lue, miten voit luoda ja käyttää varmenteet Microsoft Azure" 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="certificates-overview-for-azure-cloud-services"></a>Azure pilvipalveluihin yleistä varmenteet
Varmenteiden käytetään Azure pilvipalveluihin ([service todistusten](#what-are-service-certificates)) ja todennustapa hallinnan API ([management varmenteet](#what-are-management-certificates) Azure perinteinen portaalin ja ei-ARM käytettäessä). Tässä ohjeaiheessa antaa varmenteen molempien tietotyyppien yleiskatsaus siitä, miten voit [luoda](#create) ja [ottaa käyttöön](#deploy) Azure ne.

Azure-tietokannassa sertifikaatit ovat x.509 v3 varmenteet ja voit allekirjoittaa luotettu varmenteen toiseen tai ne voivat olla itse allekirjoitettua. Itse allekirjoitetun varmenteen on allekirjoitettu omassa tekijän mukaan, minkä vuoksi, ei luotettujen oletusarvoisesti. Useimmissa selaimissa voit ohittaa tämän. Itse allekirjoitetun varmenteet kannattaa käyttää vain itse kehittä ja testaus cloud Services-palvelut. 

Azure käyttämät varmenteet voi olla yksityisen tai julkinen avain. Varmenteet on allekirjoitus, jonka avulla voidaan tunnistaa yksiselitteisesti tavalla. Tämä allekirjoitus käytetään Azure- [kokoonpanotiedosto](cloud-services-configure-ssl-certificate.md) tunnistaa käytettävän varmenteen pilvipalveluun tulisi käyttää. 

## <a name="what-are-service-certificates"></a>Mitä service todistusten?
Palvelun varmenteet on liitetty cloud services ja suojattu tietoliikenne ja ja -palvelusta. Esimerkiksi web-rooli on otettu käyttöön, jos haluat antaa varmenne, jota voi todentaa näkyviä HTTPS-päätepisteen. Palvelun varmenteet-palvelu-määrityksessä määritetty käyttöön automaattisesti virtuaalikoneen, jossa on käytössä tehtäväsi esiintymä. 

Voit ladata service todistusten Azure perinteinen-portaaliin käyttämällä joko Azure perinteinen portaalin tai käyttämällä Service Management API. Palvelun varmenteet liittyvät tiettyyn pilvipalvelussa ja määritetty-palvelun määritystiedoston käyttöönottoa.

Palvelun varmenteita voi hallita erikseen palvelujen ja hallitsee eri henkilöille. Esimerkiksi kehittäjä voi ladata palvelupakettiin, joka viittaa IT-hallinta on äsken Azure käytettävä varmenne. IT-hallinta voit hallita ja uusia sertifikaatin muuttaminen palvelun määrityksen tarvitsematta lataaminen uuteen palvelupakettiin. Tämä on mahdollista, koska looginen nimi varmenteen ja sen liikkeen nimi ja sijainti on määritetty palvelun määritystiedoston, kun varmenteen allekirjoitus on määritetty palvelun määritystiedostossa. Päivitettävä varmenne on vain ladata uutta varmennetta ja muuttaa palvelun määritystiedostossa allekirjoitus-arvoa.

## <a name="what-are-management-certificates"></a>Mitkä ovat hallinta varmenteet?
Hallinnan varmenteet mahdollistavat Azure perinteinen myöntämä Service Management Ohjelmointirajapinnan todentamismenetelmä. Monta ohjelmat ja työkalut (esimerkiksi Visual Studio tai Azure SDK) käytetään nämä varmenteet automatisoida määrittämistä ja käyttöönottoa eri Azure palveluista. Näitä ei todella liity pilvipalveluun. 

>[AZURE.WARNING] Ole varovainen! Varmenteiden seuraavanlaisiin sallia kenen tahansa todentaa heidän kanssaan niihin liittyviä tilauksen hallintaan. 

### <a name="limitations"></a>Rajoitukset
100 hallinta varmenteet tilauskohtaisten enimmäismäärä on. On myös enintään 100 hallinta varmenteet kaikkien tilausten kohdassa tietyn palvelun järjestelmänvalvojan käyttäjätunnusta. Jos järjestelmänvalvojan tilin Käyttäjätunnus on jo käytetty Lisää 100 hallinta varmenteet tarvitaan lisää varmenteet, voit lisätä muiden järjestelmänvalvoja, voit lisätä muita varmenteet. 

Ennen kuin lisäät yli 100 varmenteet-kohdassa käyttää varmenne. Varmenteen hallintaprosessin avulla apuyhteyshenkilöiden Lisää mahdollisesti tarpeettomat monimutkaisuus.


<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Uuden itse allekirjoitetun varmenteen luominen
Voit käyttää mitä tahansa työkalu voi luoda itse allekirjoitettua varmennetta, kunhan ne noudattaa näitä asetuksia:

* X.509-varmenne.
* Sisältää yksityinen avain.
* Luo avaimen exchange (.pfx-tiedosto).
* Aihe on vastattava käyttää pilvipalvelussa sijaitsevaan toimialueen. 
    > Hankittava ei voi lisätä toimialueen SSL varmenne cloudapp.net (tai mitä tahansa Azure liittyvät); Varmenteen aihe on vastattava sovellus käyttää mukautettua toimialuenimeä. Esimerkiksi **contoso.net**, **contoso.cloudapp.net**.
* Vähintään 2048-bittinen salaus.
* **Palvelun varmenteen vain**: Asiakkaan varmenne on sijaittava *Henkilökohtainen* varmenteen säilö.

Kahdella tavalla luomiseen varmenne, joka on Windows `makecert.exe` apuohjelma tai IIS.

### <a name="makecertexe"></a>Makecert.exe

Apuohjelma on vähennetty ja enää on kuvattu seuraavassa. Lue lisätietoja [MSDN-artikkelissa](https://msdn.microsoft.com/library/windows/desktop/aa386968) .

### <a name="powershell"></a>PowerShellin

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

>[AZURE.NOTE] Jos haluat käyttää sen sijaan, että toimialueen IP-osoite, käytä IP-osoite - DnsName-parametrin.


Jos haluat antaa tämän [todistuksen hallinta-portaalissa](../azure-api-management-certs.md), vieminen **.cer** -tiedosto:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet Information Services (IIS)

On useita sivuja Internetissä, jotka kattavat hallinnasta IIS: N kanssa. [Seuraavassa](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) on hyvä löydy luulen kerrotaan paikan päällä. 

### <a name="java"></a>Java
Voit käyttää [varmenteen](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate)Java.

### <a name="linux"></a>Linux
[Tässä](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) artikkelissa käsitellään Luo varmenteet SSH kanssa.

## <a name="next-steps"></a>Seuraavat vaiheet

[Lataa Palveluvarmennetta Azure perinteinen-portaaliin](cloud-services-configure-ssl-certificate.md) (tai [Azure portal](cloud-services-configure-ssl-certificate-portal.md)).

Lataa [management API varmenteen](../azure-api-management-certs.md) Azure perinteinen-portaaliin.

>[AZURE.NOTE] Azure portaalin Käytä hallinta varmenteet käyttämään Ohjelmointirajapinnan, mutta käyttää sen sijaan käyttäjätilit.
