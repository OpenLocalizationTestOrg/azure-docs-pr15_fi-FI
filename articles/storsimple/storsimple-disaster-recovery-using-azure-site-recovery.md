<properties 
   pageTitle="Automatisoida DR varten jaettujen tiedostojen käyttäminen Azure palauttaminen StorSimple | Microsoft Azure"
   description="Tässä artikkelissa kuvataan vaiheet ja tietojen palauttaminen ratkaisua isännöimät StorSimple tallennustilan tiedostoresurssit luomisen parhaat käytännöt."
   services="storsimple"
   documentationCenter="NA"
   authors="vidarmsft"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/16/2016"
   ms.author="vidarmsft" />

# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Azure palauttaminen käytön isännöimät StorSimple jaettujen tiedostojen palauttaminen automatisoitu

## <a name="overview"></a>Yleiskatsaus

Microsoft Azure StorSimple on hybrid cloud tallennustilan ratkaisu, joka korjaa rakenteeton tiedostoresurssit yleisesti liittyviä tietoja monimutkaisia. StorSimple käyttää pilvitallennustilaa tiedostotunnistetta paikallisen ratkaisun ja automaattisesti tasoa tiedot näkyviin paikallisen tallennustilan ja pilvitallennustilaa. Tietojen suojaaminen integroitu paikallisen ja cloud tilannevedoksia, ei tarvitse sprawling storage-infrastruktuurin.

[Azure sivuston palautus](../site-recovery/site-recovery-overview.md) on Azure-pohjaiset palvelu, joka tarjoaa tietojen palauttaminen (DR) ominaisuuksia orchestrating replikoinnin automaattisesti ja näennäiskoneiden palauttaminen. Azure palauttaminen tukee replikoinnin tekniikoita johdonmukaisesti replikoida, suojata ja näennäiskoneiden ja sovellusten yksityinen/julkinen vai isännöityä paveikslėlis saumattomasti epäonnistuu.

Azure palauttaminen, virtuaalikoneen replikoinnin ja StorSimple cloud tilannevedoksen ominaisuuksien avulla voit suojata kokonaisen tiedoston server-ympäristössä. Häiriöitä, jos voit tuoda tiedostoresurssit verkossa Azure vain muutaman minuutin kuluttua yhdellä napsautuksella.

Tässä asiakirjassa kerrotaan, miten voit luoda tietojen palauttaminen ratkaista oman tiedostoresurssit isännöimät StorSimple tallennus- ja suorittaa suunniteltujen, suunnittelematon sekä testata failovers käyttämällä yhdellä napsautuksella palauttaminen. Se näyttää oikeastaan, voit muokata palautus suunnitteleminen Azure sivuston palautus-säilöön käyttöön StorSimple failovers aikana tietojen skenaarioita. Lisäksi se kuvaa tuetut määrityksiä ja edellytykset. Tämän asiakirjan oletetaan, että olet tutustunut Azure palauttaminen ja StorSimple arkkitehtuureihin perusteet.

## <a name="supported-azure-site-recovery-deployment-options"></a>Tuetut Azure palauttaminen käyttöönottoasetukset

Asiakkaat voivat tiedostopalvelimien käyttöönottaminen fyysiset palvelimet tai näennäiskoneiden (VMs) Hyper-V tai VMware ja luo sitten carved StorSimple loppunut asemista tiedostoresurssit. Azure palauttaminen suojata sekä fyysinen ja virtuaalinen ominaisuuksissa toissijainen sivustoon tai Azure. Tässä asiakirjassa kerrotaan DR ratkaista Azure kuin AM isännöidä Hyper-V tiedostopalvelimeen palautus-sivusto ja valitse StorSimple tallennustilan jaettujen tiedostojen tietoja. Muissa tilanteissa, joiden tiedostopalvelimeen AM on VMware AM tai fyysinen koneen voidaan toteuttaa vastaavasti.

## <a name="prerequisites"></a>Edellytykset

Yhdellä napsautuksella tietojen palauttaminen-ratkaisun, joka käyttää Azure palauttaminen isännöimät StorSimple tallennustilan tiedostoresurssit toteuttaminen on seuraavat edellytykset:

-   Paikallinen Windows Server 2012 R2-tiedosto server AM isännöidä Hyper-V tai VMware tai fyysinen machine

-   StorSimple tallennustilan laitteen paikallisen rekisteröityjä Azure StorSimple hallinta

-   StorSimple Cloud laitteen luotu Azure StorSimple hallintaan (tämä voidaan säilyttää sulkea alaspäin tilassa)

-   Jaettujen tiedostojen isännöimät määritetty StorSimple tallennustilan laitteeseen tietomääristä

-   [Azure palauttaminen services säilö](../site-recovery/site-recovery-vmm-to-vmm.md) luotu Microsoft Azure-tilaus

Lisäksi Azure onko palauttaminen sivuston VMs varmistaa, että ne ovat yhteensopivia Azure VMs ja Azure palauttaminen suorittaa [Azure virtuaalikoneen valmiuden-työkalu](http://azure.microsoft.com/downloads/vm-readiness-assessment/) .

Voit välttää ongelmat (joka voi johtaa suurempiin kustannukset), varmista, että StorSimple Cloud laitteen, automaatio-tili ja tallennustilaa luominen viive käyttäjätiliä samalla alueella.

## <a name="enable-dr-for-storsimple-file-shares"></a>Ota käyttöön StorSimple tiedostoresurssit DR  

Paikallisen ympäristön kunkin osan on suojattu valmis replikoinnin ja palautus otetaan käyttöön. Tässä osassa kuvataan, miten voit:

-   Active Directory- ja DNS-replikoinnin määrittäminen (valinnainen)

-   Azure palauttaminen avulla voit ottaa käyttöön tiedostopalvelimeen AM suojaus

-   StorSimple tietomääristä suojauksen ottaminen käyttöön

-   Verkon määrittäminen

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Active Directory- ja DNS-replikoinnin määrittäminen (valinnainen)

Jos haluat suojata käyttävät Active Directory- ja DNS niin, että ne ovat käytettävissä sivustossa DR koneet, tarvitset suojaaminen lukitsemalla erikseen (niin, että tiedostopalvelimet ei käytettävissä, kun virheiden todennusta). Vaihtoehtoja on kaksi suositellut asiakkaan paikallisen ympäristön monimutkaisuuden perusteella.

#### <a name="option-1"></a>Vaihtoehto 1

Jos asiakkaan on pieni useita sovelluksia, yhden toimialueen ohjauskoneen käyttöön koko paikallisen sivuston ja olla viallinen päällä koko sivuston sitten Suosittelemme replikoida toimialueen ohjauskoneen tietokoneeseen (tämä on käytettävissä sivuston sivuston ja sivuston Azure) toissijaisen sivuston Azure palauttaminen replikoinnin avulla.

#### <a name="option-2"></a>Vaihtoehto 2

Jos asiakkaan on paljon sovellukset, Active Directory-metsää on käynnissä ja kaatuvat päälle joitain sovelluksia kerrallaan ja valitse Suosittelemme, että muut ohjauskoneen DR-sivuston määrittäminen (toissijainen sivuston tai Azure-tietokannassa).

Katso [automaattisen DR](../site-recovery/site-recovery-active-directory.md) ratkaisuun Active Directory-ja Azure palauttaminen käyttämällä DNS ohjeita, kun teet toimialueen ohjauskoneen DR-sivustossa. Tämän asiakirjan jäljellä on olettaa toimialueen ohjauskoneen on saatavana DR-sivustosta.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Azure palauttaminen avulla voit ottaa käyttöön tiedostopalvelimeen AM suojaus

Tämä vaihe edellyttää valmisteleminen paikallisen tiedoston server-ympäristössä, luominen ja valmisteleminen Azure sivuston palautus-säilö ja Ota suojaus, AM.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Valmistele paikallisen tiedoston server-ympäristössä

1.  **Älä koskaan ilmoita**asettaa **Käyttäjätilien valvonta** . Tämä tarvitaan, jotta Azure Automaatiokomentosarjojen avulla voit muodostaa iSCSI-kohteiden virheiden Azure palauttaminen korvaa jälkeen.

    1.  Paina Windows-näppäin + Q ja **Käyttäjätilien**etsiminen.

    2.  Valitse **Muuta käyttäjätilien valvonta-asetukset**.

    3.  Vedä palkkia kohti **Ilmoittaa aina**alaspäin.

    4.  Valitse **OK** ja valitse sitten **Kyllä** pyydettäessä.

        ![](./media/storsimple-dr-using-asr/image1.png)

1.  Asenna AM-agentti kunkin tiedostopalvelimeen VMs. Tämä tarvitaan, jotta voit suorittaa Azure automaatio-komentosarjat-epäonnistunutta VMs päälle.

    1.  [Lataa agentti](http://aka.ms/vmagentwin) `C:\\Users\\<username>\\Downloads`.

    2.  Avaa Windows PowerShellin järjestelmänvalvoja-tilassa (Suorita järjestelmänvalvojana) ja kirjoita seuraava komento Siirry download sijaintiin:

        `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

        > [AZURE.NOTE] Tiedostonimen voivat muuttua version mukaan.

1.  Valitse **Seuraava**.

2.  Hyväksy **Käyttöoikeussopimus** ja valitse sitten **Seuraava**.

3.  Valitse **Valmis**.


1.  Carved StorSimple loppunut tietomääristä jaettujen tiedostojen luominen Lisätietoja on artikkelissa [StorSimple hallintapalvelu hallittavan asemat](storsimple-manage-volumes.md).

    1.  Paikallisen-VMs Paina Windows-näppäin + Q ja etsiä **iSCSI**.

    2.  Valitse **iSCSI-käynnistäjä**.

    3.  Valitse **määritys** -välilehti ja kopioi käynnistäjä nimi.

    4.  Kirjaudu sisään [Azure perinteinen portal](https://manage.windowsazure.com/).

    5.  Valitse **StorSimple** -välilehti ja valitse sitten StorSimple hallintapalveluun, joka sisältää fyysinen laite.

    6.  Luo aseman riittävästä ja luo sitten asema(t). (Asemat ovat tiedoston share(s) tiedostopalvelimessa VMs). Kopioi käynnistäjä nimi ja antaa Accessin ohjausobjektin tietueiden nimi, kun luot asemat.

    7.  Valitse **määritys** -välilehti ja Huomautus alaspäin laitteen IP-osoite.

    8.  Valitse paikallisen-VMs Siirry **iSCSI-käynnistäjä** uudelleen ja kirjoita IP nopea yhdistäminen-osassa. Valitse **Nopean yhteyden** (laitteen pitäisi nyt olla yhdistetty).

    9.  Avaa Azure hallinta-portaalin ja valitse sitten **asemat ja laitteet** -välilehti. Valitse **automaattinen määrittäminen**. Äänenvoimakkuuden juuri luomasi pitäisi näkyä.

    10. Portaalissa, valitse sitten **laitteet** -välilehti ja valitse sitten **luoda uuden Virtual.** (Virtual laite voidaan käyttää, jos vikasietotila ilmenee). Uuden virtual laitteen voidaan säilyttää offline-tilassa voit välttää lisäkustannuksia. Offline-tilaan virtual laitteen, siirry kohtaan **näennäiskoneiden** portaalissa ja sulje se.

    11. Siirry takaisin paikalliseen VMs ja avaa Levynhallinta (paina Windows-näppäin + X ja valitse **Disk Management**).

    12. Huomaat, että jotkin ylimääräisiä levyjä (sen mukaan, olet luonut tietomääristä määrä). Ensimmäisen vaiheen, valitse **Alusta levy**hiiren kakkospainikkeella ja valitse sitten **OK**. **Unallocated** -osassa hiiren kakkospainikkeella, valitse **Uusi tavallinen asema**, liittää kirjaimen ja ohjatun.

    13. Toista vaihe l levyjä. Nyt näet kaikki levyjen **Tämä tietokone** Windowsin Resurssienhallinnassa.

    14. Tiedoston ja tallennustilaa palvelut-roolin avulla voit luoda tiedostoresurssit asemat.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Luominen ja valmisteleminen Azure sivuston palautus-säilö

Tarkista aloittaa Azure palauttaminen ennen suojaamista tiedostopalvelimeen AM [Azure palauttaminen ohjeissa](../site-recovery/site-recovery-hyper-v-site-to-azure.md) .

#### <a name="to-enable-protection"></a>Jos haluat ottaa käyttöön suojaus

1.  Katkaise iSCSI target(s) paikallisen VMs, jonka haluat suojata Azure palauttaminen kautta:

    1.  Paina Windows-näppäin + Q ja etsiä **iSCSI**.

    2.  Valitse **iSCSI-käynnistäjä määrittäminen**.

    3.  Irrota StorSimple laitteella, joka aiemmin muodostettu. Vaihtoehtoisesti voit siirtyä käytöstä tiedoston muutaman minuutin ajan kun suojauksen ottaminen käyttöön.

    > [AZURE.NOTE] Tämä aiheuttaa tiedostoresurssit on tilapäisesti poissa käytöstä

1.  Tiedoston palvelimen AM Azure sivuston palautus-portaalista [virtuaalikoneen suojausta](../site-recovery/site-recovery-hyper-v-site-to-azure.md##step-6-enable-replication) .

2.  Ensimmäinen synkronointi alkaa, kun olet taas kohde uudelleen. Siirry iSCSI-käynnistäjä, valitse laite, StorSimple ja valitse **Yhdistä**.

3.  Kun synkronointi on valmis, ja AM tila on **suojattu**, valitse AM, **määritys** -välilehti ja päivittää AM verkon vastaavasti (tämä on verkossa, joka epäonnistui päälle VM(s) on osa). Jos verkon ei näy, se tarkoittaa, että Synkronoi on kyse edelleen.

### <a name="enable-protection-of-storsimple-volumes"></a>StorSimple tietomääristä suojauksen ottaminen käyttöön

Jos et ole valinnut StorSimple levyasemien **käyttöön tämän aseman oletusarvoisesti varmuuskopio** -asetus, siirry **Varmuuskopion käytännöt** StorSimple hallinta-palvelussa ja luoda kaikki asemat sopivan varmuuskopion käytännön. On suositeltavaa varmuuskopioiden taajuus asettaminen palautus pisteen tavoitteen (RPO), jotka haluat nähdä sovelluksen.

### <a name="configure-the-network"></a>Verkon määrittäminen

Tiedoston palvelimen AM määrittää verkkoasetukset Azure palauttaminen niin, että AM verkkojen liitetyt oikean DR verkon jälkeen automaattisesti.

Voit valita AM **VMM Cloud** tai **Suojaus-ryhmä** verkko-asetusten määrittämiseen seuraavassa kuvassa esitetyllä tavalla.

![](./media/storsimple-dr-using-asr/image2.png)

## <a name="create-a-recovery-plan"></a>Suunnittele palauttaminen

Voit luoda ASR automatisoida automaattisesti jaettujen tiedostojen palauttaminen-suunnitelma. Jos häiriöitä, voit noutaa jaettujen tiedostojen kanssa vain yhdellä napsautuksella muutaman minuutin kuluttua. Tämä automaatio käyttöön on Azure automaatio-tiliä.

#### <a name="to-create-the-account"></a>Luo tili

1.  Siirry Azure perinteinen-portaaliin ja siirry **automaatio** -osaan.

1.  Luo uusi automaatio-tili. Pidä se geo/alue saman jossa StorSimple Cloud laitteen ja tallennustilaa tilit on luotu.

2.  **Uusi** &gt; **Sovelluksen Services** &gt; **automaatio** &gt; **Runbookin** &gt; **-Valikoima** tarvittavat runbooks tuominen automaatio-tiliin.

    ![](./media/storsimple-dr-using-asr/image3.png)

1.  Lisää seuraavat runbooks valikoiman **Palauttaminen** -ruudusta:

    -   Epäonnistua StorSimple äänenvoimakkuuden säilöjen päälle

    -   Puhdista StorSimple asemista jälkeen testi automaattisesti (TFO)

    -   Ota asemat StorSimple laitteen automaattisesti jälkeen

    -   Käynnistä StorSimple Virtual laitteen

    -   Mukautettu komentosarja Azure AM laajennuksen asennuksen poistaminen

        ![](./media/storsimple-dr-using-asr/image4.png)


1.  Julkaise kaikki komentosarjat valitsemalla: n runbookin automaatio-tili ja siirtymällä **Tekijä** -välilehteen. Tässä vaiheessa jälkeen **Runbooks** -välilehti tulee näkyviin seuraavasti:

     ![](./media/storsimple-dr-using-asr/image5.png)

1.  Automaatio-tilin **resurssit** -välilehti, valitse **Lisää asetuksia** &gt; **Lisää tunnistetiedot**sekä lisätä Azure tunnistetiedot – resurssi AzureCredential nimi.

    Käytä Windows PowerShell-tunnistetietoja. Tämä on oltava tunnistetiedon, joka sisältää Organisaatiotunnus käyttäjänimi ja salasana access Azure tähän tilaukseen sekä monimenetelmäisen todentamisen käytöstä. Tämä on pakollinen tarkistamiseen käyttäjän puolesta failovers ja tuo näkyviin tiedoston palvelimen asemista DR-sivustossa.

1.  Automaatio-tilin **resurssit** -välilehti ja valitse sitten **Lisää asetuksia** &gt; **Lisää muuttujan** ja lisää seuraavat muuttujat. Voit salata nämä resurssit. Muuttujat ovat palauttamista suunnitelman – koskevien. Jos yhteyttä palautus suunnitteleminen (jonka luot seuraavan vaiheen) nimi on TestPlan ja valitse käyttäjän muuttujat pitäisi olla TestPlan StorSimRegKey ja TestPlan AzureSubscriptionName.

    -   *RecoveryPlanName* **-StorSimRegKey**: StorSimple hallintapalvelu rekisteröinti-näppäintä.

    -   *RecoveryPlanName* **-AzureSubscriptionName**: Azure tilauksen nimeä.

    -   *RecoveryPlanName* **-ResourceName**:, jossa on StorSimple laitteen StorSimple resurssin nimi.

    -   *RecoveryPlanName* **-Laitenimi**: laitteella, joka on epäonnistui päälle.

    -   *RecoveryPlanName* **-TargetDeviceName**: StorSimple Cloud laitteen joina säilöt ovat epäonnistui päälle.

    -   *RecoveryPlanName* **-VolumeContainers**: CSV-merkkijono aseman säilöjä laitteessa vaikuttavat epäonnistui; esimerkiksi volcon1, volcon2, volcon3.

    -   RecoveryPlanName**-TargetDeviceDnsName**: kohdelaitteen palvelun nimen (tämä löytyy **virtuaalikoneen** -osassa: palvelunimi on sama kuin DNS-nimi).

    -   *RecoveryPlanName* **-StorageAccountName**: jossa komentosarja (joka on suoritettaviksi epäonnistunutta päälle AM) tallennetaan tallennustilan tilin nimi. Tämä voi olla minkä tahansa tallennustilan-tili, jolla on tilaa tallentamiseen komentosarja tilapäisesti.

    -   *RecoveryPlanName* **-StorageAccountKey**: edellä tallennustilan tilin pikanäppäin.

    -   *RecoveryPlanName* **-ScriptContainer**: nimi, jossa komentosarja tallennetaan pilveen säilö. Jos säilö ei ole valittu, se luodaan.

    -   *RecoveryPlanName* **-VMGUIDS**: yhteydessä suojaaminen AM Azure palauttaminen määrittää jokaisen AM yksilöllinen tunnus, jotka antavat tietoja epäonnistunutta AM päälle. Voit hankkia VMGUID, valitse **Palautus palvelut** -välilehti ja valitse sitten **Suojattu kohteen** &gt; **Suojaus ryhmien** &gt; **koneet** &gt; **Ominaisuudet**. Jos sinulla on useita VMs, Lisää GUID CSV-merkkijonona.

    -   *RecoveryPlanName* **-AutomationAccountName** – automaatio-tili, johon olet lisännyt runbooks ja varat nimi.

    Esimerkiksi jos palautus-palvelupaketti on fileServerpredayRP, valitse **resurssit** -välilehden pitäisi näkyä seuraavasti lisättyäsi ne kalusteet.

    ![](./media/storsimple-dr-using-asr/image6.png)


1.  Siirry **Palautus Services** -osassa ja valitse aiemmin luomasi Azure palauttaminen säilö.

2.  **Suunnitelmien palautus** -välilehti ja luo uusi palautus suunnitelma seuraavasti:

    a.  Nimi ja valitse haluamasi **Suojaus-ryhmä**.

    b.  Valitse Suojaus-ryhmä, jonka haluat sisällyttää palautus-suunnitelma VMs.

    c-näppäinyhdistelmää.  Kun palautus suunnitelma luodaan, valitse Avaa palautus suunnitelman mukauttaminen-näkymä.

    d.  Valitse **Kaikki ryhmät Sammuta**, valitse **komentosarja**ja valitse **Lisää ensisijainen komentosarja, ennen kuin kaikki ryhmän Sammuta**.

    e.  Valitse automaatio-tili (jonka olet lisännyt runbooks) ja valitse sitten **epäonnistua over-StorSimple-asema-säilöt** -runbookin.

    f.  Valitse **ryhmän 1: Käynnistä**, valitse **näennäiskoneiden**, ja lisää VMs, joita voi suojata palautus-suunnitelma.

    g.  Valitse **ryhmän 1: Käynnistä**, valitse **komentosarja**, ja lisää seuraavat komentosarjat **jälkeen ryhmän 1** vaiheet järjestyksessä.

    - Käynnistä-StorSimple-Virtual-laitteen runbookin
    - Yli-StorSimple-asema-säilöjen runbookin epäonnistuu
    - Käyttöönotto-asemat-jälkeen-automaattisesti runbookin
    - Poista-mukautettu-komentosarja-tunniste runbookin

1.  Manuaalinen toiminnon jälkeen edellä 4 komentosarjojen lisääminen saman **ryhmän 1: jälkeiset toimet** osa. Tämä toiminto on kohta, josta voit tarkistaa, että kaikki toimii oikein. Tämä toiminto on voidaan lisätä vain osana testiä automaattisesti (joten vain Valitse **Testaa automaattisesti** -valintaruutu).

2.  Manuaalinen toiminnon jälkeen Lisää samalla tavalla kuin muiden runbooks käytit uudelleenjärjestäminen komentosarja. Suunnitelman palauttaminen.

    > [AZURE.NOTE] Testaa automaattisesti suoritettaessa kaikki manuaaliset toiminnon vaihe kannattaa tarkistaa sillä StorSimple asemista, joka oli on kopioitu kohdeosoite laitteeseen poistetaan osana tyhjennys manuaalinen toiminnon jälkeen.

    ![](./media/storsimple-dr-using-asr/image7.png)

## <a name="perform-a-test-failover"></a>Suorita testi automaattisesti

Active Directory huomioitavista [Active Directory DR ratkaisu](../site-recovery/site-recovery-active-directory.md) avustaja-opas viitata testi vikasietotilaa aikana. Paikallisen asennuksen ei häiriintyvät lainkaan, Testaa vikasietotilaa yhteydessä. StorSimple asemista, joka on liitetty paikallisen AM on kopioitu Azure-StorSimple Cloud-laitteeseen. AM testaus tuodaan Azure-tietokannassa ja kloonatun tietomääristä liitetyt AM.

#### <a name="to-perform-the-test-failover"></a>Jos haluat lajitella testi vikasietotilaa

1.  Azure perinteinen-portaalissa Valitse sivuston palautus-säilö.

1.  Valitse Luo tiedostopalvelimeen AM palautus-suunnitelma.

2.  Valitse **Testaa automaattisesti**.

3.  Valitse Aloita testi automaattisesti virtual verkkoon.

    ![](./media/storsimple-dr-using-asr/image8.png)

1.  Toissijainen ympäristön ollessa ylöspäin voit suorittaa oman vahvistukset.

2.  Kun vahvistukset ovat valmiit, valitse **Vahvistukset valmiiksi**. Poistetaan automaattisesti testiympäristössä ja TFO-toiminto on valmis.

## <a name="perform-an-unplanned-failover"></a>Suorittaa suunnittelematon automaattisesti

Aikana suunnittelematon automaattisesti StorSimple asemista epäonnistui päälle virtual laitteeseen, replikan AM tuotava Azure- ja asemat liitetyt AM.

#### <a name="to-perform-an-unplanned-failover"></a>Jos haluat lajitella suunnittelematon automaattisesti

1.  Azure perinteinen-portaalissa Valitse sivuston palautus-säilö.

1.  Valitse Luo tiedostopalvelimeen AM palautus-suunnitelma.

2.  Valitse **automaattisesti** , ja valitse sitten **Suunnittelematon automaattisesti**.

    ![](./media/storsimple-dr-using-asr/image9.png)

1.  Valitse kohde verkko ja valitse sitten tarkistuksen kuvake ✓ automaattisesti Aloita.

## <a name="perform-a-planned-failover"></a>Suorittaa suunnitellun automaattisesti

Aikana suunnitellun automaattisesti paikallisen tiedoston palvelimen AM suljetaan tilanteen ja StorSimple laitteen asemista varmuuskopion tilannevedoksen otetaan pilvestä. StorSimple tietomääristä epäonnistui päälle virtual laitteeseen replikan AM tuodaan Azure- ja asemat liitetyt AM.

#### <a name="to-perform-a-planned-failover"></a>Suorittamiseen suunnitellun automaattisesti

1.  Azure perinteinen-portaalissa Valitse sivuston palautus-säilö.

1.  Valitse Luo tiedostopalvelimeen AM palautus-suunnitelma.

2.  Valitse **automaattisesti** , ja valitse sitten **Suunniteltu automaattisesti**.

3.  Valitse kohde verkko ja valitse sitten tarkistuksen kuvake ✓ automaattisesti Aloita.

## <a name="perform-a-failback"></a>Suorittaa tuntisesta

Tuntisesta StorSimple äänenvoimakkuuden säilöt ovat epäonnistui aikana kautta takaisin fyysinen laite sen jälkeen, kun ne on otettu.

#### <a name="to-perform-a-failback"></a>Jos haluat lajitella tuntisesta

1.  Azure perinteinen-portaalissa Valitse sivuston palautus-säilö.

1.  Valitse Luo tiedostopalvelimeen AM palautus-suunnitelma.

2.  Valitse **automaattisesti** , ja valitse **suunniteltu automaattisesti** tai **suunnittelematon automaattisesti**.

3.  Valitse **Muuta suunta**.

4.  Valitse haluamasi tietojen synkronointi ja AM luonnin asetukset.

5.  Valitse tarkistuksen kuvake ✓ tuntisesta Aloita.

    ![](./media/storsimple-dr-using-asr/image10.png)

## <a name="best-practices"></a>Parhaat käytännöt

### <a name="capacity-planning-and-readiness-assessment"></a>Kapasiteetin suunnittelu ja valmiuden arviointi


#### <a name="hyper-v-site"></a>Hyper-V-sivusto

[Käyttäjän kapasiteetin suunnittelutyökalua](http://www.microsoft.com/download/details.aspx?id=39057) avulla voit suunnitella palvelimeen, varasto ja verkkoinfrastruktuuria Hyper-V replikan ympäristössä.

#### <a name="azure"></a>Azure

Voit suorittaa [Azure virtuaalikoneen valmiuden työkalun](http://azure.microsoft.com/downloads/vm-readiness-assessment/) VMs varmistaa, että ne ovat yhteensopivia Azure VMs ja Azure sivuston palauttaminen palvelut. Arvioinnin Valmiustyökalu tarkistaa AM määritykset ja ilmoittaa, kun määritykset eivät ole yhteensopivia Azure. Esimerkiksi se ongelmat varoitus, jos c-asema on suurempi kuin 127 Gigatavua.


Kapasiteetin suunnittelu koostuu vähintään kaksi tärkeää prosessit:

-   Yhdistämismäärityksen paikalliset Hyper-V VMs Azure AM koon (kuten A6, A7, A8 ja A9).

-   Määrittämiseen tarvittavat Internet-kaistanleveys.

## <a name="limitations"></a>Rajoitukset

- Tällä hetkellä vain 1 StorSimple laitteen voi olla epäonnistui päälle (yksi StorSimple-pilvi laitteen). Tiedostopalvelimessa, joka ulottuu eri StorSimple laitteiden skenaario ei vielä tueta.

- Jos saat virheilmoituksen AM suojauksen otettaessa, varmista, että yhteys on katkaistu iSCSI-kohteita.

- Kaikki aseman säilöt, jotka on ryhmitelty yhdessä vuoksi varmuuskopion käytännöt, jotka kestävät koko aseman säilöjen epäonnistuu päälle yhdessä.

- Kaikki valitsemasi aseman säilöjen asemat epäonnistuu päälle.

- Asemat, lisätä enintään yli 64 TT ei voi epäonnistui päälle, koska yksittäisen StorSimple Cloud-laitteen suurin kapasiteetin 64 Teratavua.

- Jos VMs luodaan Azure suunniteltu/suunnittelematon vikasietotilaa epäonnistuu, ei puhtaasti ylös VMs. Toimi sen sijaan tuntisesta. Jos poistat VMs sitten paikallisen VMs ei voi ottaa käyttöön uudelleen.

- Jälkeen automaattisesti, jos et näe asemista, VMs Siirry, Avaa Levynhallinta, asemat ja tuo ne verkossa.

- Joissakin tapauksissa DR sivuston asemakirjaimet voivat olla erilaisia kuin kirjaimet paikallisen. Näin tapahtuu, jos haluat korjata ongelman manuaalisesti vikasietotilaa päätyttyä.

- Monimenetelmäisen todentamisen ole käytössä, joka on syötetty automaatio-tilin tilana amerikkalaisen Azure tunnistetiedon. Jos todennus ei ole käytössä, komentosarjat eivät voi suorittaa automaattisesti ja palautus-suunnitelma epäonnistuu.

- Automaattisesti aikakatkaisun: StorSimple komentosarjan aikakatkaistaan Jos aseman säilöjä vikasietotilaa kauemmin kuin Azure palauttaminen yläraja komentosarjan (tällä hetkellä 120 minuuttia).

- Varmuuskopiointityön aikakatkaisu: StorSimple komentosarjan aikakatkaistaan Jos tietomääristä varmuuskopion kauemmin kuin Azure palauttaminen yläraja komentosarjan (tällä hetkellä 120 minuuttia).
 
    > [AZURE.IMPORTANT] Suorita varmuuskopiointi manuaalisesti Azure-portaalista ja suorita sitten palautus-suunnitelma uudelleen.

- Kloonaa aikakatkaisun: StorSimple komentosarjan aikakatkaistaan Jos tietomääristä kloonaamalla kauemmin kuin Azure palauttaminen yläraja komentosarjan (tällä hetkellä 120 minuuttia).

- Aika synkronointivirhe: StorSimple komentosarjojen virheitä, ajattelevat varmuuskopioista ei muutettu vaikka varmuuskopioinnin onnistuu portaalissa. Mahdollinen syy tähän voi olla StorSimple laitteen aika voi olla synkronoitu aikavyöhykkeen nykyinen aika.
 
    > [AZURE.IMPORTANT] Synkronoi laitteen kellonajan ja aikavyöhykkeen nykyisen kellonajan.

- Laitteen automaattisesti virhe: StorSimple komentosarjan saattaa epäonnistua, jos laitteen automaattisesti, kun palautus-palvelupaketti on käynnissä.
    
    > [AZURE.IMPORTANT] Suorita palauttaminen suunnitelman laitteen vikasietotilaa päätyttyä.

## <a name="summary"></a>Yhteenveto

Käytä Azure palauttaminen, voit luoda valmis automaattisen tietojen palauttaminen suunnitteleminen tiedostopalvelimeen AM ottaa tiedostoresurssit isännöimät StorSimple tallennustilan. Voit aloittaa vikasietotilaa muutamassa missä tahansa häiriöitä ja hanki sovellus tekemiseksi muutaman minuutin kuluttua.
