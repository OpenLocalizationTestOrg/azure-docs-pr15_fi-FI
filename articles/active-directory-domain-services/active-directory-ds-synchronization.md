<properties
    pageTitle="Azure Active Directory-toimialueen palveluista: Synkronointi hallitun toimialueiden | Microsoft Azure"
    description="Synkronoinnin Azure Active Directory-toimialueen palveluista hallitun toimialueen ymmärtäminen"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Azure AD-toimialueen palveluista hallitun toimialueen synkronointi
Seuraavassa kaaviossa on kuvattu, miten synkronointi toimii Azure AD-toimialueen palveluihin hallitun toimialueet.

![Synkronoinnin topologian Azure AD-toimialueen palveluihin](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Azure AD-vuokraajan synkronointi paikallisen-hakemistosta
Azure AD Connect synkronointi voidaan synkronoida käyttäjätilit, ryhmän jäsenyyksiä ja tunnistetiedon tekstikopioon Azure AD-asiakasympäristöön. Määritteet käyttäjän tilien, kuten UPN ja paikallisen SUOJAUSTUNNUS (suojaustunnus) synkronoidaan. Jos käytät Azure AD-toimialueen palveluista, aiempien versioiden tunnistetiedon hajautusarvot NTLM ja Kerberos-todennus vaaditaan synkronoidaan myös Azure AD-vuokraajan.

Jos määrität takaisinkirjoituksen, Azure Active Directoryn muutokset synkronoidaan takaisin paikallisen Active Directory. Esimerkiksi jos muutat Azure AD-käyttäjien Omatoiminen salasanan muuttaminen ominaisuuksilla salasanasi, muutettu salasana päivitetään paikallisen oman AD-toimialue.

> [AZURE.NOTE] Käytä aina uusimman version Azure AD Connect varmistaaksesi, että sinulla on kaikki tunnetut virheet korjaukset.


## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Azure AD-vuokraajan synkronoinnin hallitun toimialueeseen
Käyttäjätilit ja ryhmäjäsenyyksiä tunnistetiedon hajautusarvot synkronoituvat Azure AD-vuokraajan Azure AD-toimialueen palveluista hallitun toimialueen. Synkronoinnin hallitaan automaattisesti. Sinun ei tarvitse määrittää, valvoa ja synkronoinnin hallinta. Synkronointi on myös yhteen-way/yksisuuntainen laatu. Hallitut toimialueesi on suurelta vain luku-minkä tahansa voit luoda mukautetun organisaatioyksiköiden lukuun ottamatta. Ei voi tehdä muutoksia vuoksi käyttäjä määritteitä, salasanat tai ryhmäjäsenyyksiä hallitun toimialueella. Tuloksena on hallitun toimialueeltasi muuttuu takaisin Azure AD-vuokraajan käänteisen synkronoida.


## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Usean metsää paikallisen ympäristön synkronointi
Monta organisaatioilla on useita tilin metsien koostuva varsin monimutkainen paikallisen identity-infrastruktuuria. Azure AD Connect tukee synkronoiminen käyttäjät, ryhmät ja tunnistetiedon hajautusarvot usean metsää ympäristöissä from Azure AD-vuokraajan.

Sen sijaan Azure AD-vuokraajan on paljon yksinkertaisempi ja tasainen nimitila. Käyttäjätilien eri toimialuepuuryhmiin yli UPN ristiriitojen ratkaiseminen, jotta käyttäjät voivat käyttää luotettavasti suojattu Azure AD-sovelluksia. Azure AD-toimialueen palvelut-hallitun toimialueen karhut Sulje resemblance Azure AD-vuokraajan. Tämän vuoksi tasainen OU-rakenne näkyy hallitun toimialueen. Kaikki käyttäjät ja ryhmät on tallennettu "AADDC käyttäjät"-säilön paikallisen toimialueen tai metsää, josta ne on synkronoitu-riippumatta. Olet ehkä määrittänyt hierarkkisia OU rakenne-ympäristöön. Kuitenkin hallitun toimialueen edelleen rakenne on yksinkertainen kiinteänä OU.


## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Poikkeukset - mitä ei ole synkronoitu hallitun toimialueen
Seuraavat objekteja tai määritteiden ei ole synkronoitu Azure AD-vuokraajan tai hallitun toimialueen:

- **Pois määritteitä:** Voit halutessasi synkronoidu paikalliseen toimialueeltasi käyttämällä Azure AD Connect Azure AD-vuokraajan tietyt määritteet ulkopuolelle. Pois jätettyjen määritteisiin eivät ole käytettävissä hallitun toimialueen.

- **Ryhmän käytännöt:** Määrittää paikallisen toimialueen ryhmäkäytännöt ei ole synkronoitu hallitun toimialueen.

- **SYSVOL Jaa:** Vastaavasti paikallisen toimialueen asiakaskoneiden sisältö ei ole synkronoitu hallitun toimialueeseen.

- **Tietokoneobjektien:** Tietokoneen objektit toimialueellesi paikalliseen tietokoneeseen ei ole synkronoitu hallitun toimialueen. Näiden tietokoneiden ei ole hallitun toimialueen luottamussuhde ja paikallisen toimialueen vain kuulut. Hallitut toimialueen löydät tietokoneen objekteja vain tietokoneille, jotka olet eksplisiittisesti toimialue-liittynyt hallitun toimialueeseen.

- **SidHistory määritteet käyttäjät ja ryhmät:** Ensisijainen käyttäjän ja ryhmän ensisijainen suojaustunnukset paikallisen toimialueen synkronoidaan hallitun toimialueen. Kuitenkin olemassa olevien käyttäjien ja ryhmien SidHistory määritteiden ei ole synkronoitu paikallisen toimialueen hallitun toimialueeseen.

- **Organisaation yksiköt (OU) rakenteet:** Määrittää paikallisen toimialueen organisaatioyksiköiden ei synkronoida hallitun toimialueen. Hallitut toimialueesi on kaksi valmiin organisaatioyksiköiden. Oletusarvon mukaan hallitun toimialueen tasainen OU rakenne on. Voit halutessasi sijaan voit [luoda mukautetun OU hallitun toimialueesi](./active-directory-ds-admin-guide-create-ou.md).


## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Miten erikoisominaisuuksia synkronoidaan hallitun toimialueen
Seuraavassa taulukossa luetellaan joitakin yleisiä määritteitä, ja kuvataan, miten ne synkronoidaan hallitun toimialueen.

| Hallitut toimialueesi määrite | Lähde | Huomautuksia |
|:---|:---|:---|
|TÄYDELLISEN KÄYTTÄJÄTUNNUKSEN|Azure AD-vuokraajan käyttäjän UPN määrite|Azure AD-vuokraajan UPN määritteen synkronoituvat on hallitun toimialueen. Tämän vuoksi luotettavasti kirjautumaan hallitun toimialueen käyttää oman UPN.|
|SAMAccountName|Käyttäjän mailNickname määritteen Azure AD-vuokraajan tai luo automaattisesti|SAMAccountName-määrite on Orpotermit mailNickname Azure AD-vuokraajan määritteestä. Jos useita käyttäjätilejä, joilla on sama mailNickname-määrite, SAMAccountName on automaattisesti luodut. Jos käyttäjän mailNickname tai UPN etuliite on enintään 20 merkkiä, SAMAccountName on automaattisesti luodut tyydyttämiseksi SAMAccountName määritteissä 20 merkkiä.|
|Salasanat|Käyttäjän salasanan Azure AD-vuokraajan|Azure AD-vuokraajan synkronoitujen tunnistetiedon hajautusarvot vaaditaan NTLM tai Kerberos-todennus (kutsutaan myös täydentävät tunnistetiedot). Jos Azure AD-vuokraajan on synkronoitu palvelutili, kyseiset tunnistetiedot ovat Orpotermit paikallisen toimialueen.|
|Ensisijainen käyttäjän/ryhmän SUOJAUSTUNNUS|Automaattisesti luodut|Ensisijainen SUOJAUSTUNNUS käyttäjän/ryhmän tileissä on automaattisesti luodut hallitun toimialueen. Tämän määritteen vastaa ensisijainen SUOJAUSTUNNUS paikallisen oman objektin käyttäjän/ryhmän AD-toimialue. Tämä johtuu hallitun toimialueen on eri SUOJAUSTUNNUS nimitila kuin paikallisen toimialueen.|
|SUOJAUSTUNNUS historiaa käyttäjät ja ryhmät|Paikallisen ensisijaisen käyttäjien ja ryhmien SUOJAUSTUNNUS|Käyttäjien ja ryhmien hallitun toimialueesi SidHistory-määrite on määritetty vastaamaan vastaavan ensisijainen käyttäjän tai ryhmän paikallisen toimialueen SUOJAUSTUNNUS. Tämä ominaisuus auttaa helpottaa hissin ja vaihto paikallisen sovellusten hallitun toimialueeseen, sillä sinun ei tarvitse uudelleen Käyttöoikeusluettelon resurssit.|

> [AZURE.NOTE] **Täydellisen Käyttäjätunnuksen muodossa hallitun toimialueen kirjautuminen:** SAMAccountName-määrite voi olla automaattisesti luodut joitakin hallitun toimialueen käyttäjätilit. Jos useat käyttäjät, joilla on sama mailNickname-määrite tai käyttäjillä on liian pitkä UPN etuliitteiden, SAMAccountName nämä käyttäjät voi olla automaattisesti luodut. Tämän vuoksi SAMAccountName-muodossa (esimerkiksi CONTOSO100\joeuser) ei aina luotettavasti kirjautumaan toimialueen. Käyttäjien automaattisesti luodut SAMAccountName voi vaihdella niiden UPN etuliite. Täydellisen Käyttäjätunnuksen muodossa (esimerkiksi 'joeuser@contoso100.com') kirjautumaan hallitun toimialueen luotettavasti.


## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Objektit, joissa ei ole synkronoitu, Azure AD-vuokraajan hallitun toimialueeltasi
Tämän artikkelin edellisessä osassa kuvattua ei synkronoida hallitun toimialueeltasi takaisin Azure AD-vuokraajan. Voit halutessasi luoda [Mukautettu organisaatioyksikkö (OU)](./active-directory-ds-admin-guide-create-ou.md) hallitun toimialueesi. Voit lisäksi luoda organisaatioyksiköiden, käyttäjille, ryhmät tai palvelutilejä sisällä näitä mukautettuja organisaatioyksiköiden. Mukautetun organisaatioyksiköiden puitteissa luotujen objektien on synkronoitu takaisin Azure AD-vuokraajan. Nämä objektit ovat käytettävissä vain hallitun toimialueellasi sijaitseville. Tämän vuoksi nämä objektit eivät näy Azure AD PowerShellin cmdlet-komennot, Azure AD-kaavio-Ohjelmointirajapinnan käyttäminen tai Azure AD-hallinnan Käyttöliittymän avulla.


## <a name="related-content"></a>Aiheeseen liittyvää sisältöä
- [Ominaisuudet - Azure AD-toimialueen palvelut](active-directory-ds-features.md)

- [Käyttöönottoskenaariot - Azure AD-toimialueen palveluista](active-directory-ds-scenarios.md)

- [Huomioitavaa Azure AD-toimialueen palveluista verkko](active-directory-ds-networking.md)

- [Azure AD-toimialueen palveluiden käytön aloittaminen](active-directory-ds-getting-started.md)
