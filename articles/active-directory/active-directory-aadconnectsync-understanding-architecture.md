<properties
   pageTitle="Azure AD Connect synkronointi: tietoja arkkitehtuuri | Microsoft Azure"
   description="Tässä ohjeaiheessa kuvataan Azure AD Connect synkronointi arkkitehtuuri ja kerrotaan, termit."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/31/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure AD Connect synkronointi: tietoja arkkitehtuuri
Tässä aiheessa kerrotaan Azure AD Connect synkronointi basic arkkitehtuuri. Monia on samankaltainen kuin sen edeltäjät MIIS 2003: n, ILM 2007 ja FIM 2010. Azure AD Connect synkronointi on kehitys tekniikalla. Jos olet tutustunut jokin näistä aiemmissa tekniikoista, tämän ohjeaiheen sisältö ovat sinulle tuttuja sekä. Jos ole ennen käyttänyt synkronoinnin, valitse tässä ohjeaiheessa on. Ei kuitenkaan pyyntö tietää tämän ohjeaiheen onnistuu mukautusten synkronoitava Azure AD Connect (kutsutaan sync engine sisältö)-tietoja.

## <a name="architecture"></a>Arkkitehtuuri
Synkronoi-ohjelma luo integroitu näkymän objekteja, jotka on tallennettu eri yhdistetyn tietolähteistä ja hallitsee kyseisten tietolähteiden tunnistetiedot. Tämä integroitu näkymä määräytyy noutaa yhdistetyn tietolähteet ja säännöt, jotka määrittävät, kuinka käsittelevät tunnistetietoja.

### <a name="connected-data-sources-and-connectors"></a>Yhdistettyjen tietolähteiden ja yhdistimet
Synkronoi-ohjelma käsittelee tunnistetietojen eri tietojen säilöjen tietoihin, kuten Active Directory- tai SQL Server-tietokantaan. Jokaisen tietojen säilö, joka järjestää tiedot tietokannan kaltaisessa muodossa ja, joka sisältää vakio tietojen käyttötapoja on mahdollinen tietojen lähteen candidate sync Engine. Tietoja säilöjen tietoihin, jotka synkronoidaan sync engine kutsutaan **yhteys tietolähteisiin** tai **yhdistetty kansioita** (CD-levy).

Synkronoi-ohjelma kapseloi vuorovaikutus yhdistetty tietolähteeseen, moduulista, kutsutaan **yhdistin**. Kullakin virhelajilla on yhdistetty tietolähteeseen on tietyn yhdistin. Yhdistimen kääntää tarvittava toiminto, joka on yhdistetty tietolähteeseen ymmärtää-muotoon.

Yhdistimien soittaa API Exchange tunnistetietoja (sekä lukeminen ja kirjoittaminen) yhdistetty tietolähteeseen. On myös mahdollista lisätä mukautetun yhdistimen extensible connectivity framework avulla. Seuraavassa kuvassa näkyy, kuinka yhdistin yhdistää yhdistetty tietolähde synkronointi-ohjelma.

![Ark1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

Tiedonkulku voit jompaankumpaan suuntaan, mutta sitä ei voi samanaikaisesti molempiin suuntiin flow. Toisin sanoen yhdistin on määritetty sallimaan data flow sync engine yhdistetyn tietolähteen tai sync engine yhdistetty tietolähteeseen, mutta vain yksi toimintoihin voi ilmetä, kun yksi objekti ja määritteen tiettynä hetkenä. Suunta voi olla erilainen eri objektien ja eri määritteet.

Voit määrittää yhdistimen, Määritä objektityypit, jonka haluat synkronoida. Objektit, jotka sisältyvät synkronointiprosessia laajuus määrittäminen objektityypit määrittää. Seuraava vaihe on Valitse määritteet, jos haluat synkronoida, joka on nimeltään määrite-luetteloon. Näitä asetuksia voi muuttaa tahansa muutosten business säännöt. Kun ohjattu asennus Azure AD Connect, nämä asetukset on määritetty puolestasi.

Yhdistetty tietolähde-objektien vieminen määrite-luetteloon on oltava vähintään yhdistetyn tietolähteen määritetyn objektityypin luominen edellyttää vähintään määritteet. Esimerkiksi **sAMAccountName** -määrite on sisällytettävä määrite luetteloon vieminen Active Directory user-objekti, koska kaikki Active Directory-käyttäjän objekteissa on oltava määritetty **sAMAccountName** -määrite. Ohjattu asennus tekee uudelleen määritysten puolestasi.

Jos yhdistetyt tiedot lähteen käyttää rakenteellisia osia, kuten osioiden tai säilöjen järjestämiseen objekteja, joita voit rajoittaa alueet lähteen yhdistetyt tiedot, joita käytetään tietyn ratkaisu.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Sisäinen rakenne sync engine nimitila
Koko sync engine nimitila koostuu kahdesta nimitilan, jota tallentaa tunnistetiedot. Kaksi nimitilan ovat seuraavat:

- Yhdistimen tilaa (CS)
- Metaverse (MA)

**Yhdistimen tila** on väliaikainen alue, joka sisältää määritetyt objektit esityksiä yhdistetty tietolähde ja määritteen sisällyttäminen luettelossa määritettyjen määritteet. Synkronointi-ohjelma käyttää yhdistimen tilaa, voit tarkistaa, mikä on muuttunut yhdistetyn tietolähteen ja vaiheen saapuvien muutokset. Synkronoi-ohjelma käyttää myös connector-tilaa vaiheen Vie lähteen yhdistetyt tiedot lähteviin muutokset. Synkronoi-ohjelma ylläpitää eri yhdistimen välilyönnillä väliaikaisen alueen kunkin yhdistimen.

Vaiheet-alueen avulla sync engine pysyy riippumaton yhdistetyn tietolähteet ja ei vaikuta niiden saatavuus ja. Tuloksena väliaikaisen alueen tietojen avulla voit käsitellä tunnistetietoja milloin tahansa. Synkronoi-ohjelma pyytää vain tehdyt muutokset sisällä yhdistetyn tietolähteen jälkeen viimeisen viestintä istunnon lopetettu tai push ulos vain muutokset tunnistetiedot, jotka on yhdistetty tietolähteeseen ei ole vielä saanut, joka vähentää verkkoliikennettä sync engine ja yhdistetyn tietolähteen välille.

Lisäksi sync engine tallentaa kaikki objektit, että se vaiheiden connector-tilaa tilatietoja. Uusien tietojen saapuessa synkronointi ohjelma laskee aina, tiedot on jo synkronoitu.

**Metaverse** on tallennustilan alue, joka sisältää yhdistetyt tiedot useista lähteistä, tarjoaa yhden Yleinen ja integroitu näkymän kaikkien yhdistetyn objektien koostetun tunnistetietoja. Metaverse objektit luodaan tunnistetiedot, jotka noudetaan yhdistetyn tietolähteet ja sääntöjä, joiden avulla voit mukauttaa synkronointiprosessia perusteella.

Seuraavassa kuvassa on yhdistimen tilaa nimitilan ja metaverse nimitilan sync engine kuluessa.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Sync engine tunnistetietojen objektit
Sync engine objekteja ovat joko objektien yhdistetyn tietolähteen esityksiä tai integroitu näkymä, joka synkronoi ohjelma on objektit. Jokainen sync engine-objekti on oltava yksilöllinen tunnus (GUID). GUID tarjoavat tietojen eheyden ja express objektien välisiä suhteita.

### <a name="connector-space-objects"></a>Yhdistimen tilaa objektien
Kun sync engine kommunikoi yhdistetty tietolähteeseen, se lukee yhdistetyn tietolähteen tunnistetietoja ja käyttää tietojen esittäminen käyttäjätieto-objekti luominen yhdistin-tilaa. Et voi luoda tai poistaa nämä objektit yksitellen. Voit kuitenkin poistaa kaikki objektit yhdistimen tilaan manuaalisesti.

Kaikissa objekteissa yhdistimen tilaa on kaksi määritteet:

- GUID-tunnus (GUID)
- DN-nimi (tunnetaan myös nimellä DN)

Jos yhdistetty tietolähde määrittää yksilölliset määrite objekti, valitse yhdistin tilaa objektien voi olla myös ankkuri-määrite. Ankkuri-määrite yksilöi objektin yhdistetty tietolähteeseen. Synkronoi-ohjelma käyttää ankkurin Etsi objektin vastaavan esittäminen yhdistetty tietolähteeseen. Sync engine oletetaan, että objektin ankkurin muuttuu koskaan elinkaaren objektin päälle.

Monia yhdistimet avulla tunnetut yksilöllisen tunnuksen luo ankkurin kullekin objektille automaattisesti, kun se on tuotu. Esimerkiksi Active Directory Connectorin käyttää ankkurin **objectGUID** -määrite. Yhdistettyjen tietolähteiden, joilla ei ole selvästi määritetty yksilöllinen voit määrittää ankkurin sukupolven osana Connector-kokoonpanon määritys.

Tapauksessa ankkurin muodostanut yhden tai useamman objektin yksilöllinen määritteiden Kirjoita kumpikaan muutokset ja yksiselitteisesti määrittää objektin yhdistimen tilaa (esimerkiksi työntekijänumero tai Käyttäjätunnus).

Yhdistimen tilaa-objektin voi olla jokin seuraavista:

- Väliaikaisen objekti
- Paikkamerkki

### <a name="staging-objects"></a>Väliaikaisen objektit
Väliaikaisen objekti edustaa nimettyjen objektityypit esiintymä yhdistetyn tietolähteestä. GUID-tunnus ja DN-nimi lisäksi väliaikaisen objekti on aina arvo, joka osoittaa objektilaji.

Ankkuri-määritteen arvon on oltava väliaikaisen objekteja, jotka on tuotu aina. Ankkuri-määritteen arvo ei ole väliaikaisen objekteja, jotka on valmisteltu juuri sync engine mukaan ja on yhdistetty tietolähteeseen luomisen yhteydessä.

Väliaikaisen objektien myös suorittaa nykyiset arvot business määritteet ja käyttötietoja tarvitsemia synkronointi ohjelma suorittaa synkronoinnin aikana. Käyttötietoja sisältää merkintöjä, jotka ilmaisevat päivitykset, jotka ovat vaiheistettu väliaikaisen objektin tyyppi. Jos väliaikaisen objekti on vastaanottanut uuden tunnistetietoja yhdistetyn tietolähteestä, joka ei ole vielä käsitelty, objekti on Vääränlainen **odottavat tuo**. Jos väliaikaisen objekti on uusi tunnistetietoja, joka ei vielä ole viedä yhdistetty tietolähteeseen, se on merkitty **odottavat Vie**.

Väliaikaisen objekti voi olla Tuo tai Vie objektin. Synkronoi-ohjelma luo Tuo objektin käyttämällä objektin vastaanotetut yhdistetyn tietolähteen tiedot. Kun sync engine saa tietoa uuden objektin, joka vastaa valittuna yhdistimen objektityypin, se luo Tuo objektin yhdistimen tilaa esittäminen objektin lähteen yhdistetyt tiedot.

Seuraavassa kuvassa näkyy Tuo-objekti, joka esittää objektin yhdistetty tietolähteeseen.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

Synkronoi-ohjelma luo Vie objektin käyttämällä metaverse objektin tiedot. Objektien vieminen viedään yhdistetty tietolähteeseen seuraava viestintä-istunnon aikana. Synkronoi-ohjelma kannalta objektien vieminen ei ole yhdistetty tietolähteeseen vielä. Tämän vuoksi Vie objektin ankkuri-määritettä ei ole käytettävissä. Sen jälkeen, kun se vastaanottaa objektin sync engine, yhdistetty tietolähde Luo ankkuri-määritteen objektin yksilöllinen arvo.

Seuraavassa kuvassa näkyy, miten Vie-objekti on luotu käyttämällä tunnistetietoja metaverse.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

Synkronoi-ohjelma vahvistaa Vie objektin mukaan reimporting objektin yhdistetyn tietolähteestä. Vie objektit muuttuvat Tuo objektit, kun sync engine vastaanottaa ne yhdistetyn tietolähteestä seuraavan tuonnin aikana.

### <a name="placeholders"></a>Paikkamerkit
Synkronoi-ohjelma käyttää tasainen nimitilan objekteja. Jotkin yhdistetyn tietolähteistä, kuten Active Directory kuitenkin käyttää hierarkkisia nimitilan. Synkronoi ohjelma käyttää voi muuntaa tietoja hierarkkisia nimitilan tasainen nimitilan, paikkamerkit säilyttää hierarkian.

Kunkin paikkamerkki objektin hierarkkisia nimi, jonka synkronoinnin ohjelma ei tuoda mutta tarvitaan muodostaa hierarkkisia nimen osa (kuten organisaatioyksikkö). Viittaukset yhdistetyn tietolähteen objekteja, jotka ovat ei väliaikaisen connector-tilaa objektien luoma puutteiden täyttämisen.

Synkronoi-ohjelma käyttää myös paikkamerkit viitatun objekteja, joita ei ole vielä tuotu tallentamiseen. Esimerkiksi jos synkronointi on määritetty käyttämään *Abbie Spencer* -objekti ja vastaanotettujen arvon hallinta-määrite on objekti, joka ei ole vielä, tuodut, kuten *CN = lehti Sperry CN = käyttäjät, Ohjauskoneen = fabrikam Ohjauskoneen = com*, hallinta-tiedot tallennetaan connector-tila-sovelluksessa paikkamerkkeinä. Jos myöhemmin hallinta-objekti on tuotu, paikkamerkin objektin korvaavat väliaikaisen objekti, joka esittää esimies.

### <a name="metaverse-objects"></a>Metaverse objektit
Metaverse-objekti sisältää koostettu näkymä, että sync engine on väliaikaisen objektit connector-tilaa. Synkronoi ohjelma luo metaverse objektien edellä mainittujen tietojen avulla Tuo objektit. Useita yhdistimen tilaa objektien voidaan linkittää yhteen metaverse-objektiin, mutta yhdistimen tilaa-objektia ei voi linkittää useita metaverse objekti.

Metaverse objekteja ei voi luoda manuaalisesti tai poistettu. Synkronoi-ohjelma poistaa automaattisesti metaverse objekteja, joita ei ole yhdistimen tilaa objektin linkittäminen connector-tilaa.

Voit yhdistää yhdistetty tietolähde objektien vastaavan objektityyppi sisällä metaverse-sync engine on extensible rakenteen, valmiiksi määritetyistä objektityypit ja liittyvän määritteiden kanssa. Voit luoda uuden objektityypit ja määritteet metaverse objekteille. Määritteet voi olla yksi- tai moniarvoisen ja määritteen tiedostotyypit voivat olla merkkijonoja, viittauksia, lukujen ja totuusarvot.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Väliaikaisen ja metaverse objektien väliset yhteydet
Nimitilan sync engine tiedonkulun on käytössä, linkki suhteen väliaikaisen ja metaverse objekteja. Väliaikaisen objekti, joka on linkitetty objekti on metaverse kutsutaan **liitetty objekti** (tai **yhdistin objekti**). Väliaikaisen objekti, joka ei ole linkitetty objekti on metaverse kutsutaan **jäsenyytesi objektin** (tai **disconnector objekti**). Liittynyt ja jäsenyytesi ehdot ovat ensisijaisia sekoittaa ei ole vastuussa tietojen tuomisen ja viemisen yhdistetyn hakemistosta yhdistimiä.

Paikkamerkkien linkitetään koskaan metaverse-objekti

Liitetty objekti käsittää väliaikaisen objekti ja sen linkitetyn yhteyttä yhteen metaverse-objekti. Liitettyjen objekteja käytetään synkronoimaan määritteiden arvot yhdistimen tilaa objektin ja metaverse objektin välillä.

Kun väliaikaisen objekti on liitetty objekti synkronoinnin aikana, voit flow määritteet väliaikaisen objektin ja metaverse objektin välillä. Määritteen työnkulku on kaksisuuntainen ja määritetään käyttämällä Tuo määrite ja vie määrite sääntöjä.

Yksittäisen yhdistimen tilaa-objektin voidaan linkittää vain yhteen metaverse objektiin. Kuitenkin metaverse kunkin objektin voidaan linkittää useita yhdistimen tilaa objektien samassa tai eri yhdistimen välilyöntejä, seuraavassa kuvassa esitetyllä tavalla.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

Linkitetyn suhteen väliaikaisen objekti ja metaverse-objekti on jatkuva ja voi poistaa vain säännöt, jotka määrität.

Irrotetussa-objekti on väliaikainen, joka ei ole linkitetty metaverse objektit. Irrotetussa objektin arvot eivät ole määritettä käsitellä jokin edelleen metaverse kuluessa. Yhdistetyn tietolähteen vastaavaa objektin määritteiden-arvot eivät päivity sync engine mukaan.

Irrotetussa objektien avulla voit tallentaa tunnistetietoja sync Engine ja käsitellä niitä myöhemmin. Väliaikaisen objektin pitäminen irrotetussa objektina yhdistimen tilaa on monia etuja. Koska järjestelmä on jo vaiheistettu objektista tarvittavat tiedot, se ei tarvitse luoda esityksen objektin uudelleen Seuraava tuonnin aikana yhdistetyn tietolähteestä. Tällä tavalla sync engine on aina tilannevedoksen on yhdistetty tietolähteeseen, vaikka ei ole yhdistetty tietolähteeseen nykyistä yhteyttä. Irrotetussa objektit voidaan muuntaa liitettyjen objekteja ja päinvastoin, säännöt, jotka määrität mukaan.

Tuo objektin luodaan irrotetussa objektina. Vie objektin on oltava liitetty objekti. Järjestelmän logiikan ottaa tämän säännön käyttöön ja poistaa kaikki Vie-objekti, joka ei ole liitetty objekti.

## <a name="sync-engine-identity-management-process"></a>Sync engine tunnistetietojen hallintaprosessi
Käyttäjätietojen hallinta-prosessin ohjausobjektit, miten tunnistetietoja päivitetään eri yhdistetyn tietolähteiden välinen. Tunnistetietojen hallinta ilmenee kolme prosessit:

- Tuo
- Synkronointi
- Vie

Tuonnin aikana sync engine arvioi saapuvat käyttäjätietoja yhdistetyn tietolähteestä. Kun muutoksia havaitaan, se luo uusia väliaikaisen objekteja tai päivittää aiemmin luodut väliaikaisen objektit synkronoinnin yhdistimen tilaa.

Synkronoinnin aikana sync engine päivittää metaverse muutokset, jotka on tapahtunut connector-tilaan ja päivittää muutokset, jotka on tapahtunut metaverse connector-tilaa.

Vie aikana sync engine Vie muutokset, jotka ovat vaiheistettu-väliaikaisen objektit ja, joka on merkitty Vie odottavat ulos.

Seuraavassa kuvassa kohtaa, johon jokainen prosesseista esiintyy tunnistetietojen tiedot muodostuvalle yhdistetyt tiedot lähteen toiseen.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Tuontiprosessi
Tuonnin aikana sync engine arvioi tunnistetietoja päivitykset. Sync engine vertaa yhdistetty tietolähteeseen, jossa väliaikaisen objektin tunnistetietojen tietoja vastaanotti käyttäjätietoja ja määrittää, onko väliaikaisen objektin edellyttää päivityksiä. Jos se on tarpeen päivittää väliaikaisen objektin uusilla tiedoilla, väliaikaisen objekti on Vääränlainen tuo odottaa.

Mukaan väliaikaisen synkronoinnin yhdistimen väliä objektien sync engine voit käsitellä käyttäjätietoja, joka on muuttunut. Tämä toimenpide on seuraavat edut:

- **Tehokas synkronointi**. Tietojen käsitteleminen synkronoinnissa määrä on pienennetty.
- **Tehokas uudelleensynkronointi**. Voit muuttaa sync engine käsittelytavat tunnistetietoja ilman muodostaminen tietolähteen synkronointi-ohjelma uudelleen.
- **Voit esikatsella synkronoinnin mahdollisuuden**. Voit esikatsella synkronoinnin, varmista, että käyttäjätietojen hallinta-prosessin oletuksia ovat oikein.

Kullekin objektille on määritetty yhdistimen synkronointi-ohjelma yrittää Etsi esitys objektin yhdistimen connector-tilaa ensin. Sync engine tarkistaa kaikki väliaikaisen objektit connector-tilaan ja yrittää löytää vastaavan väliaikaisen objekti, jossa on vastaava ankkuri-määrite. Jos ei ole aiemmin väliaikaisen objektin on toisiaan vastaavat ankkuri-määrite, synkronointi-ohjelma yrittää löytää vastaavan väliaikaisen objekti, jossa on sama DN-nimi.

Kun synkronointi ohjelma löytää väliaikaisen objekti, joka vastaa DN-nimi, mutta ankkurin mukaan, määräten tapahtuu seuraavaa:

- Jos sijaitsevat connector-tila ei ole ankkuri-sync engine poistaa tämän objektin connector-tilaa ja merkitsee metaverse-objektiin, se on linkitetty kuin **valmistelu suorittaa seuraavan synkronoinnin uudelleen**. Valitse Luo uusi tuonti-objekti.
- Jos yhdistimen tilaa sijaitsevat ankkurin, sync engine oletetaan, että objekti on joko nimetä uudelleen tai poistettu yhdistetyn hakemistossa. Määrittää yhdistimen tilaa objektin väliaikaisen, uusi DN nimi niin, että se vaiheen saapuvien objekti. Vanha objekti tulee **lyhytkestoisia**, odotetaan yhdistimen tuo uudelleennimeäminen tai poiston ratkaista.

Jos Synkronoi ohjelma etsii väliaikaisen objekti, joka vastaa yhdistimen määritetyn objektin, määrittää millaisia muutokset käyttöön. Esimerkiksi sync engine voi nimetä uudelleen tai poistaa yhdistetyn tietolähteen objekti tai se voi päivittää vain objektin määritteiden arvot.

Tuo odottavat parhaat väliaikaisen objektit, joissa on päivitetyt tiedot. Odottaa tuonnin erityyppisiä ovat käytettävissä. Tuontiprosessin saatu mukaan väliaikaisen objektin connector-tilaan on odottava tuontityypit seuraavasti:

- **Ei mitään**. Muutoksia ei ole mitään väliaikaisen objektin määritteet ovat käytettävissä. Synkronoi ohjelma ei merkitse tällaista tuo odottaa.
- **Lisää**. Väliaikaisen objekti on uuden Tuo objektin connector-tilaan. Synkronoi ohjelma merkitsee tällaista tuonti metaverse muita käsittelyyn odottaa.
- **Päivitä**. Sync engine etsii vastaavan väliaikaisen objektin connector-tilaa ja merkitsee tällaista kuin odottavat tuo metaverse voidaan käsitellä päivityksiä määritteet. Päivityksiin kuuluvat objektin uudelleennimeäminen.
- **Poista**. Sync engine etsii vastaavan väliaikaisen objektin connector-tilaa ja merkitsee tällaista kuin odottavat tuo liitettyjen objektin voi poistaa.
- **Poista/Lisää**. Synkronoi ohjelma etsii vastaavan väliaikaisen objektin connector-tilaa, mutta objektityypit eivät täsmää. Tässä tapauksessa Poista Lisää muokkaus on vaiheistettu. A Poista-Lisää muutoksen ilmaisee sync engine valmis uudelleensynkronointi objektin esiintyä, koska säännöt erilaisia arvojoukkoja käyttää tätä objektia, kun objektityyppi muutokset.

Määrittämällä väliaikaisen objektin odottavat tuo tila on mahdollista vähentää huomattavasti käsitellä synkronoinnin aikana, koska tämä sallii käsitellä vain objekteja, jotka on päivittänyt tietojen järjestelmän tiedot.

### <a name="synchronization-process"></a>Synkronointi
Synkronoinnin kuuluu kaksi liittyvät prosessit:

- Saapuvien synkronoinnin, kun metaverse sisältö päivitetään käyttämällä tietoja connector-tilaa.
- Lähtevän synkronoinnin, kun yhdistimen tilan sisältö päivitetään metaverse tietojen avulla.

Käyttämällä vaiheistettu yhdistimen tilan tietoja saapuvien synkronointiprosessia Luo metaverse integroitu näkymän tietoihin, jotka on tallennettu yhdistetyn tietolähteen. Väliaikaisen objektien tai vain ne odotetaan tuo tiedot yhdistetään, sen mukaan, miten säännöt määritetään.

Lähtevän synkronoinnin prosessin päivitykset viedä objekteja, kun metaverse objektit muuttuvat.

Saapuvien synkronoinnin Luo integroitu näkymän tunnistetiedot, jotka on vastaanotettu yhdistetyn tietolähteistä metaverse. Sync engine voit käsitellä käyttämällä uusimman käyttäjätietoja, jossa se on yhdistetty tietolähteen tunnistetietoja milloin tahansa.

**Saapuvien synkronointi**

Saapuvien synkronoinnin sisältää seuraavat prosessit:

- **Valmistelu** (kutsutaan myös **Ennuste** jos se on tärkeää lähtevä synkronoinnin valmistelun yhteydessä erottaa). Synkronoi-ohjelma luo uuden metaverse objektin väliaikaisen objektin perusteella ja linkit. Säännöstä on objektin tason toiminto.
- **Liity**. Synkronoi-ohjelma linkittää väliaikaisen objektin objektin metaverse. Liitoksen on objektin tason toiminto.
- **Tuo määrite työnkulku**. Sync engine päivittää määritteen arvot-määritteen työnkulku metaverse objektin nimi. Tuo määrite työnkulku on määrite tason toiminto, joka edellyttää väliaikaisen objektia ja metaverse objektia välisen linkin.

Säännöstä on vain prosessi, joka luo objektit metaverse. Säännöstä vaikuttaa vain Tuo objektit, jotka ovat irrotetussa objekteja. Synkronoi ohjelma luo aikana säännöstä, metaverse-objekti, joka vastaa objektin tuominen objektityypin ja muodostaa molemmat objektit luodaan näin liitetty objekti välisen linkin.

Liity-prosessin muodostaa myös Tuo objektit ja metaverse objektin välisen linkin. Kutsu osallistujat ja säännöstä välinen ero on liity-prosessi edellyttää, että Tuo-objekti on linkitetty metaverse objektin, johon säännöstä prosessin Luo uuden metaverse objektin.

Synkronoi-ohjelma yrittää Tuo objektin liittäminen metaverse objektin käyttämällä ehtoja, joka on määritetty synkronoinnin säännön määritykset.

Säännöstä ja liity-prosessien aikana sync engine linkittää metaverse-objekti, että ne liittyneet irrotetussa objekti. Jälkeen objektin tason näitä toimintoja sync engine päivittää liittyvän metaverse objektin määritteiden arvot. Tätä prosessia kutsutaan tuo määrite työnkulku.

Tuo määrite työnkulku ilmenee kaikki Tuo objektit, jotka on linkitetty metaverse-objekti ja kuljettaminen uudet tiedot.

**Lähtevän synkronointi**

Lähtevän synkronoinnin päivitykset viedä objekteja, kun metaverse objektin muuttaminen, mutta ei poisteta. Lähtevän synkronoinnin tavoitteena on arvioida, onko metaverse objekteihin tehdyt muutokset edellyttävät päivitykset väliaikaisen objekteihin yhdistimen välilyöntejä. Joissakin tapauksissa muutokset voit edellyttää, että väliaikaisen kaikki yhdistimen välilyönnit objektien voi päivittää. Vie, että ne vie objektit odottavat on merkitty väliaikaisen objekteja, jotka ovat muuttuneet. Nämä objektit myöhemmin siirretty yhdistetty tietolähteeseen viemisen aikana Vie.

Lähtevän synkronointi on kolme prosessit:

- **Valmistelu**
- **Valmistelun poistaminen**
- **Vie määrite työnkulku**

Valmistelu ja valmistelun poistaminen ovat molemmat objektin tason toimintoja. Valmistelun poistaminen määräytyy sen mukaan, koska vain valmistelu voi käynnistää sen valmistelu. Valmistelun poistaminen käynnistyy, kun valmistelu poistaa metaverse objektin ja Vie objektin välisen linkin.

Valmistelu käynnistyy aina, kun tehdyt muutokset tulevat voimaan metaverse-objekteja. Kun metaverse objekteihin tehdään muutoksia, synkronointi ohjelma suorittaa seuraavia tehtäviä valmistelun yhteydessä:

- Luo liitetyn objektit, joissa metaverse-objekti on linkitetty juuri luomasi Vie objektin.
- Anna liitetty objekti.
- Disjoin metaverse-objekti ja väliaikaisen objektit luodaan irrotetussa objekti välisten linkkien tarkasteleminen.

Jos Synkronoi ohjelma luo uuden yhdistimen objektin valmistelu edellyttää, väliaikaisen objekti, johon metaverse objekti linkitetään on aina Vie objektin, koska objektia ei vielä ole yhdistetty tietolähteeseen.

Jos valmistelu edellyttää synkronointi-ohjelma, disjoin liitetty objekti, luodaan irrotetussa objekti valmistelun poistaminen käynnistyy. Deprovisioning prosessi poistaa objektin.

Aikana valmistelun poistaminen, Vie objektin poistaminen fyysisesti Poista objekti. Objekti on merkitty **poistetaan**, mikä tarkoittaa, että poistotoiminto vaiheistettu objektin.

Lähtevän synkronoinnin aikana-samalla tavalla kuin tuo määrite työnkulku ilmenee, saapuvat synkronoinnissa ilmenee myös Vie määrite työnkulku. Vie määrite työnkulku tapahtuu vain metaverse ja vie objekteja, jotka ovat liittyneet välillä.

### <a name="export-process"></a>Vienti
Vie aikana sync engine käy läpi kaikki Vie objekteja, jotka on merkitty Vie yhdistimen tilaan odottaa ja lähettää päivityksiä yhdistetty tietolähteeseen.

Synkronoi-ohjelma selviää vienti onnistuu, mutta se ei voi määrittää riittävän käyttäjätietojen hallinta-prosessi on valmis. Objektien yhdistetyn tietolähteen voi muuttaa aina muiden prosessien. Koska synkronointi ohjelma ei ole pysyvä yhteys yhdistetty tietolähteeseen, ei ole riittäviä tehdä oletuksia objektin ominaisuuksien perusteella viemistä ilmoituksen yhdistetty tietolähteeseen.

Esimerkiksi yhdistetyn tietolähteen prosessi voi muuttua objektin määritteet takaisin alkuperäiset arvot (eli yhdistetyn tietolähteen voitu korvaa arvot heti, kun tiedot on sync engine mukaan miten ja asensi yhdistetty tietolähteeseen).

Synkronoi ohjelma tallentaa vieminen ja tuominen eri väliaikaisen objekteihin tilatietoja. Jos attribuuteista, jotka on määritelty määritettä luetteloon arvot ovat muuttuneet viimeisen viennin, tallennustilaan tuominen ja vieminen tila mahdollistaa synkronointi ohjelma, keskustelevat asianmukaisesti. Synkronoi ohjelma käyttää tuontiprosessin Vahvista määritteiden arvot, jotka on viety yhdistetty tietolähteeseen. Tuodut ja viedä tietoja vertailu seuraavassa kuvassa esitetyllä tavalla avulla voit määrittää, onko vienti onnistui tai jos se on toistettava sync engine.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Esimerkiksi jos sync engine Vie määrite C, jonka arvon 5, on yhdistetty tietolähteeseen, se tallentaa C = 5 sen vie tilaa muistiin. Kunkin muita viennin valitsemalla tämä objekti tulokset yritettiin viedä yhdistetty tietolähteeseen C = 5 uudelleen koska sync engine oletetaan, että tämä arvo ei ole pysyvästi kohdistettu objektiin (eli, ellei jokin toinen arvo on tuotu viimeksi yhdistetyn tietolähteestä). Vie-muistia ole valittu, kun C = 5 vastaanotetaan objektin tuontitoiminnon aikana.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [Azure AD Connect synkronointi](active-directory-aadconnectsync-whatis.md) -määritys.

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
