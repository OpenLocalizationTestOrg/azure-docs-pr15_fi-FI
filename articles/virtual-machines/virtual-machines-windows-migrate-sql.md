<properties
    pageTitle="Siirtää SQL Server-tietokannan SQL Server VM | Microsoftin Azure"
    description="Lisätietoja paikalliseen käyttäjätietokantaan siirtäminen SQL Server-Azure VM."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sabotta"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="carlasab"/>


# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>Siirtää SQL Server-tietokannan SQL Server-Azure-VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]Resurssien hallinnan malli.


Useilla eri tavoilla siirtämisestä käyttäjän paikalliseen SQL Server-tietokantaan SQL Azure-VM palvelimelle. Tässä artikkelissa lyhyesti keskustella eri menetelmillä suosittelee eri tilanteissa paras tapa ja sisältää [opetusohjelma](#azure-vm-deployment-wizard-tutorial) opastaa **käyttöönotto Microsoftin Azure VM: n SQL Server-tietokantaan** ohjatun toiminnon käytön aikana. 

[Opas](#azure-vm-deployment-wizard-tutorial) on kuvattu **käyttöönotto Microsoftin Azure VM: n SQL Server-tietokantaan** ohjatun toiminnon avulla menetelmä on tarkoitettu vain perinteinen käyttöönottomalli. 

## <a name="what-are-the-primary-migration-methods"></a>Mitä ovat ensisijaisen siirtotiedoston menetelmiä?

Ensisijaisen siirtotiedoston menetelmät ovat:

- Käytä käyttöönotto Microsoft Azure VM ohjattu SQL Server-tietokantaan
- Pakkauksen käyttäminen paikallinen varmuuskopio ja manuaalisesti kopioida varmuuskopiotiedoston Azure virtual machine
- URL-osoitteen URL-Osoitteita ja Azure virtual machine yhdeksi palauttaminen varmuuskopio
- Irrota ja kopioi data-ja Azure blob-etäsäilöpalvelun ja liittää SQL Azure VM palvelimelle URL-osoitteesta
- Muuntaa paikallisen koneen fyysinen Hyper-V VHD, Azure Blob-etäsäilöpalvelun ladata ja ottaa käyttöön Näennäiskiintolevyn ladattu uusi VM käyttäen kuin
- Alusta kiintolevy tuominen ja vieminen Windows-palvelun avulla
- Jos käytät AlwaysOn-käyttöönoton tiloissa, [Azure replikan lisäämistoiminnon](virtual-machines-windows-classic-sql-onprem-availability.md) avulla voit luoda replikaa Azure ja vikasietoisuus, osoittamalla käyttäjät Azure tietokantaesiintymää
- Avulla voit määrittää Azure SQL Server-esiintymän tilaajana ja sitten Poista se käytöstä, osoittamalla käyttäjät Azure tietokantaesiintymää SQL Serverin [tapahtumapohjaista replikointia](https://msdn.microsoft.com/library/ms151176.aspx)



## <a name="choosing-your-migration-method"></a>Että siirtotavan valitseminen

Parhaan mahdollisen tietojen siirto suorituskyvyn siirron yhdeksi pakattu varmuuskopiosta Azure VM: N tietokantatiedostot on yleensä paras tapa. Tämä on menetelmä, joka käyttää [SQL Server-tietokantaan Microsoft Azure VM ohjattu käyttöönotto](#azure-vm-deployment-wizard-tutorial) . Tämä ohjattu toiminto on suositeltava tapa siirtämisestä paikallisen käyttäjän tietokanta käynnissä SQL Server 2005- tai SQL Server-2014 suurempi tai suurempi, kun pakattu tietokannan varmuuskopiotiedosto on pienempi kuin 1 TB.

Käytä tietokannan siirtämisen aikana Käyttökatkosten minimoimiseksi AlwaysOn-vaihtoehto tai tapahtumapohjaista replikointia vaihtoehto.

Ellei ole mahdollista käyttää edellä mainittuja menetelmiä, voit siirtää tietokannan manuaalisesti. Tällä menetelmällä voit yleensä alkaa sen jälkeen kopion tietokannan varmuuskopio tietokannan varmuuskopiointi levy Azure ja suorita tietokannan palautus. Voit myös kopioida tietokannan tiedostot itse Azure ja liittää ne. Siellä useita tapoja, joilla voit tehdä tämän manuaalinen prosessi, tietokannan siirtämisestä ‑järjestelmään Azure-VM.

> [AZURE.NOTE] Kun päivität vanhempien versioiden SQL Server SQL Server 2014 tai SQL Server-2016, kannattaa harkita muutoksia tarvitaan. Microsoft suosittelee, että osoite ei tue SQL Serverin uuden version osana siirron projektin ominaisuuksien kaikki riippuvuudet. Saat lisätietoja tuettujen versioiden ja skenaarioita, [Päivitä SQL Server](https://msdn.microsoft.com/library/bb677622.aspx).

Seuraavassa taulukossa luetellaan kunkin ensisijaisen siirtotiedoston menetelmiä ja kerrotaan, milloin kutakin menetelmää on sopivin.

| Menetelmä  | Lähteen tietokannan versio  |  Kohteen tietokannan versio | Lähteen tietokannan varmuuskopion koko rajoitus  | Huomautukset  |
|---|---|---|---|---|
| [Käytä käyttöönotto Microsoft Azure VM ohjattu SQL Server-tietokantaan](#azure-vm-deployment-wizard-tutorial) | SQL Server 2005 tai uudempi | SQL Serverin 2014 tai suurempi | < 1 TB  | Nopein ja yksinkertaisin tapa käyttö aina kun mahdollista siirtää Azure VM-uuteen tai aiemmin luotuun SQL Server-esiintymä | 
| [Käyttöä lisätä Azure replikan ohjatun](virtual-machines-windows-classic-sql-onprem-availability.md) | SQL Server 2012 tai uudempi | SQL Server 2012 tai uudempi | [Azure VM: N tallennustila](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Aikaa, kun käyttöönoton AlwaysOn tiloissa on pienentää |
| [Käytä SQL Server tapahtumapohjaista replikointia](https://msdn.microsoft.com/library/ms151176.aspx) | SQL Server 2005 tai uudempi | SQL Server 2005 tai uudempi | [Azure VM: N tallennustila](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Kun haluat minimoida aikaa eikä ole AlwaysOn paikallisen käyttöönoton |
| [Pakkauksen käyttäminen paikallinen varmuuskopio ja manuaalisesti kopioida varmuuskopiotiedoston Azure virtual machine](#backup-to-file-and-copy-to-vm-and-restore) | SQL Server 2005 tai uudempi | SQL Server 2005 tai uudempi | [Azure VM: N tallennustila](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Käytä vain kun et voi käyttää ohjattua luomista, kuten milloin kohde tietokannan versio on pienempi kuin SQL Server 2012: n SP1-CU2 tai tietokannan varmuuskopion koko on suurempi kuin 1 TB (12.8 TB kanssa SQL Server-2016) |
| [URL-osoitteen URL-Osoitteita ja Azure virtual machine yhdeksi palauttaminen varmuuskopio](#backup-to-url-and-restore) | SQL Server 2012: n SP1-CU2 tai suurempi | SQL Server 2012: n SP1-CU2 tai suurempi | < 12.8 TB, SQL Server-2016, muuten < 1 TB | Yleensä [varmuuskopioinnin URL-osoite](https://msdn.microsoft.com/library/dn435916.aspx) on vastaava suorituskykyä käyttämällä ohjattua ja ei niin helposti |
| [Irrota ja kopioida sitten data-ja Azure blob-etäsäilöpalvelun ja liitä sitten SQL Server Azure VM-URL-osoitteesta](#detach-and-copy-to-url-and-attach-from-url) | SQL Server 2005 tai uudempi | SQL Serverin 2014 tai suurempi | [Azure VM: N tallennustila](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Käytä tätä menetelmää, kun aiot [tallentaa tiedostot käyttäen Azure Blob storage](https://msdn.microsoft.com/library/dn385720.aspx) ja liittää käynnissä Azure-VM, erityisen hyvin suuria tietokantoja SQL Server |
| [Muuntaa paikallisen koneen näennäiskiintolevyjen Hyper-V, Azure Blob-etäsäilöpalvelun ladata ja sitten uudelle näennäiskoneelle avulla ladatun Näennäiskiintolevyn käyttöönotto](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) | SQL Server 2005 tai uudempi | SQL Server 2005 tai uudempi | [Azure VM: N tallennustila](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Käytä tätä, kun [näitä tietoja SQL Server-käyttöoikeus](../sql-database/sql-database-paas-vs-sql-server-iaas.md), kun siirretään tietokantaan, kun suoritetaan SQL Server tai vanhempi versio siirrettäessä järjestelmän ja käyttäjän tietokannoissa yhdessä osana tietokannan muiden tietokantojen käyttäjän ja järjestelmän tietokannoista riippuvainen siirron. |
| [Alusta kiintolevy tuominen ja vieminen Windows-palvelun avulla](#ship-hard-drive) | SQL Server 2005 tai uudempi | SQL Server 2005 tai uudempi | [Azure VM: N tallennustila](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | [Tuominen ja vieminen Windows-palvelu](../storage/storage-import-export-service.md) käyttää, kun manuaalinen kopiointimenetelmä on liian hidas, kuten erittäin suuria tietokantoja |

## <a name="azure-vm-deployment-wizard-tutorial"></a>Azure VM: N käyttöönoton ohjattu opetusohjelma

SQL Server 2005: n siirtäminen Microsoft SQL Server Management Studio **käyttöönotto Microsoftin Azure VM: n SQL Server-tietokantaan** ohjatun toiminnon avulla, SQL Server 2008: n, SQL Server 2008 R2, SQL Server 2012, SQL Server 2014 tai SQL Server-2016 paikallisen käyttäjän tietokanta (enintään 1 TB) SQL Server 2014 tai SQL Server-2016 Azure VM. Tämän ohjatun toiminnon avulla voit siirtää käyttäjän tietokannan aiemmin Azure näennäiskonetta tai Azure VM: SQL Serverin ohjatun toiminnon luoma siirtoprosessin aikana. Kun siirrät tietokannan SQL Server versioon, tietokanta päivitetään automaattisesti prosessin aikana.

Menetelmä on tarkoitettu vain perinteinen käyttöönottomalli. 

### <a name="get-latest-version-of-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Hae uusin versio Ota käyttöön Microsoftin Azure VM-ohjattu SQL Server-tietokantaan

Varmista, että **Ota Microsoftin Azure VM: n SQL Server-tietokantaan** ohjatun toiminnon uusin versio uusin versio SQL Serverin Microsoft SQL Server Management Studion avulla. Tämän ohjatun toiminnon uusin versio sisältää uusimmat päivitykset classic Azure-portaaliin ja tukee Azure VM: N uusimmat kuvat valikoimaan (vanhempien versioiden ohjatun toiminnon saattaa ei toimi). Hanki uusin versio Microsoft SQL Server Management Studio, SQL-palvelin, [se lataa](http://go.microsoft.com/fwlink/?LinkId=616025) ja asentaa asiakastietokoneeseen on tietokanta, joka suunnitelman siirtäminen ja Internet-yhteys.

### <a name="configure-the-existing-azure-virtual-machine-and-sql-server-instance-if-applicable"></a>Määritä aiemmin Azure näennäiskone ja SQL Server-esiintymään (tarvittaessa)

Jos olet siirtymässä olemassa Azure-VM, tarvitaan seuraavat määritykset:

- Määritä Azure VM: N ja SQL Server-esiintymä muodostaa yhteys toisesta tietokoneesta noudattamalla [SSMS toisessa tietokoneessa-SQL Server VM](virtual-machines-windows-sql-connect.md)-esiintymään. Vain valikoima 2014 SQL Serverin ja SQL Server-2016 kuvia tuetaan, jos siirryt ohjatun toiminnon avulla.
- Microsoftin Azure-yhdyskäytävä, jossa on yksityinen portti 11435 Avaa päätepiste SQL Server Cloud sovitin palvelun konfiguroimista varten. Tämä portti on luotu osana SQL Server 2014 tai SQL Server-2016 valmistelu Microsoftin Azure-VM. Cloud-sovittimen myös Luo Windowsin palomuurissa sääntö, joka sallii sen saapuvan TCP-yhteyksien oletusportti 11435 osoitteessa. Tämän päätepisteen avulla ohjattu toiminto käyttää Cloud sovitin palvelun varmuuskopiotiedostojen kopioidaan paikallinen esiintymä Azure-VM. Lisätietoja [SQL Serverin Cloud-sovitin](https://msdn.microsoft.com/library/dn169301.aspx).

    ![Luo Cloud sovitin päätepiste](./media/virtual-machines-windows-migrate-sql/cloud-adapter-endpoint.png)

### <a name="run-the-use-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Microsoftin Azure VM-ohjattu käyttöönotto SQL Server-tietokantaan suorittamalla käyttö

1. Avaa Microsoft SQL Server Management Studio for Microsoft SQL Server-2016 ja muodostaa yhteyden SQL Server-esiintymään, jossa käyttäjätietokantaan, joka siirtyminen Azure-VM.
2. Napsauta tietokannan, joka olet siirtymässä, tehtävät ja valitse sitten Ota Microsoftin Azure-VM.

    ![Käynnistä ohjattu toiminto](./media/virtual-machines-windows-migrate-sql/start-wizard.png)

3. Esittely-sivulle valitsemalla Seuraava.
4. Lähdekoodin asetukset-sivulla muodostaa yhteyden SQL Server-esiintymä, joka sisältää tietokannan, että aiot siirtää Azure-VM.
5. Määritä tiedostojen varmuuskopioinnin tilapäiseen sijaintiin. Jos yhteyden etäpalvelimeen, sinun on määritettävä verkkoasemaan.

    ![Tietolähteiden asetukset](./media/virtual-machines-windows-migrate-sql/source-settings.png)

6. Valitse Seuraava.
7. Microsoftin Azure-sisäänkirjaus sivun napsauttamalla Kirjaudu sisään ja kirjaudu sisään tilillesi Azure.
8. Valitse tilaus, jota haluat käyttää, ja valitse Seuraava.

    ![Azure sisään](./media/virtual-machines-windows-migrate-sql/azure-signin.png)

9. Käyttöönottoasetukset-sivulla voit määrittää uuden tai aiemmin luodun pilvipalvelu ja Näennäiskoneen nimi:

 - Määritä uusi luodaksesi uuden pilvi-palvelun uuden Azure näennäislaitteen käyttämällä SQL Server-2014 tai SQL Server-2016 valikoima kuva Cloud palvelunimi ja Virtual Machine nimi.

     - Jos määrität uuden pilvi-palvelun nimi, Määritä varastointi tilille, jota käytät.

     - Jos määrität aiemmin luodun pilvipalvelu nimeä, storage-tili haetaan ja olet syöttänyt.

 - Määritä olemassa oleva pilvipalvelu nimi ja uusi luodaksesi uuden Azure näennäislaitteen aiemmin pilvipalvelu virtuaalikoneen nimi. Määritä SQL Server-2014 tai SQL Server-2016 valokuvavalikoiman kuvaa vain.
 - Määritä aiemmin pilvipalvelu nimi ja Virtual Machine-nimi, jota käytetään aiemmin Azure VM. Tämä on näköistiedoston avulla SQL Server-2014 tai SQL Server-2016 valokuvavalikoiman kuvaa.

        ![Deployment Settings](./media/virtual-machines-windows-migrate-sql/deployment-settings.png)

10. Valitse asetukset
  - Jos olemassa pilvipalvelu ja Näennäiskoneen nimi on määritetty, sinun pyydetään antamaan käyttäjänimi ja salasana.

        ![Azure koneen asetuksia](./media/virtual-machines-windows-migrate-sql/azure-machine-settings.png)

    - Jos olet määrittänyt uuden virtuaalikoneen nimi, sinun pyydetään voit valita kuvan valikoima kuvia ja antaa seuraavat tiedot:
      - Kuva: Valitse SQL Server-2014 tai SQL Server-2016
        - Käyttäjänimi
        - Uusi salasana
        - Vahvista salasana
        - Sijainti
        - Kokoa.
    - Lisäksi hyväksymään itse luotu sertifikaatti, tämä uusi Microsoftin Azure Virtual Machine valitsemalla ja valitse sitten OK.

    ![Azure uuden koneen asetuksia](./media/virtual-machines-windows-migrate-sql/azure-new-machine-settings.png)

11. Määritä kohde tietokannan nimi, jos eri kuin lähteen tietokannan nimi. Jos kohdetietokantaan on jo olemassa, järjestelmä automaattisesti kasvattaa tietokannan nimi sijaan korvaa aiemmin luotu tietokanta.
12. Valitse Seuraava ja valitse sitten Valmis.

    ![Tulokset](./media/virtual-machines-windows-migrate-sql/results.png)

13. Kun ohjattu toiminto on valmis, muodostaa virtuaalikoneen ja varmista, että tietokanta on siirretty.
14. Jos olet luonut uuden näennäislaitteen, Määritä Azure virtual machine ja SQL Server-esiintymän noudattamalla [SSMS toisessa tietokoneessa-SQL Server VM](virtual-machines-windows-sql-connect.md)-esiintymään.

## <a name="backup-to-file-and-copy-to-vm-and-restore"></a>Backup tiedosto ja kopioi VM ja palautus

Käytä tätä menetelmää, kun Ota käyttöön Microsoftin Azure VM-ohjattu SQL Server-tietokantaan ei voi käyttää koska olet siirtymässä ennen SQL Server 2014 SQL Server versioon tai varmuuskopiotiedosto on suurempi kuin 1 TB. Jos varmuuskopiotiedosto on suurempi kuin 1 TB, se on stripe koska VM-levyn enimmäiskoko on 1 TB. Avulla voit siirtää käyttäjän tietokannan manuaalinen tällä menetelmällä seuraavasti:

1.  Suorittaa tietokannan täydellinen varmuuskopio paikalliseen sijaintiin.
2.  Luo tai Lataa näennäiskoneessa haluttu SQL Serverin version kanssa.
3.  Asennusohjelma connectivity tarpeiden mukaan. [Muodostaa yhteyden SQL Server-näennäislaite Azure (Resurssinhallinta)-](virtual-machines-windows-sql-connect.md)kohdassa.
4.  Kopioi varmuuskopio tiedostot oman VM: N Etätyöpöytä, Resurssienhallinnan tai kopioi-komentoa komentokehotteesta.

## <a name="backup-to-url-and-restore"></a>URL-osoite ja palauta varmuuskopio

Käytä [varmuuskopioinnin URL](https://msdn.microsoft.com/library/dn435916.aspx) menetelmää, kun käyttöönotto Microsoft Azure VM ohjattu SQL Server-tietokantaan ei voi käyttää, koska varmuuskopiotiedosto on suurempi kuin 1 TB ja siirryt- ja SQL Server-2016. Pienempi kuin 1 TB tai SQL Server-2016 ennen SQL Serverin versio tietokantoja on suositeltavaa käyttää ohjatun toiminnon. SQL Server-2016 Raidallinen varmuuskopiojoukot tuetaan, on suositeltava suorituskykyä ja ylittää kokoa kohti blob edellyttää. Hyvin suuria tietokantoja, [Tuominen ja vieminen Windows-palvelun](../storage/storage-import-export-service.md) käyttö on suositeltavaa.

## <a name="detach-and-copy-to-url-and-attach-from-url"></a>Irrota ja kopioida URL-osoite ja liitä URL-osoitteesta

Käytä tätä menetelmää, kun aiot [tallentaa tiedostot käyttäen Azure Blob storage](https://msdn.microsoft.com/library/dn385720.aspx) ja liittää SQL Azure-VM, erityisen hyvin suuria tietokantoja-palvelimeen. Avulla voit siirtää käyttäjän tietokannan manuaalinen tällä menetelmällä seuraavasti:

1.  Irrota tietokantatiedostot tiloissa tietokantaa esiintymästä.
2.  Irrotettu tietokantatiedostot kopioida Azure blob-etäsäilöpalvelun [AZCopy-komentoriviapuohjelman](../storage/storage-use-azcopy.md)avulla.
3.  Liitä Azure URL-tietokantatiedostot Azure VM: ssä SQL Server-esiintymään.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>VM: N muuntaminen ja URL ladata ja asentaa kuin uusi VM

Tämän menetelmän avulla voit siirtää kaikki paikalliseen SQL Server-esiintymän järjestelmän ja käyttäjän tietokannoissa Azure virtual machine. Siirtää koko SQL Server-esiintymän manuaalinen tällä menetelmällä käyttämällä seuraavasti:

1.  Muunna fyysiset tai virtuaalikoneet Hyper-V näennäiskiintolevyjen [Microsoft Virtual Machine-muuntimen](http://technet.microsoft.com/library/dn873998.aspx)avulla.
2.  Azure-tallennustilan VHD tiedostojen lataaminen käyttämällä [Lisää-AzureVHD-cmdlet-komennolla](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3.  Ottaa käyttöön uuden näennäislaitteen ladatun Näennäiskiintolevyn avulla.

> [AZURE.NOTE] Siirtämään koko sovelluksen harkita [Azure palauttaminen](../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Alusta kiintolevy

[Tuonti ja vienti palvelumenetelmä](../storage/storage-import-export-service.md) avulla voit siirtää suuria määriä tiedostotietoja Azure Blob-etäsäilöpalvelun tilanteissa, jossa ladataan verkosta on kohtuuttoman kallista tai ei ole toteutettavissa. Tämän palvelun avulla voit lähettää yhden tai useamman kiintolevyt sisältävät tiedot Azure, data-Center, johon tiedot ladataan tilillesi varastointi.

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja Azure virtuaalisten laitteiden SQL Server palvelimena [Azure virtuaalikoneet yleistä SQL-palvelimeen](virtual-machines-windows-sql-server-iaas-overview.md).

Ohjeita luomalla siepattua kuvaa Azure SQL Server Virtual Machine on CSS SQL Server insinöörien blogi [vihjeet ja ideat-kloonausta Azure SQL virtuaalikoneet siepatun kuvia](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) .
