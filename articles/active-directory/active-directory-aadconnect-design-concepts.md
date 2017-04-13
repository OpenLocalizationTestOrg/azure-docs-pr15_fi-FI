<properties
   pageTitle="Azure AD Connect: Suunnitella käsitteitä | Microsoft Azure"
   description="Tässä ohjeaiheessa kuvataan käyttöönoton suunnittelun tiettyjen alueiden"
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.custom = "azure-ad-connect"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/13/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Rakenne käsitteitä
Tässä ohjeaiheessa tarkoituksena on alueita, joka on määritetty Azure AD Connect käyttöönotto-suunnittelun aikana. Tässä aiheessa on laaja perinpohjaisesti käsittelevään artikkeliin-tiettyjen alueiden ja käsitteiden kuvataan lyhyesti muita aiheeseen.

## <a name="sourceanchor"></a>sourceAnchor
SourceAnchor-määrite on määritetty *määritteen pysyvä objektin elinkaaren aikana*. Se on yksilöi objektin siten, että saman objektin paikallisen ja Azure AD. Määrite kutsutaan myös **immutableId** ja kaksi nimeä käytetään vaihdettavissa.

Word, joka on pysyvä, "ei voi muuttaa", on tärkeää tämän ohjeaiheen. Koska tämän määritteen arvo ei voi muuttaa, kun se on määritetty, on tärkeää valita rakenne, joka tukee käyttämässäsi skenaariossa.

Määrite käytetään seuraavissa tilanteissa:

- Kun uuden sync engine palvelimen muodostettu tai tietojen palauttaminen tilanne sen jälkeen uudelleen, tämän määritteen linkittää ennestään olevat objektit Azure AD paikalliseen objektien kanssa.
- Jos siirrät vain pilvipalveluita tunnistetietojen synkronoidun identity-mallin, tämän määritteen avulla kohteet "Kova vastine" aiemmin kohteisiin Azure AD-ympäristöön objektien kanssa.
- Jos käytät federaatio, yhdessä **userPrincipalName** -määritteen käytetään vaatimus-käyttäjä tunnistetaan yksilöllisesti.

Tässä ohjeaiheessa puheen vain sourceAnchor tietoja, miten se liittyy käyttäjille. Kaikki objektityypit sovelletaan samoja sääntöjä, mutta se on tarkoitettu vain käyttäjien Tämä ongelma on yleensä merkitystä.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Hyvä sourceAnchor-määritteen valitseminen
Määritteen arvo on noudatettava seuraavia sääntöjä:

- On pienempi kuin 60 merkkiä
    - Merkkejä ei a – ö-A-Ö tai 0-9 koodattu ja laskettu 3 merkkiä
- Ei sisällä erikoismerkkejä: & #92;! # $ % & * + / = ? ^ & #96; { } | ~ < > () '; : , [ ] " @ _
- On oltava yksilöivä
- On oltava merkkijono, kokonaisluku tai binaariluvuksi
- Ei olisi perusteella käyttäjän nimi, muutokset
- Ei kirjainkoko on merkitsevä ja välttää arvot, jotka voivat vaihdella kirjainkoko
- Määritetään, kun objekti on luotu

Jos valittu sourceAnchor ei ole merkkijonomuotoisen, Azure AD yhteyden Base64Encode määritteen varmistamiseksi ei näy erikoismerkkejä näkyviin. Jos käytät toisen liittoutumispalvelimen kuin ADFS, varmista, että voit myös Base64Encode määrite palvelimellesi.

SourceAnchor-määrite on merkitsevä. "Veikkos" arvo ei ole sama kuin "veikkos". Mutta ei pitäisi olla kaksi eri objektit, joissa on vain tapauksessa ero.

Jos sinulla on yksittäinen metsää paikallisen, valitse käytettävä määrite on **objectGUID**. Tämä on käytetään, kun käytät pika-asetuksia Azure AD Connect määrite ja käyttää DirSync määrite.

Jos toimialuepuuryhmiä on useita, älä siirrä käyttäjät metsien ja toimialueiden välinen **objectGUID** on hyvä määrite käyttää myös tässä tapauksessa.

Jos siirrät käyttäjät metsien ja toimialueiden välinen, löydät määrite, joka ei muutu tai voidaan siirtää käyttäjien kanssa siirron aikana. Suositeltu tapa on oma synteettistä määrite. Määrite, joka saattaa pitämällä järjestelmässä, joka näyttää GUID-tunnus on sopiva. Objektin luonnin aikana uusi GUID-tunnus luodaan ja leimata käyttäjä. Synkronoi ohjelma luo tämän arvon perusteella **objectGUID** ja Päivitä Lisää valitun määritteen palvelimen voidaan luoda mukautetun synkronointi säännön. Kun siirrät objektia, varmista, että voit myös kopioida sisältöä, tämä arvo.

Ratkaisu on valita aiemmin luodun määritteen tiedät ei muutu. Tavallisimmat määritteet ovat **työntekijän tunnus**. Jos pidät määrite, joka sisältää kirjaimia, varmista, että ei ole mahdollisuutta kirjaimet (isot kirjaimet pieniksi kirjaimiksi ja) voit muuttaa määritteen arvon. Virheelliset määritteet, joita ei voi käyttää Sisällytä nämä määritteet käyttäjän nimi. Avioliiton tai avioeroa nimi on tarkoitus muuttaa, mikä ei ole sallittua tämän määritteen. Tämä on myös yksi syy, miksi määritteitä, kuten **userPrincipalName**-, **Sähköposti**- ja **targetAddress** eivät ole myös mahdollista valita Azure AD Connect ohjatun asennuksen. Nämä määritteet sisältää myös @-character, mikä ei ole sallittua sourceAnchor.

### <a name="changing-the-sourceanchor-attribute"></a>Muuttaminen sourceAnchor-määrite
SourceAnchor määritteen arvo ei voi muuttaa, kun objekti on luotu Azure AD- ja tunnistetiedot on synkronoitu.

Tästä syystä Azure AD Connect koskevat seuraavat rajoitukset:

- SourceAnchor-määrite voidaan määrittää vain ensimmäisen asennuksen yhteydessä. Suorita ohjattu asennus, jos tämä asetus on vain luku-tilassa. Jos haluat muuttaa asetusta, sinun täytyy poistaa ja asenna se uudelleen.
- Jos olet asentanut toiseen Azure AD Connect-palvelimeen, on valittava sama kuin aiemmin käyttänyt sourceAnchor-määrite. Jos olet aiemmin käyttänyt Dirsync-työkalun ja siirry Azure AD Connect, **objectGUID** on käytettävä sillä se on Dirsync-työkalun käyttämä määrite.
- Jos sourceAnchor arvo muutetaan jälkeen objekti on viety Azure AD-sitten Azure AD Connect synkronointi ilmoittaa virheestä ja ei salli muutoksia, ennen kuin ongelma on korjattu objekteja ja sourceAnchor muutetaan takaisin lähde hakemistossa.

## <a name="azure-ad-sign-in"></a>Azure AD-kirjautuminen
Kun Azure AD-integraation paikallisesta hakemistosta, on tärkeää ymmärtää, kuinka synkronointiasetukset voivat vaikuttaa tapaa, jolla käyttäjä todentaa. Azure AD käyttää todennetaan userPrincipalName (UPN). Kun synkronoit käyttäjille, sinun on valittava, jota käytetään userPrincipalName arvo huolellisesti määrite.

### <a name="choosing-the-attribute-for-userprincipalname"></a>UserPrincipalName-määritteen valitseminen
Määrite tarjoamiseksi valittaessa UPN käytettäviksi Azure yksi arvo on varmistettava

- Täydellisen Käyttäjätunnuksen syntaksin (RFC 822), joka on luultavasti muodon mukainen määritteiden arvotusername@domain
- Liitteen arvojen vastaa Azure AD-johonkin varmennettu mukautetut toimialueet

Pika-asetuksissa oletettu valinta-määritteen on userPrincipalName. Jos userPrincipalName-määritteen ei sisällä arvoa haluat käyttäjien kirjautua Azure ja valitse sinun on valittava **Mukautettu asennus**.

### <a name="custom-domain-state-and-upn"></a>Mukautetun toimialueen tila ja UPN
On tärkeää varmistaa, että vahvistama täydellisen Käyttäjätunnuksen jälkiliitteen varten.

Teemu on käyttäjän contoso.com. Haluat käyttää paikallisen täydellisen Käyttäjätunnuksen John john@contoso.com kirjautumaan Azure, kun olet synkronoinut käyttäjät ja Azure Active directory-contoso.onmicrosoft.com. Voit tehdä tarvitset lisää ja tarkista kuin mukautetun toimialueen contoso.com Azure AD, ennen kuin voit aloittaa synkronoinnin käyttäjille. Jos täydellisen Käyttäjätunnuksen jälkiliitteen, John, esimerkiksi Contoso-ei täsmää varmennettu toimialueen Azure AD-Azure AD korvaa täydellisen Käyttäjätunnuksen jälkiliitteen contoso.onmicrosoft.com kanssa.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Muut reititettävä paikallisen toimialueet ja Azure AD UPN
Joissakin organisaatioissa on reititettävä toimialueet, kuten contoso.local tai yksinkertainen Yksittäinen tarra-toimialueet, kuten contoso. Et voi Azure AD-reititettävä toimialueen tarkistaminen. Azure AD Connect voidaan synkronoida vain vahvistama Azure AD. Kun luot Azure AD-kansio, Luo reititettävä toimialueen, joka tulee oletustoimialueen Azure AD esimerkiksi contoso.onmicrosoft.com. Tämän vuoksi on tarpeen Vahvista muiden reititettävä toimialue tällaisessa tapauksessa siltä varalta, että et halua synkronoida oletusarvoinen onmicrosoft.com-toimialuetta.

Lue [Lisää mukautettua toimialuenimeä Azure Active Directory](active-directory-add-domain.md) lisätietojen saamiseksi lisääminen ja tarkistaminen toimialueet.

Azure AD Connect havaitsee, jos käytössäsi on reititettävä toimialueen käyttäjät ja asianmukaisesti varoittaa siirtymästä eteenpäin Expressin asetukset. Jos käyttämälläsi-reititettävä toimialueeseen, on todennäköistä, että käyttäjät UPN on liikaa-reititettävä liitteet. Esimerkiksi, jos käytössäsi on kohdassa contoso.local, Azure AD Connect ehdottaa voit käyttää mukautettuja asetuksia sen sijaan, että käyttämällä pika-asetuksia. Mukautettujen asetusten avulla pystyt määrittämään, jota käytetään kuin UPN kirjautumaan Azure, kun käyttäjät on synkronoitu Azure AD-määrite.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
