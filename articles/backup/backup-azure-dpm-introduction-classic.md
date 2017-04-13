<properties
    pageTitle="Johdanto Azure DPM varmuuskopiointi | Microsoft Azure"
    description="Johdanto varmuuskopioiminen DPM palvelinten Azure varmuuskopiointi-palvelun avulla"
    services="backup"
    documentationCenter=""
    authors="Nkolli1"
    manager="shreeshd"
    editor=""
    keywords="System Center tietojen suojauksen hallinta-tietojen suojauksen hallinta, dpm varmuuskopiointi"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="trinadhk;giridham;jimpark;markgal"/>

# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Voit varmuuskopioida työmääriä Azure kanssa DPM valmisteleminen

> [AZURE.SELECTOR]
- [Azure varmuuskopion Server](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Azure varmuuskopiointi Server (perinteinen)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (perinteinen)](backup-azure-dpm-introduction-classic.md)


Tässä artikkelissa esitellään työmääriä huolehtivat System Center tietojen suojauksen hallinta (DPM) palvelinten Microsoft Azure Varmuuskopiointi-apuohjelman avulla. Luettavaksi, tietoja:

- Azure DPM palvelimen varmuuskopiointi toiminta
- Tavoitteet varmuuskopiointi tasainen edellytykset
- Tyypillinen virheet ja miten voit käsitellä niitä
- Tuetut tilanteet

System Center DPM varmuuskopioi tiedoston ja sovelluksen tiedot. DPM varmuuskopioida tiedot olla tallennettuna nauha-levyllä, tai Azure Microsoft Azure varmuuskopioimalla varmuuskopioida. DPM toimii Azure varmuuskopioimalla seuraavasti:

- **DPM käyttöön kuin fyysinen palvelimeen tai paikalliseen virtual-koneen** — Jos DPM on otettu käyttöön, fyysistä palvelinta tai paikalliseen Hyper-V virtuaalikoneen voit varmuuskopioida tiedot Azure varmuuskopion säilöön, levy ja nauha lisäksi voit varmuuskopion.
- **DPM käyttöön kuin Azure virtuaalikoneen** – System Center 2012 R2 ja Päivitä 3, valitse DPM otetaan käyttöön Azure virtuaalikoneen nimellä. Jos DPM otetaan käyttöön kuin Azure virtuaalikoneen, voit varmuuskopioida tiedot Azure-levyjen liitetty DPM Azure virtuaalikoneen tai voit purku tietojen tallentaminen mukaan varmuuskopioiminen Azure varmuuskopiointi-säilö ylöspäin.

## <a name="why-backup-your-dpm-servers"></a>Miksi varmuuskopioida DPM palvelinten?

Liiketoiminnan etuja Azure varmuuskopioinnista, varmuuskopioiminen DPM palvelimet ovat:

- Paikallisen DPM käyttöönoton voit Azure varmuuskopiointi nauha pitkään käyttöönoton vaihtoehtoisena.
- DPM käyttöönotoissa Azure-tietokannassa Azure varmuuskopioinnin avulla voit purku tallennustilan Azure-levyltä, jonka avulla voi skaalata tallentamalla vanhat tiedot Azure varmuuskopiointi- ja uuden levyn tiedot.

## <a name="how-does-dpm-server-backup-work"></a>Miten DPM palvelimen varmuuskopiointi toimii?
Voit varmuuskopioida virtual machine ensin tiedot ajankohta tilannevedoksen tarvita. Azure varmuuskopio-palvelu aloittaa varmuuskopiointityön määritettynä ajankohtana ja käynnistää varmuuskopioinnin laajennus ottaa talteen tilannevedoksia. Varmuuskopion tunniste koordinoi saavuttamiseksi yhdenmukaisuuden Vieras-VSS-palvelussa ja käynnistää Azuren tallennustilaan palvelun blob tilannevedos-Ohjelmointirajapinta, kun yhdenmukaisuuden on saavutettu. Saat tilannevedoksen yhdenmukaisia virtuaalikoneen-levyjä ilman, että se tehdään.

Kun tilannevedoksen on tehty, tiedot siirretään Azure varmuuskopiointi-palvelun varmuuskopiointi säilö. Palvelun kestää varoen tunnistaminen ja siirtäminen lohkot, jotka ovat muuttuneet viimeisen varmuuskopiosta tekeminen varmuuskopioiden tallentamista ja verkon tehokkaasti. Kun tietojen siirto on valmis, tilannevedoksen poistetaan ja palautus-kohta on luotu. Palautus vaiheessa näkevät Azure perinteinen-portaalissa.

>[AZURE.NOTE] Linux näennäiskoneiden vain tiedoston-yhdenmukaisia varmuuskopio on mahdollista.

## <a name="prerequisites"></a>Edellytykset
Valmistele Azure varmuuskopio varmuuskopioida DPM tiedot seuraavasti:

1. **Luo varmuuskopio säilö** – Luo säilöön Azure varmuuskopiointi-konsolissa.
2. **Lataa säilö tunnistetiedot** – Azure varmuuskopion Lataa luomasi hallinta-varmenteen säilö.
3. **Asenna Backup Azure-agentti ja rekisteröidä palvelimen** – From Azure varmuuskopion, asenna agentti DPM kussakin palvelimessa ja rekisteröidä DPM palvelimen varmuuskopion säilöön.

[AZURE.INCLUDE [backup-create-vault](../../includes/backup-create-vault.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]


## <a name="requirements-and-limitations"></a>Vaatimukset (ja rajoituksia)

- DPM voi olla käynnissä fyysistä palvelinta tai Hyper-V virtual machine-asennuksen System Center 2012 SP1 tai System Center 2012 R2: n. Se on käytössä myös nimellä Azure virtual-tietokoneessa, jossa on vähintään System Center 2012 R2 ja valitse DPM 2012 R2: n päivityskokoelma 3 tai Windows-virtual machine-VMWare käytössä vähintään System Center 2012 R2 ja Update Rollup 5.
- Jos käytössäsi on DPM System Center 2012 SP1: een, sinun täytyy asentaa Update Rollup 2 System Center Data Protection Manager SP1. Tämä on pakollinen, ennen kuin asennat Azure Backup-agentti.
- DPM palvelin on oltava Windows PowerShell- ja .net Framework 4.5 asennettuna.
- DPM voit varmuuskopioida useimmat työmääriä Azure varmuuskopion. Mikä on tuettu Katso täysi luettelo Azure-varmuuskopiointi tue kohteiden alla.
- Azure varmuuskopiointi tallennettuja tietoja ei voi palauttaa "Kopioi nauha"-vaihtoehto.
- Tarvitset Azure-tili Azure varmuuskopiointi-toiminto on käytössä. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja [Azure varmuuskopiointi hinnat](https://azure.microsoft.com/pricing/details/backup/).
- Azure varmuuskopioinnista edellyttää Azure Backup Agent on asennettava palvelimissa, jotka haluat varmuuskopioida. Kunkin palvelimen on oltava vähintään 10 prosenttia varmuuskopioidaan, paikallinen lisätallennustilaa maksutta käytettävissä olevat tiedot koon. Esimerkiksi 100 gt tietoja varmuuskopioiminen edellyttää vähintään 10 Gigatavua vapaata tilaa taittopöydällä sijainnissa. Pienin arvo on 10 %, on suositeltavaa 15 % vapaata paikallisen tallennustilaa, jota käytetään välimuistin sijaintia.
- Tiedot tallennetaan Azure säilö muistiin. Ei ole voit voit takaisin ylös Azure-varmuuskopiointi vault tietojen määrää rajoitettu mutta tietolähde (esimerkiksi virtuaalikoneen tai tietokannan) koko ei kannata suurempi kuin 54,400 Gigatavua.

Tiedostotyyppejä tuetaan takaisin ylös Azure:

- Salattu (vain koko varmuuskopiot)
- Pakattu (lisäävän varmuuskopioinnin tuettu)
- Lyhyet (lisäävän varmuuskopioinnin tuettu)
- Pakattu ja lyhyet (käsitellään Sparse)

Ja nämä ei tueta:

- Kirjainkoon tiedostojärjestelmän palvelimiin ei tueta.
- Linkit (ohitettu)
- Uudelleenjäsennyskohdat (ohitettu)
- Salattujen ja pakattu (ohitettu)
- Salattujen ja lyhyet (ohitettu)
- Pakattu virta
- Lyhyet muodossa

>[AZURE.NOTE] Kohteesta-System Center 2012 DPM SP1 alkaen, voit varmuuskopioida toiminnoista, jotka on suojattu DPM Azure käyttämällä Microsoft Azure varmuuskopioinnin määrittäminen.
