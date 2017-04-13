<properties
    pageTitle="Mitä työmääriä voit suojata Azure palauttaminen kanssa?"
    description="Azure palauttaminen suojaa toiminnoista ja sovellusten sovittamalla replikoinnin, automaattisesti ja palautus paikallisen näennäiskoneiden ja fyysinen palvelimien Azure tai toissijaisen paikallisen-sivustoon"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>

# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Mitä työmääriä voit suojata Azure palauttaminen kanssa?


Tässä artikkelissa kuvataan toiminnoista ja sovelluksia, voit toistaa Azure sivuston palautus-palvelussa.

Kirjaa kommentteja tai kysymyksiä alaosassa on tämän artikkelin tai [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Yleiskatsaus

Organisaatioiden on liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) strategia säilyttää toiminnoista ja tietojen turvallisten ja käytettävissä olevien aikana suunnitellut ja suunnittelemattomat käyttökatkot ja palauttaa tavallisen käytön edellytykset mahdollisimman pian.

Sivuston palautus on Azure-palvelu prosentuaalista osuutta yrityksen BCDR määrittäminen. Käytä sivustojen palauttamisen, voit ottaa sovelluksen tukeva replikoinnin pilveen tai toissijaisen sivustoon. Selvitä sovellusten sijainti Windows tai Linux-pohjaiset, jossa on käytössä fyysinen palvelimissa, VMware tai Hyper-V palauttaminen avulla voit orchestrate replikoinnin, suorittaa tietojen palauttaminen testaus, ja suorita failovers ja tuntisesta.


Sivuston palautus on integroitu Microsoft-sovellusten, kuten SharePointin, Exchangen, Dynamics, SQL Server ja Active Directoryn. Microsoft toimii myös tiiviisti alussa toimittajista, esimerkiksi Oracle ja SAP, IBM punainen on. Voit mukauttaa replikoinnin ratkaisuja app sovelluksen perusteella.

## <a name="why-use-site-recovery-for-application-replication"></a>Miksi käyttää sovelluksen replikoinnin palauttaminen?

Sivuston palauttaminen prosentuaalista osuutta sovellustason ja palauttamisesta seuraavasti:

- Sovelluksen ympäristöstä riippumattomalla tavalla, antamisen kaikki tuetut tietokoneessa käynnissä työmääriä replikoinnin.
- Lähellä-painikkeen replikointia, RPOs mahdollisimman alhainen tarpeiden kannalta yrityssovelluksia 30 sekuntia.
- Sovelluksen yhdenmukaisia tilannevedoksia, yksi- tai monitasoisten sovellusten.
- SQL Server AlwaysOn ja muiden sovellustason replikoinnin tekniikoiden kanssa, mukaan lukien AD replikoinnin SQL AlwaysOn, Exchange tietokannan käytettävyys ryhmiä (DAGs) ja Oracle tietojen suojaa partnership integraation.
- Joustava palautus suunnitelmat, joka, joiden avulla voit palauttaa koko sovelluksen-pino yhdellä napsautuksella ja sisältää ulkoisen komentosarjojen ja manuaalinen toiminnot sisällytettävät suunnitelma.
- Kehittyneiden hallinnan sivuston palauttaminen ja Azure yksinkertaistaa sovelluksen Verkkovaatimukset, mukaan lukien pätevyys varaa IP-osoitteet, määrittää kuormituksen tasaamisen ja integrointi kanssa Azure liikenteen hallinta pienen RTO verkon switchovers.
-  Rich automaatio-kirjasto, joka sisältää tuotannon-sivuille, sovelluksen kielikohtaiset komentosarjoja, voit ladata ja palautus suunnitelmien integroitu.



## <a name="workload-summary"></a>Kuormituksen yhteenveto

Sivuston palauttaminen toistaa minkä tahansa-sovelluksen tuetut tietokoneessa käynnissä. Lisäksi on olet organisaatio loi Researchin kanssa tuoteryhmien suorittamaan app kielikohtaiset testata.

**Kuormituksen** | **Toistaa Hyper-V VMs toissijainen sivustoon** | **Toistaa Hyper-V VMs Azure** | **Toistaa VMware VMs toissijainen sivustoon** | **Toistaa VMware VMs Azure**
---|---|---|---|---
Active Directory-DNS | Y | Y | Y | Y
Web apps (IIS, SQL) | Y | Y | Y | Y
System Center Operations Manager | Y | Y | Y | Y
SharePoint | Y | Y | Y | Y
SAP: IN<br/><br/>SAP-sivuston replikoida Azure ei klusterin | Y (testattu Microsoft) | Y (testattu Microsoft) | Y (testattu Microsoft) | Y (testattu Microsoft)
Exchange (-DAG) | Y | Tulossa pian | Y | Y
Remote Desktop/VDI | Y | Y | Y | PUUTTUU
Linux (käyttöjärjestelmän ja sovellukset) | Y (testattu Microsoft) | Y (testattu Microsoft) | Y (testattu Microsoft) | Y (testattu Microsoft)
Dynamics AX | Y | Y | Y | Y
Dynamics CRM: | Y | Tulossa pian | Y | Tulossa pian
Oracle | Y (testattu Microsoft) | Y (testattu Microsoft) | Y (testattu Microsoft) | Y (testattu Microsoft)
Windows-tiedostopalvelimeen | Y | Y | Y | Y


## <a name="replicate-active-directory-and-dns"></a>Toistaa Active Directory- ja DNS: STÄ

Active Directory- ja DNS-infrastruktuurin on olennainen useimpien sovellusten. Tarvitset aikana palauttaminen, suojata ja palauttaa infrastruktuurin komponentit ennen palauttaminen toiminnoista ja sovellukset.

Voit käyttää sivuston palauttaminen luo valmis automaattisen tietojen palauttaminen suunnitelma Active Directory-ja DNS. Jos haluat epäonnistua esimerkiksi päälle SharePoint and SAP-ensisijaisen toissijainen sivustoon, voit määrittää palautus suunnitelma, joka siirtyy ensin Active Directory ja sovelluksen kielikohtaiset palautuksen aiot epäonnistua muista sovelluksista, jotka perustuvat Active Directory päälle.

[Lue lisää](site-recovery-active-directory.md) suojaaminen Active Directory- ja DNS: STÄ.

## <a name="protect-sql-server"></a>SQL Serverin suojaaminen

SQL Server tarjoaa data services-foundation paikallisen-tietokeskuksen monta yrityssovelluksia tietopalvelut.  Sivuston palauttaminen voidaan suojata usean tasoisen enterprise-sovellukset, jotka käyttävät SQL Server SQL Server HA/DR tekniikoiden kanssa. Sivuston palautus on:

- Perus- ja edullinen tietojen palauttaminen ratkaista SQL Serveriä varten. Toistaa useita versioita ja SQL Server erillisestä palvelinten ja klustereiden-versioissa Azure tai toissijaisen sivustoon.  
- Integrointi SQL AlwaysOn käytettävyys ryhmät-automaattisesti ja tuntisesta Azure palauttaminen palautus-palvelupakettien kanssa.
- Lopusta loppuun palautus suunnittelee kaikki tasoa sovelluksessa, kuten SQL Server-tietokannat.
- Sivuston palauttaminen ja skaalausta piikin SQL Serverin Lataa mukaan "bursting" ne yhdeksi suurempi IaaS virtuaalikoneen koot Azure.
- Helppo testaus SQL Server-palauttaminen. Voit suorittaa testin failovers analysoida tietoja ja suorittaa yhteensopivuuden tarkistukset ilman vaikuttavat tuotannon-ympäristössä.

[Lue lisää](site-recovery-sql.md) siitä, että SQL Serverin suojaaminen.

##<a name="protect-sharepoint"></a>Suojaa SharePoint

Azure palauttaminen auttaa suojaamaan SharePoint-käyttöönotoissa, seuraavasti:

- Ei tarvitse ja valmius-klusterin palauttaminen infrastruktuuri kustannuksiin. Sivuston palauttaminen avulla voit toistaa koko palvelinfarmin (Web app- ja tietokannan tasoa) Azure tai toissijaisen sivustoon.
- Yksinkertaistaa sovelluksen käyttöönottoa ja hallinta. Ensisijainen sivuston käyttöön päivitykset automaattisesti replikoida ja ovat näin käytettävissä automaattisesti ja toissijainen Sivuston klusterin palautuksen jälkeen. Laskee myös muuta hallinta monimutkaisuuden ja valmius-klusterin päivittäminen liittyvät kustannukset.
- Yksinkertaistaa SharePoint-sovellusten kehittämisen ja luomalla tuotannon kaltaisessa kopioi tarvittaessa replikan-ympäristössä, testaus ja virheiden testaus.
- Sivuston palauttaminen avulla voit siirtää SharePoint-käyttöönotoissa Azure yksinkertaistaa siirtymän pilvipalveluun.

[Lisätietoja](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae) SharePoint suojaaminen.


## <a name="protect-dynamics-ax"></a>Dynamics AX suojaaminen

Azure palauttaminen auttaa suojaamaan Dynamics AX ERP-ratkaisun mukaan:

- Orchestrating replikoinnin koko Dynamics AX ympäristön (Internetin kautta tai AOS tasoa, tietokannan tasoa SharePoint) Azure tai toissijaisen sivustoon.
- Yksinkertaistaminen siirto Dynamics AX ominaisuuksissa pilveen (Azure).
- Yksinkertaistaminen Dynamics AX sovellusten kehittämisen ja luomalla tuotannon kaltaisessa kopioi tarvittaessa, testaus ja virheiden testaus.

[Lue lisää](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) suojaaminen dynaaminen AX.

## <a name="protect-rds"></a>Suojaa RDS

Remote Desktop Services (RDS) mahdollistaa virtual desktop infrastructure (VDI), istunnon perustuva työpöytiä ja sovelluksia, jonka avulla käyttäjät voivat käyttää missä tahansa. Azure palauttaminen avulla voit:

- Replikoida hallitun tai ei-hallitun Poolisen virtual työpöytiä toissijaisen sivuston ja sovellusten ja toissijainen sivustoon tai Azure istuntoja.
- Näin voit toistaa:

**RDS** | **Toistaa Hyper-V VMs toissijainen sivustoon** | **Toistaa Hyper-V VMs Azure** | **Toistaa VMware VMs toissijainen sivustoon** | **Toistaa VMware VMs Azure** | **Toistaa fyysiset palvelimet toissijainen sivustoon** | **Toistaa fyysiset palvelimet Azure**
---|---|---|---|---|---|---
**Poolisen Virtual Desktop (ei-hallittu)** | Kyllä | Ei | Kyllä | Ei | Kyllä | Ei
**Poolisen Virtual Desktop (hallittuja ja ilman UPD)** | Kyllä | Ei | Kyllä | Ei | Kyllä | Ei
**Sovellusten ja työpöydän istuntoja (ilman UPD)** | Kyllä | Kyllä | Kyllä | Kyllä | Kyllä | Kyllä


[Lue lisää](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) RDS. suojaaminen


## <a name="protect-exchange"></a>Suojaa VAIHTO

Sivuston palauttaminen auttaa suojaamaan Exchange-seuraavasti:

- Pieni Exchange-käyttöönotoissa, yksi tai erillisestä palvelinten, kuten palauttaminen voi replikoida ja epäonnistua päälle Azure tai toissijaisen sivustoon.
- Suurempi-versioiden palauttaminen integroitu Exchange DAGS.
- Exchange-DAGs ovat Exchange palauttaminen yrityksen suositeltu ratkaisu.  Sivuston palauttaminen palautus suunnitelmien sisällyttää DAGs, orchestrate DAG automaattisesti sivustoissa.


[Lue lisää](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) suojaaminen Exchange.

## <a name="protect-sap"></a>SAP: N suojaaminen

Sivuston palauttaminen avulla voit suojata SAP-käyttöönoton seuraavasti:

- Ottaa koko SAP-käyttöönoton suojauksen käyttöön replikoiminen eri käyttöönoton kerrokset Azure tai toissijaisen sivustoon.
- Yksinkertaistaa cloud siirron SAP-käyttöönoton siirtäminen Azure palauttaminen avulla.
- SAP-kehittämisen ja testauksen luomalla tuotannon kaltaisessa kopioi pyydettäessä testaus ja virheenkorjaus sovellukset.

[Lisätietoja](http://aka.ms/asr-sap) SAP suojaaminen.

## <a name="next-steps"></a>Seuraavat vaiheet

[Sivustojen palauttamisen käyttöönotto valmisteleminen](site-recovery-best-practices.md) 
