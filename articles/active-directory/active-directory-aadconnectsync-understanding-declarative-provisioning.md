<properties
    pageTitle="Azure AD Connect synkronointi: tietoja määritettäviä valmistelu | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan Azure AD Connect määritettäviä valmistelun määritysmalli."
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
    ms.date="08/29/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect synkronointi: tietoja määritettäviä valmistelu
Tässä ohjeaiheessa kerrotaan Azure AD Connect määritysmalli. Mallin kutsutaan määritettäviä valmistelu ja sen avulla voit tehdä määrityksen muuttaminen vaivattomasti. Monella eri tavalla, joka on kuvattu tämän artikkelin Lisäasetukset ja useimmissa asiakkaan skenaarioissa ei tarvita.

## <a name="overview"></a>Yleiskatsaus
Määritettäviä valmistelu käsittelee peräisin lähteen yhdistetyt hakemiston objektien ja määrittää, miten objektien ja määritteiden muunnetaan lähteen kohde. Objektin käsitellään synkronointi putkijohto ja putkijohto on sama saapuvan ja lähtevän liikenteen säännöt. Saapuvan liikenteen sääntö on connector-tilasta metaverse ja lähtevällä säännöllä löytyy metaverse connector-kohtaan.

![Synkronoi myyntijakso](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

Putkijohto on useita eri moduuleissa. Yhden objektin synkronoinnin käsite vastaa yksitellen.

![Synkronoi myyntijakso](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

- Lähteen lähdeobjekti
- [Laajuus](#scope), etsii kaikki synkronoinnin säännöt, jotka ovat laajuus
- [Liittyminen](#join), määrittää yhdistimen tilaa ja metaverse välinen suhde
- [Muunna](#transform), laskee, kuinka muuntaa määritteet ja työnkulku
- [Ohittaa](#precedence)Korjaa ristiriitaiset määrite maksut
- Kohde-kohdeobjekti

## <a name="scope"></a>Laajuus
Laajuus-moduuli on arvioimisen objektin ja määrittää sääntöjä, jotka ovat laajuus ja se lisätään käsittelyä. Objektin määritteet-arvot mukaan eri synkronointi säännöt arvioidaan alueella. Esimerkiksi käytöstä käyttäjä, jolla ei ole Exchange-postilaatikkoon on eri sääntöjä kuin käytössä käyttäjän postilaatikon kanssa.  
![Laajuus](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

Alue on määritetty ryhmät ja lauseita. Lauseet ovat ryhmän sisällä. Looginen ja käytetään ryhmän kaikkia lausekkeen välillä. Esimerkiksi (osaston IT-ja maa = = Tanska). Looginen OR käytetään ryhmien välillä.

![Laajuus](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
Laajuuden Tässä kuvassa pitäisi luetaan (osaston IT-ja maa = = Tanska) tai (maa = Ruotsi). Jos ryhmä 1 tai 2 ryhmän arvioidaan true ja sitten sääntö on alueella.

Laajuus-moduulin tukee seuraavia toimintoja.

Toiminto | Kuvaus
--- | ---
NOTEQUAL YHTÄ SUURI KUIN | Merkkijono Vertaile, joka antaa tulokseksi, jos arvo on yhtä suuri kuin arvo-määritteen. Katso moniarvoinen määritteitä, ISIN ja ISNOTIN.
LESSTHAN, LESSTHAN_OR_EQUAL | Merkkijono Vertaile, joka antaa tulokseksi, jos arvo on pienempi kuin -määritteen arvon.
SISÄLTÄÄ, NOTCONTAINS | Merkkijono Vertaile, joka antaa tulokseksi, jos arvo löytyy johonkin sisällä arvoa määrite.
STARTSWITH, NOTSTARTSWITH | Merkkijono Vertaile, joka antaa tulokseksi, jos arvo on-määritteen arvon.
ENDSWITH, NOTENDSWITH | Merkkijono Vertaile, joka antaa tulokseksi, jos arvo on-määritteen arvon loppuun.
GREATERTHAN, GREATERTHAN_OR_EQUAL | Merkkijono Vertaile, joka antaa tulokseksi, jos arvo on suurempi kuin-määritteen arvon.
ISNULL-ISNOTNULL | Arvioi, jos määrite ei ole-objekti. Jos määrite ei ole ja näin ollen null-sääntö on laajuus.
ISIN, ISNOTIN | Laskee parametrin arvon, jos arvo on määritetty määrite. Tämä toiminto on yhtä suuri kuin ja NOTEQUAL moniarvoinen variaation. Määrite pitäisi olla moniarvoinen määritettä ja arvo löytyvät määritteiden arvot, jos sääntö on alueella.
ISBITSET, ISNOTBITSET | Arvioi, jos tietty bitti on määritetty. Esimerkiksi avulla voidaan arvioida bittien userAccountControl nähdäksesi, jos käyttäjä on käytössä tai poissa käytöstä.
ISMEMBEROF, ISNOTMEMBEROF | Arvon on oltava DN connector-tilaa ryhmään. Jos objekti on määritetty ryhmän jäsen, sääntö on alueella.

## <a name="join"></a>Liittyminen
Synkronoi putkijohto liity-moduulissa on vastuussa etsiminen objektin lähde- ja objektin suhde kohde. Saapuvan liikenteen sääntö tätä yhteyttä olisi objektin etsiminen objektin suhde metaverse yhdistimen välilyönti.  
![Liittyminen cs ja ma välillä](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
Tavoitteena on artikkelissa, jos on objektin jo metaverse luoma toisen yhdistimen, se liittyy. Esimerkiksi-tilin resurssin metsää tilin metsää käyttäjien olisi liitettävä resurssin metsää käyttäjien kanssa.

Liitokset käytetään useimmiten saapuvan liikenteen säännöt-yhdistin tilaa objektien liitettäväksi metaverse samaan objektiin.

Liitos on määritetty vähintään yksi ryhminä. Ryhmän sinun on lauseita. Looginen ja käytetään ryhmän kaikkia lausekkeen välillä. Looginen OR käytetään ryhmien välillä. Ryhmien käsitellään järjestykseen ylhäältä alaspäin. Kun ryhmä on löytänyt täsmälleen yhden objektin vastaa kohde-, ei liity-säännöt käsitellään. Jos nolla tai enemmän kuin yksi objekti löytyy, käsittely säilyy seuraavalla sääntöryhmän. Tästä syystä sääntöjen on oltava luotu eniten eksplisiittinen ensimmäisen järjestyksessä ja lisää epäselvältä lopussa.  
![Liittyminen määritys](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
Tämä kuva liitokset käsitellään ylhäältä alas. Ensin synkronointi putkijohto näkee, onko työntekijätunnus vastinetta. Muussa tapauksessa toisen säännön näkee, jos objektit liitettäväksi voidaan tilin nimi. Jos tämä ei ole vastaavaa joko, Kolmanneksi sinun sääntö on enemmän epäselvältä vastine käyttämällä käyttäjän nimi.

Jos kaikki liity säännöt arvioitu ja yksi vastinetta ei ole täysin, käytetään **Linkkityyppi** **kuvaus** -sivulla. Jos tämä asetus on määritetty **valmistelu**, uuden objektin kohde luodaan.  
![Säännöstä tai liity](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Objektin tulee olla vain yhden yksittäisen synkronointi säännön liity sääntöjen alueessa. Jos määritettynä on useita synkronointi sääntöjä, jossa liity on määritetty, ilmenee virhe. Ohittaa ei käytetä liity ratkaiseminen. Objektin on oltava liity säännön vaikutusalueen määritteet flow saman saapuva ja lähtevä suuntaa. Jos haluat työnkulun määritteiden, sekä saapuvan ja lähtevän samaan objektiin, tarvitset sekä saapuvan ja lähtevän synkronointi-sääntö, jonka liity.

Lähtevän liity on erikoistoimintoja, kun se yrittää valmistella objektin kohde connector-kohtaan. Yritä ensin vastakkainen liitoksen käytetään DN-määrite. Jos kohde connector-tilaa samaa DN on jo objektin, objektien liitettyinä.

Liity-moduulin arvioidaan vain, kun kun synkronointi uuden säännön tulee laajuus. Kun objekti on liitetty, se ei on disjoining, vaikka liity ehto täyttyy enää. Jos haluat disjoin objektin, synkronointi sääntö, joka liittyneet objektit on Siirry alueen ulkopuolella.

### <a name="metaverse-delete"></a>Metaverse poistaminen
Metaverse objektin säilyy pitkään muotoilussa yhden alueen kanssa **Linkkityyppi** synkronointi säännön arvoksi **säännöstä** tai **StickyJoin**. StickyJoin käytetään, kun yhdistin ei sallita valmistelu uuden objektin ja metaverse, mutta kun se on yhdistetty, se täytyy poistaa lähde-ennen metaverse objekti poistetaan.

Kun metaverse objekti poistetaan, kaikki objektit, jotka liittyvät merkityt **säännöstä** lähtevä synkronointi-sääntö on merkitty delete.

## <a name="transformations"></a>Muunnokset
Muunnokset avulla voit määrittää, miten määritteet olisi flow lähteestä kohteeseen. Kulkee voi olla jokin seuraavista **Työnkulkutyypit**: suorien, vakio tai lauseke. Suora työnkulku jatkuu määritearvo kuin – ei ole lisämuunnokset. Vakion arvo määrittää määritetyllä arvolla. Lausekkeen käyttää määritettäviä valmistelun lausekielellä express miten muunnoksen pitäisi olla. Tietoja lausekkeen kieli löytyy [tietoa määritettäviä valmistelun lausekielellä](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) artikkelissa.

![Säännöstä tai liity](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

**Käytä kerran** -valintaruutu määrittää, että määrite kannattaa määrittää vain, kun objekti luodaan. Esimerkiksi Tämä määritys voidaan alkuperäinen salasana, uuden käyttäjäobjektin.

### <a name="merging-attribute-values"></a>Yhdistäminen määritteiden arvot
Määritteen muodostuvalle on määrittääksesi, jos moniarvoinen määritteet yhdistää useita eri yhdysviivat-asetus. Oletusarvo on **päivitys**, jossa näkyy ylimpänä käsittelyjärjestyksessä synkronointi säännön olisi win.

![Yhdistä tyypit](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

On myös **yhdistää** ja **MergeCaseInsensitive**. Näiden asetusten avulla voit yhdistää eri lähteistä arvot. Esimerkiksi se voidaan yhdistää useita eri saatavista jäsenen tai proxyAddresses-määrite. Kun käytät tätä vaihtoehtoa, kaikki alueen objektin synkronointi säännöt käytettävä yhdistämisen samantyyppisten. Et voi määrittää yhden yhdistimen **päivitys** ja toisesta **Yhdistäminen** . Jos yrität, näyttöön tulee virhesanoma.

**Yhdistä** ja **MergeCaseInsensitive** välinen ero on siitä, miten haluat käsitellä kaksoiskappaleita määritteiden arvot. Synkronoi-ohjelma varmistetaan, että arvojen kaksoiskappaleet on ei ole lisätty kohdemäärite. **MergeCaseInsensitive**, jossa kaksoiskappaleet on vain ero siltä varalta, että ei käyttäjästä tulee esitä. Esimerkiksi molemmat olisi ei näy "SMTP:bob@contoso.com" ja "smtp:bob@contoso.com" kohde-määritteeseen. **Yhdistäminen** on vain katsoo useita arvoja ja tarkka arvojen ei vain ero palvelupyynnön voi olla käytössä.

**Korvaa** -vaihtoehto on sama kuin **päivitys**, mutta sitä ei käytetä.

### <a name="control-the-attribute-flow-process"></a>Ohjausobjektin määrite työnkulku-prosessi
Kun osallistuja on sama metaverse-määrite on määritetty saapuva synkronointi useita sääntöjä, järjestys käytetään määrittämään eniten. Synkronoi säännön ylimpänä käsittelyjärjestyksessä (pienin numeerinen arvo) suorittaminen osallistuja-arvo. Sama tapahtuu lähtevän liikenteen säännöt. Synkronoi kanssa suurimman ohittaa voittaa sääntö ja osallistua yhdistetyn hakemiston arvo.

Joissakin tapauksissa sijaan osallistuja arvoa, synkronointi säännön Määritä muita sääntöjä olisi toiminta. On joitakin erityisiä literaalien tässä esimerkissä käytetään.

Saapuvan liikenteen synkronoinnin säännöissä literal **NULL** voidaan osoittamassa, että työnkulku ei ole arvoa osallistuja. Toinen sääntö on alemman laskentajärjestys voi edistää arvon. Jos mikään sääntö kuulunut arvon metaverse-määrite on poistettu. Lähtevän säännön, jos **NULL** on viimeinen arvo, kun kaikki synkronoinnin säännöt ovat käsitelleet, valitse arvo on poistettu yhdistetyn hakemistossa.

Literal **AuthoritativeNull** on samanlainen kuin **Null** , mutta sillä erotuksella, järjestys-sääntöjä ei alemman voivat osallistua arvon.

Määritteen-työnkulku käyttää myös **IgnoreThisFlow**. On samanlainen kuin NULL-mielessä, että se osoittaa ei ole enää mitään, jos haluat. Ero on se, että se Poista jo olemassa olevaan arvoon kohde. On esimerkiksi määrite työnkulku on käytettävissä.

Tässä on esimerkki:

Valitse *ulos AD - käyttäjän Exchange hybrid* seuraavat kulun löytyy:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Tämä lauseke luetaan kuin: Jos sen käyttäjän postilaatikon sijaitsee Azure AD-flow Azure AD-AD-määrite. Jos et ole flow mitään takaisin Active Directory. Tässä tapauksessa se säilyttää AD olemassa olevaan arvoon.

### <a name="importedvalue"></a>ImportedValue
Funktio ImportedValue on erilainen kuin muut Funktiot, koska määritteen nimi on kirjoitettava hakasulkeisiin sijaan tarjoukset:  
`ImportedValue("proxyAddresses")`.

Yleensä synkronoinnissa määrite käyttää odotettua arvoa, vaikka se ei ole vielä viety tai virhe vastaanotettiin vietäessä ("alkuun torni"). Saapuvien synkronoinnin oletetaan, että määrite, joka ei ole vielä saavuttanut yhdistetyn kansio ilmestyy saavuttaa sen. Joissakin tapauksissa on tärkeää Synkronoi vain arvo, joka on vahvistettu yhdistetyn hakemiston ("vakiohologrammi ja tuoda torni delta").

Esimerkki funktiota löytyvät ruutuun synkronoinnin säännön *-AD – käyttäjän yleisiä Exchangesta: stä*. Exchange Hybrid lisätty Exchange online arvo olisi voidaan synkronoida vain kun on vahvistettu arvo on viety:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Järjestys
Useita synkronointi sääntöjä yritettäessä edistää saman määritteen kohteeseen ohittaa arvoa käytetään määrittämään eniten. Sääntö on suurin laskentajärjestys pienin numeerinen arvo, siirtymällä osallistuja ristiriidan määritettä.

![Yhdistä tyypit](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

Tämä järjestys voidaan määrittää tarkemmin määritettä kulkee vain pienen osan objekteja varten. Esimerkiksi ulos-ja-ruutuun-sääntöjä Varmista attribuutit käytössä tilistä (**Käyttäjän AccountEnabled**) on muista tileistä järjestys.

Ohittaa voidaan määrittää yhdistimet välillä. Joka sallii yhdistimet osallistuja-arvot ensin paremmin tiedoilla.

### <a name="multiple-objects-from-the-same-connector-space"></a>Useita objekteja saman connector-tilasta
Jos sinulla on useita objekteja metaverse sama objekti liitetään samaan connector-tilaan, järjestys on mukautetaan. Jos useita objekteja ovat saman synkronointi säännön aluetta, sync engine ei ole enää käsittelyjärjestykseen. Se on epäselvä lähteen objektissa on osallistuja metaverse arvo. Tässä määrityksessä ilmoitetaan moniselitteinen vaikka määritteet lähde on sama arvo.  
![Useita objekteja, jotka on liitetty ma samaan objektiin](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

Tässä tapauksessa sinun on muuttaa synkronoinnin säännöt laajuus, jotta lähde-objekteissa on eri synkronointi säännöt alueessa. Voit määrittää eri järjestys.  
![Useita objekteja, jotka on liitetty ma samaan objektiin](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja on artikkelissa [tietoja määritettäviä valmistelu](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)lausekkeissa lausekielellä.
- Katso, miten määritettäviä valmistelu on käytetty ulos valmiin ymmärtää [Oletusarvo-määritys](active-directory-aadconnectsync-understanding-default-configuration.md).
- Katso, miten voit muuttaa käytännön avulla [Voit tehdä muutoksia työnkulkuun oletusarvon](active-directory-aadconnectsync-change-the-configuration.md)määritettäviä valmistelu.
- Lue, miten käyttäjät ja yhteystietojen toimivat yhdessä [tietoja käyttäjät](active-directory-aadconnectsync-understanding-users-and-contacts.md)ja yhteystiedot edelleen.

**Yleistä aiheita**

- [Azure AD Connect synkronointi: tietoja ja mukauttaa synkronointi](active-directory-aadconnectsync-whatis.md)
- [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

**Ohjeaiheista**

- [Azure AD Connect synkronointi: Funktiot viittaus](active-directory-aadconnectsync-functions-reference.md)
