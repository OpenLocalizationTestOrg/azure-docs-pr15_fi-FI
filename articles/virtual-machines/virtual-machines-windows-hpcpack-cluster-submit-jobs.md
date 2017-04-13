<properties
 pageTitle="Lähetä työt HPC Pack klusterin Azure | Microsoft Azure"
 description="Opi määrittämään paikalliseen tietokoneeseen, voit lähettää työt HPC Pack-klusterin Azure-tietokannassa"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>Lähettää HPC työt paikallisen tietokoneen käyttöön Azure HPC Pack-klusterin

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Määrittää paikallisen asiakastietokone lähettää työt [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) -klusterin Azure-tietokannassa. Tämän artikkelin avulla voit määrittää paikallisen tietokoneen kanssa asiakastyökalut esittämään työn HTTPS-protokollan välityksellä klusterin Azure-tietokannassa. Tällä tavalla useita klusterin käyttäjät voivat lähettää työt pilvipohjainen HPC Pack-klusterin, mutta jotka eivät muodostamisesta pään solmu AM tai Azure tilauksen käyttäminen.

![Lähetät työn klusteriin Azure-tietokannassa][jobsubmit]

## <a name="prerequisites"></a>Edellytykset

* **Azure-AM otettu käyttöön HPC Pack pään solmu** - Suosittelemme, että käytät automaattisia työkaluja, kuten käyttöön [Azure pikaopas mallia](https://azure.microsoft.com/documentation/templates/) tai [Azure PowerShell-komentosarjaa](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) pään solmu ja klusterin käyttöönotto. Tarvitset pään solmun DNS-nimen ja klusterin järjestelmänvalvoja, jotta voit suorittaa tämän artikkelin vaiheet tunnistetiedot.

* **Asiakastietokone** - tarvitset Windows- tai Windows Server asiakastietokoneen, joka toimii HPC Pack asiakkaan apuohjelmien (katso [järjestelmävaatimukset](https://technet.microsoft.com/library/dn535781.aspx)). Jos haluat vain lähettää työt HPC Pack web-portaaliin ja REST API avulla, voit käyttää mitä tahansa valittua asiakastietokone.

* **HPC Pack asennustietoväline** - ja asenna HPC Pack asiakkaan apuohjelmat HPC Pack (HPC Pack 2012 R2) uusimman version vapaa asennuspaketti on käytettävissä [Microsoft Download Centeristä](http://go.microsoft.com/fwlink/?LinkId=328024). Varmista, että lataat HPC-paketti, johon on asennettu pään solmu AM samaa versiota.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Vaihe 1: Asenna ja määritä pään solmun verkko-osat

Salli REST-käyttöliittymän, voit lähettää työt klusterin HTTPS-protokollan välityksellä, varmistat, että HPC Pack verkko-osat on määritetty HPC Pack pään solmun. Jos ne eivät ole jo asennettu, asentaa verkko-osien suorittamalla asennustiedosto HpcWebComponents.msi. Määritä sitten osat suorittamalla HPC PowerShell-komentosarjaa **Määrittäminen HPCWebComponents.ps1**.

Saat yksityiskohtaiset ohjeet, [Asenna Microsoft HPC Pack Web-osat](http://technet.microsoft.com/library/hh314627.aspx).

>[AZURE.TIP] Azure pikaopas tiettyjä malleja HPC-paketti, asenna ja määritä verkko-osat automaattisesti. Jos luot klusterin [HPC Pack IaaS käyttöönoton komentosarjan](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) avulla, voit halutessasi Asenna ja määritä verkko-osien käyttöönoton osana.

**Web-osien asentaminen**

1. Muodosta yhteys pään solmu AM klusterin järjestelmänvalvojan tunnistetiedoilla.

2. HPC Pack-asennus-kansioon ja suorita HpcWebComponents.msi pään solmu.

3. Noudata ohjatun web-osien asentaminen

**Voit määrittää WWW-osat**

1. Käynnistä pään solmu HPC PowerShell järjestelmänvalvojana.

2. Jos haluat muuttaa kansion kokoonpanon komentosarjaa sijainti, kirjoita seuraava komento:

    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. REST-käyttöliittymän ja HPC WWW-palvelun käynnistäminen, kirjoita seuraava komento:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```

4. Kun sinua pyydetään Valitse varmenne, valitse varmenne, joka vastaa pään solmun julkisen DNS nimi. Esimerkiksi jos otat käyttöön pään solmu AM perinteinen käyttöönotto-mallin, varmenteen nimi näyttää CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Jos käytät resurssien hallinnan käyttöönottomalli, varmenteen nimi näyttää CN =&lt;*HeadNodeDnsName*&gt;. &lt; *alueen*&gt;. cloudapp.azure.com.

    >[AZURE.NOTE] Voit valita tämän todistuksen myöhemmin, kun teet töitä pään solmu paikalliseen tietokoneeseen. Älä valitse tai määritä varmenne, joka vastaa pään solmu Active Directory-toimialueen nimi (esimerkiksi CN =*MyHPCHeadNode.HpcAzure.local*).

5. Määrittäminen web-portaalin työn lähettämistä varten, kirjoita seuraava komento:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Sen jälkeen, kun komentosarja on valmis, Lopeta ja Käynnistä HPC työn ajoitus-palvelu uudelleen kirjoittamalla seuraavia komentoja:

    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Vaihe 2: Asenna HPC Pack asiakas-apuohjelmat paikalliseen tietokoneeseen

Jos haluat asentaa tietokoneeseen HPC Pack asiakas-apuohjelmien, lataa HPC Pack-asennustiedostojen (täydellinen asennus) [Microsoft Download Centeristä](http://go.microsoft.com/fwlink/?LinkId=328024). Kun aloitat asennuksen, valitse Asetukset-vaihtoehto **HPC Pack asiakkaan apuohjelmat**.

HPC Pack asiakastyökalut avulla voit lähettää pään solmu AM työt, myös haluat varmenteen vieminen pään solmu ja asenna se asiakastietokoneen. Varmenne on oltava. CER muoto.

**Varmenteen tuominen pään solmu**

1. Lisätä MMC paikallistietokoneen tilin pään solmu Varmenteet-laajennuksen. Katso kohdasta Lisää laajennuksen [Lisää Varmenteet-laajennus MMC: hen](https://technet.microsoft.com/library/cc754431.aspx).

2. Laajenna konsolipuussa **Varmenteet paikallistietokoneen** > **Omat**, ja valitse sitten **Varmenteet**.

3. Etsi varmenne, jonka määritit HPC Pack web Components [Vaihe 1: Asenna ja määritä verkko-osien pään solmun](#step-1:-install-and-configure-the-web-components-on-the-head-node) (esimerkiksi CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net).

4. Kaksoisnapsauta varmennetta ja valitse **Kaikki tehtävät** > **Vie**.

5. Valitse ohjatussa varmenteen vieminen **Seuraava**ja varmista, että **ei, älä vie yksityinen avain** on valittuna.

6. Noudattamalla annettuja ohjeita ohjatun toiminnon Vie DER encoded binaarinen X.509-varmenne (. CER)-muoto.


**Asiakastietokoneen varmenteen tuominen**


1. Kopioi varmenne, jota asiakastietokone kansioon viety pään solmu.

2. Asiakastietokoneen Suorita certmgr.msc.

3. Varmenteen hallinta Laajenna **Varmenteet nykyisen käyttäjän** > **Luotettujen päämyöntäjien** **Varmenteet**hiiren kakkospainikkeella ja valitse sitten **Kaikki tehtävät** > **Tuo**.

4. Ohjatun varmenteen tuominen valitsemalla **Seuraava** ja noudata ohjeita, jotka olet vienyt pään solmu luotettujen päämyöntäjien Store-varmenteen tuominen.



>[AZURE.TIP] Näyttöön saattaa tulla suojauksen varoitus, koska pään solmun varmenteiden myöntäjä ei ole tunnistettu asiakastietokone. Testausta varten Ohita tämä varoitus ja viimeistele sertifikaatin tuominen.

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Vaihe 3: Suorita testi työt klusterin

Vahvista kokoonpanosi, yritä töitä Azure-klusterin paikallisen tietokoneen. Voit esimerkiksi käyttää HPC Pack Graafisen työkalut- tai komentorivin komentoja voit lähettää klusterin työt. Voit myös lähettää työt verkkopohjaisia portaali.


**Suorittaa työn lähetyksen komentoja asiakastietokone**


1. Avaa komentokehote asiakastietokoneessa, johon on asennettu HPC Pack asiakkaan apuohjelmia.

2. Kirjoita malli-komento. Kirjoita esimerkiksi luettelo kaikista projekteista klusterin-komento, joka on samanlainen johonkin pään solmun DNS-nimen mukaan seuraavasti:

    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
    
    tai
    
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```

    >[AZURE.TIP] DNS-nimen pään solmun ei IP-osoitteen käyttäminen ajoituksen URL-osoite. Jos määrität IP-osoite, virhe tulee näkyviin samalla tavalla kuin "palvelimen varmenne on on joko luota kelvollinen yhdistettyjen tai sijoittaa luotetun varmenteiden säilön."

3. Kun sinulta kysytään, kirjoita käyttäjänimi (lomake &lt;toimialuenimi&gt;\\&lt;käyttäjänimi&gt;) ja salasana, HPC klusterin järjestelmänvalvojana tai toinen klusterin-käyttäjä, jonka määritit. Voit tallentaa tunnistetietojen paikallisesti työn toimintoja.

    Töiden luettelo tulee näkyviin.


**HPC töiden hallinnan käyttämisestä asiakastietokone**

1. Jos et aiemmin tallentaa toimialueen klusterin käyttäjän tunnistetietoja lähetettäessä työn, voit lisätä tunnistetiedot tunnistetietojen hallinta.

    a. Asiakastietokoneen Ohjauspaneelin Käynnistä tunnistetietojen hallinta.

    b. Valitse **Windows-tunnistetiedot** > **Lisää yleinen tunnistetiedon**.

    c-näppäinyhdistelmää. Määritä Internet-osoite (esimerkiksi https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler tai https://&lt;HeadNodeDnsName&gt;.&lt; alueen&gt;.cloudapp.azure.com/HpcScheduler), ja käyttäjänimi (&lt;toimialuenimi&gt;\\&lt;käyttäjänimi&gt;) ja salasana, klusterin järjestelmänvalvojana tai toinen klusterin-käyttäjä, jonka määritit.

2. Asiakastietokoneen Käynnistä HPC töiden hallinta.

3. Kirjoita **Valitse pää-solmu** -valintaikkunaan pään alisolmun korottaminen Azure URL-osoite (esimerkiksi https://&lt;HeadNodeDnsName&gt;. cloudapp.net tai https://&lt;HeadNodeDnsName&gt;.&lt; alueen&gt;. cloudapp.azure.com).

    HPC työn esimies avautuu ja näyttää luettelon lentoyhtiöiden työt pään solmun.

**Jos haluat käyttää pää solmun web-portaalissa**

1. Käynnistä selain asiakastietokoneen ja kirjoita jompikumpi seuraavista osoitteista pään solmun DNS-nimen mukaan:

    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
    
    tai
    
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Kirjoita näkyviin tulevan suojaus-valintaikkunan toimialueen HPC klusterin järjestelmänvalvojan tunnistetietoja. (Voit myös lisätä klusterin muiden käyttäjien eri rooleja. Katso [klusterin käyttäjien hallinta](https://technet.microsoft.com/library/ff919335.aspx).)

    Web-portaalin avautuu työn luettelonäkymän.

3. Voit lähettää otoksen työn, joka palauttaa merkkijonon "Hei-maailman" klusterista valitsemalla vasemmanpuoleisesta siirtymispalkista **Uusi projekti** .

4. Valitse **Uusi projekti** -sivulla kohdassa **lähetyksen sivuilta** **HelloWorld**. Työn lähetys-sivu tulee näkyviin.

5. Valitse **Lähetä**. Pyydettäessä tunnistetiedoilla toimialueen HPC klusterin järjestelmänvalvojaan. Työn lähetetään ja työn tunnus näkyy **Omat työt** -sivulla.

6. Voit tarkastella tuloksia, jotka olet lähettänyt työn, työn tunnus ja valitse sitten **Tehtävät** , voit tarkastella komennon tulosteessa ( **tulos**)-kohdassa.

## <a name="next-steps"></a>Seuraavat vaiheet

* Voit myös lähettää Azure klusterin [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx)työt.

* Jos haluat lähettää klusterin työt Linux asiakaskoneesta, katso Python otosten [HPC Pack 2012 R2: n SDK ja -koodi](https://www.microsoft.com/download/details.aspx?id=41633).


<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
