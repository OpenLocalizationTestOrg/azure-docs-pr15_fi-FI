<properties
    pageTitle="Luo AM, jossa MySQL | Microsoft Azure"
    description="Luo Azure virtual-tietokoneessa, jossa Windows Server 2012 R2 ja perinteinen käyttöönoton mallin MySQL-tietokantaan."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="cynthn"/>


# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2012-r2"></a>Asenna MySQL käytössä Windows Server 2012 R2 perinteinen käyttöönoton mallin avulla luotu virtual tietokoneeseen

[MySQL](http://www.mysql.com) on Suositut Avaa lähde, SQL-tietokantaan. Tässä opetusohjelmassa näytetään, miten asentaminen ja suorittaminen MySQL 5.6.23 yhteisön version MySQL-palvelin käytössä Windows Server 2012 R2 virtual koneeseen. Ohjeita asennuksen MySQL Linux viitata: [MySQL-Azure asentaminen](virtual-machines-linux-mysql-install.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="create-a-virtual-machine-running-windows-server-2012-r2"></a>Luo virtual koneeseen, jossa käytetään Windows Server 2012 R2

Jos sinulla ei vielä ole AM, joka on käynnissä Windows Server 2012 R2, voit luoda virtuaalikoneen tässä [opetusohjelmassa](virtual-machines-windows-classic-tutorial.md) . 

## <a name="attach-a-data-disk"></a>Liitä tiedot DVD-levyllä

Virtuaalikoneen luomisen jälkeen voit halutessasi liittää lisätietojen-levyn. Tämä on suositeltavaa tuotannon toiminnoista ja välttää OS aseman (c), joka sisältää käyttöjärjestelmän tallennustilan loppumisesta.

Katso, [miten voit liittää tiedot DVD-levyllä Windows virtual tietokoneeseen](virtual-machines-windows-classic-attach-disk.md) ja noudata liittäminen tyhjä levy. Määritä host välimuisti-asetukseksi **ei mitään** tai **vain luku-tilassa**.

## <a name="log-on-to-the-virtual-machine"></a>Kirjaudu sisään virtuaalikoneen

Seuraavaksi saat [virtuaalikoneen kirjautua](virtual-machines-windows-classic-connect-logon.md) MySQL asennusta.

##<a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Asentaminen ja suorittaminen virtuaalikoneen yhteisön MySQL-palvelin

Asentaminen, määrittäminen ja suorittaa MySQL-palvelin yhteisön-version seuraavasti:

> [AZURE.NOTE] Nämä ohjeet koskevat 5.6.23.0 yhteisön MySQL- ja Windows Server 2012 R2-version. Käyttäjäkokemuksen voi olla erilainen eri versioiden MySQL- tai Windows Server.

1.  Kun olet muodostanut yhteyden etätyöpöydän virtuaalikoneen, valitse **Internet Explorerin** aloitusnäytön.
2.  Valitse **Työkalut** -painiketta oikeassa yläkulmassa (cogged kiekkopainiketta kuvake) ja valitse sitten **Internet-asetukset**. Valitse **Suojaus** -välilehti, napsauta **Luotetut sivustot** -kuvaketta ja valitse sitten **sivustot** . Lisää http://*. mysql.com luotettujen sivustojen luetteloon. Valitse * *Sulje**, ja valitse sitten * *OK**.
3.  Valitse-osoite Internet Explorerin osoiteriville, kirjoita http://dev.mysql.com/downloads/mysql/.
4.  MySQL-sivuston avulla voit etsiä ja lataa uusin versio Windows MySQL-asennusohjelma. MySQL-asennusohjelma valittaessa Lataa versio, joka on valmis tiedoston määrittäminen (esimerkiksi mysql-installer-yhteisön-5.6.23.0.msi tiedoston koko on 282.4 Mt) ja Tallenna asennusohjelma.
5.  Kun asennusohjelma on ladannut, valitse **Suorita** käynnistää asetukset.
6.  **Käyttöoikeussopimus** -sivulla Hyväksy käyttöoikeussopimus ja valitse **Seuraava**.
7.  **Valitse asennustyyppi** -sivulla haluamasi asetukset tyyppi ja valitse sitten **Seuraava**. Seuraavissa vaiheissa oletetaan **vain palvelin** asennustyyppi valintaa.
8.  Valitse **Suorita** **asennus** -sivulla. Kun asennus on valmis, valitse **Seuraava**.
9.  **Tuotteen määritys** -sivulla valitsemalla **Seuraava**.
10. Määritä uudet määritykset tyyppi ja yhteyden asetusten, kuten TCP-portin tarvittaessa **tyyppi ja verkko** -sivulla. Valitse **Näytä lisäasetukset**ja valitse sitten **Seuraava**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_TypeNetworking.png)

11. Valitse **tilit ja roolit** -sivulla Määritä MySQL pääkansion vahva salasana. Lisää MySQL-käyttäjätilejä tarpeen mukaan ja valitse sitten **Seuraava**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AccountsRoles_Filled.png)

12. **Windows-palvelun** sivulla käynnissä MySQL-palvelin Windows-palvelun tarvittaessa muutoksia oletusasetusten määrittäminen ja valitse sitten **Seuraava**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_WindowsService.png)

13. Valitse **Lisäasetukset** -sivulla Määritä kirjaamisasetusten muutokset tarpeen mukaan ja valitse sitten **Seuraava**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AdvOptions.png)

14. **Käytä palvelimen määritys** -sivulla valitsemalla **Suorita**. Kun määritykset on tehty, valitse **Valmis**.
15. **Tuotteen määritys** -sivulla valitsemalla **Seuraava**.
16. Valitse **Asennus on valmis** -sivu, **Kopioi Leikepöydälle loki** , jos haluat tarkastella myöhemmin ja valitse sitten **Valmis**.
17. Aloitusnäytön Kirjoita **mysql**ja valitse sitten **MySQL 5.6 komentoriviltä asiakas**.
18. Pääkansio salasana, jonka olet määrittänyt vaiheessa 11 ja sinun esitettävä tulee kehote kohtaa, johon voit myöntää määrittäminen MySQL-komentojen. Komentoja ja syntaksia, Lisätietoja on artikkelissa [MySQL viittaus oppaat](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_CommandPrompt.png)

19. Voit myös määrittää server configuration oletusarvon asetuksia, kuten perus- ja kansiot ja asemat, jossa C:\Program Files (x86) \MySQL\MySQL palvelimen 5.6\my-default.ini-tiedostossa. Lisätietoja on artikkelissa [5.1.2 Server Configuration oletukset](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Määritä päätepisteet

Jos MySQL-palvelin-palvelua voi käyttää MySQL-asiakastietokoneiden Internetissä, päätepisteen joina MySQL-palvelin-palvelu Kuuntele TCP-portin määrittäminen ja luoda lisäsäännön Windowsin palomuuri. Tämä on porttinumeroa 3306, paitsi jos olet määrittänyt eri portin **tyyppi ja verkko** -sivulla (vaihe 10 edellisen toimenpiteen).


> [AZURE.NOTE] Ota huomioon huolellisesti tietoturvaan tekoa, koska tällöin MySQL-palvelin-palvelun käytettävissä kaikissa tietokoneissa Internetissä. Voit määrittää lähteen IP-osoitteet, jotka voivat käyttää päätepisteen kanssa-luettelon Käyttöoikeusluettelon (Access Control) määrittäminen. Katso lisätietoja, [miten voit määrittää määrittäminen päätepisteet Virtual Machine](virtual-machines-windows-classic-setup-endpoints.md).


Voit määrittää päätepisteen MySQL-palvelin-palvelua varten seuraavasti:

1.  Azure perinteinen portaalissa **näennäiskoneiden**ja MySQL-virtuaalikoneen nimeä **päätepisteet**.
2.  Painikkeita Valitse **Lisää**.
3.  Valitse **Lisää päätepisteen virtual machine** -sivulla olevaa oikealle osoittavaa nuolta.
4.  Jos käytössäsi on oletusarvo, 3306 MySQL-porttinumeroa, valitse **MySQL** **nimi**ja valitse sitten valintamerkkiä.
5.  Jos käytössäsi on eri porttinumeroa, Kirjoita yksilöllinen nimi **nimi**. Valitse **TCP** -protokollan, Kirjoita porttinumero **Julkinen portti** - ja **Yksityiset portti**ja valitse sitten valintamerkkiä.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>Lisää Windowsin palomuuri sääntö, joka sallii MySQL-liikenne

Lisää Windowsin palomuuri-sääntö, joka sallii MySQL-liikenne Internetistä suorittamalla seuraavan komennon Windows PowerShellin oikeuksin suoritettava komentokehote, valitse MySQL-palvelin virtuaalikoneen.

    New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public


    
## <a name="test-your-remote-connection"></a>Testaa yhteys remote


Testaa yhteys remote Azure virtuaalikoneen käytössä MySQL-palvelin-palveluun, sinun on määritettävä vastaavien pilvipalveluun, jossa on käynnissä MySQL-palvelin virtuaalikoneen DNS-nimi.

1.  Azure perinteinen portaalissa **näennäiskoneiden**ja MySQL-palvelin virtuaalikoneen nimeä **raporttinäkymät-ikkunan**.
2.  Virtuaalikoneen-koontinäytössä Huomautus **Quick Glance** -kohdassa **DNS-nimi** -arvoa. Tässä on esimerkki:

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_DNSName.png)

3.  MySQL-tai MySQL-asiakasohjelman paikallisesta tietokoneesta Suorita seuraava komento kirjautua sisään MySQL-käyttäjänä.

        mysql -u <yourMysqlUsername> -p -h <yourDNSname>

    MySQL käyttäjän nimi dbadmin3 ja virtuaalikoneen testmysql.cloudapp.net DNS-nimi, käytä seuraavaa komentoa.

        mysql -u dbadmin3 -p -h testmysql.cloudapp.net


## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja käytössä MySQL on [MySQL-ohjeista](http://dev.mysql.com/doc/).
