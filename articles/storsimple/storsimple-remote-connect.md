<properties 
   pageTitle="Etäkäyttö StorSimple-laitteeseen | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan etähallinnan laitteen määrittäminen ja StorSimple HTTP tai HTTPS Windows PowerShellin muodostaminen."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/21/2016"
   ms.author="alkohli" />

# <a name="connect-remotely-to-your-storsimple-device"></a>Etäkäyttö StorSimple-laitteeseen

## <a name="overview"></a>Yleiskatsaus

Windows PowerShellin Etäpalvelujen avulla voit muodostaa yhteyden StorSimple laitteeseen. Kun muodostat yhteyden tällä tavalla, valikko, ei näytetä. (Näkyy valikon vain, jos käytät serial konsolin laitteeseen muodostaa.) Etäpalvelujen Windows PowerShellin avulla voit muodostaa yhteyden tietyn runspace. Voit myös määrittää näyttökielen. 

Lisätietoja Windows PowerShell Etäpalvelujen avulla voit hallita laitetta Siirry [Windows PowerShellin käyttäminen StorSimple ammattimainen StorSimple laitteen](storsimple-windows-powershell-administration.md).

Tässä opetusohjelmassa kerrotaan etähallinnan laitteen määrittäminen ja yhteyden muodostaminen Windows PowerShellin StorSimple varten. Voit käyttää HTTP tai HTTPS Windows PowerShellin Etäpalvelujen välityksellä. Kuitenkin kun ovat päätät StorSimple Windows PowerShellin muodostaminen, toimi seuraavasti: 

- Laitteen serial konsolin liittämisestä on suojattu, mutta yhteyden muodostaminen verkon valitsimet serial konsoliin ei ole. Ole varovainen suojauksen, kun yhteyden muodostaminen laitteen serial konsoliin verkon valitsimia. 

- Yhteyden kautta HTTP-istunnon saattaa tarjota paremman suojauksen kuin yhteyden kautta serial konsolin verkon kautta. Vaikka tämä ei ole turvallisin tapa on hyväksyttävä luotettujen verkoissa. 

- Yhteyden kautta HTTPS-istunnon itse allekirjoitetun varmenteen kanssa on turvallisin ja suositeltavaa vaihtoehtoa.

Voit yhdistää etäyhteyden Windows PowerShell-käyttöliittymän. Kuitenkin etäkäyttöpalvelimen kautta Windows PowerShell-liittymän StorSimple laitteeseesi ei ole käytössä oletusarvoisesti. Sinun on otettava käyttöön etähallinnan laitteeseen ensin ja sitten asiakkaan, käytetään laitteen käyttämiseen.

Tässä artikkelissa kuvatut vaiheet on suoritettu host järjestelmässä käytössä Windows Server 2012 R2.

## <a name="connect-through-http"></a>HTTP-yhteyden

Yhteyden Windows PowerShellin StorSimple HTTP-istunnon kautta tarjoaa paremman suojauksen kuin yhteyden StorSimple laitteen sarja-konsolin kautta. Vaikka tämä ei ole turvallisin tapa on hyväksyttävä luotettujen verkoissa.

Voit käyttää Azure perinteinen portaalin tai serial konsolin etähallinnan määrittämiseen. Valitse jokin seuraavista toimenpiteistä:

- [Azure perinteinen portaalin avulla voit ottaa käyttöön etähallinnan HTTP-yhteyden kautta](#use-the-azure-classic-portal-to-enable-remote-management-over-http)

- [Sarja konsolin avulla voit ottaa käyttöön etähallinnan HTTP-yhteyden kautta](#use-the-serial-console-to-enable-remote-management-over-http)

Kun otat etähallinnan, asiakkaan valmisteleminen etäyhteyden seuraavien ohjeiden avulla.

- [Asiakkaan valmisteleminen etäyhteyden](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-http"></a>Azure perinteinen portaalin avulla voit ottaa käyttöön etähallinnan HTTP-yhteyden kautta 

Seuraavien toimien käyttöön etähallinnan HTTP Azure perinteinen-portaalissa.

#### <a name="to-enable-remote-management-through-the-azure-classic-portal"></a>Jos haluat ottaa käyttöön etähallinnan palvelun Azure perinteinen-portaalissa

1. Käyttää **laitteet** > **määrittäminen** laitteeseesi.

2. Vieritä **Etähallinnan** -osaan.

3. Aseta arvoksi **Kyllä** **etähallinnan käyttöön** .

4. Voit nyt yhteys http:n. (Oletus on muodostaa HTTPS). Varmista, että HTTP-vaihtoehto on valittuna.

    >[AZURE.NOTE] Yhteyden muodostaminen HTTP on hyväksyttävä vain luotettavien verkoissa.

6. Valitse sivun alareunassa **Tallenna** .

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>Sarja konsolin avulla voit ottaa käyttöön etähallinnan HTTP-yhteyden kautta

Seuraavien toimien laitteen serial konsolissa etähallinnan käyttöön.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Jos haluat ottaa käyttöön etähallinnan laitteen sarja-konsolin kautta

1. Valitse sarja konsoli-valikon vaihtoehto 1. Lisätietoja sarja-konsolin käyttämisestä laitteeseen Siirry [yhteyden muodostaminen Windows PowerShell StorSimple laitteen serial konsolin kautta](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. Kirjoita komentoriville seuraava:`Enable-HcsRemoteManagement –AllowHttp`

3. Saat tiedon tietoja tietoturva-aukkoja HTTP avulla voit muodostaa yhteyden laitteeseen. Kun sinulta kysytään, Vahvista kirjoittamalla **Y**.

4. Varmista, että HTTP on käytössä kirjoittamalla:`Get-HcsSystem`

5. Varmista, että **RemoteManagementMode** -kentässä on **HttpsAndHttpEnabled**. Seuraavassa kuvassa näkyy painovärit, muste nämä asetukset.

     ![Sarja HTTPS- ja käytössä HTTP](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>Asiakkaan valmisteleminen etäyhteyden

Seuraavien toimien asiakkaan etähallinnan käyttöön.

#### <a name="to-prepare-the-client-for-remote-connection"></a>Asiakkaan valmisteleminen etäyhteyden

1. Windows PowerShell-istunnon aloittaminen järjestelmänvalvojana.

2. Kirjoita seuraava komento StorSimple laitteen IP-osoitteen lisääminen asiakkaan luotettujen isännät-luetteloon: 

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

     Korvaa <*device_ip*> laitteesi; IP-osoite Esimerkki: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Kirjoita Tallenna laitteen tunnistetiedot muuttujaan seuraava komento: 

     *$cred = get-tunnistetiedot*

4. Valintaikkunassa näkyvä

    1. Kirjoita käyttäjänimi tässä muodossa: *device_ip\SSAdmin*.
    2. Kirjoita laitteen järjestelmänvalvoja salasanaa, jolla on määritetty, kun laite on määritetty ohjatun määritystoiminnon. Oletus-salasana on *Salasana1*.

7. Windows PowerShell-istunnon aloittaminen laitteeseen kirjoittamalla seuraava komento:

     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`

     >[AZURE.NOTE] Luodaksesi Windows PowerShell-istunnon käytettäväksi StorSimple virtual laitteeseen, liittäminen `–Port` parametri ja määritä Julkinen portti, jonka määritit Etäpalvelujen StorSimple Virtual laitteen.

     Tässä vaiheessa on oltava aktiivinen Windows PowerShell-etäistunnon laitteeseen.

    ![PowerShellin Etäpalvelujen http:n](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>Muodosta yhteys HTTPS kautta

Yhteyden Windows PowerShellin StorSimple HTTPS-istunnon kautta on eniten suojatun ja suositellut tapa etäyhteyden muodostaminen Microsoft Azure StorSimple laitteen. Seuraavien kerrotaan, miten serial konsolin ja asiakkaan tietokoneiden määrittäminen niin, että voit muodostaa Windows PowerShellin StorSimple HTTPS.

Voit käyttää Azure perinteinen portaalin tai serial konsolin etähallinnan määrittämiseen. Valitse jokin seuraavista toimenpiteistä:

- [Azure perinteinen portaalin avulla voit ottaa käyttöön etähallinnan https-VÄLITYSPALVELIMEN kautta](#use-the-azure-classic-portal-to-enable-remote-management-over-https)

- [Sarja konsolin avulla voit ottaa käyttöön etähallinnan https-VÄLITYSPALVELIMEN kautta](#use-the-serial-console-to-enable-remote-management-over-https)

Kun otat etähallinnan, avulla isäntä valmisteleminen etähallinnan merkitseminen ja muodosta yhteys laitteen siirretään seuraavasti.

- [Isännän valmisteleminen etähallinta](#prepare-the-host-for-remote-management)

- [Yhteyden muodostaminen laitteen siirretään](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-https"></a>Azure perinteinen portaalin avulla voit ottaa käyttöön etähallinnan https-VÄLITYSPALVELIMEN kautta

Seuraavien toimien Azure perinteinen portaalissa käyttöön etähallinnan HTTPS-protokollan välityksellä.

#### <a name="to-enable-remote-management-over-https-from-the-azure-classic-portal"></a>Haluat ottaa käyttöön perinteinen Azure-portaalista HTTPS etähallinta

1. Käyttää **laitteet** > **määrittäminen** laitteeseesi.

2. Vieritä **Etähallinnan** -osaan.

3. Aseta arvoksi **Kyllä** **etähallinnan käyttöön** .

4. Voit nyt muodostaa yhteyden HTTPS. (Oletus on muodostaa HTTPS). Varmista, että HTTPS on valittuna. 

5. Valitse **Lataa etähallinnan**. Määritä tiedoston tallennussijainti. Tarvitset tätä varmenteen asentaminen asiakas- tai host tietokone, jota käytetään yhteyden muodostamiseen laitteen.

6. Valitse sivun alareunassa **Tallenna** .

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>Sarja konsolin avulla voit ottaa käyttöön etähallinnan https-VÄLITYSPALVELIMEN kautta

Seuraavien toimien laitteen serial konsolissa etähallinnan käyttöön.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Jos haluat ottaa käyttöön etähallinnan laitteen sarja-konsolin kautta

1. Valitse sarja konsoli-valikon vaihtoehto 1. Lisätietoja sarja-konsolin käyttämisestä laitteeseen Siirry [yhteyden muodostaminen Windows PowerShell StorSimple laitteen serial konsolin kautta](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. Kirjoita komentoriville seuraava: 

     `Enable-HcsRemoteManagement`

    Tämä on otettava käyttöön HTTPS laitteessasi.

3. Varmista, että HTTPS käytössä kirjoittamalla: 

     `Get-HcsSystem`

    Varmista, että **RemoteManagementMode** -kentässä on **HttpsEnabled**. Seuraavassa kuvassa näkyy painovärit, muste nämä asetukset.

     ![Sarja HTTPS käytössä](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)

4. Tulosteesta `Get-HcsSystem`kopioimalla laitteen järjestysnumeron ja tallentaa sen myöhempää käyttöä varten.

    >[AZURE.NOTE] Järjestysluvun yhdistää varmenteen CN-nimi.

5. Etähallinnan varmenteen hankkiminen kirjoittamalla: 
 
     `Get-HcsRemoteManagementCert`

    Varmenteen seuraavankaltaiselta tulee näkyviin.

    ![Hae etähallinnan sertifikaatti](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)

5. Kopioi tiedot varmenteen **---Aloita VARMENTEEN---** **---** Lopeta VARMENTEEN---tekstieditorissa esimerkiksi Muistiossa ja tallenna se .cer-tiedostona. (Kopioidaan tämän tiedoston remote isäntään kun valmistelet isäntä.)

    >[AZURE.NOTE] Voit luoda uutta varmennetta `Set-HcsRemoteManagementCert` cmdlet-komento.

### <a name="prepare-the-host-for-remote-management"></a>Isännän valmisteleminen etähallinta

Valmistautuminen etäyhteyden, joka käyttää HTTPS-istunnon isäntätietokoneen tekemällä seuraavat toimet:

- [Tuo päämyöntäjien säilöön asiakkaan tai etäpalvelin .cer-tiedosto](#to-import-the-certificate-on-the-remote-host).

- [Lisää laitteen sarjanumerot remote isännän isännät-tiedostoon](#to-add-device-serial-numbers-to-the-remote-host).

Kaikki seuraavat toimenpiteet on kuvattu alla.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Siirretään varmenteen tuominen

1. .Cer-tiedostoa hiiren kakkospainikkeella ja valitse **Asenna varmenne**. Tämä käynnistää ohjatun tuontitoiminnon varmenne.

    ![Ohjattu sertifikaatin tuominen 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)

2. **Säilön sijainti**Valitse **Paikallinen kone**ja valitse sitten **Seuraava**.

3. Valitse **Sijoita kaikki varmenteet seuraavaan säilöön**, ja valitse sitten **Selaa**. Siirry remote isännän pääkansio-kaupan ja valitse sitten **Seuraava**.

    ![Ohjattu sertifikaatin tuominen 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)

4. Valitse **Valmis**. Näyttöön tulee sanoma, joka ilmoittaa, että tuonti onnistui.

    ![Ohjattu sertifikaatin tuominen 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Voit lisätä laitteen sarjanumerot siirretään

1. Muistion käynnistäminen järjestelmänvalvojana ja avaa sitten osoitteessa \Windows\System32\Drivers\etc isännät-tiedosto.

2. Lisää seuraavat kolme merkinnät isännät-tiedostoon: **tietojen 0 IP-osoite**, **Controller 0 kiinteä IP-osoite**ja **ohjauskoneen 1 kiinteä IP-osoite**.

3. Anna aiemmin tallentamasi laitteen sarjanumero. Yhdistä tämä IP-osoite, seuraavassa kuvassa esitetyllä tavalla. Controller 0 ja 1 ohjauskoneen liittää **Controller0** ja **Controller1** järjestysnumeron (CN nimi) lopussa.

    ![CN nimen lisääminen isännät-tiedostoon](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)

4. Tallenna isännät-tiedosto.

### <a name="connect-to-the-device-from-the-remote-host"></a>Yhteyden muodostaminen laitteen siirretään

Kirjoita SSAdmin-istunnon laitteen etäpalvelin tai asiakkaan Windows PowerShellin ja SSL. Vaihtoehto 1 [serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console) -valikossa laitteesi yhdistää SSAdmin istunto.

Toimi seuraavasti tietokoneessa, josta haluat tehdä Windows PowerShell-etäyhteyden.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Voit kirjoittaa SSAdmin-istunnon laitteen käyttämällä Windows PowerShellin ja SSL

1. Windows PowerShell-istunnon aloittaminen järjestelmänvalvojana.

2. Lisää laitteen IP-osoite asiakkaan luotettujen isännät kirjoittamalla:

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

    Missä <*device_ip*> laitteesi; IP-osoite Esimerkki: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Luo uusi tunnistetiedon kirjoittamalla: 

     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`

    Missä <*IP kohde laitteen*> laitteeseesi; 0 tietojen IP-osoite esimerkiksi **10.126.173.90** isännät-tiedoston edellisessä kuvassa esitetyllä tavalla. Lisäksi antaa järjestelmänvalvojan salasana laitteeseesi.

4. Luo istunnon kirjoittamalla:

     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`

    Cmdlet - tietokoneen nimi-parametrin Anna <*järjestysnumeron kohdeosoitteen laitteen*: n >. Tämä sarjanumero on nyt yhdistetty tietojen isännät-tiedostoon tekemäsi etäpalvelin; 0 IP-osoite esimerkiksi **SHX0991003G44MT** seuraavassa kuvassa esitetyllä tavalla.

5. Tyyppi: 

     `Enter-PSSession $session`

6. Sinun on Odota muutama minuutti ja sitten yhteys laitteeseen https SSL-yhteyden kautta. Näyttöön tulee sanoma, joka ilmaisee, että olet muodostanut yhteyden laitteeseen.

    ![PowerShellin Etäpalvelujen HTTPS ja SSL käyttäminen](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [Windows PowerShellin StorSimple laitteen hallintaan](storsimple-windows-powershell-administration.md).

- Lisätietoja [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun avulla](storsimple-manager-service-administration.md).
