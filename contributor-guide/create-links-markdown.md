<properties
   pageTitle="Linkkien luominen korotuksia artikkelit" description="Tässä artikkelissa kuvataan korotuksia crosslinks koodi." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="02/03/2015" ms.author="tysonn" />

# <a name="linking-guidance-for-azure-technical-content"></a>Linkittäminen Azure teknistä sisältöä ohjeet

### <a name="links-from-one-acom-article-to-another"></a>Linkit ACOM artikkelissa toiseen

Voit luoda tekstiin linkki tekniset ACOM-artikkelista toiseen ACOM tekniset artikkeliin käyttämällä linkki seuraavaa syntaksia:  

- Artikkelin service-Sivustohakemiston linkkien toiseen artikkeliin palvelun samassa kansiossa:

  `[link text](article-name.md)`

- Artikkelissa linkit palvelun alikansio pääkansiossa artikkeleihin:

  `[link text](../article-name.md)`

- Artikkelin pääkansion Sivustohakemiston linkkien palvelun alikansiossa artikkeleihin: 

  `[link text](./service-directory/article-name.md)`

- Palvelun alikansion linkit artikkeleihin toisen palvelun alikansiossa artikkelissa:

  `[link text](../service-directory/article-name.md)`
 

## <a name="links-to-anchors"></a>Ankkurit linkit

Sinun ei tarvitse luoda ankkurit – ne luodaan automaattisesti kaikki H2 otsikot aika julkaista. Sinun on tehtävä riittää on luoda linkkejä H2-osiin.

- Linkittäminen saman artikkelin otsikkoon:

  `[link](#the-text-of-the-H2-section-separated-by-hyphens)`  
  `[Create cache](#create-cache)`

- Voit linkittää toiseen saman alikansiossa artikkelin ankkurin:

  `[link text](article-name.md#anchor-name)`
  `[Configure your profile](media-services-create-account.md#configure-your-profile)`

- Voit linkittää ankkurin toisen palvelun alikansiossa:

  `[link text](../service-directory/article-name.md#anchor-name)`
  `[Configure your profile](../service-directory/media-services-create-account.md#configure-your-profile)`

Linkkien luominen automaattisesti luodut ankkuri-linkit artikkeleita automatisoida yksi tapa on [MarkdownAnchorLinkGenerator - työkalun luomiseen ankkurin linkit ACOM oikeassa muodossa](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/ACOMMarkdownAnchorLinkGenerator).

## <a name="links-from-includes"></a>Linkit sisältää

Koska sisältävät tiedostot sijaitsevat toiseen kansioon, sinun on käytettävä enää suhteelliset polut alla kuvatulla tavalla. Voit linkittää artikkelin Sisällytä tiedostosta, käytä tätä muotoa:

    [link text](../articles/service-folder/article-name.md)
    
Lisätietoja käyttämisestä tiedosto [Mukautettu korotuksia tunnisteet ohjeita](custom-markdown-extensions.md#includes).

## <a name="links-in-selectors"></a>Valitsimien linkit

Jos sinulla on valitsimia, jotka on upotettu Sisällytä, käytä tämän tyyppinen linkittäminen: 

    > [AZURE. VALITSIN-luettelo (Dropdown1 | Dropdown2)]     -  [(teksti1 | Esimerkki1)](../articles/service-folder/article-name1.md)
    - [(teksti1 | Esimerkki2)](../articles/service-folder/article-name2.md)
    - [(teksti2 | Esimerkki3)](../articles/service-folder/article-name3.md)
    - [(teksti2 | Esimerkki4)](../articles/service-folder/article-name4.md)


## <a name="reference-style-links"></a>Viittaukset linkit

Voit helpottaa lukemista tietolähteen sisältöä tyylin linkkejä. Tyyli linkkejä korvaa sisäinen linkin syntaksi yksinkertaistettu syntaksi, jonka avulla voit siirtää pitkä URL-osoitteista on artikkelissa loppuun. Seuraavassa on esimerkki uskalluksen Fireball:

Sitoutuva teksti:

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

Linkki viittaukset on artikkelin lopussa:

    <!--Reference links in article-->
    [1]: http://google.com/
    [2]: http://search.yahoo.com/  
    [3]: http://search.msn.com/

Varmista, että voit lisätä tilaa jälkeen kaksoispiste ennen linkkiä. Kun linkität muiden artikkeleja, jos unohdat sisältämään tilan, linkki voidaan jakaa julkaistun artikkelin. 

## <a name="link-to-acom-pages-that-are-not-part-of-the-technical-documentation-set"></a>Linkki kohteeseen ACOM sivut, jotka eivät kuulu teknisten määrittäminen

Linkin ACOM sivulle (esimerkiksi sivun hinnoittelutiedot, SLA sivun tai muu, joka ei ole asiakirjat-artikkelissa), käytä absoluuttinen URL-osoite, mutta jättää aluekohtaisia asetuksia. Tavoite tähän on linkkejä tukevat GitHub ja hahmontamisen sivustossa:

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)


## <a name="link-to-msdn-or-technet"></a>Linkki: MSDN- tai TechNet

Kun haluat linkittää MSDN tai TechNet-koko linkin aiheeseen ja poista fi-yhteyttä kieliasetus-linkistä. 

### <a name="use-friendly-link-text-for-all-links"></a>Kaikkien linkkien helpossa muodossa linkkiteksti käyttämällä

Voit lisätä linkin sanat pitäisi olla helpossa muodossa - toisin sanoen niiden on oltava Normaali englanninkielisten sanojen tai linkitettävän sivun otsikkoon. Älä käytä "napsauttamalla tätä". Se on virheellinen Hakukoneoptimoinnin ja ei ole riittävästi kuvaa kohdetta.

**Korjaaminen:**

- `For more information, see the [contributor guide index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`

- `For more details, see the [SET TRANSACTION ISOLATION LEVEL](https://msdn.microsoft.com/library/ms173763.aspx) reference.`

**Virheellinen:**

- `For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).`

- `For more information, click [here](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`


## <a name="fwlinks"></a>FWLinks

Vältä FWLinks (uudelleenohjauksen järjestelmään) azure.microsoft.com sisällön. Niitä käytetään vain siinä tapauksessa, kun haluat luoda linkin, jonka et tiedä vielä URL-osoite-sivun. Niitä tarvitaan todella tuskin koskaan. ACOM määritetään tiedostonimi, jotta saat tietää, mikä se on, etuajassa. Kirjaston artikkelista, johon ei ole vielä julkaistu voit luoda linkin, joka käyttää aiheen GUID-tunnus, niin, että sinun ei tarvitse käyttää FW-linkki.

Jos sinun on käytettävä FW-linkki verkkosivuun, Sisällytä P-parametri, jotta se pysyvä uudelleenohjaus:

    http://go.microsoft.com/fwlink/p/?LinkId=389595

Kun liität FW-linkki-työkalun kohteen URL-osoite, muista poistaa aluekohtaisia asetuksia, jos Kohdelinkki on ACOM, tai MSDN- tai TechNet-kirjaston.

## <a name="remember-the-azure-library-chrome"></a>Muista Azure kirjaston chrome!
Jos haluat linkittää Azure kirjaston-aiheen joka sijaitsee [Tässä](https://msdn.microsoft.com/library/azure)solmun, muista määrittää Azure chrome-linkki (/ azure /). Azure chrome ACOM Siirtymisasetukset jakaa ja näyttää ainoastaan Azure sisällön MSDN-kirjastossa. Oikein kohdistetuissa linkki näyttää seuraavanlaiselta:

    http://msdn.microsoft.com/library/azure/dd163896.aspx

Muussa tapauksessa sivu hahmontaa MSDN vakionäkymässä, näkyvissä koko MSDN puun kanssa.

### <a name="contributors-guide-links"></a>Osallistujat-opas linkit

- [Yleistä artikkelissa](./../README.md)
- [Indeksi ohjeet artikkeleista](./contributor-guide-index.md)

<!--image references-->
[1]: ./media/create-tables-markdown/table-markdown.png
[2]: ./media/create-tables-markdown/break-tables.png
