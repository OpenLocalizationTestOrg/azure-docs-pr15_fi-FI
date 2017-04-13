<properties
    pageTitle="Hyper-V kapasiteetin suunnittelu-työkalun suorittaminen sivuston hyödynnettäviksi | Microsoft Azure"
    description="Tämä artikkeli sisältää ohjeet Hyper-V kapasiteetin suunnittelutyökalua käytön Azure sivuston palauttaminen"
    services="site-recovery"
    documentationCenter="na"
    authors="rayne-wiselman"
    manager="jwhit"
    editor="" />
<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/12/2016"
    ms.author="raynew" />

# <a name="run-the-hyper-v-capacity-planner-tool-for-site-recovery"></a>Suorita Hyper-V kapasiteetin suunnittelutyökalua sivuston palauttaminen

Tarvitset Azure sivustojen palauttamisen käyttöönotto osana replikointi ja lyncserver2010 laskemisesta. Sivuston palauttaminen Hyper-V kapasiteetin suunnittelu-työkalun avulla voit Hyper-V virtuaalikoneen replikoinnin replikointi ja kaistanleveyden tarpeen selvittäminen.


Tässä artikkelissa käsitellään Hyper-V kapasiteetin suunnittelu-työkalua. Tämä työkalu tulisi käyttää sivustoa kapasiteetin suunnittelutyökalujen ja kuvatut [kapasiteetin suunnitteleminen palauttaminen](site-recovery-capacity-planner.md)tiedot.


## <a name="before-you-start"></a>Ennen aloittamista

Kun suoritat työkalun Hyper-V server- tai klusterin solmun ensisijainen sivuston. Työkalun suorittaminen edellyttää Hyper-V host-palvelimissa:

- Käyttöjärjestelmä: Windows Server® 2012: n tai Windows Server® 2012 R2
- Muisti: 20 Megatavun (vähintään)
- Suoritin: 5 prosentin katseltavan (vähintään)
- Levytila: 5 Megatavua (vähintään)

Ennen kuin suoritat työkalun sinun on ensisijainen Työmaan. Jos olet replikoiminen välillä kaksi paikallisen sivustot ja haluat tarkistaa kaistanleveyden, sinun on valmisteleminen replikan-palvelimeen.


## <a name="step-1-prepare-the-primary-site"></a>Vaihe 1: Ensisijainen Työmaan
1. Tee ensisijainen sivuston luettelo kaikista Hyper-V näennäiskoneiden, jonka haluat toistaa ja Hyper-V isännät/varausyksiköiden, ne sijaitsevat. Työkalu suorittaa aina, kun useita erillinen isännät- tai yksittäisen klusterin mutta ei molempia yhdessä. Se myös on suoritettava erikseen jokaiselle käyttöjärjestelmälle olisi kerää ja Huomaa Hyper-V-palvelimet seuraavasti:

  - Windows Server® 2012 erillisestä palvelinten
  - Windows Server® 2012 klustereiden
  - Windows Server® 2012 R2 erillisestä palvelinten
  - Windows Server® 2012 R2 klustereiden

3. Ota käyttöön kaikissa Hyper-V isännät-ja klustereiden WMI etäkäyttö. Suorita tämä komento kunkin palvelinklusterin Varmista, että palomuurisäännöt ja käyttöoikeudet on määritetty:

        netsh firewall set service RemoteAdmin enable

5. Ota käyttöön suorituskyvyn seurantaa palvelimia ja klustereiden, seuraavasti:

  - Avaa Windowsin palomuuri **Laajennettu** -laajennus ja valitse Ota käyttöön seuraavat saapuvan liikenteen säännöt: **COM + verkkokäyttö (DCOM IN)** ja kaikki **etähallinnan tapahtumalokin ryhmässä**säännöt.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Vaihe 2: Valmistele replikan server (paikallisen paikallisen replikointi)

Ei tarvitse tehtävä tämä, jos olet replikoiminen Azure avulla.

Suosittelemme, että olet määrittänyt yhden Hyper-V host palautus palvelimeksi niin, että se tarkistaa kaistanleveyden voi replikoida tyhjä AM.  Voit ohittaa tämän, mutta et voi mitata kaistanleveyden, ellei niitä.

1. Jos haluat käyttää klusterisolmu se määrittää Hyper-V replikan broker:

    - **Palvelimen hallinta**ja Avaa **Automaattisesti klusterin hallinta**.
    - Yhteyden muodostaminen klusterin, korosta klusterinimeä ja valitse **Toiminnot** > **Määritä rooli** ja Avaa Ohjattu suuren käytettävyyden.
    - Valitse **Valitse roolin** **Hyper-V replikan Broker**. Ohjatun toiminnon Anna **NetBIOS nimi** ja **IP-osoite** , jota käytetään (jota kutsutaan asiakkaan tukiasemaa) klusterin yhteyspistettä. **Hyper-V replikan Broker** määritetään, tuloksena on client access kohdan nimi, jonka Huomaa.
    - Varmista, että Hyper-V replikan Broker rooli kirjautuu onnistuneesti ja voi epäonnistua päälle, kaikki solmujen klusterin välillä. Voit tehdä tämän roolin napsauttamalla hiiren kakkospainikkeella, osoita **Siirrä**ja valitse sitten **Valitse solmu**. Valitse solmu > **OK**.
    - Jos käytät varmenteen, varmista, että kukin klusterisolmu ja tukiasemaa kaikilla on asennettu sertifikaatti asiakkaan.
2.  Ota käyttöön replikan-palvelimeen:

    - Klusterin Avaa virheen klusterin hallinta, klusterin yhdistäminen ja valitse **roolit** > Valitse rooli > **Replikoinnin**asetukset > **Ota tämän klusterin replikan palvelimeksi**. Huomaa, että jos käytät klusterin kuin se sinun on oltava esitä Hyper-V replikan Broker roolin sekä ensisijaisessa klusterin.
    - Yksittäisen palvelimen Avaa Hyper-V hallinta. Valitse **Toiminnot** -ruudusta haluat ottaa käyttöön palvelimen **Hyper-V asetukset** ja **Replikoinnin** määrityksessä Valitse **Ota käyttöön tämän tietokoneen replikan palvelimeksi**.
3. Todentamisen määrittäminen:

    - Valitse **todennus ja portit** miten tarkistamiseen ensisijainen palvelin ja todennus-portit. Jos käytät varmenteen valitsemalla **Valitse varmenne** , voit valita yhden. Käytä Kerberos, jos ensisijainen ja hyödyntämistä Hyper-V isännät eivät samaa toimialuetta tai luotettuja toimialueita. Käytä varmenteita eri toimialueen tai työryhmän käyttöönotto.
    - **Todennus ja tallennustilaa** -osasta Salli **todennetut (perus) palvelimen replikoinnin tietojen lähettäminen replikan tähän palvelimeen** . Valitse **OK** tai **Käytä**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

    - Voit tarkistaa, että määrittämäsi protokolla/portti suoritetaan kuuntelua **netsh http Näytä servicestate** suorittaminen  
4. Määritä palomuurit. Hyper-V asennuksen aikana palomuurisäännöt luodaan sallimaan oletusarvoiseen tietoliikenteen portit (HTTPS 443, Kerberos-80). Ota seuraavat säännöt seuraavasti:

        - Certificate authentication on cluster (443): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}**
        - Kerberos authentication on cluster (80): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}**
        - Certificate authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"**
        - Kerberos authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"**

## <a name="step-3-run-the-capacity-planner-tool"></a>Vaihe 3: Kapasiteetin suunnittelu-työkalun suorittaminen

Kun olet valmistellut ensisijainen sivuston ja palautus-palvelimen määrittäminen voit suorittaa työkalu.

1. [Lataa](https://www.microsoft.com/download/details.aspx?id=39057) Microsoft Download Centeristä-työkalu.
2. Suorita työkalu olevista ensisijaiset palvelimet (tai solmujen ensisijainen klusterista tunnus). .Exe-tiedostoa hiiren kakkospainikkeella ja valitse sitten **Suorita järjestelmänvalvojana**.
3. **Ennen aloittamista** : Määritä, kuinka kauan haluat kerätä tietoja. Suosittelemme, että suoritat työkalun varmistaa, että tiedot ovat edustajan tuotannon tunnin aikana. Jos yrität vain Vahvista verkkoyhteyden, voit kerätä vain minuutti.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. **Ensisijainen** lisätiedot Määritä palvelimen nimi tai FQDN erillinen isännän tai klusterin määrittää asiakkaan FQDN Hyväksy piste, klusterinimi tai minkä tahansa klusterin solmu ja napsauta sitten **Seuraava**. Työkalu tunnistaa automaattisesti-palvelimen nimi. Työkalu vastataan VMs, joka voidaan valvoa määritetyn palvelimia.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. **Replikan sivuston tiedot** Jos olet replikoiminen Azure, tai jos olet replikoiminen toissijaisen palvelinkeskukseen ja ole vielä määrittänyt replikan palvelimeen, valitse **Ohita testien henkilöihin liittyvät replikan sivuston**. Jos ovat replikoiminen toissijainen palvelinkeskukseen ja olet määrittänyt Replikalaji FQDN-yksittäiseen palvelimeen tai klusterin- **palvelimen nimen (**tai) Hyper-V replikan Broker pää client access-kohta.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. **Laajennettu replikan** lisätiedot ottaa käyttöön **ohittaa henkilöihin liittyvät laajennettu replikan sivuston testejä**. Ne eivät tue palauttaminen.
7. **Toisinto Valitse VMs** työkaluja muodostaa yhteyden palvelimeen tai klusterin ja näyttää VMs ja ensisijainen palvelimessa levyjen asetusten mukaisesti määrittämäsi **Ensisijainen sivuston tiedot** -sivulla. Huomaa, että VMs, jotka ovat jo käytössä replikoinnin tai jotka eivät ole käytössä, ei tule näkyviin. Valitse VMs, jonka haluat kerätä arvot. Valitsemalla näennäiskiintolevyt automaattisesti kerää tietoja varten VMs liian.
9. Jos olet määrittänyt replikan palvelimeen tai klusterin, **verkkotiedot** Määritä lähes WAN-kaistanleveyden mielestäsi käytetään perus- ja replikan-sivustoissa ja valitse varmenteet, jos olet määrittänyt todennus.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

10. **Yhteenveto** -asetusten tarkistaminen ja Aloita valitsemalla **Seuraava** kerääminen arvot. **Laske kapasiteetti** -sivulla näkyy työkalun edistymistä ja tilaa. Kun käynnissä työkalu lopettaa valitsemalla **Näytä raportti** Siirry tulosteen päälle. Oletusarvoisesti raportteja ja lokit tallennetaan **%systemdrive%\Users\Public\Documents\Capacity suunnittelu**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)


## <a name="step-4-interpret-the-results"></a>Vaihe 4: N tulkitseminen tulokset
Seuraavassa on tärkeää arvot. Voit ohittaa arvot, joka ei ole mainittu tässä. Ne eivät asiaa sivuston palauttamista varten.

### <a name="on-premises-to-on-premises-replication"></a>Paikalliseen paikallisen replikointi
  - Vaikutus replikoinnin ensisijainen host Laske-muistia
  - Perus-palautus isännät tallennustilan levytilaa IOPS replikoinnin vaikutus
  - Yhteensä kaistanleveyden vaaditaan replikoinnin delta (Mbps)
  - Havaitut kaistanleveys ensisijainen isännän ja palautus host (Mbps) välillä
  - Ihanteellinen määrän kaksi isännät/klustereiden väliset aktiivinen rinnakkain siirrot ehdotus

### <a name="on-premises-to-azure-replication"></a>Paikalliseen Azure replikointi
  - Vaikutus replikoinnin ensisijainen host Laske-muistia
  - Replikoinnin ensisijainen host tallennustilan levytila, IOPS vaikutus
  - Yhteensä kaistanleveyden vaaditaan replikoinnin delta (Mbps)

## <a name="more-resources"></a>Lisää resursseja

- Lisätietoja työkalun lukea tiedoston mukana toimitettava työkalun lataus.
- Katso työkalun Keijo Mayer [TechNet-blogi](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx)vaiheista.
- Tutustu suorituskyvyn testaaminen paikalliset paikallisen Hyper-V replikointi [tulokset](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md)



## <a name="next-steps"></a>Seuraavat vaiheet

Kun lopetat kapasiteetin suunnittelua voit aloittaa sivuston palauttamisen käyttöönotto:

- [Replikoida Azure-VMM paveikslėlis VMs Hyper-V](site-recovery-vmm-to-azure.md)
- [Replikoida Azure Hyper-V VMs (ilman VMM)](site-recovery-hyper-v-site-to-azure.md)
- [Toistaa Hyper-V VMs VMM sivustosta toiseen](site-recovery-vmm-to-vmm.md)
- [Toistaa Hyper-V VMs SAN VMM sivustojen välillä](site-recovery-vmm-san.md)
- [Toistaa hyper-V VMs yhteen VMM palvelimeen](site-recovery-single-vmm.md)