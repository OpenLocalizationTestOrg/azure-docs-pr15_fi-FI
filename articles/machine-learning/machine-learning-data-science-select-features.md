<properties
    pageTitle="Ryhmän tietojen tiede prosessin valinnan ominaisuus | Microsoft Azure" 
    description="Tässä artikkelissa kerrotaan ominaisuudet aihe ja on esimerkkejä tietojen laajentamisesta prosessin koneen opiskelun rooli."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Ominaisuudet--työryhmän tiedot tiede prosessi (TDSP)

Tässä artikkelissa kerrotaan ominaisuudet tarkoitetaan ja esimerkkejä tietojen laajentamisesta prosessin koneen opiskelun sen rooli. Näissä esimerkeissä piirretään Azure koneen Learning Studio. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Tässä aiheessa kerrotaan ominaisuudet aihe ja on esimerkkejä sen koneen opiskelun tietojen laajentamisesta-prosessin rooli. Näissä esimerkeissä piirretään Azure koneen Learning Studio. 

Tekniset Funktiot ja ominaisuudet valinta on yksi osa kuvatut TDSP [ryhmän tietojen tiede prosessin ominaisuudet?](data-science-process-overview.md). Ominaisuuden tekniikka ja valinnan ovat osat TDSP **kehittäminen ominaisuudet** -vaihe.

* **ominaisuuden tekniikka**: Tämä toimenpide yrittää asiaa lisäominaisuuksia luominen aiemmin raaka toimintoja tiedot ja oppiminen-algoritmin ennakoivan esitysmuotoa lisäämiseksi.

* **Ominaisuudet**: Tämä prosessi valitsee alkuperäisen tietoihin liittyviä toimintoja avaimen alijoukko yrittää pienentää koulutus ongelman dimensionaalisuus.

Tavallisesti **ominaisuus tekniikka** käytetään ensin Luo lisäominaisuuksia ja valitse **Ominaisuudet** -vaihe suoritetaan taustaäänten ole merkitystä, ylimääräisiä tai erittäin Korreloidun ominaisuuksia.


## <a name="filtering-features-from-your-data---feature-selection"></a>Suodatusominaisuuksista tiedoista - ominaisuudet 

Ominaisuudet on prosessi, jota käytetään usein koulutus tietojoukkoja ennakoivan mallinnus tehtävien luokittelu tai regression tehtäviä, kuten rakentaminen. Tavoitteena on alijoukkoa ominaisuuksia luettelosta alkuperäinen tietojoukko, siinä olevat mitat pienentäminen ominaisuuksia vähimmäismäärää edustavan varianssin tietojen enimmäismäärän. Tämän osan ominaisuuksia ovat sitten sisällytetään kouluttaminen mallin vain ominaisuuksia. Ominaisuudet on kaksi tärkeimmät tarkoitusta.

* Ensin ominaisuudet suurentaa usein poistamalla ole merkitystä ja tarpeettomat luokitus tarkkuutta tai Korreloidun erittäin ominaisuuksia.
* Se vähentää ominaisuuksien määrän, joka tekee mallin koulutusprosessin tehokkaamman. Tämä on erityisen tärkeää oppijat, jotka ovat esimerkiksi tuki vector koneet kouluttaminen.

Vaikka ominaisuudet Etsi käytettävä kouluttaminen mallin tietojoukko toimintojen Vähennä, sitä ei yleensä viitataan termi "dimensionaalisuus vähentäminen". Ominaisuuden valinnan menetelmät Pura alkuperäisen ominaisuuksia tietojen alijoukkoa muuttamatta niitä.  Dimensionaalisuus vähentäminen menetelmät lomakkeen rakenne ominaisuudet, joita voit Muunna alkuperäisen ominaisuudet ja muokkaa niitä. Dimensionaalisuus vähentäminen menetelmiä esimerkiksi lyhennys osan analyysi, canonical korrelaatio analysointia ja yksittäinen arvo Hajotus.

Yksi yleisesti käytetty luokka ominaisuus valinnan tavoista valvonnan alaisena kontekstissa, muun muassa kutsutaan "perustuvan suodattimen ominaisuudet". Arvioidaan kullekin ominaisuudelle ja kohdemääritettä korrelaatio näistä tavoista Käytä pistemäärän liittäminen kullekin ominaisuudelle tilastollinen mitta. Ominaisuuksien luokittelu sitten tuloksen, joka voidaan määrittää pitäminen tai poistamisesta tietyn ominaisuuden raja-arvon mukaan. Esimerkkejä näistä tavoista käyttää kokonaisarvoja ovat henkilön korrelaatio, keskinäinen ja Chi-neliön testi.

Azure koneen Learning Studiossa on tarkoitettu ominaisuudet moduulit. Seuraavassa kuvassa esitetyllä näitä moduuleja ovat [Suodatin-pohjainen ominaisuudet] [ filter-based-feature-selection] ja [Fisher lineaarinen Discriminant analyysi][fisher-linear-discriminant-analysis].

![Ominaisuuden valinnan Esimerkki](./media/machine-learning-data-science-select-features/feature-Selection.png)


Harkitse esimerkiksi [Suodatin-pohjainen ominaisuudet] käyttöä[ filter-based-feature-selection] moduuli. Helppokäyttöisyys, varten on edelleen käyttää tekstin kaivos esimerkin edellä kuvattujen. Oletetaan, että haluamme luonnissa regression mallin 256 ominaisuusjoukko – [Ominaisuus hajautuksessa] luomisen jälkeen[ feature-hashing] moduulin ja että vastauksen muuttuja on "Sar1" ja kirjan Tarkista luokitusten väliltä 1-5. Määrittämällä "Ominaisuus tulosmalli menetelmä" on "Pearsonin tulomomenttikorrelaatiokertoimen", "kohdesarakkeessa" "Sar1" ja "Määrän haluamasi ominaisuudet" 50. Valitse moduuli [Suodatin-pohjainen ominaisuudet] [ filter-based-feature-selection] tuottaa tietojoukon sisältävä 50 ominaisuudet ja kohdemääritteen "Sar1". Seuraavassa kuvassa on tämän kokeen ja vain kuvattu syöteparametrit.

![Ominaisuuden valinnan Esimerkki](./media/machine-learning-data-science-select-features/feature-Selection1.png)

Seuraavassa kuvassa on syntynyt tietojoukkoja. Kullekin ominaisuudelle on tulos, Pearson korrelaatio itse ja kohdemääritettä "Sar1" perusteella. Ominaisuudet, joita yläreunan tulosten säilytetään.

![Ominaisuuden valinnan Esimerkki](./media/machine-learning-data-science-select-features/feature-Selection2.png)

Valittujen ominaisuuksien vastaavan tulosten on esitetty seuraavassa kuvassa:

![Ominaisuuden valinnan Esimerkki](./media/machine-learning-data-science-select-features/feature-Selection3.png)

Käyttämällä [Suodatin-pohjainen ominaisuudet] [ filter-based-feature-selection] moduulissa 50 poissa 256 ominaisuudet on valittu, koska ne on eniten Korreloidun ominaisuudet, joita kohde muuttujan "Sarake1" perusteella tulosmalli menetelmä "Pearsonin tulomomenttikorrelaatiokertoimen".

## <a name="conclusion"></a>Tekemistä
Ominaisuuden tekniset Funktiot ja ominaisuudet on kaksi yleisesti muutetaan ja valittujen ominaisuuksien tehokkuuden koulutus prosessin joka purkaa tärkeimmät tiedot yrittää sisältämät tiedot. Ne parantavat myös näiden mallien power luokitella syöttötiedot tarkasti ja halutut tulokset ennustetaan Lisää robustly. Jotta oppimista Lisää laskennallisesti tractable voit myös yhdistää ominaisuus tekniikka ja valinta. Se tekee parantaminen ja vähentää ominaisuuksien tarvitaan kalibroida tai kouluttaminen mallin. Matemaattisesti puhua valittuna mallia kouluttaminen ominaisuudet ovat vähimmäismäärää riippumattoman muuttujan, joka kerrotaan tietojen kuvioiden ja ennustaa tulosten onnistuneesti.

Huomaa, että aina ei ole välttämättä suorittamiseen tekniikka tai ominaisuus ominaisuudet. Onko tarvitaan vai ei määräytyy emme ole tai kerätä tietoja, emme Valitse algoritmin ja kokeilla tavoitteena.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
 
