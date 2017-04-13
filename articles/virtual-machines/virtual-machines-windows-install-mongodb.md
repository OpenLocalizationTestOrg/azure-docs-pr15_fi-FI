<properties
    pageTitle="Asenna Windows AM MongoDB | Microsoft Azure"
    description="Opettele MongoDB asentaminen Azure-AM, käytössä Windows Server 2012 R2 luotu resurssien hallinnan käyttöönottomalli."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Asenna ja määritä MongoDB Windows-AM Azure-tietokannassa
[MongoDB](http://www.mongodb.org) on Suositut Avaa lähde, tehokas NoSQL tietokanta. Tässä artikkelissa opastaa asentaminen ja määrittäminen MongoDB Windows Server 2012 R2 virtual tietokoneeseen (AM) Azure-tietokannassa. Voit myös [asentaa MongoDB-Linux-AM Azure-tietokannassa](virtual-machines-linux-install-mongodb.md).


## <a name="prerequisites"></a>Edellytykset

Ennen kuin voit asentaa ja määrittää MongoDB, joudut Luo AM ja lisää tiedot DVD-levyllä Ihannetapauksessa siihen. Seuraavissa artikkeleissa AM luomaan ja lisäämään tietoja DVD-levyllä:

- [Luo Windows Server-AM käyttämällä Azure portaalin](virtual-machines-windows-hero-tutorial.md) tai [käyttämällä PowerShellin Azure Windows Server-AM luominen](virtual-machines-windows-ps-create.md)
- [Liitä tiedot levyn Windows Server-AM Azure portaalin](virtual-machines-windows-attach-disk-portal.md) tai [Liitä Windows Server-AM tietojen levyn käyttämisen PowerShellin Azure](https://msdn.microsoft.com/library/mt603673.aspx)
    
Aloita asentaminen ja määrittäminen MongoDB, [Kirjaudu sisään Windows Server-AM](virtual-machines-windows-connect-logon.md) Etätyöpöydän avulla.


## <a name="install-mongodb"></a>Asenna MongoDB

> [AZURE.IMPORTANT] MongoDB suojausominaisuuksista, kuten todennus- ja IP-osoite sidonta, eivät ole käytössä oletusarvoisesti. Suojaustoiminnot olisi otettava käyttöön, ennen kuin otat MongoDB tuotantoympäristössä. Lisätietoja on artikkelissa [MongoDB suojaus- ja](http://www.mongodb.org/display/DOCS/Security+and+Authentication).

1. Kun yhdistettyjä oman AM etätyöpöydän, Avaa Internet Explorer AM **Käynnistä** -valikosta.

2. Valitse **tietoturva, tietosuoja ja yhteensopivuusasetusten suositeltavaa käyttää** , kun Internet Explorerin ja valitse **OK**.

3. Internet Explorerin parannettu suojaus on käytössä oletusarvoisesti. Lisää MongoDB sivuston sallittujen sivustojen luetteloon:

    - Valitse **Työkalut** -kuvaketta oikeassa yläkulmassa.
    - **Internet-asetukset**Valitse **Suojaus** -välilehti ja valitse sitten **Luotetut sivustot** -kuvaketta.
    - Valitse **sivustot** -painiketta. Lisää _https://\*. mongodb.org_ luetteloon luotettujen sivustojen ja sulje sitten valintaikkuna.

    ![Internet Explorerin tietosuoja-asetusten määrittäminen](./media/virtual-machines-windows-install-mongodb/configure-internet-explorer-security.png)

4. Siirry (http://www.mongodb.org/downloads) [MongoDB -](http://www.mongodb.org/downloads) lataussivulta.

5. Oletusarvon mukaan se on valittava **Yhteisöpalvelin** vuosi- ja nykyisen vakaata uusin Windows Server 2008 R2: n 64-bittinen ja uudempi versio. Lataa asennusohjelma napsauttamalla **Lataa (msi)**.

    ![Lataa MongoDB installer](./media/virtual-machines-windows-install-mongodb/download-mongodb.png)

    Suorittamalla asennusohjelma, kun lataus on valmis.

6. Lue ja hyväksy käyttöoikeussopimus. Kun sinua pyydetään, valitse **Valmis** Asenna.

7. Lopullinen näytössä Valitse **Asenna**.


## <a name="configure-the-vm-and-mongodb"></a>AM ja MongoDB määrittäminen

1. Polun muuttujat eivät päivity MongoDB asennusohjelma. Ilman, MongoDB `bin` kohtaan liikerata muuttuja, sinun on määritettävä koko polku aina, kun käytät MongoDB suoritettavat. Voit lisätä liikerata muuttuja sijainnin seuraavasti:

    - Napsauta hiiren kakkospainikkeella **Käynnistä** -valikko ja valitse **Järjestelmä**.
    - Valitse **järjestelmän Lisäasetukset**ja valitse sitten **Ympäristömuuttujat**.
    - Valitse **Järjestelmämuuttujat**Valitse **polku**ja valitse sitten **Muokkaa**.

    ![Määritä POLKU muuttujat](./media/virtual-machines-windows-install-mongodb/configure-path-variables.png)

    Lisää polku oman MongoDB `bin` kansio. MongoDB asennetaan yleensä `C:\Program Files\MongoDB`. Varmista, että AM asennuspolku. Seuraava esimerkki Lisää MongoDB sijainti, johon haluat asentaa oletusarvo `PATH` muuttujan:

    ```
    ;C:\Program Files\MongoDB\Server\3.2\bin
    ```

    > [AZURE.NOTE] Muista lisätä eteen puolipistettä (`;`) osoittamaan, että olet lisäämässä sijainti, että `PATH` muuttuja.

2. Luo MongoDB tiedot ja kirjaudu kansioiden tietojen levytilaa. Valitse **Käynnistä** -valikosta **komentokehote**. Seuraavissa esimerkeissä Luo hakemistojen asemaan f

    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```

3. Aloita MongoDB esiintymää seuraavalla komennolla, muuttamalla tietojen polku ja kirjaudu kansioiden vastaavasti:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```

    Voi kestää useita minuutteja MongoDB varata Päivyri-tiedostot ja kuuntele yhteydet. Log-viestit ohjataan *F:\MongoLogs\mongolog.log* tiedoston `mongod.exe` server käynnistyy ja varaa Päivyri tiedostot.

    > [AZURE.NOTE] Komentokehote pysyy aktiivisen tehtävän MongoDB esiintymää on käynnissä. Komentokehote-ikkunan jättäminen auki Jatka MongoDB suorittamista. Tai jos asennat MongoDB palveluna seuraavassa vaiheessa esitetyllä tavalla.

4. Asenna tehokkaamman MongoDB-toiminto `mongod.exe` palveluna. Palvelun luomiseen tarkoittaa sitä, sinun ei tarvitse jättää suorittaminen aina, kun haluat käyttää MongoDB komentorivi-ikkuna. Luo palvelun seuraavasti, muuttamalla tiedot ja kirjaudu kansioiden polku vastaavasti:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```

    Edeltävä komento luo nimeltä MongoDB, sekä kuvaukset siitä "Mongo DB-palvelu. Seuraavat parametrit on määritetty:

    - `--dbpath` Asetus määrittää datakansion sijainti.
    - `--logpath` Vaihtoehto on käytettävä Määritä lokitiedostoon, koska käytössä palvelu ei ole näyttämään tulos komentorivi-ikkuna.
    - `--logappend` Asetus määrittää palvelun uudelleenkäynnistys aiheuttaa tulosteen liittäminen aiemmin luotuun lokitiedostoon.

  Voit käynnistää MongoDB-palvelun suorittamalla seuraavan komennon:

    ```
    net start MongoDB
    ```

    Lisätietoja MongoDB palvelun luomisesta on artikkelissa [määrittäminen MongoDB Windows-palvelu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-the-mongodb-instance"></a>Testaa MongoDB-esiintymä

MongoDB on kuin yhden esiintymän tai palvelu on asennettu, avulla voit nyt aloittaa luomisesta ja käyttämisestä tietokantoja. Voit aloittaa MongoDB järjestelmänvalvojan käyttöliittymä, avaa toisen komentorivi-ikkuna **Käynnistä** -valikosta ja kirjoita seuraava komento:

```
mongo  
```

Voit lisätä tietokannoista `db` komento. Lisää tietoja seuraavasti:

```
db.foo.insert( { a : 1 } )
```

Etsiä tietoja seuraavasti:

```
db.foo.find()
```

Tulos on seuraavassa esimerkissä:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Lopeta `mongo` konsoli seuraavasti:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Palomuurin ja verkko-käyttöoikeusryhmän sääntöjen määrittäminen
Nyt kun MongoDB on asennettu ja käytössä, avata portin Windowsin palomuuri, jotta voit etäyhteyden muodostaa yhteyden MongoDB. Jos haluat luoda uuden saapuvan sääntö, joka sallii porttinumeroa 27017, Avaa järjestelmänvalvojan PowerShell-kehotteessa ja kirjoita seuraava komento:

```powerShell
New-NetFirewallRule -DisplayName "Allow MongoDB" -Direction Inbound `
    -Protocol TCP -LocalPort 27017 -Action Allow
```

Voit myös luoda säännön **Windowsin laajennettu palomuuri** graafinen hallinta-työkalun avulla. Luo uusi saapuvien sääntö, joka sallii porttinumeroa 27017.

Tarvittaessa luoda verkon käyttöoikeusryhmän säännön annettavien MongoDB aiemmin Azure virtual verkon aliverkon ulkopuolella. Voit luoda verkon käyttöoikeusryhmän säännöt [Azure portal](virtual-machines-windows-nsg-quickstart-portal.md) tai [PowerShellin Azure](virtual-machines-windows-nsg-quickstart-powershell.md)avulla. Windowsin palomuurin säännöt ja Salli porttinumeroa 27017 MongoDB AM VPN-käyttöliittymän.

> [AZURE.NOTE] Porttinumeroa 27017 on MongoDB käyttämä oletusportti. Voit muuttaa portin avulla `--port` parametrin käynnistettäessä `mongod.exe` manuaalisesti tai -palvelusta. Jos muutat portin, muista päivittää Windowsin palomuurin ja verkko-käyttöoikeusryhmän säännöt edellä kuvatut toimet.


## <a name="next-steps"></a>Seuraavat vaiheet
Tässä opetusohjelmassa opit, asentaa ja määrittää MongoDB Windows-AM. Nyt voit käyttää MongoDB Windows-AM, valitse seuraamalla Lisäasetukset aiheet [MongoDB ohjeissa](https://docs.mongodb.com/manual/).
