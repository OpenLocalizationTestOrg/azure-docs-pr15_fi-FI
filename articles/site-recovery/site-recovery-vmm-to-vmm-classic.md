<properties
    pageTitle="Toistaa Hyper-V näennäiskoneiden-VMM paveikslėlis toissijainen VMM sivuston | Microsoft Azure"
    description="Tässä artikkelissa käsitellään replikoida Hyper-V VMs-VMM paveikslėlis toissijaisen VMM sivuston Azure palauttaminen."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Rinnakkaiset Hyper-V näennäiskoneiden VMM paveikslėlis toissijainen VMM-sivustoon

> [AZURE.SELECTOR]
- [Azure portal](site-recovery-vmm-to-vmm.md)
- [Perinteinen portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - resurssien hallinta](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Azure palauttaminen-palvelun osaltaan mukaan orchestrating replikoinnin, automaattisesti ja näennäiskoneiden ja fyysiset palvelimet yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen. Koneet voi replikoida Azure tai toissijaisen paikallisen tietokeskuksen. Lue pikaisesti [Azure palauttaminen ominaisuudet?](site-recovery-overview.md)

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa käsitellään replikoida Hyper-V näennäiskoneiden Hyper-V host-palvelimissa, jotka hallitaan VMM paveikslėlis käyttämällä Azure palauttaminen toissijainen VMM-sivustoon.

Artikkelin sisältää edellytykset, kuinka saat määrittäminen sivuston palauttaminen säilöön, Azure sivuston palauttaminen Providerin asentaminen lähde- ja kohdeluettelo VMM palvelinten, rekisteröidä palvelimet säilö, määrittäminen VMM paveikslėlis suojausasetuksia ja ottaa Hyper-V VMs suojaus. Valmis testaamalla vikasietotilaa varmistaaksesi, että kaikki toimii odotetulla tavalla.

Kirjaa kommentteja tai kysymyksiä alaosassa on tämän artikkelin tai [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Arkkitehtuuri

Alla olevassa kuvassa näkyy eri viestintä kanavat ja Azure palauttaminen tiedonsiirron ja replikoinnin käyttämä portit

![E2E topologian](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Ennen aloittamista

Varmista, että seuraavat edellytykset on paikassa:

**Edellytykset** | **Tiedot**
--- | ---
**Azure**| Tarvitset [Microsoft Azure](https://azure.microsoft.com/) -tiliin. Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/). [Lue lisää](https://azure.microsoft.com/pricing/details/site-recovery/) siitä, että sivuston palauttaminen hinnat.
**VMM** | Tarvitset vähintään yksi VMM palvelimen.<br/><br/>VMM palvelimessa on käytössä vähintään System Center 2012 SP1 ja kumulatiiviset päivitykset.<br/><br/>Jos haluat määrittää yksittäisen VMM palvelimen suojaaminen, sinun on vähintään kaksi paveikslėlis määritetty palvelimeen.<br/><br/>Jos haluat ottaa käyttöön suojaus, jonka kaksi VMM palvelimessa, kunkin palvelimen on oltava vähintään yksi cloud määritetty suojattava ensisijainen VMM palvelimeen ja yksi cloud-toissijainen VMM-palvelimeen, jota haluat käyttää suojaus ja palauttaminen määritetty<br/><br/>Kaikki VMM paveikslėlis on oltava Määritä Hyper-V ominaisuuksien profiili.<br/><br/>Tietolähteen pilveen, jonka haluat suojata on sisällettävä vähintään yksi VMM host ryhmät.<br/><br/>Lisätietoja määrittäminen VMM paveikslėlis [vaiheittaisissa ohjeissa: luominen yksityinen paveikslėlis System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) Keijo Mayer blogista.
**Hyper-V** | Tarvitset vähintään yksi Hyper-V host palvelinten ensisijainen ja toissijainen VMM host-ryhmien ja vähintään yhden näennäiskoneiden kunkin Hyper-V Host (isäntä)-palvelimeen.<br/><br/>Host (isäntä)- ja kohdesivustojen Hyper-V-palvelimet on oltava vähintään Windows Server 2012 Hyper-V-roolin kanssa, muttet asentanut uusimmat päivitykset.<br/><br/>Mikä tahansa Hyper-V-palvelimeen, jossa haluat suojata VMs on sijaittava VMM pilvestä.<br/><br/>Jos käytössäsi on Hyper-V-klusterin, Huomaa, että klusterin broker ei ole luodaan automaattisesti, jos sinulla on staattinen IP osoite-pohjainen klusterin. Sinun täytyy määrittää klusterin broker manuaalisesti. [Lue lisää](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) Aidan Finn blogimerkinnän.
**Verkon määritys** | Voit määrittää ja varmista, että replikoitua näennäiskoneiden optimaalisesti sijoitetaan toissijainen Hyper-V host-palvelimissa jälkeen automaattisesti ja sopiva AM verkkoihin, että he voivat muodostaa verkon määritys. Jos verkon määritys ei määritetä, replikan VMs ei ole yhdistetty minkä tahansa verkkoon jälkeen automaattisesti.<br/><br/>Määrittäminen verkon määritys käyttöönoton aikana, varmista, että näennäiskoneiden lähteen Hyper-V Host (isäntä)-palvelimeen yhteydessä VMM AM verkkoon. Verkon linkittää looginen verkko, joka on liitetty cloud. < br /<br/>Toissijaisen VMM-palvelimeen, jota käytetään hyödynnettäviksi kohde pilveen pitäisi olla vastaava AM verkon määritetty ja se puolestaan on linkitettävä vastaavan looginen verkko, joka on liitetty kohde pilveen.<br/><br/>[Lue lisää](site-recovery-network-mapping.md) siitä, verkon määritys.
**Tallennustilan yhdistäminen** | Oletusarvon mukaan lähde Hyper-V palvelin virtual tietokoneen replikoinnin kohde Hyper-V Host (isäntä)-palvelimeen, replikoitua tiedot on tallennettu oletussijainti, joka on merkitty kohde Hyper-V isännän Hyper-V hallinnassa. Jos haluat hallita replikoitua tietojen tallennussijainti, voit määrittää tallennustilan yhdistäminen<br/><br/> Määritä tallennustilan yhdistämistä, joudut lähteen tallennustilan luokitusten määrittäminen ja kohdistaa VMM palvelimet, ennen kuin aloitat, käyttöönotto. [Lue lisää](site-recovery-storage-mapping.md).


## <a name="step-1-create-a-site-recovery-vault"></a>Vaihe 1: Luo sivuston palautus-säilö

1. Kirjaudu [Hallinta-portaalin](https://portal.azure.com) haluat rekisteröidä VMM palvelimesta.

2. Laajenna- **Tietopalvelut** > **Palautus Services** ja sitten **Sivuston palauttaminen säilö**.

3. Valitse **Luo uusi** > **nopea luominen**.

4. Kirjoita **nimi**-kenttään kutsumanimi tunnistavan säilö.

5. Valitse **alueen** maantieteellisen alueen säilö varten. Tarkista tuettujen alueiden artikkelissa maantieteelliset käytettävyys [Azure sivuston palauttaminen hinnat tiedot](http://go.microsoft.com/fwlink/?LinkId=389880).

6. Valitse **Luo säilö**.

    ![Luo säilöön](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Tarkista tilarivillä säilö on luotu. Säilö merkitään **aktiiviseksi** pääsivulta palautus-palvelut.

## <a name="step-2-generate-a-vault-registration-key"></a>Vaihe 2: Luo säilöön rekisteröinti avain

Luo säilö rekisteröinti-näppäintä. Kun olet ladannut Azure sivuston palauttaminen tarjoaja ja asenna se VMM-palvelimeen, käytät tätä näppäintä voit rekisteröidä VMM palvelimen säilö.

1. Valitse Pika-aloitus-sivun avaamiseen säilö **Palautus palveluita** -sivulle. Pikaopas voi avata myös milloin tahansa napsauttamalla kuvaketta.

    ![Pika-aloitusopas-kuvake](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)

2. Valitse avattavasta luettelosta **kaksi paikallisen VMM sivustojen välillä**.
3. **Valmistele VMM palvelimet**Valitse **Luo rekisteröinti avaimen tiedosto**. Avaimen tiedosto luodaan automaattisesti ja 5 päivää sen luonnin jälkeen on voimassa. Jos olet ei käytä Azure portaalin VMM palvelimesta haluat kopioida tämän tiedoston palvelimeen.

    ![Rekisteröinti avain](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Vaihe 3: Azure sivuston palauttaminen Providerin asentaminen

4. **Pika-aloitus** -sivulla **valmisteleminen VMM palvelimissa**Valitse **Lataa Microsoft Azure sivuston palauttaminen tarjoajan asennuksen VMM palvelimissa** tarjoajan asennustiedosto uusimman version hankkiminen.

2. Suorita tiedosto jossakin VMM lähdepalvelimeen.

    >[AZURE.NOTE] Jos VMM on otettu käyttöön klusterin ja asennat palvelua ensimmäistä kertaa asentaminen aktiivisen solmun ja viimeistele asennus VMM palvelimen rekisteröidään säilö. Valitse Providerin asentaminen muissa solmuissa. Huomaa, että jos päivität palveluntarjoajan sinun on päivitettävä kaikissa solmuissa, koska ne kaikki suoritetaan samalla palveluntarjoajan versio.

3. Asennusohjelma ei muutaman **Edeltävän vaatimusten tarkistus** ja pyytää käyttöoikeuksia Aloita tarjoajan asennus VMM-palvelun pysäyttäminen. VMM-palvelu käynnistetään automaattisesti, kun asennus on valmis. Jos asennat VMM-klusterin, sinua pyydetään lopettavat klusterin rooli.

4. **Microsoft Update** -voit valita päivitykset. Kun tämä asetus on käytössä tarjoajan päivitykset asennetaan Microsoft Update-käytännön mukaan.

    ![Microsoft-päivitykset](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)

5. Asennuksen sijainnin on määritetty ** <SystemDrive>Files\Microsoft System Center 2012 R2\Virtual tietokoneen Manager\bin**. Valitse Aloita asennus palveluntarjoajan Asenna-painike.

    ![InstallLocation](./media/site-recovery-vmm-to-vmm-classic/install-location.png)

6. Palvelun asentamisen jälkeen valitsemalla **Rekisteröi** rekisteröidä palvelimen säilö.

    ![InstallComplete](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
9. Tarkista nimi, johon palvelimen rekisteröity säilö **säilö nimi**. Valitse *Seuraava*.

    ![Palvelimen rekisteröinti](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. Määritä **Internet-yhteys** miten VMM palvelimessa palvelu on yhteydessä Internetiin. Valitse **Yhdistä aiemmin välityspalvelimen asetusten kanssa** käytettävä oletusarvoinen Internet-yhteyden asetukset määritetty palvelimeen.

    ![Internet-asetukset](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Jos haluat käyttää mukautettua välityspalvelimen olisi määritystä ennen Providerin asentaminen. Kun mukautettu välityspalvelimen asetusten määrittäminen testi toimii tarkistamaan yhteyden välityspalvelimen kautta.
    - Jos käytössäsi on mukautettu välityspalvelimen tai oletusarvo-välityspalvelimen edellyttää käyttöoikeuden tarkistusta, anna välityspalvelimen tiedot, mukaan lukien välityspalvelimen osoite ja portin.
    - Seuraavien URL-osoitteiden on oltava käytettävissä olevat VMM palvelimeen ja Hyper-v-isäntien
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Salli IP-osoitteiden kuvattu [Azure-palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja HTTPS (443)-protokollaa. Sinun on valkoinen luettelo IP-alueita Azure alueen, jota aiot käyttää ja että Länsi US.
    - Jos käytössäsi on mukautettu välityspalvelimen VMM RunAs-tili (DRAProxyAccount) luodaan automaattisesti määritetyn käyttäjätietojen avulla. Määritä välityspalvelin, niin, että tilin voi todentaa onnistuneesti. VMM RunAs Tiliasetukset voi muokata VMM konsolissa. Voit tehdä tämän Avaa **asetukset** -työtila, laajenna **Suojaus**, **Suorita kuin**tilit ja muuta salasana DRAProxyAccount varten. Sinun on VMM-palvelun käynnistäminen uudelleen niin, että tämä asetus tulee voimaan.


8. **Rekisteröinti-näppäintä**Valitse avain Azure palauttaminen ladataan ja kopioi VMM palvelimeen.


10.  Salausasetusta käytetään vain, kun olet replikoiminen Hyper-V VMs-VMM paveikslėlis, Azure. Jos olet replikoiminen toissijainen sivustoon se ei ole käytössä.

11.  Määritä **palvelimen nimi**-tunnistavan säilö VMM-palvelimen nimi. Määritä klusterin kokoonpano VMM klusterin roolinimi.
12.  **Synkronoi cloud metatietojen** Valitse, haluatko VMM palvelimessa kaikki paveikslėlis metatietojen synkronointi säilö. Tämä toiminto on vain tapahtua kerran jokaisessa palvelimessa. Jos et halua synkronoida kaikki paveikslėlis, voit jättää tämän asetuksen valinta ja synkronoida kunkin cloud yksitellen VMM konsolissa cloud-ominaisuudet.

13.  Valitse **Seuraava** loppuun. Metatietojen VMM palvelimesta noutaa Azure palauttaminen rekisteröinnin jälkeen. Palvelimen näkyy **VMM palvelinten** > säilö-**palvelimiin** .

    ![Palvelinten](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Komentorivin asennus

Azure sivuston palauttaminen tarjoajan voidaan asentaa myös komentoriviltä. Tämän menetelmän avulla voidaan Providerin asentaminen Server CORE for Windows Server 2012 R2.

1. Lataa Provider-asennuksen tiedosto- ja rekisteröintitietojen avaimen kansioon. Esimerkki C:\ASR.
2. System Center Virtual Machine Manager-palvelun pysäyttäminen
3. Poimia palveluntarjoajan asennusohjelma suorittamalla seuraavat komennot komentoriviltä, jolla on **järjestelmänvalvojan** oikeudet seuraavasti:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Providerin asentaminen suorittamalla:

        C:\ASR> setupdr.exe /i

5. Rekisteröi palveluntarjoajan suorittamalla:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Jos parametrit ovat:

 - **/Credentials**: pakollinen parametri, joka määrittää sijainti, johon rekisteröinti avaimen tiedosto sijaitsee  
 - **/FriendlyName**: pakollinen parametri, joka näkyy Azure palauttaminen portaalissa Hyper-V Host (isäntä)-palvelimen nimi.
 - **/EncryptionEnabled**: valinnaisten parametrien, joita haluat käyttää vain VMM Azure-skenaario, jos tarvitset oman näennäiskoneiden osoitteessa Azure loput salaamista. Varmista, että annat **.pfx** -tunniste on tiedoston nimi.
 - **/proxyAddress**: valinnainen parametri, joka määrittää välityspalvelimen osoitteen.
 - **/proxyport**: valinnainen parametri, joka määrittää välityspalvelimen porttiin.
 - **/proxyUsername**: valinnainen parametri, joka määrittää välityspalvelimen käyttäjänimi (Jos välityspalvelimen edellyttää käyttöoikeuden tarkistusta).
 - **/proxyPassword**: valinnainen parametri, joka määrittää salasanan todennustapa välityspalvelimeen (Jos välityspalvelimen edellyttää käyttöoikeuden tarkistusta).  

## <a name="step-4-configure-cloud-protection-settings"></a>Vaihe 4: Määritä cloud suojausasetuksia

Kun rekisteröidyt VMM palvelimissa, voit määrittää cloud suojausasetuksia. Jos otit vaihtoehto **Synkronoi cloud tietojasi säilö** palveluntarjoajan asentaessasi niin kaikki paveikslėlis VMM palvelimessa näkyvät säilö **Suojatut osat** -välilehden. Jos sinulla ei voi synkronoida tietyn pilvestä Azure palauttaminen VMM konsolissa cloud ominaisuudet-sivun **Yleiset** -välilehdessä.

![Julkaistun pilveen](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. Napsauta Pika-aloitus-sivulla **VMM paveikslėlis suojauksen määrittäminen**.
2. Valitse **VMM Paveikslėlis** -välilehden pilveen, jonka haluat määrittää ja **määritys** -välilehti.
3. Valitse **kohde**, **VMM**.
4. Valitse **kohdesijainti**, paikalla VMM-palvelin, joka hallitsee pilveen, jota haluat käyttää palauttamista varten.
4. Valitse **kohde cloud**haluat käyttää näennäiskoneiden lähteen pilvipalvelussa automaattisesti kohde pilveen. Huomaa, että:

    - On suositeltavaa, että valitset kohteen pilvestä, joka vastaa palautus näennäiskoneiden suojata.
    - Pilvestä voi kuulua vain yhden cloud pari – ensisijaisen tai kohde pilvestä.

5. **Kopioi korkojakso**, Määritä kuinka usein tiedot synkronoidaan 5he lähde- ja sijaintien välillä. Huomaa, että tämä asetus koskee vain, kun Hyper-V host on käynnissä Windows Server 2012 R2. Käytetään muiden palvelinten viisi minuuttia oletusasetusten määrittäminen.
6. Määritä **palautuksen pistettä**, haluatko luoda palautuksen pistettä. Oletusarvo 0 osoittaa, että vain uusimmat palautuspisteen ensisijainen virtual tietokoneelle on tallennettu replikan palvelin. Huomaa, että ottaminen käyttöön useita palautus pisteiden edellyttää lisätallennustilaa tilannevedoksia, jotka on tallennettu palautus kussakin vaiheessa. Oletusarvon mukaan palautus pisteiden luodaan tunnin välein, niin, että kunkin palautus on tietoja tunti eurolla. Palautus pistearvo, jotka määrität virtuaalikoneen VMM konsolissa ei saa olla pienempi kuin arvo, joka määrittää Azure sivuston palautus-konsolissa.
7. **Sovelluksen yhdenmukaisia tilannevedoksia taajuus**Määritä, kuinka usein sovelluksen yhdenmukaisia tilannevedosten luominen. Hyper-V käyttää kahdenlaisia tilannevedoksia, vakio tilannevedoksen, joka sisältää koko virtuaalikoneen vaiheittainen tilannevedoksen ja sovelluksen yhdenmukaisia tilannevedoksen, joka luo ajankohta tilannevedoksen sovelluksen tietojen virtuaalikoneen sisällä. Sovelluksen yhdenmukaisia tilannevedoksia varmistaa, että sovellukset ovat yhdenmukaisia tilaan tilannevedoksen otettaessa aseman varjostus kopioi Service (VSS) avulla. Huomaa, että jos otat sovelluksen yhdenmukaisia tilannevedoksia, se vaikuttaa lähteen näennäiskoneiden sovellusten suorituskykyä. Varmista, että olet määrittänyt arvo on pienempi kuin määrität palautuksen pisteiden lukumäärä.

    ![Suojauksen asetusten määrittäminen](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)

8. **Tietojen siirtäminen pakkaus**Määritä onko replikoitua tiedot, jotka siirretään pakata.
9. Määritä **todennusta**, miten liikenne todennetaan perus- ja hyödyntämistä Hyper-V Host (isäntä)-palvelinten välillä. Valitse HTTPS, ellei toimii Kerberos-ympäristö on määritetty. Azure palauttaminen määritetään automaattisesti varmenteet HTTPS-todennusta. Ei ole manuaalista määritystä ei tarvita. Jos valitset Kerberos-Kerberos-lippu käytettäviä molemminpuoleista Host (isäntä)-palvelimiin. Oletusarvoisesti Windowsin palomuurin Hyper-V host-palvelimissa avataan portti 8083 ja 8084 (varten-varmenteet). Huomaa, että tämä asetus koskee vain Hyper-V Host (isäntä)-palvelinta Windows Server 2012 R2.
10. Muokkaa **portin**, joina tietokoneiden lähde- ja kuuntele replikoinnin tietoliikenteen porttinumero. Voit esimerkiksi muokata asetus, jos haluat käyttää palvelun laatua (QoS) verkon kaistanleveyden replikoinnin tietoliikenteen rajoitusta. Tarkista, että portti ei ole käyttää toisessa sovelluksessa ja että se on avattu palomuuriasetukset.
11. Määritä **toistoa menetelmä**, kuinka tietolähteen kohde sijainteihin ensimmäisen replikoinnin käsitellään, ennen kuin tavallinen replikoinnin alkaa:

    - **Verkon kautta**, tietojen kopioiminen verkossa voi olla aikaa ja paljon resursseja. On suositeltavaa, että käytät tätä vaihtoehtoa, jos pilveen sisältää näennäiskoneiden suhteellisen pieni virtual kiintolevyillä kanssa ja jos ensisijainen sivusto on yhdistetty toissijaisen sivuston leveä kaistanleveyden-yhteyden kautta. Voit määrittää, että kopion kannattaa aloittaa heti tai valitsemalla aika. Jos käytät verkon replikoinnin, on suositeltavaa, että se ajoittaa myöhemmin.
    - **Offline-tilassa**– Tämä menetelmä määrittää, että ensimmäinen replikointi suoritetaan käyttämällä ulkoisen media. Se on hyödyllinen, jos haluat välttää heikkeneminen verkon suorituskykyyn tai maantieteellisesti remote sijainneista. Tätä menetelmää Määritä Vientisijainti, lähdettä-pilvi ja kohdeosoite cloud tuo-kohtaa. Kun otat virtual machine suojauksen, virtual kovalevy kopioidaan määritetyn Vie haluamaasi kohtaan. Lähettäminen kohdesivuston ja kopioi se tuo haluamaasi kohtaan. Järjestelmä kopioi replikan näennäiskoneiden tuodut tiedot.

12. Valitse **Poista replikan virtuaalikoneen** voit määrittää, että replikan virtuaalikoneen olisi poistetaan, jos lopetat suojaaminen virtuaalikoneen valitsemalla Tunnistepilven ominaisuudet näennäiskoneiden-välilehden **Poista virtuaalikoneen suojaus** -vaihtoehto. Kun tämä asetus on käytössä, kun poistat suojaus virtuaalikoneen poistetaan Azure palauttaminen virtuaalikoneen palauttaminen asetusten poistetaan VMM-konsolin ja se poistetaan.

    ![Suojauksen asetusten määrittäminen](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

Kun olet tallentanut asetukset työn luodaan ja voidaan valvoa **työt** -välilehden. Kaikki Hyper-V Host (isäntä)-palvelimet VMM lähde pilvipalvelussa määritetään replikoinnin. Cloud-asetuksia voi muokata **määrittäminen** -välilehdessä. Jos haluat muokata kohteen sijainnin tai kohde pilveen Poista Tunnistepilven määritykset ja määritä pilveen.

### <a name="prepare-for-offline-initial-replication"></a>Offline-tilassa ensimmäisen replikoinnin valmisteleminen

Sinun on suoritettava valmisteleminen ensimmäisen replikoinnin offline-tilassa seuraavat toimet:

- Lähde-palvelimeen Määritä polku-sijaintiin, josta tietojen viennin tapahtuu. Määritä täydet oikeudet NTFS ja jakaminen käyttöoikeuksien Vie polulla VMM-palveluun. Kohde-palvelimeen Määritä polku-sijaintiin, josta tietojen tuominen tapahtuu. Määritä samat käyttöoikeudet tuonti-polun.
- Jos tuonti- tai polku on jaettu, määrittää järjestelmänvalvojan, tehokäyttäjän, Tulosta-operaattoria tai palvelimen ylläpitäjä Ryhmäjäsenyyden VMM palvelutilin etätietokoneessa, jossa jaetun sijaitsee.
- Jos käytössäsi on Suorita nimellä kaikki tilit, voit lisätä isännät-tuonti ja vienti polut, määrittää luku- ja kirjoitusoikeudet VMM Suorita nimellä-tilit.
- Tuo ja vie osakkeet olisi ei löydy millä tahansa tietokoneella käytetään Hyper-V palvelin, koska silmukan määritys ei tue Hyper-V.
- Active Directory-kunkin Hyper-V Host (isäntä)-palvelimeen, joka sisältää näennäiskoneiden, jonka haluat suojata, ottaminen käyttöön ja määrittäminen Rajoitettu delegointi luotetaanko etätietokoneiden, joina tuonti ja vienti polut sijaitsevat, seuraavasti:
    1. Toimialueen ohjauskoneen Avaa **Active Directoryn käyttäjät ja tietokoneet**.
    2. Valitse konsolipuussa **toimialuenimi** > **tietokoneissa**.
    3. Hyper-V host palvelimen nimeä hiiren kakkospainikkeella > **Ominaisuudet**.
    4. Valitse **delegointi** -välilehdessä T**rust tämän tietokoneen delegoinnin vain määritetyn palveluihin**.
    5. Napsauta **mitä tahansa todennus-protokollaa**.
    6. Valitse **Lisää** > **käyttäjät ja tietokoneet**.
    7. Tietokone, jossa vientipolku nimi > **OK**. Käytettävissä olevien palveluiden luettelosta, pidä CTRL-näppäintä painettuna ja valitse **cifs** > **OK**. Toista tietokone, jossa Tuontipolku nimi. Toista tarvittaessa muita Hyper-V host-palvelimissa.

## <a name="step-5-configure-network-mapping"></a>Vaihe 5: Määritä verkon määritys
1. Napsauta Pika-aloitus-sivulla **Yhdistä verkot**.
2. Valitse, joista haluat yhdistää verkkojen VMM lähdepalvelin ja valitse sitten kohde VMM palvelin, johon verkkojen yhdistetään. Lähde-verkkojen ja niiden liittyvä kohde verkkojen luettelo näytetään. Tyhjä arvo näkyy verkkojen, joka ei ole määritetty.
3. Valitse verkko- **verkon lähde-** > **kartta**. Palvelu tunnistaa AM verkkojen kohde-palvelimeen, ja näyttää ne. Valitse tiedot-kuvaketta, voit tarkastella kunkin verkon aliverkosta lähde- ja verkko-nimen vieressä.

    ![Määritä verkon määritys](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)

4. -Valintaikkunassa valitse yksi AM verkostojen VMM palvelimesta.

    ![Valitse kohde verkko](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)

5. Kun valitset kohde verkkoon, suojatun paveikslėlis, jotka käyttävät lähde-verkon näytetään. Käytettävissä kohde-verkoissa, jotka liittyvät suojaus käytettäviä paveikslėlis näkyvät myös. On suositeltavaa, että valitset kohteen verkkoon, joka on käytettävissä kaikkien käytät suojaa paveikslėlis. Tai voit siirtyä VMM palvelimeen ja lisää vastaavan AM verkkoon, jonka haluat valita looginen verkko cloud-ominaisuuksien muokkaaminen.
6. Valitse Viimeistele yhdistäminen valintamerkkiä. Työn käynnistää yhdistämisen edistymisen seuranta. Voit tarkastella sitä **työt** -välilehden.


## <a name="step-6-configure-storage-mapping"></a>Vaihe 6: Määritä tallennustilan yhdistäminen
Oletusarvon mukaan lähde Hyper-V palvelin virtual tietokoneen replikoinnin kohde Hyper-V Host (isäntä)-palvelimeen, replikoitua tiedot on tallennettu oletussijainti, joka on merkitty kohde Hyper-V isännän Hyper-V hallinnassa. Jos haluat hallita replikoitua tietojen tallennussijainti, voit määrittää tallennustilan yhdistämismääritykset seuraavasti:



1. Määritä tallennustilan luokitukset lähde- ja kohdesivustojen VMM-palvelimiin. [Lue lisää](https://technet.microsoft.com/library/gg610685.aspx). Luokitusten on oltava käytettävissä lähde- ja paveikslėlis Hyper-V host-palvelimissa. Luokitusten ei tarvitse samantyyppisten lisätallennustilaa. Voit esimerkiksi yhdistää lähde-luokittelu, joka sisältää kohteen luokitukseen, joka sisältää CSVs SMB osakkeet.
2. Kun luokitukset ovat paikallaan voit luoda määritykset. Voit tehdä tämän sivun **Pika-aloitus** > **Yhdistä tallennustilan**.
3. **Tallennustilan** -välilehti > **Yhdistä tallennustilan luokitukset**.
4. **Yhdistä tallennustilan luokitukset** -välilehdessä Valitse lähteen luokitukset ja kohdistaa VMM-palvelimiin. Tallenna asetukset.

    ![Valitse kohde verkko](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)


## <a name="step-7-enable-virtual-machine-protection"></a>Vaihe 7: Ota käyttöön virtuaalikoneen suojaus
Kun palvelimissa, paveikslėlis ja käyttäminen on määritetty oikein, voit ottaa suojauksen näennäiskoneiden pilveen.

1. Valitse pilveen, jossa virtuaalikoneen sijaitsee **näennäiskoneiden** -välilehdessä **Ota suojaus käyttöön** > **Lisää näennäiskoneiden**.
2. Valitse haluamasi suojaa näennäiskoneiden pilveen-luettelosta.

    ![Virtuaalikoneen suojauksen ottaminen käyttöön](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)

3. **Työt** -välilehti, mukaan lukien ensimmäisen replikoinnin Salli suojaus-toiminnon edistymisen seuraaminen. Kun viimeisteleminen suojaus suoritetaan virtuaalikoneen on valmiina automaattisesti. Kun suojaus on käytössä ja näennäiskoneiden on replikoida, voit voi tarkastella niitä Azure.

    ![Virtuaalikoneen suojaus työ](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

>[AZURE.NOTE] Voit myös ottaa suojauksen näennäiskoneiden VMM konsolissa. Valitse työkaluriviltä virtuaalikoneen ominaisuuksiin **Azure sivuston palautus** -välilehdessä **Salli suojaus** .

### <a name="on-board-existing-virtual-machines"></a>Junassa näennäiskoneiden

Jos sinulla on aiemmin näennäiskoneiden VMM-Hyper-V replikaan replikoiminen, sinun on määrän ne Azure palauttaminen suojautumista seuraavasti:

1. Tarkista, sinulla on ensisijainen ja toissijainen paveikslėlis. Varmista Hyper-V isäntäpalvelimen aiemmin virtuaalikoneen sijaitsee ensisijainen pilveen ja Hyper-V isäntäpalvelimen replikan virtuaalikoneen sijaitsee toissijainen pilveen. Varmista, että olet määrittänyt paveikslėlis suojausasetuksia. Asetukset on vastattava määritetyt Hyper-V replikan. Muussa tapauksessa virtuaalikoneen replikointi eivät ehkä toimi oikein.
2. Valitse Ota käyttöön ensisijainen virtuaalikoneen suojaus. Azure palauttaminen ja VMM varmistavat saman replikan isännän ja virtuaalikoneen havaitaan ja Azure palauttaminen uudelleen ja muodostaa replikoinnin aikana Tunnistepilven määritykset määritetty asetusten avulla.


## <a name="test-your-deployment"></a>Testata käyttöönoton

Jos haluat testata käyttöönoton voit suorittaa testi-automaattisesti virtual yhdelle tietokoneelle, tai useita näennäiskoneiden koostuva palautus suunnitelman luominen ja Suorita testi-automaattisesti palvelupaketin.  Testaa automaattisesti varmistaminen automaattisesti ja palautus-järjestelmä eristetyssä verkossa.

### <a name="create-a-recovery-plan"></a>Suunnittele palauttaminen

1. Valitse **Palautus suunnitelmien** -välilehdessä **Luo palautus suunnitteleminen**.
2. Määritä nimi palautus suunnitelma ja lähde- ja VMM-palvelimiin. Lähdepalvelimeen on oltava näennäiskoneiden, joissa on otettu käyttöön automaattisesti ja palauttaminen. Valitse Näytä vain paveikslėlis, jotka on määritetty Hyper-V replikoinnin **Hyper-V** .

    ![Luo suunnitelma palauttaminen](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)

3. Valitse **Valitse virtuaalikoneen**replikoinnin ryhmät. Kaikki replikoinnin ryhmään liittyvien näennäiskoneiden valittuna ja lisätään palautus-suunnitelma. Nämä näennäiskoneiden on lisätty palautus suunnitelman oletusryhmään, ryhmä 1. Voit tarvittaessa lisätä Lisää ryhmiä. Huomaa, että kun replikoinnin näennäiskoneiden alkaa palautus suunnitteluryhmät järjestyksen mukaisesti.

    ![Lisää näennäiskoneiden](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

Palautuksen jälkeen palvelupaketti on luotu, se näkyy **Palautus suunnitelmien** -välilehteen.

###<a name="run-a-test-failover"></a>Suorita testi automaattisesti

1. **Suunnitelmien palautus** -välilehdessä palvelupaketti, ja valitse **Testaa automaattisesti**.
2. **Vahvista testi automaattisesti** sivulla Valitse **ei mitään**. Huomaa, että tämä vaihtoehto on käytössä epäonnistunutta replikan näennäiskoneiden päälle ei liitettävä minkä tahansa verkkoon. Tämä testaa virtuaalikoneen siirtyy odotetulla tavalla, mutta saa testata replikoinnin Verkkoympäristössä. Katso lisätietoja käyttämisestä eri asetukset [Suorita testi automaattisesti](site-recovery-failover.md#run-a-test-failover) .
3. Testaa virtuaalikoneen luodaan samaan isännän kuin isäntä, jossa replikan virtuaalikoneen on olemassa. Se lisätään samaan pilveen, jossa replikan virtuaalikoneen sijaitsee.

### <a name="run-a-recovery-plan"></a>Suorita palautus-suunnitelma
Replikoinnin jälkeen replikan virtuaalikoneen ei ehkä ole IP-osoite, joka on sama kuin ensisijaisen virtuaalikoneen IP-osoite. Näennäiskoneiden päivittää DNS-palvelimeen, joita hän käyttää sen jälkeen, kun he alkavat. Voit myös lisätä komentosarjan, joka päivittää varmistamiseksi ajoissa update DNS-palvelimeen.

#### <a name="script-to-retrieve-the-ip-address"></a>Komentosarjan, joka noutaa IP-osoite
Suorittamalla tämä esimerkki noutaa IP-osoite.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-to-update-dns"></a>Päivitä DNS-komentosarja
Suorittamalla tämän otoksen Päivitä DNS-IP-osoite, voit hakea käyttämällä Edellinen otoksen komentosarja, joka määrittää.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>Sivuston palauttaminen tietosuojatiedot

Tässä osassa on tietosuojaa tietoja Microsoft Azure sivuston palauttaminen-palvelun ("palvelu"). Microsoft Azure-palveluiden tietosuojatiedot on artikkelissa [Microsoft Azure-tietosuojatiedot](http://go.microsoft.com/fwlink/?LinkId=324899)

**Toiminto: rekisteröinti**

- **Kuvaus**: Rekisteröi server-palvelun kanssa, niin, että näennäiskoneiden voidaan suojata
- **Tietojen kerääminen**: kun rekisteröiminen palvelun kerää käsittelee ja välittää VMM palvelimeen, joka on nimennyt tarjoamaan käyttämällä palvelun VMM-palvelimen nimen ja virtuaalikoneen paveikslėlis nimi VMM palvelimeen palauttaminen hallinta varmenteen tiedot.
- **Tietojen käyttäminen**:
    - Hallinta-varmenteen — käytetään tunnistamista ja todennus käyttämään palvelua rekisteröity VMM palvelimeen. Palvelun käyttää varmenteen avaimen julkisen osan suojaaminen tunnuksen, vain rekisteröity VMM palvelin voivat käyttää. Palvelin on käytettävä Service ominaisuudet tunnuksessa.
    - VMM palvelimen nimi – VMM palvelimen nimi on pakollinen tunnistaa ja yhteydenpito tarvittavat VMM-palvelimeen, joka paveikslėlis sijaitsevat.
    - Cloud nimien VMM – cloud nimi on pakollinen käytettäessä palvelun pilveen laiteparin/unpairing ominaisuus seuraavalla tavalla. Kun päätät pariliitos oman cloud ensisijaiseksi Centeristä toiseen cloud palautus tietokeskuksen kanssa, kaikki palauttamisen tietojen Centeristä paveikslėlis nimet esitetään.

- **Valinta**: nämä tiedot on olennainen osa palvelun rekisteröintiprosessi, sillä se auttaa sinua ja palvelu tunnistaa, jonka haluat antaa Azure palauttaminen suojaus-sekä siitä, että se tunnistaa oikea rekisteröity VMM palvelimen VMM palvelimen. Jos et halua lähettää tiedot-palveluun, älä käytä palvelun. Jos rekisteröidä palvelimellesi ja sitten haluat myöhemmin poistaa sen, voit tehdä poistamalla VMM palvelimen tiedot Service-portaalista (joka on Azure portaalin).

**Ominaisuus: Ota käyttöön Azure palauttaminen suojaus**

- **Kuvaus**: Azure sivuston palauttaminen tarjoajan asennettu VMM-palvelimeen on siirtokanava viestinnän vakiomenettelyt-palvelussa. Palvelu on ylläpidettävä VMM prosessin dll-kirjaston (DLL). Kun palvelu on asennettu, "palvelinkeskuksen" palautustoiminto saa käyttöön VMM järjestelmänvalvojan konsolissa. Mikä tahansa pilvestä uuteen tai aiemmin luotuun näennäiskoneiden ottaa ominaisuus nimeltä "Palvelinkeskuksen palautus" virtuaalikoneen suojautua. Kun tämä ominaisuus on määritetty, palveluntarjoajan lähettää nimi ja virtuaalikoneen tunnus-palveluun. Virtuaalinen suojaus on käytössä, Windows Server 2012: ssa tai Windows Server 2012 R2 Hyper-V replikoinnin tekniikka. Virtuaalikoneen tiedot saa replikoida isännästä Hyper-V toiseen (yleensä sijaitsevat eri "palautus" tietokeskuksen).

- **Tietojen kerääminen**: palvelun kerää, käsittelee ja välittää metatiedot virtuaalikoneen, joka sisältää nimen, tunnus, VPN ja pilveen nimi, johon se kuuluu.

- **Tietojen käyttäminen**: palvelu käyttää edellä mainittujen tietojen täyttämiseen virtuaalikoneen tiedot Service-portaalissa.

- **Valinta**: Tämä on olennainen osa palvelun ja voi poistaa käytöstä. Jos et halua lähettää palvelun tiedot, älä ota käyttöön kaikki näennäiskoneiden Azure palauttaminen suojaus. Huomaa, että kaikki tiedot lähetetään tarjoajan palveluun lähetetään HTTPS-protokollan välityksellä.

**Toiminto: Palautus suunnitelma**

- **Kuvaus**: tämän ominaisuuden avulla voit luoda "palautus" tietokeskuksen tiedonsiirron-suunnitteleminen. Voit määrittää järjestyksen, jossa näennäiskoneiden tai ryhmän näennäiskoneiden on käynnistettävä palautus-sivustossa. Voit myös määrittää automaattisia komentosarjat on otettava palautus milloin kunkin virtuaalikoneen suoritetaan tai mitä tahansa Manuaalinen toiminto. Automaattisesti (kattaa seuraavan osion) käynnistyy yleensä palautus suunnitteleminen tasolla koordinoidun palauttamista varten.

- **Tietojen kerääminen**: Service kerää, käsittelee ja välittää palautus-palvelupakettia, mukaan lukien virtuaalikoneen metatietojen ja metatietojen Automaatiokomentosarjojen ja Manuaalinen toiminto muistiinpanot-metatiedot.

- **Tietojen käyttäminen**: edellä kuvatun metatietoja käytetään luonnissa palautus suunnitelman Service-portaalissa.

- **Valinta**: Tämä on olennainen osa palvelun ja voi poistaa käytöstä. Jos et halua, että nämä tiedot lähetetään palveluun, ei luoda palautus suunnitelmien tämä palvelu.

**Ominaisuus: Verkon määritys**

- **Kuvaus**: tämän ominaisuuden avulla voit yhdistää tiedot verkon ensisijainen tietokeskuksen palauttamisen tietojen käyttöoikeudet. Kun näennäiskoneiden on palautettu palautus-sivustossa, yhdistämisen avulla vahvistamiseksi verkkoyhteyden niiden.

- **Tietojen kerääminen**: osana verkon määritys-ominaisuus palvelun kerää käsittelee ja välittää metatiedot looginen verkostojen kunkin sivuston (ensisijainen ja palvelinkeskuksen).

- **Tietojen käyttäminen**: palvelu käyttää metatiedot täytä Service-portaalin kohtaa, johon voit yhdistää verkkotiedot.

- **Valinta**: Tämä on olennainen osa palvelun ja voi poistaa käytöstä. Jos et halua lähettää palvelun tiedot, älä käytä verkon määritys-ominaisuus.

**Ominaisuus: Automaattisesti - suunniteltujen, suunnittelematon-testi**

- **Kuvaus**: tämän ominaisuuden avulla automaattisesti virtual koneen yhtä hallittuja VMM tietokeskuksen kohteesta toiseen VMM hallitun tietokeskuksen. Automaattisesti-toiminto käynnistyy, kun käyttäjä niiden Service-portaalissa. Mahdollisia syitä automaattisesti ovat suunnittelematon tapahtuman (esimerkiksi luonnollinen disaster0; kyseessä suunnitellun tapahtuma (esimerkiksi palvelinkeskuksen kuormituksen); testi automaattisesti (esimerkiksi palautus suunnitelman Harjoitus).

VMM palvelimessa tarjoaja saavat ilmoituksen tapahtuman-palvelusta ja suorittaa automaattisesti toiminnon Hyper-V isännän VMM liittymät kautta. Windows Server 2012: ssa tai Windows Server 2012 R2 Hyper-V replikoinnin-tekniikka käsittelee virtuaalikoneen isännästä Hyper-V toiseen (yleensä käynnissä eri "palautus" tietokeskuksen) todellinen automaattisesti. Vikasietotilaa päätyttyä "palautus" tietokeskuksen VMM-palvelimessa asennettuna tarjoajan lähettää success tietoja palvelu.

- **Tietojen kerääminen**: palvelu käyttää edellä mainittujen tietojen täyttäminen automaattisesti toiminnon tiedot-palvelun portaalin tila.

- **Tietojen käyttäminen**: Service käyttää edellä mainittujen tietojen seuraavalla tavalla:

    - Hallinta-varmenteen — käytetään tunnistamista ja todennus käyttämään palvelua rekisteröity VMM palvelimeen. Palvelun käyttää varmenteen avaimen julkisen osan suojaaminen tunnuksen, vain rekisteröity VMM palvelin voivat käyttää. Palvelin on käytettävä Service ominaisuudet tunnuksessa.
    - VMM palvelimen nimi – VMM palvelimen nimi on pakollinen tunnistaa ja yhteydenpito tarvittavat VMM-palvelimeen, joka paveikslėlis sijaitsevat.
    - Cloud nimien VMM – cloud nimi on pakollinen käytettäessä palvelun pilveen laiteparin/unpairing ominaisuus seuraavalla tavalla. Kun päätät pariliitos oman cloud ensisijaiseksi Centeristä toiseen cloud palautus tietokeskuksen kanssa, kaikki palauttamisen tietojen Centeristä paveikslėlis nimet esitetään.

- **Valinta**: Tämä on olennainen osa palvelun ja voi poistaa käytöstä. Jos et halua lähettää palvelun tiedot, älä käytä palvelun.

## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet suorittanut testi-automaattisesti Tarkista ympäristön toimii oikein, [tietoja](site-recovery-failover.md) eri failovers tyyppiä.
