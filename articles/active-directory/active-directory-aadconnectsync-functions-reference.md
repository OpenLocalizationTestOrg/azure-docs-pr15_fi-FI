<properties
    pageTitle="Azure AD Connect synkronointi: Funktiot viittaus | Microsoft Azure"
    description="Azure AD Connect synkronointi määritettäviä valmistelu lausekkeiden viittaus."
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
    ms.date="08/23/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Connect synkronointi: Funktiot viittaus

Azure AD Connect-funktioiden avulla voit käsitellä synkronoinnissa määrite-arvo.  
Syntaksi on esitetty seuraavassa muodossa:  
`<output type> FunctionName(<input type> <position name>, ..)`

Jos ylikuormitetaan funktiota voidaan käyttää useita muodot, näkyvät kaikki kelvollinen muodot.  
On erittäin kirjoitetaan ja varmistavat, että tyyppi välitetty vastineita kuvata tyyppi.  
Jos tyyppi ei täsmää, ilmenee virhe.

Tiedostotyypit ilmaistaan käyttäen seuraavaa syntaksia:

- **luokka** – binaariluvuksi
- **bool** – totuusarvo
- **dt** – UTC -ajan päivämäärä ja aika
- **luettelo** – tunnetut vakiot luettelointi
- **eksponentti** – lausekkeen, joka on tarkoitus arvo on totuusarvo
- **mvbin** – Multi-Valued binaarinen
- **mvstr** – Multi-Valued merkkijono
- **mvref** – Multi-Valued viittaus
- **num** – numeerinen
- **ref** – viittaus
- **str** – merkkijono
- **var** – variantin (lähes) mikä tahansa muu
- **Tyhjä** – ei palauta arvoa

Funktiot, joiden tyypit **mvbin**, **mvstr**ja **mvref** vain käyttää moniarvoinen määritteet. Funktiot, joiden **bin** **str**ja **ref** käyttää sekä yksittäisen arvon ja moniarvoisen määritteet.

## <a name="functions-reference"></a>Funktiot viittaus

Toimintojen luettelo | | | | |  
--------- | --------- | --------- | --------- | --------- | ---------
**Muuntaminen** |  
[CBool](#cbool) | [CDate](#cdate) | [CGuid](#cguid) | [ConvertFromBase64](#convertfrombase64)
[ConvertToBase64](#converttobase64) | [ConvertFromUTF8Hex](#convertfromutf8hex) | [ConvertToUTF8Hex](#converttoutf8hex) | [CNum](#cnum)
[CRef](#cref) | [CStr](#cstr) | [StringFromGuid](#StringFromGuid) | [StringFromSid](#stringfromsid)
**Päivämäärä / aika** |  
[DateAdd](#dateadd) | [DateFromNum](#datefromnum) | [FormatDateTime](#formatdatetime) | [Nyt](#now)
[NumFromDate](#numfromdate) |  
**Hakemisto** |  
[DNComponent](#dncomponent) | [DNComponentRev](#dncomponentrev) | [EscapeDNComponent](#escapedncomponent)
**Arviointi** |  
[IsBitSet](#isbitset) | [IsDate](#isdate) | [IsEmpty](#isempty) | [IsGuid](#isguid)
[IsNull](#isnull) | [IsNullOrEmpty](#isnullorempty) | [IsNumeric](#isnumeric) | [IsPresent](#ispresent) |
[IsString](#isstring) |  
**Matemaattiset** |  
[Bitti.ja](#bitand) | [Bitti.tai](#bitor) | [RandomNum](#randomnum)
**Moniarvoinen** |  
[Sisältää](#contains) | [Laske](#count) | [Kohteen](#item) | [ItemOrNull](#itemornull)
[Liittyminen](#join) | [RemoveDuplicates](#removeduplicates) | [Jaa](#split) |
**Valitseminen** |  
[Virhe](#error) | [IIF](#iif)  | [Vaihda](#switch)
**Teksti** |  
[GUID-TUNNUS](#guid) | [InStr](#instr) | [InStrRev](#instrrev) | [LCase](#lcase)
[Vasemmalle](#left) | [Pituus](#len) | [LTrim](#ltrim) | [Mid](#mid)
[PadLeft](#padleft) | [PadRight](#padright) | [PCase](#pcase) | [Korvaa](#replace)
[ReplaceChars](#replacechars) | [Oikealle](#right) | [RTrim](#rtrim) | [Rajaaminen](#trim)
[UCase](#ucase) | [Word](#word)

----------
### <a name="bitand"></a>Bitti.ja

**Kuvaus:**  
Bitti.ja-funktio määrittää määritetyn bittien arvo.

**Syntaksi:**  
`num BitAnd(num value1, num value2)`

- Arvo1; arvo2: numeerisia arvoja, jotka kuuluvat toimintovalitsimet yhdessä

**Huomautuksia:**  
Tämä funktio muuntaa molemmat parametrit binaarimuotoinen ja määrittää seuraavasti:

- 0 – Jos vähintään toinen vastaavan bittien *peite* ja *Merkitse* on 0
- 1 – Jos vastaavan bittien ehdot 1.

Toisin sanoen se palauttaa 0 kaikissa tapauksissa, paitsi jos molemmat parametrit on vastaava bittien ovat 1.

**Esimerkki:**  
`BitAnd(&HF, &HF7)`  
Palauttaa 7, koska heksadesimaaliluvun "F" ja "F7" antaa tulokseksi arvon.

----------
### <a name="bitor"></a>Bitti.tai

**Kuvaus:**  
Bitti.tai-funktio määrittää määritetyn bittien arvo.

**Syntaksi:**  
`num BitOr(num value1, num value2)`

- Arvo1; arvo2: numeerisia arvoja, jotka on oltava yhdistetty OR-operaattorilla yhdessä

**Huomautuksia:**  
Tämä funktio muuntaa molemmat parametrit binaarimuotoinen ja määrittää hieman 1, jos vähintään toinen vastaavan bittien peite ja Merkitse on 1 ja 0, jos vastaava bittien ehdot 0. Toisin sanoen se palauttaa 1 kaikissa tapauksissa, paitsi jos molemmat parametrit on vastaava bittien ovat 0.

----------
### <a name="cbool"></a>CBool

**Kuvaus:**  
CBool-funktio palauttaa totuusarvon arvioitu lausekkeen perusteella

**Syntaksi:**  
`bool CBool(exp Expression)`

**Huomautuksia:**  
Jos lausekkeen arvo erisuuri kuin nolla-arvo on CBool palauttaa arvon TOSI, muuten se palauttaa totuusarvon EPÄTOSI.

**Esimerkki:**  
`CBool([attrib1] = [attrib2])`  

Palauttaa arvon TOSI, jos molemmat määritteet on sama arvo.

----------
### <a name="cdate"></a>CDate

**Kuvaus:**  
CDate-funktio palauttaa merkkijonon UTC-ja. DateTime ei ole synkronoitu alkuperäisen määritteen tyyppi, mutta jotkin toiminnot käyttävät.

**Syntaksi:**  
`dt CDate(str value)`

- Arvo: Merkkijonon, jossa päivämäärä, aika- ja halutessasi aikavyöhyke

**Huomautuksia:**  
Palautetun merkkijonon on aina UTC-aika.

**Esimerkki:**  
`CDate([employeeStartTime])`  
Palauttaa DateTime perusteella työntekijän aloitusaika

`CDate("2013-01-10 4:00 PM -8")`  
Palauttaa päivämäärä ja aika, joka edustaa "2013-01 – 11 klo 24.00"

----------
### <a name="cguid"></a>CGuid

**Kuvaus:**  
CGuid-funktio muuntaa sen binaarimuotoinen GUID-tunnus string-esitys.

**Syntaksi:**  
`bin CGuid(str GUID)`

- Merkkijonon muotoiltu tätä mallia: lt-xxxx-xxxx-xxxx-xxxxxxxxxxxx tai {lt-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

----------
### <a name="contains"></a>Sisältää

**Kuvaus:**  
Contains-funktio etsii merkkijonon moniarvoinen määritteen sisään

**Syntaksi:**  
`num Contains (mvstring attribute, str search)`-merkeiksi  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-merkeiksi

- määrite: moniarvoinen määritettä etsimiseen.
- Etsi: Etsi-määritteen merkkijono.
- Casetype: CaseInsensitive tai CaseSensitive.

Palauttaa indeksin moniarvoinen määrite, jos merkkijonoa löydy. Jos merkkijonoa ei löydy, palautuu 0.

**Huomautuksia:**  
Moniarvoinen merkkijono määritteiden haku etsii alimerkkijonoja arvot.  
Viittaus määritteiden etsittävä merkkijono on oltava täsmälleen samat pidetään vastaava arvo.

**Esimerkki:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Jos proxyAddresses-määritteeseen on ensisijainen sähköpostiosoitteesi (merkitty isoja "SMTP:"), Palauta proxyAddress-määrite, muussa tapauksessa palauttaa virheen.

----------
### <a name="convertfrombase64"></a>ConvertFromBase64

**Kuvaus:**  
ConvertFromBase64-funktio muuntaa tavallisen merkkijonon määritetyn base64-koodattuja arvo.

**Syntaksi:**  
`str ConvertFromBase64(str source)`-olettaa Unicode koodaustoiminnon  
`str ConvertFromBase64(str source, enum Encoding)`

- tietolähteen: Base64-koodatun merkkijonon  
- Koodaus: Unicode-ASCII-UTF8

**Esimerkki**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Sekä esimerkkejä palauttaa "*Hei maailma!*"

----------
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex

**Kuvaus:**  
ConvertFromUTF8Hex-funktio muuntaa tekstimerkkijonon UTF8 Hex koodattu määritetyn arvon.

**Syntaksi:**  
`str ConvertFromUTF8Hex(str source)`

- tietolähteen: UTF8 2-tavuinen koodattu merkkijono

**Huomautuksia:**  
Tämä funktio ja -ConvertFromBase64([],UTF8), tulos on helppokäyttöinen DN-määritteen välinen ero.  
Tässä muodossa käyttävät Azure Active Directory DN.

**Esimerkki:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Palauttaa arvon "*Hei maailma!*"

----------
### <a name="converttobase64"></a>ConvertToBase64

**Kuvaus:**  
ConvertToBase64-funktio muuntaa tekstimerkkijonon base64 Unicode-merkkijono.  
Muuntaa arvon kokonaislukujen taulukko sen vastaavan merkkijonon esitystä, joka on koodattu base-64-numeroihin.

**Syntaksi:**  
`str ConvertToBase64(str source)`

**Esimerkki:**  
`ConvertToBase64("Hello world!")`  
Palauttaa arvon "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

----------
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex

**Kuvaus:**  
ConvertToUTF8Hex-funktio muuntaa tekstimerkkijonon koodattu UTF8 Hex-arvo.

**Syntaksi:**  
`str ConvertToUTF8Hex(str source)`

**Huomautuksia:**  
Tämä funktio esitysmuotoa käyttämä DN-määritteen muodossa Azure Active Directory.

**Esimerkki:**  
`ConvertToUTF8Hex("Hello world!")`  
Palauttaa 48656C6C6F20776F726C6421

----------
### <a name="count"></a>Laske

**Kuvaus:**  
Laske-funktio palauttaa elementtien määrä on moniarvoinen määrite

**Syntaksi:**  
`num Count(mvstr attribute)`

----------
### <a name="cnum"></a>CNum

**Kuvaus:**  
CNum-funktio tekstimerkkijonon ja palauttaa numeerisen tietotyypin.

**Syntaksi:**  
`num CNum(str value)`

----------
### <a name="cref"></a>CRef

**Kuvaus:**  
Muuntaa tekstimerkkijonon viittaus-määrite

**Syntaksi:**  
`ref CRef(str value)`

**Esimerkki:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

----------
### <a name="cstr"></a>CStr

**Kuvaus:**  
CStr-funktio muuntaa merkkijono-tietotyyppi.

**Syntaksi:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

- arvo: voi olla numeerinen arvo, viittaus määrite tai totuusarvo.

**Esimerkki:**  
`CStr([dn])`  
Voi palauttaa "cn = Esko, ohjauskoneen = contoso-ohjauskoneen = com"

----------
### <a name="dateadd"></a>DateAdd

**Kuvaus:**  
Palauttaa päivämäärän, joka sisältää päivämäärän, johon on lisätty määritetty aikaväli.

**Syntaksi:**  
`dt DateAdd(str interval, num value, dt date)`

- aikavälin: merkkijonolauseke, joka on lisättävä aikaväli haluat lisätä. Merkkijonon on oltava jokin seuraavista arvoista:
 - yyyy vuosi
 - q vuosineljännes
 - m kuukausi
 - vuoden päivä y
 - d päivä
 - w viikonpäivä
 - ww viikko
 - h tunnit
 - n minuutin
 - s toisen
- arvo: käyttöasteen, johon haluat lisätä. Voi olla positiivinen (tulevaisuudessa päivämäärät) tai negatiivinen (aiemman päivämäärän).
- Päivämäärä: edustava, johon on lisätty aikavälin päivämäärän DateTime.

**Esimerkki:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Lisää kolme kuukautta ja palauttaa DateTime, joka edustaa "2001-04-01".

----------
### <a name="datefromnum"></a>DateFromNum

**Kuvaus:**  
DateFromNum-funktio muuntaa arvon AD päivän päivämäärän muotoileminen DateTime-tyypin.

**Syntaksi:**  
`dt DateFromNum(num value)`

**Esimerkki:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Palauttaa DateTime, joka edustaa 2012-01-01 23:00:00

----------
### <a name="dncomponent"></a>DNComponent

**Kuvaus:**  
DNComponent-funktio palauttaa arvon määritetyn DN-osan siitä vasemmalle.

**Syntaksi:**  
`str DNComponent(ref dn, num ComponentNumber)`

- DN: merkitys viittaus-määrite
- ComponentNumber: Palauttaa DN-osa

**Esimerkki:**  
`DNComponent([dn],1)`  
Jos dn on "cn = Esko, ou =...," Esko palauttaa

----------
### <a name="dncomponentrev"></a>DNComponentRev

**Kuvaus:**  
DNComponentRev-funktio palauttaa arvon määritetyn DN-osan siitä oikealle (end).

**Syntaksi:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- DN: merkitys viittaus-määrite
- ComponentNumber - palauttaa DN-osa
- Asetukset: Ohjauskoneen – ohittaa kaikki komponentit kanssa "ohjauskoneen ="

**Esimerkki:**  
Jos dn on "cn = Esko, ou = Atlanta, ou = GA, ou = US, ohjauskoneen = contoso-ohjauskoneen = com" sitten  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
US palauttavat.

----------
### <a name="error"></a>Virhe

**Kuvaus:**  
Virhe-funktiota käytetään palauttamaan mukautettu virhe.

**Syntaksi:**  
`void Error(str ErrorMessage)`

**Esimerkki:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Jos määrite accountName ei ole käytössä, objektin palauttaa virheen.

----------
### <a name="escapedncomponent"></a>EscapeDNComponent

**Kuvaus:**  
EscapeDNComponent-funktio käsittelee DN yksi osa ja escapes se, jotta se voi esittää LDAP.

**Syntaksi:**  
`str EscapeDNComponent(str value)`

**Esimerkki:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Varmistetaan, että objekti voidaan luoda LDAP-hakemiston vaikka näyttönimi-määrite on merkit, jotka on ohitettuja LDAP.

----------
### <a name="formatdatetime"></a>FormatDateTime

**Kuvaus:**  
FormatDateTime-funktiota käytetään muotoilla DateTime-merkkijonon määritetyssä muodossa

**Syntaksi:**  
`str FormatDateTime(dt value, str format)`

- arvo: arvo DateTime-muodossa
- muoto: merkkijonon muuntaminen-muotoon.

**Huomautuksia:**  
Täältä löytyvät mahdolliset arvot muodossa: [Käyttäjän määrittämiä päivämäärä ja kellonaika Formats (Format-funktio)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Esimerkki:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
"2007-12-25" tulokset.

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
Voi johtaa "20140905081453.0Z"

----------
### <a name="guid"></a>GUID-TUNNUS

**Kuvaus:**  
GUID-tunnus-funktio luo uusi satunnaisia GUID-tunnus

**Syntaksi:**  
`str GUID()`

----------
### <a name="iif"></a>IIF

**Kuvaus:**  
IIF-funktio palauttaa yhden mahdolliset arvojen määritetyn ehdon perusteella.

**Syntaksi:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- ehto: mikä tahansa arvo tai lauseke, joka voi tuottaa tulokseksi tosi tai EPÄTOSI.
- arvojostosi: Jos ehto on TOSI, palautettu arvo.
- arvojosepätosi: Jos ehto on EPÄTOSI, palautettu arvo.

**Esimerkki:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Jos käyttäjä on Harjoittelijat, palauttaa käyttäjän, jolla "t-" sitä, muuta alkuun lisätään alias palauttaa käyttäjän alias on.

----------
### <a name="instr"></a>InStr

**Kuvaus:**  
InStr-funktio etsii merkkijonon ensimmäisen esiintymän alimerkkijonon

**Syntaksi:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- merkkijonovalinta: merkkijono, jota etsitään
- merkkijonovastine: merkkijono, joka löytyy
- aloittaminen: Etsi alimerkkijonon aloituskohta
- Vertaa: vbTextCompare tai vbBinaryCompare

**Huomautuksia:**  
Palauttaa sijainnin, josta alimerkkijonon löytyi tai 0, jos ei löydy.

**Esimerkki:**  
`InStr("The quick brown fox","quick")`  
Evalues 5

`InStr("repEated","e",3,vbBinaryCompare)`  
Arvo on 7

----------
### <a name="instrrev"></a>InStrRev

**Kuvaus:**  
InStrRev-funktio etsii merkkijonon alimerkkijonon edellisen esiintymän

**Syntaksi:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- merkkijonovalinta: merkkijono, jota etsitään
- merkkijonovastine: merkkijono, joka löytyy
- aloittaminen: Etsi alimerkkijonon aloituskohta
- Vertaa: vbTextCompare tai vbBinaryCompare

**Huomautuksia:**  
Palauttaa sijainnin, josta alimerkkijonon löytyi tai 0, jos ei löydy.

**Esimerkki:**  
`InStrRev("abbcdbbbef","bb")`  
Palauttaa 7

----------
### <a name="isbitset"></a>IsBitSet

**Kuvaus:**  
Funktion IsBitSet testien Jos bit asetuksena on tai ei

**Syntaksi:**  
`bool IsBitSet(num value, num flag)`

- arvo: numeerinen arvo, joka on evaluated.flag: numeerinen arvo, joka on bitin arvioidaan

**Esimerkki:**  
`IsBitSet(&HF,4)`  
Palauttaa arvon TOSI, sillä "4-bittinen on määritetty heksadesimaaliarvoa"F"

----------
### <a name="isdate"></a>IsDate

**Kuvaus:**  
Jos lauseke voi olla tulos DateTime-muodossa, valitse IsDate-funktio on TOSI.

**Syntaksi:**  
`bool IsDate(var Expression)`

**Huomautuksia:**  
Voidaan määrittää, voidaanko CDate() onnistui.

----------
### <a name="isempty"></a>IsEmpty

**Kuvaus:**  
Jos määrite on CS tai ma, mutta arvo on tyhjä merkkijono, IsEmpty-funktio suorittaa laskentaa tosi.

**Syntaksi:**  
`bool IsEmpty(var Expression)`

----------
### <a name="isguid"></a>IsGuid

**Kuvaus:**  
Jos merkkijonoa voi muuntaa GUID-tunnus, IsGuid-funktio lasketaan tosi.

**Syntaksi:**  
`bool IsGuid(str GUID)`

**Huomautuksia:**  
GUID-tunnus on määritetty seuraavan jokin näistä kuviot merkkijonona: lt-xxxx-xxxx-xxxx-xxxxxxxxxxxx tai {lt-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Voidaan määrittää, voidaanko CGuid() onnistui.

**Esimerkki:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Jos StrAttribute on GUID-muoto, Palauta binaarimuotoinen, muussa tapauksessa palauttaa Null.

----------
### <a name="isnull"></a>IsNull

**Kuvaus:**  
Jos lausekkeen arvo on Null-IsNull-funktio palauttaa arvon TOSI.

**Syntaksi:**  
`bool IsNull(var Expression)`

**Huomautuksia:**  
Määritteen-määrite puuttuminen ilmaistaan Null.

**Esimerkki:**  
`IsNull([displayName])`  
Palauttaa arvon TOSI, jos määrite ei ole CS tai ma.

----------
### <a name="isnullorempty"></a>IsNullOrEmpty

**Kuvaus:**  
Jos lauseke on null tai tyhjä merkkijono, IsNullOrEmpty-funktio palauttaa arvon TOSI.

**Syntaksi:**  
`bool IsNullOrEmpty(var Expression)`

**Huomautuksia:**  
Määritteen tämä ovat tosia, jos määrite on poissa tai ei sisällä tietoja mutta on tyhjä merkkijono.  
Tämän funktion käänteisfunktio on nimeltään IsPresent.

**Esimerkki:**  
`IsNullOrEmpty([displayName])`  
Palauttaa arvon TOSI, jos määrite ei ole käytettävissä tai on tyhjä merkkijono CS tai ma.

----------
### <a name="isnumeric"></a>IsNumeric

**Kuvaus:**  
IsNumeric-funktio palauttaa totuusarvon, joka ilmaisee, voiko lausekkeen arvioida numero tyypiksi.

**Syntaksi:**  
`bool IsNumeric(var Expression)`

**Huomautuksia:**  
Määrittää, voidaanko CNum() onnistuneen jäsentää lauseketta käytetään.

----------
### <a name="isstring"></a>IsString

**Kuvaus:**  
Jos lauseke voi tuottaa tulokseksi merkkijonotyyppi, IsString-funktio suorittaa laskentaa tosi.

**Syntaksi:**  
`bool IsString(var expression)`

**Huomautuksia:**  
Määrittää, voidaanko CStr() onnistuneen jäsentää lauseketta käytetään.

----------
### <a name="ispresent"></a>IsPresent

**Kuvaus:**  
Jos lausekkeen arvo ei ole tyhjä ja ei ole tyhjä merkkijono, IsPresent-funktio palauttaa arvon TOSI.

**Syntaksi:**  
`bool IsPresent(var expression)`

**Huomautuksia:**  
Tämän funktion käänteisfunktio on nimeltään IsNullOrEmpty.

**Esimerkki:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

----------
### <a name="item"></a>Kohteen

**Kuvaus:**  
Kohde-funktio palauttaa yhden kohteen Moniarvoinen merkkijono/määrite.

**Syntaksi:**  
`var Item(mvstr attribute, num index)`

- määrite: moniarvoinen määrite
- indeksi: indeksi kohteen Moniarvoinen merkkijono.

**Huomautuksia:**  
Kohde-funktiosta on hyötyä sisältää-funktion kanssa, koska jälkimmäisessä-funktio palauttaa hakemiston kohteen moniarvoinen määrite.

Ilmoittaa virheestä, jos indeksi on rajoitusten mukainen.

**Esimerkki:**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Palauttaa ensisijainen sähköpostiosoitteesi.

----------
### <a name="itemornull"></a>ItemOrNull

**Kuvaus:**  
ItemOrNull-funktio palauttaa yhden kohteen Moniarvoinen merkkijono/määrite.

**Syntaksi:**  
`var ItemOrNull(mvstr attribute, num index)`

- määrite: moniarvoinen määrite
- indeksi: indeksi kohteen Moniarvoinen merkkijono.

**Huomautuksia:**  
ItemOrNull-funktiosta on hyötyä sisältää-funktion kanssa, koska jälkimmäisessä-funktio palauttaa hakemiston kohteen moniarvoinen määrite.

Jos indeksi on rajojen, palauttaa Null-arvoja.

----------
### <a name="join"></a>Liittyminen

**Kuvaus:**  
Liity-funktio moniarvoinen tekstimerkkijonon ja palauttaa yhden arvon merkkijonon määritetyn erottimella kunkin kohteen väliin.

**Syntaksi:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

- määrite: moniarvoinen määritteen sisältävä yhdistetään.
- erottimen: mikä tahansa merkkijono erotetaan alimerkkijonot palautetun merkkijonon. Jos jätetään pois, välilyönnin ("") käytetään. Jos erotin on tyhjä merkkijono ("") tai ei mitään, kaikki luettelon kohteet liitetään toisiinsa ilman erottimia.

**Huomautuksia**  
Kutsu osallistujat ja jaa-funktion väliin on välistä eroa. Liity-funktio käsittelee matriisin merkkijonoja, ja yhdistää ne käyttämällä erottimen merkkijonon, voit palauttaa yhden merkkijonon. Jaa-funktio käsittelee merkkijonon ja erottaa osoitteessa erottimen palauttaa matriisin merkkijonoja. Tärkein ero on kuitenkin, että liity yhdistää merkkijonoja erottimen mikä tahansa merkkijono, Jaa vain erottaa yksittäistä merkkiä erottimen käyttäminen merkkijonojen.

**Esimerkki:**  
`Join([proxyAddresses],",")`  
Voi palauttaa:"SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

----------
### <a name="lcase"></a>LCase

**Kuvaus:**  
LCase-funktio muuntaa kaikki merkkijonon merkkien pienet kirjaimet.

**Syntaksi:**  
`str LCase(str value)`

**Esimerkki:**  
`LCase("TeSt")`  
Palauttaa arvon "testi".

----------
### <a name="left"></a>Vasemmalle

**Kuvaus:**  
Left-funktio palauttaa määritetyn määrän merkkejä merkkijonon vasemmalta.

**Syntaksi:**  
`str Left(str string, num NumChars)`

- merkkijono: palauttaa merkkejä merkkijonon
- NumChars: numero, joka määrittää merkkien määrän palauttaa merkkijonon alusta (vasen)

**Huomautuksia:**  
Merkkijonon, joka sisältää merkkijonon ensimmäisen merkin numChars:

- Jos numChars = 0, palauttaa tyhjän merkkijonon.
- Jos numChars < 0, Palauta syötteen merkkijono.
- Jos merkkijono on tyhjä, palauttaa tyhjän merkkijonon.

Jos merkkijono sisältää vähemmän kuin luvun määritetty numChars merkkejä, funktio palauttaa merkkijonon, joka on sama kuin merkkijono (, joka on, joka sisältää kaikki merkit-parametrissa 1).

**Esimerkki:**  
`Left("John Doe", 3)`  
Palauttaa arvon "Joh".

----------
### <a name="len"></a>Pituus

**Kuvaus:**  
Len-funktio palauttaa tekstimerkkijonon merkkien määrän.

**Syntaksi:**  
`num Len(str value)`

**Esimerkki:**  
`Len("John Doe")`  
Palauttaa 8

----------
### <a name="ltrim"></a>LTrim

**Kuvaus:**  
LTrim-funktio poistaa merkkijonon alussa olevat välilyönnit.

**Syntaksi:**  
`str LTrim(str value)`

**Esimerkki:**  
`LTrim(" Test ")`  
Palauttaa arvon "testi"

----------
### <a name="mid"></a>Mid

**Kuvaus:**  
Mid-funktio palauttaa määritetyn määrän merkkejä merkkijonon haluttuun kohtaan.

**Syntaksi:**  
`str Mid(str string, num start, num NumChars)`

- merkkijono: palauttaa merkkejä merkkijonon
- aloittaminen: palauttaa merkkejä merkkijonon sijainnin numero, joka määrittää aloitus
- NumChars: numero, joka määrittää merkkien määrän palauttaa merkkijonon sijainnin

**Huomautuksia:**  
Palauttaa merkkijonon sijainnin alusta alkaen numChars-merkkiä.  
Merkkijonon, joka sisältää numChars merkkejä merkkijonon alusta sijainti:

- Jos numChars = 0, palauttaa tyhjän merkkijonon.
- Jos numChars < 0, Palauta syötteen merkkijono.
- Jos Käynnistä > merkkijonon pituuden palauttaa syötteen merkkijono.
- Jos Käynnistä < = 0, syötteen merkkijono.
- Jos merkkijono on tyhjä, palauttaa tyhjän merkkijonon.

Jos tietokoneella ei ole jäljellä olevan merkkijonon sijainnin alusta numChar merkkejä kuin mahdollisimman monta merkkiä palautetaan.

**Esimerkki:**  
`Mid("John Doe", 3, 5)`  
Palauttaa arvon "hn Älä".

`Mid("John Doe", 6, 999)`  
Palauttaa arvon "Doe"

----------
### <a name="now"></a>Nyt

**Kuvaus:**  
Nyt-funktio palauttaa DateTime, joka määrittää nykyisen päivämäärän ja kellonajan tietokoneen järjestelmäpäivämäärän ja kellonajan mukaan.

**Syntaksi:**  
`dt Now()`

----------
### <a name="numfromdate"></a>NumFromDate

**Kuvaus:**  
NumFromDate-funktio palauttaa päivämäärän AD päivän päivämäärä-muodossa.

**Syntaksi:**  
`num NumFromDate(dt value)`

**Esimerkki:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Palauttaa 129699324000000000

----------
### <a name="padleft"></a>PadLeft

**Kuvaus:**  
PadLeft funktion vasemmalle-pakettien merkkijonon määritetyn pituinen, käyttämällä annettua täyttö-merkki.

**Syntaksi:**  
`str PadLeft(str string, num length, str padCharacter)`

- merkkijono: kirjoituskentän merkkijonon.
- pituus: haluamasi merkkijonon pituus kokonaisluvun.
- padCharacter: merkkijono, joka sisältää yksittäistä merkkiä käyttämään pad-merkkinä

**Huomautuksia:**

- Jos merkkijonon pituus on pienempi kuin pituus, alkuun (vasen) merkkijonon kunnes pituus on yhtä suuri pituus lisätään toistuvasti padCharacter.
- PadCharacter voi olla välilyöntiä, mutta se ei voi olla null-arvoa.
- Jos merkkijonon pituuden on yhtä suuri kuin tai suurempi kuin pituus, muuttumattomina funktio palauttaa merkkijonon.
- Jos merkkijonon pituus on suurempi tai yhtä suuri pituus, funktio palauttaa merkkijonon, joka on sama kuin merkkijonon.
- Jos merkkijonon pituus on pienempi kuin pituus, uuden merkkijonon haluamasi pituinen palautetaan numerojärjestykseen kanssa padCharacter sisältävä merkkijono.
- Jos merkkijono on tyhjä, funktio palauttaa tyhjän merkkijonon.

**Esimerkki:**  
`PadLeft("User", 10, "0")`  
Palauttaa arvon "000000User".

----------
### <a name="padright"></a>PadRight

**Kuvaus:**  
PadRight funktion oikealle-pakettien merkkijonon määritetyn pituinen, käyttämällä annettua täyttö-merkki.

**Syntaksi:**  
`str PadRight(str string, num length, str padCharacter)`

- merkkijono: kirjoituskentän merkkijonon.
- pituus: haluamasi merkkijonon pituus kokonaisluvun.
- padCharacter: merkkijono, joka sisältää yksittäistä merkkiä käyttämään pad-merkkinä

**Huomautuksia:**

- Jos merkkijonon pituus on pienempi kuin pituus, valitse padCharacter on toistuvasti perään (oikea) merkkijonon, kunnes se on yhtä suuri pituus pituus.
- padCharacter voi olla välilyöntiä, mutta se ei voi olla null-arvoa.
- Jos merkkijonon pituuden on yhtä suuri kuin tai suurempi kuin pituus, muuttumattomina funktio palauttaa merkkijonon.
- Jos merkkijonon pituus on suurempi tai yhtä suuri pituus, funktio palauttaa merkkijonon, joka on sama kuin merkkijonon.
- Jos merkkijonon pituus on pienempi kuin pituus, uuden merkkijonon haluamasi pituinen palautetaan numerojärjestykseen kanssa padCharacter sisältävä merkkijono.
- Jos merkkijono on tyhjä, funktio palauttaa tyhjän merkkijonon.

**Esimerkki:**  
`PadRight("User", 10, "0")`  
Palauttaa arvon "User000000".

----------
### <a name="pcase"></a>PCase

**Kuvaus:**  
PCase-funktio muuntaa merkkijonossa välilyöntierotin kunkin sanan ensimmäisen merkin isoiksi kirjaimiksi ja kaikki muut merkit muunnetaan pienet kirjaimet.

**Syntaksi:**  
`String PCase(string)`

**Huomautuksia:**

- Tämä funktio ei tarjoa ERISNIMI kannen muuntaa sanaa, joka on kokonaan isoilla, kuten lyhenne tällä hetkellä.

**Esimerkki:**  
`PCase("TEsT")`  
Palauttaa arvon "Testi".

`PCase(LCase("TEST"))`  
Palauttaa arvon "testi"

----------
### <a name="randomnum"></a>RandomNum

**Kuvaus:**  
RandomNum-funktio palauttaa satunnaisluvun määritetyn aikavälin välillä.

**Syntaksi:**  
`num RandomNum(num start, num end)`

- aloittaminen: numero, joka määrittää satunnaisen arvon alaraja luomiseen
- Lopeta: numero, joka määrittää satunnaisen arvon yläraja luomiseen

**Esimerkki:**  
`Random(100,999)`  
Voit palauttaa 734.

----------
### <a name="removeduplicates"></a>RemoveDuplicates

**Kuvaus:**  
RemoveDuplicates-funktio Moniarvoinen merkkijono ja varmista, että jokainen arvo on yksilöllinen.

**Syntaksi:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Esimerkki:**  
`RemoveDuplicates([proxyAddresses])`  
Palauttaa puhdistettu proxyAddress-määrite, jossa kaikki arvojen kaksoiskappaleet on poistettu.

----------
### <a name="replace"></a>Korvaa

**Kuvaus:**  
Korvaa-funktio korvaa tekstimerkkijonon johonkin toiseen merkkijonoon kaikki esiintymät.

**Syntaksi:**  
`str Replace(str string, str OldValue, str NewValue)`

- merkkijono: Korvaa arvot merkkijonon.
- OldValue: Merkkijono voit etsiä ja korvata.
- UusiArvo: Jos haluat korvata merkkijono.

**Huomautuksia:**  
Funktio tunnistaa seuraavat määräten monikerit:

- \n – uusi rivi
- \r – rivinvaihto
- \t – välilehti

**Esimerkki:**  
`Replace([address],"\r\n",", ")`  
Korvaa CRLF pilkku ja välilyönti ja saattaa johtaa "Yhden Microsoft Way, Redmond, WA, USA"

----------
### <a name="replacechars"></a>ReplaceChars

**Kuvaus:**  
ReplaceChars-funktio korvaa kaikki esiintymät ReplacePattern merkkijonon merkkejä.

**Syntaksi:**  
`str ReplaceChars(str string, str ReplacePattern)`

- merkkijono: Jos haluat korvata merkkejä merkkijonon.
- ReplacePattern: merkkijonon, joka sisältää sanaston merkeillä, jos haluat korvata.

Muoto on {lähde1}: {target1}, {lähde2}: {target2}, {lähdeN}, {targetN} missä lähteen etsiminen ja korvaaminen merkkijonossa tavoitearvojen merkin.

**Huomautuksia:**

- Funktio on määritetty lähteiden jokaiselle esiintymälle ja korvaa ne kohteet.
- Lähteen on oltava täsmälleen merkin (unicode).
- Lähde ei voi olla tyhjä tai kauemmin kuin yhden merkin (jäsennyksen virhe).
- Kohde voi olla useita merkkejä, kuten ö:oe, β:ss.
- Kohde voi olla tyhjä, joka ilmaisee, että merkki poistetaan.
- Lähteen kirjainkoko on merkitsevä, ja se täytyy tarkkaa vastinetta.
- (Pilkku) ja: (kaksoispiste) ovat varattuja merkkejä ja ei voi korvata tätä toimintoa.
- Välilyöntejä ja muut valkoinen merkit ReplacePattern merkkijonon ohitetaan.

**Esimerkki:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Palauttaa Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Palauttaa arvon "ONeil", yksi jakoviivojen määritetään poistetaan.

----------
### <a name="right"></a>Oikealle

**Kuvaus:**  
Right-funktio palauttaa määritetyn määrän merkkejä merkkijonon oikealle (end).

**Syntaksi:**  
`str Right(str string, num NumChars)`

- merkkijono: palauttaa merkkejä merkkijonon
- NumChars: numero, joka määrittää merkkien määrän palauttamaan (oikea) loppuun merkkijono.

**Huomautuksia:**  
NumChars merkit palautetaan merkkijonon viimeisen merkistä lähtien.

Merkkijonon, joka sisältää merkkijonon viimeisen numChars-merkkiä:

- Jos numChars = 0, palauttaa tyhjän merkkijonon.
- Jos numChars < 0, Palauta syötteen merkkijono.
- Jos merkkijono on tyhjä, palauttaa tyhjän merkkijonon.

Jos merkkijono sisältää vähemmän kuin luvun määritetty NumChars merkkejä, funktio palauttaa merkkijonon, joka on sama kuin merkkijonon.

**Esimerkki:**  
`Right("John Doe", 3)`  
Palauttaa arvon "Doe".

----------
### <a name="rtrim"></a>RTrim

**Kuvaus:**  
RTrim-toiminto poistaa merkkijonon lopussa olevat välilyönnit.

**Syntaksi:**  
`str RTrim(str value)`

**Esimerkki:**  
`RTrim(" Test ")`  
Palauttaa arvon "Testi".

----------
### <a name="split"></a>Jaa

**Kuvaus:**  
Jaa-funktio käsittelee erottimen erottimena merkkijonon ja on Moniarvoinen merkkijono.

**Syntaksi:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

- arvo: erotinmerkki erottaa merkkijonon.
- erottimen: yksittäinen merkki, jota käytetään erottimena.
- raja: enimmäismäärä arvoja, joita voi palauttaa.

**Esimerkki:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Palauttaa Moniarvoinen merkkijono, joka sisältää 2 osat hyötyä proxyAddress-määrite.

----------
### <a name="stringfromguid"></a>StringFromGuid

**Kuvaus:**  
StringFromGuid-funktio käsittelee binaarinen GUID-tunnus ja muuntaa sen merkkijono

**Syntaksi:**  
`str StringFromGuid(bin GUID)`

----------
### <a name="stringfromsid"></a>StringFromSid

**Kuvaus:**  
StringFromSid-funktio muuntaa sisältävän suojaustunnuksen merkkijonon DBCS-matriisin.

**Syntaksi:**  
`str StringFromSid(bin ObjectSID)`  

----------
### <a name="switch"></a>Vaihda

**Kuvaus:**  
Vaihda-funktiota käytetään palauttaa yhden arvon, joka on arvioitu ehtojen perusteella.

**Syntaksi:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- lauseke: Variant-lauseke, jonka haluat arvioida.
- arvo: arvo, joka palautetaan, jos vastaava lauseke on TOSI.

**Huomautuksia:**  
Vaihda-funktion argumenttiluettelon koostuu lausekkeita ja arvot. Lausekkeet arvioidaan vasemmalta oikealle ja tosi ensimmäiseen lausekkeeseen liittyvän arvon. Jos osat eivät ole oikein parillinen, Suorituksenaikainen virhe ilmenee.

Jos Lauseke1 on TOSI, Vaihda palauttaa arvo1. Jos lauseke-1 on EPÄTOSI, mutta lauseke-2 on TOSI, Vaihda palauttaa arvon 2 ja niin edelleen.

Vaihda palauttaa mitään Jos:

- Kaikki lausekkeet ovat tosi.
- Ensimmäisen tosi-lausekkeen on vastaava arvo on Null.

Vaihda arvioi kaikki lausekkeet, vaikka se palauttaa vain yhden. Tästä syystä olisi Katso ei-toivottujen puoli tehosteita. Esimerkiksi jos minkä tahansa lausekkeen arvioinnin tuloksena on jako nollalla-virhe, ilmenee virhe.

Arvo on myös virhefunktion, jotka palauttavat Mukautettu merkkijono.

**Esimerkki:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Palauttaa puhuttavista joissakin pää kaupungeissa kieli, palauttaa virheen.

----------
### <a name="trim"></a>Rajaaminen

**Kuvaus:**  
Poista.välit-funktion poistaa alussa ja lopussa olevat välilyönnit merkkijonosta.

**Syntaksi:**  
`str Trim(str value)`  

**Esimerkki:**  
`Trim(" Test ")`  
Palauttaa arvon "Testi".

`Trim([proxyAddresses])`  
Poistaa alussa ja lopussa olevia välilyöntejä kullekin arvolle proxyAddress-määrite.

----------
### <a name="ucase"></a>UCase

**Kuvaus:**  
UCase-funktio muuntaa merkkijonossa kaikki merkit isoiksi kirjaimiksi.

**Syntaksi:**  
`str UCase(str string)`

**Esimerkki:**  
`UCase("TeSt")`  
Palauttaa arvon "Testi".

----------
### <a name="word"></a>Word

**Kuvaus:**  
Word-funktio palauttaa sanan sisälsi merkkijonossa kuvaava erottimet ja word-luvun ja palauttaa parametrien perusteella.

**Syntaksi:**  
`str Word(str string, num WordNumber, str delimiters)`

- merkkijono: sanan palauttaa merkkijonon.
- WordNumber: mikä word numero tunnusnumero palautetaan.
- erottimien: merkkijonon, joka edustaa delimiter(s), jota käytetään tunnistavan sanat

**Huomautuksia:**  
Kukin merkkijono erotettu erottimet merkit yhden merkkijonon tunnistetaan sanat:

- Jos luku < 1, funktio palauttaa tyhjän merkkijonon.
- Jos merkkijono on tyhjä, palauttaa tyhjän merkkijonon.

Jos merkkijono on pienempi kuin luvun sanat tai merkkijono ei sisällä merkittyä erottimet sanat, funktio palauttaa tyhjän merkkijonon.

**Esimerkki:**  
`Word("The quick brown fox",3," ")`  
Palauttaa arvon "Lahtinen"

`Word("This,string!has&many separators",3,",!&#")`  
Palauttavat "on"

## <a name="additional-resources"></a>Lisäresursseja

* [Tietoja määritettäviä valmistelu lausekkeet](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD yhteyden synkronointi: Synkronoinnin asetusten mukauttamisen](active-directory-aadconnectsync-whatis.md)
* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)
