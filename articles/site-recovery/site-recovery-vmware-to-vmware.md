<properties
    pageTitle="Toisinto paikallisen VMware näennäiskoneiden tai toissijaisen sivuston fyysinen palvelinten | Microsoft Azure"
    description="Tämän artikkelin avulla voit toistaa VMware VMs tai Windows-tai Linux fyysiset palvelimet toissijainen sivustoon Azure palauttaminen."
    services="site-recovery"
    documentationCenter=""
    authors="nsoneji"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="nisoneji"/>


# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Toistaa paikallisen VMware näennäiskoneiden tai fyysinen palvelimia toissijainen sivustoon


## <a name="overview"></a>Yleiskatsaus

InMage Scout-Azure palauttaminen on reaaliaikainen replikoinnin paikallisen VMware sivustojen välillä. InMage Scout sisältyy Azure palauttaminen palvelun tilaukset.


## <a name="prerequisites"></a>Edellytykset

**Azure-tili**: tarvitaan [Microsoft Azure](https://azure.microsoft.com/) -tili. Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/). [Lue lisää](https://azure.microsoft.com/pricing/details/site-recovery/) siitä, että sivuston palauttaminen hinnat.


## <a name="step-1-create-a-vault"></a>Vaihe 1: Luo säilöön
1. Kirjautuminen [Azure portal](https://portal.azure.com).
2. Valitse Uusi > hallinta > varmuuskopiointi ja palauttaminen (OMS). Vaihtoehtoisesti voit napsauttaa Selaa > palautus Services säilö > Lisää.
3. Määritä kutsumanimi tunnistavan säilö **nimi** . Jos sinulla on useita tilauksia, valitse jonkin niistä.
4. **Resurssiryhmä** -Luo uusi resurssiryhmä tai valitse olemassa. Määritä Azure alue, suorittamiseen tarvittavat kentät. 
5.  Valitse **sijainti**-säilö maantieteellinen alue. Tarkista tuettujen alueiden, on artikkelissa [Azure sivuston palauttaminen hinnat](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Jos haluat nopeasti käyttää koontinäytöstä säilö sitten Kiinnitä Raporttinäkymät-ikkunan ja valitse sitten Luo.
6. Uusi säilö näkyvät koontinäyttö > kaikki resurssit ja sisällön palauttaminen Services vaults sivu.

## <a name="step-2-configure-the-vault-and-download-inmage-scout-components"></a>Vaihe 2: Määritä säilö ja lataa InMage Scout osat
7. Palautus-palveluissa vaults sivu oman säilö valitsemalla ja sitten asetukset.
8. **Asetuksissa** > **Aloittaminen** Valitse **Sivuston palauttaminen** > Vaihe 1: **Valmisteleminen infrastruktuurin** > **Suojaus tavoite**.
9. Valitse **Suojaus-tavoite** palautus-sivustoon ja valitse Kyllä, jossa VMware vSphere Hypervisor. Valitse OK.
10. **Scout asetukset**Valitse Lataa voit ladata InMage Scout 8.0.1 GA ohjelmiston ja rekisteröintiä avain. Kaikki tarvittavat osat asennustiedostot ovat ladatun .zip-tiedosto.


## <a name="step-3-install-component-updates"></a>Vaihe 3: Osan päivitysten asentaminen

Lue uusimmat [päivitykset](#updates). Asentaa palvelimissa update-tiedostot seuraavassa järjestyksessä:

1. VASTAANOTTO-palvelimeen, jos sellainen on
2. Määritysten palvelimet
3. Prosessi-palvelimet
3. Perusmuodon kohde-palvelimet
4. vContinuum palvelimet
5. Lähdepalvelin (Windows ja Linux-palvelin)

Asenna päivitykset seuraavasti:

1. Lataa [päivitys](https://aka.ms/asr-scout-update4) .zip-tiedosto. Tämä .zip-tiedosto sisältää seuraavat tiedostot:

    - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.GZ
    - CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
    - UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe
    - UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
    - vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe
    - Käyttäjien järjestelmänvalvoja update4 bittien RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz 
    
2. Pura .zip-tiedostoja.<br>
3. **Saat vastaanotto-palvelin**: kopioi **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** vastaanotto-palvelimeen ja Pura se. Suorita poimitun **kansio/asentaminen**.<br>
4. **Määritysten server-prosessin palvelimen**: kopioi **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** määrityspalvelimen ja prosessin palvelimeen. Käynnistä se kaksoisnapsauttamalla sitä.<br>
5. **For Windowsin perusmuodon kohdepalvelin**: yhdistetty agentti päivittämään kopioi **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** perusmuodon kohde-palvelimeen. Kaksoisnapsauttamalla toimii puhelimessasi. Huomaa, että yhdistetty agentti on myös käytettävissä lähdepalvelimeen. Asenna se sekä-lähde-palvelimeen jäljempänä tässä luettelossa mainitut.<br>
7. **VContinuum palvelimen**: kopioi **vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe** vContinuum palvelimeen.  Varmista, että olet sulkenut vContinuum ohjatun toiminnon. Kaksoisnapsauta tiedostoa kaksoisnapsautetaan.<br>
8. **Saat Linux perusmuodon kohdepalvelimen**: yhdistetty agentti päivittämään kopioiminen **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** perusmuodon kohde palvelimeen ja Pura se. Suorita poimitun **kansio/asentaminen**.<br>
9. **Tietolähteen for Windows server**: yhdistetty agentti päivittämään kopioida **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** lähdepalvelimeen. Kaksoisnapsauttamalla toimii puhelimessasi.<br>
10. **Saat Linux lähdepalvelin**: yhdistetty agentti päivittämään kopioiminen vastaavat käyttäjien järjestelmänvalvoja-tiedoston version Linux-palvelimeen ja Pura se. Suorita poimitun **kansio/asentaminen**.  Esimerkki: RHEL 6.7 64-bittinen palvelin-kopioi **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** palvelimeen ja Pura se. Suorita poimitun **kansio/Asenna**.

## <a name="step-4-set-up-replication"></a>Vaihe 4: Määritä toistoa
1. Replikointi välillä lähteen määrittäminen ja kohdistaa VMware sivustot.
2. Ohjeita on käyttää InMage Scout asiakirjat, jotka on ladattu tuotteen kanssa. Vaihtoehtoisesti voit käyttää niitä seuraavasti:

    - [Julkaisutiedot](https://aka.ms/asr-scout-release-notes)
    - [Yhteensopivuuden matriisi](https://aka.ms/asr-scout-cm)
    - [Käyttöopas](https://aka.ms/asr-scout-user-guide)
    - [VASTAANOTTO-käyttöopas](https://aka.ms/asr-scout-rx-user-guide)
    - [Pika-asennuksen opas](https://aka.ms/asr-scout-quick-install-guide)


## <a name="updates"></a>Päivitykset

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure sivuston palauttaminen Scout 8.0.1 Päivitä 4
Scout Päivitä 4 on koottu päivitys. Siinä on kaikki update1 tähän päivään update3 ja seuraavat uuden virheenkorjauksia ja parannukset korjaukset.

**Uuden käyttöympäristön tuki** 

- Tuki on lisätty vCenter/vSphere 6.0, 6.1 ja 6.2
- Tuki on lisätty seuraavat Linux-käyttöjärjestelmät
    - Punainen on Enterprise Linux (RHEL) 7.0, 7.1 ja 7.2 
    - CentOS 7.0, 7.1 ja 7.2
    - Punainen on Enterprise Linux (RHEL) 6.8
    - CentOS 6.8

>[AZURE.NOTE]
>
> RHEL/CentOS 7 64-bittinen **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** on pakattu perus Scout GA paketin **InMage_Scout_Standard_8.0.1 GA.zip**kanssa. Lataa Scout GA paketin portal [Vaihe1](site-recovery-vmware-to-vmware.md#Step 1: Create a vault)mainittu.

**Virheenkorjauksia ja parannukset** 

- Parannettu Sammuta käsittelyn seuraavat Linux OSes ja kloonit estää ei-toivottuja uudelleen synkronoida ongelmat.
    - Punainen on Enterprise Linux (RHEL) 6.x
    - Oracle Linux (OL) 6.x
- Saat Linux-Suorita kansioiden käytön yhdistetty agentti asennuksen kansion käyttöoikeudet on rajoitettu nyt vain paikallisen käyttäjälle.
- Windows aikakatkaisun ongelman aikana varmenteiden yleisiä hajautettu yhdenmukaisuuden kirjanmerkki raskaasti ladata hajautettu sovelluksia, kuten SQL- ja jakopisteeseen klustereiden.
- Lisätty lokitiedoston, jotka liittyvät CX perus asennusohjelman korjaus.
- VMware vCLI 6.0 latauslinkki lisätään Windows perustyyli kohde perus asennusohjelma.
- Lisätä Lisää tarkistukset ja lokit verkon määrityksiä muutokset automaattisesti ja DR harjoitukset aikana.
- Joskus säilytys tietoja ei ole raportoitu CX.  
- Fyysinen klusterin on ei ole aseman vContinuum ohjatun toiminnon koon uudelleen, kun lähteen aseman Pienennä on tapahtunut.
- Klusterin suojaus epäonnistui virheen "Ei löydy levyn allekirjoituksen" Kun klusterin levy on PRDM levy.
- cxps transport palvelimen kaatumisen alue poikkeuksen vuoksi. 
- Palvelimen nimi ja IP-sarakkeet ovat nyt kokoa push Asenna-sivun vContinuum ohjatun toiminnon.
- VASTAANOTTO-Ohjelmointirajapinnan parannukset
    - On viisi uusimmat saatavilla yleisiä yhdenmukaisuuden pisteitä (vain taata tunnisteita).
    - On suojattu laitteiden kapasiteetin ja tilaa tiedot.
    - Tarjoaa lähdepalvelin Scout ohjaimen tila. 
    
>[AZURE.NOTE] 
>
>- **InMage_Scout_Standard_8.0.1_GA.zip** perus-paketti nyt on päivitetty CX perus installer **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** ja Windows perustyyli kohde perus asennusohjelma **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Kaikki uusi asennus käyttävät uusia CX ja Windows perustyyli kohde GA bittiä.
>- Päivitä 4 voidaan ottaa suoraan 8.0.1 GA.
>- Määrityspalvelimen ja vastaanotto-päivityksiä ei voi kumota, takaisin, kun ne on otettu käyttöön järjestelmässä.

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure sivuston palauttaminen Scout 8.0.1 päivitys 3
Päivitä 3 sisältää seuraavat virheenkorjauksia ja parannukset:

- Määrityspalvelimen ja vastaanotto epäonnistua Rekisteröi palauttaminen säilö, kun ne ovat välityspalvelimen takana.
- Ei käytön päivitetään tuntimäärä, joka palautus-kohdan tavoite (RPO) ei täyty kuntoraportti.
- Configuration-palvelin ei synkronoidu vastaanotto kanssa, kun ESX laitteen tiedot tai verkon tiedot sisältävät UTF-8-merkkejä.
- Windows Server 2008 R2 toimialueen ohjaimet onnistu käynnistyksen palautuksen jälkeen.
- Offline-synkronointi ei toimi odotetulla tavalla.
- Virtuaalikoneen (AM) automaattisesti, kun replikoinnin pari poistaminen jää CX-Käyttöliittymän pitkään ja käyttäjät eivät voi suorittaa loppuun tuntisesta tai jatka toimintoa.
- Yleistä toiminnoista, joita ovat tekemän yhdenmukaisuuden työ on optimoitu pakettitiedoston sovelluksen tilannevedoksen katkaisee kuten SQL-asiakkaille.
- Yhdenmukaisuuden-työkalu (VACP.exe) suorituskykyä on parannettu vähentämällä muistinkäytön, jota tarvitaan tilannevedoksen luominen Windows.
- Painallus Asenna service kaatuu, kun salasana on yli 16 merkkiä.
- vContinuum ei tarkistaminen ja tietojen kysely uudet vCenter tunnistetiedot tunnistetiedot muuttuessa.
- Linux-perusmuodon kohde välimuistin hallinnan (cachemgr) on ei tiedostojen lataaminen prosessi-palvelin, joka johtaa replikoinnin pari rajoitusta.
- Kun fyysinen automaattisesti klusterin (MSCS) levyn tilaus ei ole sama kaikissa solmuissa, replikointia ei ole määritetty joillekin klusterin asemat.
<br/>Huomaa, että klusterin on reprotected hyödyntää korjaa.  
- SMTP-toiminto ei toimi odotetulla tavalla, kun vastaanotto päivitetään Scout 7.1 Scout 8.0.1.
- Lisää tilasto on lisätty lokiin haluat seurata on toteutettu ne suoritetaan aikaa peruutus-toimintoa.
- Tuki on lisätty Linux-käyttöjärjestelmissä lähde-palvelimeen:
    - Update punainen on Enterprise Linux (RHEL) 6 7
    - CentOS 6 Päivitä 7
- VASTAANOTTO-Käyttöliittymän ja CX nyt näyttää ilmoituksen paria, joka ohjaa bittikartta tilaan.
- Seuraavat tietoturvakorjauksia on lisätty vastaanotto:

**Ongelman kuvaus**|**Käyttöönoton ohjeita**
---|---
Luvan ohittaa kautta parametrin muuttaminen|Käytön rajoitusten ei-toivottujen käyttäjille.
Sivustojenvälisen pyynnön väärennös|Toteutettu sivun tunnuksen käsitteestä, joka luo satunnaisesti jokaisen sivun. <br/>Tämän näet: <li> On vain yksi kirjautumisen esiintymä samalle käyttäjälle.</li><li>Sivun päivittäminen ei toimi--se ohjaa koontinäyttö.</li>
Haittaohjelmien tiedoston lataaminen|Tietyt laajennukset rajoitettu tiedostot. Sallittu tunnisteet ovat: 7z, aiff-asf, avi, bmp, csv-asiakirjaan, docx, Merki, flv, gif, gz, gzip, jpeg, jpg, loki mid mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, qt, RAM-muistia, rar, rm, rmi, rmvb, rtf-sdc, sitd, swf, sxc, sxw, tar, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml- ja zip.
Jatkuva sivustojenvälisen komentosarjan | Voit lisätä syötteen vahvistukset.


>[AZURE.NOTE]
>
>-  Kaikki sivuston palautus-päivitykset ovat kumulatiivisia. Päivitä 3 on päivitys 1 ja 2 päivityksen korjaukset. Päivitä 3 voidaan ottaa suoraan 8.0.1 GA.
>-  Määrityspalvelimen ja vastaanotto-päivityksiä ei voi kumota, takaisin, kun ne on otettu käyttöön järjestelmässä.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure sivuston palauttaminen Scout 8.0.1 Update (Päivitä 03 joulu 15) 2

2-päivitys korjaa muun muassa

- **Määrityspalvelimen**: korjaa ongelman, joka esti 31 päivän maksuton niin, että toiminto ei toimi odotetulla tavalla, kun configuration-palvelin on rekisteröity palauttaminen.
- **Yhdistetty agentti**: ongelmaan Päivitä 1, jotka ovat ei asenneta perusmuodon kohde-palvelimeen, kun se on päivitetty versiosta 8.0-8.0.1 päivitys korjaa.


### <a name="azure-site-recovery-scout-801-update-1"></a>Azure sivuston palauttaminen Scout 8.0.1 päivitys 1

Päivitys 1 sisältää seuraavat virheenkorjauksia ja uudet ominaisuudet:

- 31 päivän maksuton suojaus esiintymä kohden. Näin voit testata toimintoja tai seuraavista käsitteiden määrittäminen.
    - Palvelimessa, mukaan lukien automaattisesti ja tuntisesta, kaikki toiminnot ovat ilmaisia ensimmäisen 31 päivän ajalta, ajan, jonka palvelin on ensin suojattu sivusto palautus Scout lähtien.
    - 32nd päivän alkaen kunkin suojatun palvelimen laskutetaan vakio esiintymän korko Azure palauttaminen suojautumista asiakkaan omistaja-sivustoon.
    - Milloin tahansa suojatun palvelimissa, jotka veloitetaan tällä hetkellä määrä on käytettävissä Azure palauttaminen säilö Dashboard-sivulla.
- Tuen lisätty vSphere käyttöliittymä (vCLI) 5,5 päivityksen 2.
- Lisätty Linux-käyttöjärjestelmissä lähdepalvelimeen tuki:
    - RHEL 6 Päivitä 6
    - RHEL 5 Päivitä 11
    - CentOS 6 päivitys 6
    - Päivitä centOS 5 11
- Ohjelmavirhe korjaa seuraavat kohdat:
    - Säilö rekisteröinti epäonnistuu määrityspalvelimen tai vastaanotto-palvelimeen.
    - Klusterin asemat eivät näy odotetulla tavalla, kun liitetty näennäiskoneiden on reprotected, kun ne Jatka.
    - Tuntisesta epäonnistuu eri ESXi palvelimessa kuin paikallisen tuotannon näennäiskoneiden nykyisessä perusmuodon palvelimesta.
    - Asetusten tiedoston käyttöoikeuksia muutetaan, kun päivität 8.0.1, mikä heikentää suojaus ja toiminnot.
    - Uudelleensynkronointi raja-arvo ei ole pakotettu oikein, joka johtaa epäyhtenäinen replikoinnin toiminta.
    - RPO asetukset eivät näy oikein määritysten palvelimen käyttöliittymässä. Pakkaamattomat arvopiste näkyy virheellisesti pakattu arvo.
    -  Poista-toiminto ei poista vContinuum ohjatussa oikein ja replikointia ei poisteta määritysten palvelimen käyttöliittymästä.
    -  Ohjatun vContinuum levy on automaattisesti valitsemattomat napsauttaessasi **tiedot** levyn näkymän aikana MSCS näennäiskoneiden suojaus.
    - Fyysinen--virtual (P2V)-skenaario aikana tarvittavat HP-palvelut, kuten CIMnotify ja CqMgHost, eivät ole siirretty virtuaalikoneen palautus manuaalinen. Tämä aiheuttaa muita käynnistyksen aikana.
    - Perusmuodon kohde palvelimessa on yli 26 levyjen epäonnistuu Linux virtuaalikoneen suojaus.

## <a name="next-steps"></a>Seuraavat vaiheet

Julkaise kysymyksiä, voit käyttää [Azure palautus palvelut-keskustelupalstalla](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).
