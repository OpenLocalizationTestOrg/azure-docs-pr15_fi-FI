<properties 
   pageTitle="Luo itse allekirjoitetun varmenteet, pisteen sivuston virtual verkon rajat paikallisen yhteydet käyttämällä makecert | Microsoft Azure"
   description="Tämä artikkeli sisältää ohjeet, joiden avulla voit luoda itse allekirjoitettua varmenteet Windows 10: ssä makecert."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# <a name="working-with-self-signed-certificates-for-point-to-site-connections"></a>Itse allekirjoitetun varmenteet pisteen sivuston yhteyksien käyttäminen

Tämän artikkelin avulla voit luoda itse allekirjoitettua varmennetta käyttämällä **makecert**ja luo asiakkaan varmenteet siitä. Ohjeita kirjoitetaan for makecert Windows 10: ssä. Makecert on vahvistettu sertifikaatteja, joita ovat yhteensopivia P2S yhteyksien luominen. 

P2S yhteyksien varmenteiden ensisijainen tapa on käyttää yrityksen varmenteen ratkaisu, varmistetaan, että yleisiä arvo nimimuotoa varmenteet myöntää asiakkaan 'name@yourdomain.com', sen sijaan, että "NetBIOS toimialueen nimi\käyttäjänimi"-muodossa.

Jos sinulla ei ole enterprise-ratkaisun, itse allekirjoitettua varmennetta käytetään P2S asiakkaat virtual verkkoon. Olemme voidaan ottaa huomioon, että makecert on vähennetty, mutta se on edelleen voimassa menetelmä itse allekirjoitettua sertifikaatteja, joita ovat yhteensopivia P2S yhteyksien luominen. Pyrimme parhaillaan korjaamaan luomisen itse allekirjoitettua varmenteet ratkaisu, mutta tällä hetkellä makecert on suositeltavampaa.

## <a name="create-a-self-signed-certificate"></a>Itse allekirjoitetun varmenteen luominen

Makecert on yksi tapa itse allekirjoitetun varmenteen luominen. Seuraavat vaiheet opastusta käyttämällä makecert itse allekirjoitetun varmenteen luominen. Nämä vaiheet eivät ole käyttöönoton mallin tietyt. He ovat voimassa Resurssienhallinta ja perinteinen.

### <a name="to-create-a-self-signed-certificate"></a>Itse allekirjoitetun varmenteen luominen

1. Lataa ja asenna [Windows Software Development Kit (SDK) Windows 10: lle](https://dev.windows.com/en-us/downloads/windows-10-sdk)Windows 10-tietokoneessa.

2. Asennuksen jälkeen, voit etsiä tässä polussa makecert.exe-apuohjelma: C:\Program Files (x86) \Windows Kits\10\bin\<kaari >. 
        
    Esimerkki:`C:\Program Files (x86)\Windows Kits\10\bin\x64`

3. Luo seuraavaksi ja asentaa sertifikaatin tietokoneeseen lukulaitteeseen. Seuraavassa esimerkissä luodaan vastaavan *.cer* -tiedosto, voit ladata Azure P2S määritettäessä. Suorita seuraava komento järjestelmänvalvojana. Korvaa *ARMP2SRootCert* ja *ARMP2SRootCert.cer* nimi, jota haluat käyttää varmenteen.<br><br>Varmenteen sijaitsevat Varmenteet - Nykyinen User\Personal\Certificates.

        makecert -sky exchange -r -n "CN=ARMP2SRootCert" -pe -a sha1 -len 2048 -ss My "ARMP2SRootCert.cer"


###  <a name="rootpublickey"></a>Voit hankkia julkinen avain

Julkinen avain varmenteen päämyöntäjien tuodaan osana pisteen sivuston yhteydet VPN-yhdyskäytävän määrityskohde Azure.

1. Avaa **certmgr.msc**hankkiminen varmenteen .cer-tiedosto. Kaksoisnapsauta pääkansion itse allekirjoitettua varmennetta, **kaikki**tehtävät ja valitse sitten **Vie**. **Varmenteen vieminen-toiminnossa**avautuu.

2. Ohjatun Valitse **Seuraava**, valitse **ei, älä vie yksityinen avain**ja valitse sitten **Seuraava**.

3. Valitse **Viennin tiedostomuoto** -sivulla Valitse **Base-64-koodattu X.509 (. CER).** Valitse sitten **Seuraava**. 

4. Napsauta **Tiedoston vieminen** **Selaa** haluamaasi kohtaan, johon haluat viedä varmenne. **Tiedostonimi**-sertifikaatin tiedoston nimeksi. Valitse sitten **Seuraava**.

5. Valitse varmenteen vieminen **loppuun** .

 
### <a name="export-the-self-signed-certificate-optional"></a>Voit viedä itse allekirjoitetun varmenteen (valinnainen)

Haluat ehkä viedä itse allekirjoitetun varmenteen ja tallentaa sen turvallisesti. Jos tarvitse, voit myöhemmin asentaa ohjelman toiseen tietokoneeseen ja luo Lisää asiakasohjelman varmenteet tai vieminen toiseen .cer-tiedosto. Tietokoneeseen asennettu Asiakasvarmenne ja joka on määritetty myös ERISNIMI VPN-asiakasohjelman asetuksia voit muodostaa yhteyden virtual verkon kautta P2S. Tästä syystä haluat Varmista, että asiakas varmenteet luodaan ja asentaa vain tarvittaessa ja itse allekirjoitetun varmenteen tallennetaan turvallisesti.

Voit viedä itse allekirjoitetun varmenteen .pfx-pääkansio-sertifikaatti ja käyttää samoja vaiheita kuvatulla tavalla [Vie Asiakasvarmenne](#clientkey) Vie.

## <a name="create-and-install-client-certificates"></a>Luo ja asenna asiakkaan varmenteet

Älä asenna itse allekirjoitetun varmenteen suoraan asiakastietokoneen. Sinun täytyy luoda Asiakasvarmenne itse allekirjoitettua varmennetta. Voit sitten Vie ja asentaa Asiakasvarmenne asiakastietokoneeseen. Seuraavat vaiheet eivät ole käyttöönoton mallin tietyt. He ovat voimassa Resurssienhallinta ja perinteinen.

### <a name="part-1---generate-a-client-certificate-from-a-self-signed-certificate"></a>Osa 1 – Asiakasvarmenne itse allekirjoitetun varmenteen luominen

Seuraavat vaiheet käy läpi yksi tapa luoda Asiakasvarmenne itse allekirjoitettua varmennetta. Voi luoda useita asiakkaan sertifikaatteja samaa varmennetta. Kunkin Asiakasvarmenne voidaan viedä ja asennettu asiakastietokoneeseen. 

1. Avaa komentokehote samaan tietokoneeseen, jota käytit itse allekirjoitetun varmenteen luominen ja järjestelmänvalvojana.

2. Tässä esimerkissä "ARMP2SRootCert" viittaa, jotka olet luonut itse allekirjoitettua varmennetta. 
    - Muuta *"ARMP2SRootCert"* itse allekirjoitettua juuri, jotka luot Asiakasvarmenne-nimi. 
    - Muuta nimi, jonka haluat luoda Asiakasvarmenne on *ClientCertificateName* . 


    Muokkaa ja Suorita Luo asiakkaan varmenne otosten. Jos suoritat seuraavan esimerkin muuttamatta sen, tulos on asiakkaan nimi, joka on luotu varmenteen päämyöntäjien ARMP2SRootCert henkilökohtaisen varmenteen säilöstä ClientCertificateName varmenne.

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "ARMP2SRootCert" -is my -a sha1

4. Kaikki varmenteet on tallennettu oman 'Varmenteet - Nykyinen User\Personal\Certificates' tallentaa tietokoneeseen. Voit luoda, kun monta asiakkaan varmenteet tarvittaessa perustuvat seuraavasti.

### <a name="clientkey"></a>Osa 2 - asiakasohjelman varmenteen vieminen

1. Voit viedä Asiakasvarmenne, Avaa **certmgr.msc**. Napsauta hiiren kakkospainikkeella Asiakasvarmenne, jonka haluat viedä, valitse **Kaikki tehtävät**ja valitse sitten **Vie**. **Varmenteen vieminen-toiminnossa**avautuu.

2. Ohjatun Valitse **Seuraava**, sitten Valitse **Kyllä, vie yksityinen avain**ja valitse sitten **Seuraava**.

3. Valitse **Viennin tiedostomuoto** -sivulla voit jättää valittuna oletusarvoisesti. Valitse sitten **Seuraava**. 
 
4. Valitse **Suojaus** -sivulla voit suojata yksityinen avain. Jos valitset Käytä salasanaa, varmista, että tallentaa tai muista salasana, jonka olet määrittänyt tämän todistuksen. Valitse sitten **Seuraava**.

5. Napsauta **Tiedoston vieminen** **Selaa** haluamaasi kohtaan, johon haluat viedä varmenne. **Tiedostonimi**-sertifikaatin tiedoston nimeksi. Valitse sitten **Seuraava**.

6. Valitse varmenteen vieminen **loppuun** .  

### <a name="part-3---install-a-client-certificate"></a>Osa 3 - asennuksen Asiakasvarmenne

Kukin asiakas, johon haluat muodostaa yhteyden virtual verkon pisteen sivuston yhteyden avulla on oltava asennettuna asiakkaan varmenne. Tämä varmenne on lisäksi edellyttää VPN-määritys package. Seuraavat vaiheet opastusta asentaminen sertifikaattia manuaalisesti.

1. Paikanna ja kopioi asiakastietokoneeseen *.pfx* -tiedosto. Asiakastietokoneen Kaksoisnapsauta asentaminen *.pfx* -tiedosto. Jätä **Tallennuspaikan** kuin **Nykyinen käyttäjä**ja valitse sitten **Seuraava**.

2. **Tiedosto** Tuo sivun Älä tee muutoksia. Valitse **Seuraava**.

3. **Yksityisen avaimen suojaus** -sivulla Lisää sertifikaatin salasanan, jos käytetään, tai Tarkista suojaus lyhennys, asentaa varmenne ja valitse sitten **Seuraava**.

4. **Certificate Store** -sivulla jätä oletussijainti ja valitse sitten **Seuraava**.

5. Valitse **Valmis**. Valitse **Suojausvaroitus** sertifikaatin asennus Valitse **Kyllä**. Varmenne on nyt tuotu onnistuneesti.

## <a name="next-steps"></a>Seuraavat vaiheet

Jatka pisteen sivuston kokoonpano. 

- **Resurssien hallinta** käyttöönoton mallin ohjeita on kohdassa [Määritä pisteen sivuston yhteyden VNet, PowerShellin avulla](vpn-gateway-howto-point-to-site-rm-ps.md). 
- **Perinteinen** käyttöönoton mallin ohjeita on kohdassa [Määritä kohdassa sivuston VPN-yhteyden VNet, perinteinen-portaalissa](vpn-gateway-point-to-site-create.md).