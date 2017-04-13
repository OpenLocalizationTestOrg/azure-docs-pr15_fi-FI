<properties
    pageTitle="Azure-tiedoston tallennustilan vianmääritys | Microsoft Azure"
    description="Azure-tiedoston tallennustilan vianmääritys"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genli"/>

# <a name="troubleshooting-azure-file-storage-problems"></a>Azure-tiedoston tallennustilan ongelmien vianmääritys

Tämä artikkeli sisältää yleisiä ongelmia, jotka liittyvät Microsoft Azure tiedostojen tallentamisesta, kun muodostat yhteyden Windows ja Linux-asiakkaille. Se sisältää myös mahdolliset syyt ja ratkaisut seuraavat asiat.

**Yleisiä ongelmia (ilmetä Windows- ja Linux asiakkaat)**

- [Kiintiön virhe, kun yrität avata tiedoston](#quotaerror)

- [Kun käytät Azure tiedostojen tallentamisesta Windows- tai Linux hidas suorituskyky](#slowboth)

**Windows-asiakasohjelmien vianmääritys**

- [Kun käytät Azure tiedostojen tallentamisesta Windows 8.1 tai Windows Server 2012 R2 hidas suorituskyky](#windowsslow)

- [Virhe 53, kun yritän Azure-tiedostoresurssi](#error53)

- [Verkon käytön onnistui, mutta en näe Azure tiedoston jakaminen liitetyn Resurssienhallinnassa](#netuse)

- [Tallennustilan-tili sisältää "/" ja liitoskohdan komento epäonnistuu](#slashfails)

- [Oma sovelluspalvelu/Eikö liitetyn Azure tiedostojen aseman.](#accessfiledrive)

- [Lisäsuosituksia suorituskyvyn parantamiseksi](#additional)

**Linux asiakasohjelmien vianmääritys**

- [Virhe "Kopioit tiedoston kohteeseen, joka ei tue salausta" Kun Azure tiedostoja tiedostojen lataamisesta ja kopioiminen](#encryption)

- [Aiemmin luotu tiedosto "Host on alaspäin"-Virhe jaetaan tai käyttöliittymä jumittuu, kun teet liityntäkohtaan-komentojen luettelo](#errorhold)

- [Virhe 115 käyttöön, kun yrität asennustapa Azure tiedostojen Linux AM](#error15)

- [Linux AM ilmenee satunnaisia viipeitä komentoja, kuten "ls"](#delayproblem)

## <a name="general-problems"></a>Yleisiä ongelmia
<a id="quotaerror"></a>
### <a name="quota-error-when-trying-to-open-a-file"></a>Kiintiön virhe, kun yrität avata tiedoston

Windowsin näyttöön tulee virhesanomia, jotka muistuttavat seuraavasti:

**1816 ERROR_NOT_ENOUGH_QUOTA <> – 0xc0000044**

**STATUS_QUOTA_EXCEEDED**

**Kiintiö ei riitä ei käytettävissä komennon käsittelemiseen**

**Virheellinen kahva arvo GetLastError: 53**

Linux näyttöön tulee virhesanomia, jotka muistuttavat seuraavasti:

**<filename>[estetty]**

**Kiintiö on ylitetty**

#### <a name="cause"></a>Syy

Ongelma ilmenee, koska olet saavuttanut samanaikainen kahvat, joilla on tiedoston yläraja.

#### <a name="solution"></a>Ratkaisu

Vähennä samanaikainen kahvat sulkemalla joitakin kahvoista ja yritä sitten uudelleen. Lisätietoja on artikkelissa [Microsoft Azure tallennustilan suorituskyky ja skaalattavuus tarkistusluettelon](storage-performance-checklist.md).

<a id="slowboth"></a>
### <a name="slow-performance-when-accessing-file-storage-from-windows-or-linux"></a>Kun tallennus avaaminen Windows- tai Linux hidas suorituskyky

- Jos sinulla ei ole tietyn i/o koon vähimmäisvaatimus, suosittelemme käyttämään 1 Megatavu i/o koon suorituskyvyn.

- Jos tiedät, että tiedosto, joka on laajentaminen kanssa kirjoituksia lopullinen koon ja ohjelmistosi ei ole yhteensopivuusongelmia, jos ei ole vielä kirjoitettu kanta nollat sisältävää tiedostoa, Aseta etukäteen sen sijaan, että jokaisen kirjoittaminen tiedostokoko on putkien kirjoittaminen.

## <a name="windows-client-problems"></a>Windows-asiakasohjelmien vianmääritys
<a id="windowsslow"></a>
### <a name="slow-performance-when-accessing-the-file-storage-from-windows-81-or-windows-server-2012-r2"></a>Tiedostojen tallentamisesta avaaminen Windows 8.1 tai Windows Server 2012 R2 hidas suorituskyky

Asiakkaat, joilla on käytössä Windows 8.1 tai Windows Server 2012 R2 Varmista, että korjaus [KB3114025](https://support.microsoft.com/kb/3114025) on asennettu. Tämä korjaus on parannettu Luo ja Sulje kahva suorituskykyä.

Voit suorittaa seuraavaa komentosarjaa voit tarkistaa, onko korjaus on asennettu:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Jos korjaus on asennettu, näyttöön tulee seuraava tulos:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies**

**{96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1**

> [AZURE.NOTE]  Windows Server 2012 R2 kuvia Azure Marketplacesta on korjaus KB3114025 asennettu oletusarvoisesti joulukuuta 2015 alkaen.

<a id="additional"></a>
### <a name="additional-recommendations-to-optimize-performance"></a>Lisäsuosituksia suorituskyvyn parantamiseksi

Ei koskaan Luo tai avaa tiedosto välimuistissa i-/ o, joka pyytää kirjoitusoikeudet, mutta ei lukuoikeus. Kun olet Soita **CreateFile()**, Määritä koskaan vain **GENERIC_WRITE**, mutta aina määrittää **GENERIC_READ | GENERIC_WRITE**. Vain kirjoitus kahva ei voi tallentaa paikallisesti, pieni kirjoituksia silloinkin, kun se on vain avoin kahva tiedoston. Tämä asettamatta vakavia suorituskykyä huomattavasti pieni kirjoituksia. Huomautus "a" tilassa CRT **fopen()** ja Avaa vain kirjoitus-kahvaa.

<a id="error53"></a>
### <a name="error-53-when-you-try-to-mount-or-unmount-an-azure-file-share"></a>"Virhe 53" Kun yrität ottaminen käyttöön tai poistaa käytöstä Azure-tiedostoresurssin

Tämä ongelma voi johtua seuraavista ehdoista:

#### <a name="cause-1"></a>Syy 1

"Tapahtui järjestelmävirhe 53. Käyttö on estetty." Tietoturvasyistä Azure tiedostojen yhteydet on estetty, jos tietoliikenneyhteyden ei ole salataan ja yhteyden muodostaminen ei soitetaan joina Azure tiedostoresurssit asuvat samassa tietokeskuksen. Viestintä kanavan salausta ei ole annettu, jos käyttäjän asiakkaan OS eivät tue SMB-salausta. Tämä ilmaistaan "järjestelmän 53 on virhe. Käyttö estetty-virhesanoman, kun käyttäjä yrittää ottaa jaetun tiedoston paikallisen tai eri tietokeskuksen. Windows 8, Windows Server 2012: ssa ja uudemmissa versioissa neuvottelu sivupyynnön, joka sisältää SMB 3.0, joka tukee salausta.

#### <a name="solution-for-cause-1"></a>Syy 1 ratkaisu

Asiakas, joka vastaa Windows 8, Windows Server 2012: ssa tai sitä uudempi versio, tai, joka yhdistää virtual tietokoneesta, joka on saman tietokeskuksen Azuren tallennustilaan-tiliä, jota käytetään yhteyden Azure jaetun tiedostoresurssin varten.

#### <a name="cause-2"></a>Syy 2

"Järjestelmävirhe 53" Kun otat käyttöön Azure tiedostoresurssin voi ilmetä, jos portti 445 lähtevän tietoliikenteen Azure tiedostot tietokeskuksen on estetty. Napsauttamalla [tätä](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx) saat näkyviin Internet-palveluntarjoajien, salliminen tai estäminen portti 445 käytöltä yhteenveto.

Estä portin Comcast ja IT tietyissä organisaatioissa. Selvittääksesi, onko syy takana "53 Järjestelmävirhe"-sanoma, kysely TCP:445 päätepisteen Portqry avulla. Jos TCP:445 päätepisteen tulee näkyviin, kun suodatettu, TCP-portin on estetty. Tässä on esimerkki kyselyn:

`g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Jos TCP-445 estetty säännön verkon polusta, näet seuraavat tulokset:

**TCP-portti 445 (microsoft-ds palvelu): SUODATETTU**

Saat lisätietoja Portqry [Portqry.exe komentorivin apuohjelman kuvaus](https://support.microsoft.com/kb/310099).

#### <a name="solution-for-cause-2"></a>Syy 2 ratkaisu

Käsitellä IT-organisaation Avaa portti 445 lähtevä [Azure IP-alueita](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="cause-3"></a>Syy 3

"Järjestelmävirhe 53" voi vastaanottaa myös, jos NTLMv1 viestintä on otettu käyttöön asiakas. Ottaa käyttöön NTLMv1 Luo pienempi suojatun asiakas. Viestintä estetään siksi Azure-tiedostoja. Voit tarkistaa, onko tämä aiheuttaa virheen, varmista, että seuraava rekisterialiavain on määritetty arvo 3:

HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel.

Lisätietoja on ohjeaiheessa [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) TechNet.

#### <a name="solution-for-cause-3"></a>Syy 3 ratkaisu

Voit ratkaista ongelman, palautetaan oletusarvo on 3 HKLM\SYSTEM\CurrentControlSet\Control\Lsa rekisteriavain LmCompatibilityLevel-arvo.

Azure tiedostot tukee vain NTLMv2-todennusta. Varmista, että Ryhmäkäytäntö käytetään asiakkaille. Tämä estää tämän virheen ilmenemisen. Tämä on myös pidetään parhaana käytäntönä. Lisätietoja on artikkelissa [asiakkaat voivat käyttää NTLMv2 määrittämisestä ryhmäkäytännön avulla](https://technet.microsoft.com/library/jj852207(v=ws.11).aspx)

Suositeltu käytäntöasetus on **vain NTLMv2 Lähetä vastaus**. Tämä vastaa rekisteriarvon 3. Asiakkaat käyttävät vain NTLMv2-todennusta ja niiden käytä NTLMv2 Jos palvelin tukee sitä. Toimialueen ohjaimet Hyväksy LM, NTLM ja NTLMv2 todennusta.

<a id="netuse"></a>
### <a name="net-use-was-successful-but-dont-see-the-azure-file-share-mounted-in-windows-explorer"></a>Nettonykyarvon Käytä onnistui, mutta et näe Azure tiedoston jakaminen liitetyn Resurssienhallinnassa

#### <a name="cause"></a>Syy

Oletusarvon mukaan Resurssienhallinnan ei suorita järjestelmänvalvojana. Jos suoritat **verkon käytön** järjestelmänvalvojan komentorivi, voit yhdistää verkkoasemaan "Järjestelmänvalvojana." Koska yhdistetyt asemat käyttäjän keskitettyä, käyttäjätili, jolla on kirjautunut ei näy asemat, jos ne ovat käytettävissä eri käyttäjätiliä.

#### <a name="solution"></a>Ratkaisu

Ota käyttöön Jaa järjestelmänvalvoja komentoriviltä. Vaihtoehtoisesti voit noudattaa [Tässä TechNet-artikkelissa](https://technet.microsoft.com/library/ee844140.aspx) **EnableLinkedConnections** rekisteriarvon määrittämiseen.

<a id="slashfails"></a>
### <a name="my-storage-account-contains--and-the-net-use-command-fails"></a>Tallennustilan-tili sisältää "/" ja liitoskohdan komento epäonnistuu

#### <a name="cause"></a>Syy

Kun **nettonykyarvon use** -komento suoritetaan komentokehote (cmd.exe), se on jäsentää lisäämällä "/" komentorivin-asetus. Tämä aiheuttaa aseman yhdistämismäärityksen epäonnistuu.

#### <a name="solution"></a>Ratkaisu

Voit kiertää ongelman jommallakummalla seuraavasti:

• Käytä seuraavaa PowerShell-komentoa:

`New-SmbMapping -LocalPath y: -RemotePath \\server\share  -UserName acountName -Password "password can contain / and \ etc"`

Erän tiedostosta tämän voi tehdä muodossa

`Echo new-smbMapping ... | powershell -command –`

• Sijoittaa lainausmerkit ympärille salaisuus kiertää tämän ongelman, ellei "/" on ensimmäistä merkkiä. On käyttää vuorovaikutteinen tila ja kirjoita salasanasi erikseen tai luo avaimien saat avainta, joka ei käynnisty vinoviiva (/)-merkillä.

<a id="accessfiledrive"></a>
### <a name="my-applicationservice-cannot-access-mounted-azure-files-drive"></a>Oma sovelluksen tai palvelun ei voi käyttää liitetyn Azure tiedostojen aseman

#### <a name="cause"></a>Syy

Asemat on otettu käyttöön käyttäjää kohden. Jos eri käyttäjätilillä on käynnissä sovelluksen tai palvelun, käyttäjät eivät näe asemaa.

#### <a name="solution"></a>Ratkaisu

Liittää aseman saman käyttäjätilistä, jossa sovellus on. Tämän voi tehdä työkaluilla, kuten psexec.

Voit vaihtoehtoisesti Luo uusi käyttäjä, jolla on samat oikeudet kuin palvelun tai järjestelmän tili ja suorita **cmdkey** ja **verkon käytön** tilin alle. Käyttäjänimen tulisi olla tallennustilan tilin nimi ja salasana on oltava tallennustilan tilin-näppäintä. **Nettonykyarvon** käytettäväksi toinen vaihtoehto on välittää tallennustilan tilin nimi ja **nettonykyarvon use** -komennon käyttäjän käyttäjänimi ja salasana, parametrit-avain.

Kun noudatat näitä ohjeita, näyttöön voi tulla seuraava virhesanoma: "tapahtui virhe järjestelmän 1312. Istunnon määritettyyn kirjautuminen ei ole. Se jo ehkä katkaistu"Kun suoritat järjestelmän tai verkon palvelutilin **nettonykyarvon käyttöä** . Jos näin tapahtuu, varmista, että käyttäjänimi, joka lähetetään **verkon** käyttöön sisältyy toimialuetietoja (esimerkiksi: "[tallennustilan tilin nimi]. file.core.windows .net").

## <a name="linux-client-problems"></a>Linux asiakasohjelmien vianmääritys

<a id="encryption"></a>
### <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Virhe "Kopioit tiedoston kohteeseen, joka ei tue salausta"

#### <a name="cause"></a>Syy

Azure-tiedostoja voidaan kopioida BitLocker salattujen tiedostojen. Tiedostojen tallennus ei tue NTFS EFS. Tämän vuoksi todennäköisesti käytät EFS tällöin. Jos sinulla on tiedostoja, jotka ovat salattuja EFS kautta, tiedoston tallennustilan kopiointi saattaa epäonnistua, paitsi jos Kopioi-komento on salauksen kopioitu tiedosto.

#### <a name="workaround"></a>Vaihtoehtoinen menetelmä

Tiedoston kopioiminen tiedostojen tallentamisesta, se täytyy ensin purkaa. Voit tehdä tämän jollakin seuraavista tavoista:

• Käytä **Kopioi valitsinta /**.

• Määrittää seuraava rekisteriavain:

- Polku = HKLM\Software\Policies\Microsoft\Windows\System
- Arvon tyyppi = DWORD
- Nimi = CopyFileAllowDecryptedRemoteDestination
- Arvo = 1

Huomaa kuitenkin, että määrittämällä rekisteriavain vaikuttaa kaikkien kopiointi verkossa jaettuja resursseja.

<a id="errorhold"></a>
### <a name="host-is-down-error-on-existing-file-shares-or-the-shell-hangs-when-you-run-list-commands-on-the-mount-point"></a>Aiemmin luotu tiedosto "Host on alaspäin"-Virhe jakaa tai käyttöliittymä jumittuu, kun suoritat komentojen luettelo liityntäkohtaan

#### <a name="cause"></a>Syy

Tämä virhe ilmenee Linux-asiakas, kun asiakas on ollut käyttämättömänä pitkän ajan kuluessa. Kun tämä virhe ilmenee, asiakastietokoneen yhteys katkeaa ja asiakasyhteys aikakatkaistaan.

#### <a name="solution"></a>Ratkaisu

Tämä ongelma on korjattu Linux ydin osana [muuttaa](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93), odotetaan backport kyselyjä Linux-jakauman.

Works ongelman ylläpitää yhteyden ja välttää käyttämättömänä vaiheeseen, pitää tiedoston Azure jaetun tiedostoresurssin, voit kirjoittaa säännöllisesti. Tämä on oltava kirjoitus, kuten luoda tai muokata päivämäärän uudelleenkirjoitus-tiedoston avulla. Muussa tapauksessa voit saada välimuistiin tallennetut tulokset ja toimintoa ei voi käynnistää yhteys.

<a id="error15"></a>
### <a name="mount-error-115-when-you-try-to-mount-azure-files-on-the-linux-vm"></a>"Käyttöön virhe 115" Kun yrität ottaa Linux AM Azure-tiedostoja

#### <a name="cause"></a>Syy

Linux jaot eivät tue SMB 3.0 salaustoiminto vielä. Jotkin jaot käyttäjän saattaa tulla "115" virhesanoma, jos he yrittävät asennustapa Azure tiedostoja käyttämällä SMB 3.0 vuoksi puuttuva ominaisuus.

#### <a name="solution"></a>Ratkaisu

Jos Linux SMB-asiakas, jota käytetään ei tue salausta, asennustapa Azure tiedostoja käyttämällä SMB 2.1-Linux AM saman tietokeskuksen tiedostojen tallennustila-tiliksi.

<a id="delayproblem"></a>
### <a name="linux-vm-experiencing-random-delays-in-commands-like-ls"></a>Linux AM ilmenee satunnaisia viipeitä komentoja, kuten "ls"

#### <a name="cause"></a>Syy

Näin voi käydä, kun käyttöönotto-komento ei ole **serverino** -vaihtoehto. Ls-komento suoritetaan ilman **serverino** **STAT.** jokaisella.

#### <a name="solution"></a>Ratkaisu

Tarkista **serverino** "/ jne/fstab"-merkinnän:

azureuser.File.Core.Windows.NET/WMS/comer-/ aloitus/sampledir tyyppi cifs (rw, nodev, relatime, vers 2.1, = s = ntlmssp-välimuistin = tarkka, käyttäjänimi xxx, = toimialueen X file_mode = = 0755, dir_mode = 0755, serverino, rsize = 65536 wsize = 65536 actimeo = 1)

Jos **serverino** -vaihtoehto ei ole käytössä, käytöstä ja ottaa käyttöön Azure tiedostot uudelleen siten, että **serverino** -asetus on valittuna.

## <a name="learn-more"></a>Opi lisää

- [Windows Azure-tiedostosäilön käytön aloittaminen](storage-dotnet-how-to-use-files.md)

- [Azure-tiedostojen tallentamisesta Linux käytön aloittaminen](storage-how-to-use-files-linux.md)
