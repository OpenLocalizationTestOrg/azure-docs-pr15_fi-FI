<properties
    pageTitle="Azure AD Connect: Tuetut topologioissa | Microsoft Azure"
    description="Tämän artikkelin tiedot tuetut ja joita ei tueta topologioissa Azure AD Connect:"
    services="active-directory"
    documentationCenter=""
    authors="AndKjell"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="billmath"/>

# <a name="topologies-for-azure-ad-connect"></a>Azure AD topologioissa yhdistäminen
Tässä ohjeaiheessa tavoitteena on kuvaus erilaisista paikallisista ja Azure AD-topologioissa Azure AD Connect synkronoinnin avaimen integrointi ratkaisuksi. Se kuvaa tuetut-ja joita ei tueta.

Asiakirjan kuvat selite:

Kuvaus | Kuvake
-----|-----
Paikallisen Active Directory metsää| ![AD](./media/active-directory-aadconnect-topologies/LegendAD1.png)
Active Directory suodatetut tuontia| ![AD](./media/active-directory-aadconnect-topologies/LegendAD2.png)
Azure AD Connect synkronointi palvelimeen| ![Synkronointi](./media/active-directory-aadconnect-topologies/LegendSync1.png)
Azure AD Connect synkronointi "väliaikaisen palvelintila"| ![Synkronointi](./media/active-directory-aadconnect-topologies/LegendSync2.png)
GALSync FIM2010 tai MIM2016| ![Synkronointi](./media/active-directory-aadconnect-topologies/LegendSync3.png)
Azure AD Connect synkronointi palvelimen yksityiskohtaiset| ![Synkronointi](./media/active-directory-aadconnect-topologies/LegendSync4.png)
Azure Active directory |![AAD](./media/active-directory-aadconnect-topologies/LegendAAD.png)
Skenaario, joita ei tueta | ![Ei tueta](./media/active-directory-aadconnect-topologies/LegendUnsupported.png)

## <a name="single-forest-single-azure-ad-tenant"></a>Yksi metsää, yksi Azure AD-vuokraajan
![Yhden metsää yhtä vuokraajan](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

Yleisimmät topologian on yksittäinen metsää paikallisen, yksi tai useita toimialueita, ja yhden Azure AD vuokraaja. Azure AD-todennuksessa käytetään salasanojen synkronoinnin. Azure AD Connect pika-asennus tukee vain tämän topologian.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Yksittäisen metsää useita synkronointi-palvelinten yhden Azure AD-vuokraajan
![Yhden metsää suodatettu ei tueta](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Ei tueta on useita Azure AD Connect synkronointi palvelimia yhdistetty samaan Azure AD-vuokraajan, lukuun ottamatta [väliaikaisen palvelimeen](#staging-server). Se ei tueta, vaikka ne on määritetty synkronoimaan toisensa poissulkevia objektijoukon. Voit ehkä on pitää tämä, jos kaikki toimialueiden yhdestä palvelimesta tai jakaa useita palvelimen kuormituksen eivät pysty muodostamaan yhteyttä.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Useiden puuryhmien yksittäinen Azure AD-vuokraajan
![Usean metsää yhtä vuokraajan](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Monet organisaatiot työskentelee ympäristöissä useiden paikallisen Active Directory-puuryhmien. Useamman kuin yhden paikallisen Active Directory-metsää on useita syitä. Tyypillinen esimerkit rakenteen tilin resurssin metsien ja seurauksena fuusion tai hankinnan jälkeen.

Jos sinulla on useita metsien, kaikki metsien on oltava saa yhden Azure AD Connect synkronointi palvelimeen. Ei tarvitse liittää palvelimen toimialueelle. Tarvittaessa saavuttamiseksi kaikki metsää palvelin voi asettaa DMZ-verkossa.

Ohjattu asennus Azure AD Connect on erilaisia vaihtoehtoja yhdistää useiden puuryhmien esittää käyttäjille. Tavoitteena on, että käyttäjän esitetään vain kerran Azure AD. On tavallisia sijoitteluja, voit määrittää mukautetun asennuspolku ohjatun asennuksen. Valitse edustava **yksilöivä käyttäjät**-sivulla oman topologian vastaava vaihtoehto. Koonnin määritetään vain käyttäjille. Kaksoiskappaleet ryhmät ovat ei kootut oletusarvo-asetuksissa.

Tavallisia sijoitteluja käsitellään seuraavan osion: [erillinen topologioissa](#multiple-forests-separate-topologies), [koko verkko](#multiple-forests-full-mesh-with-optional-galsync)ja [Tilin kuin resursseja](#multiple-forests-account-resource-forest).

Azure AD Connect synkronointi oletusarvo-määritysten oletetaan, että:

1. Käyttäjillä on vain yksi käytössä tili ja metsää, jossa tili sijaitsee käytetään todennetaan. Tämä olettaen on sekä salasanan synkronointi ja yhdistämiseen. UserPrincipalName- ja sourceAnchor/immutableID peräisin tämän metsää.
2. Käyttäjillä on vain yksi postilaatikko.
3. Metsää käyttäjän postilaatikkoa isännöivän on paras tietojen laadun, määritteiden näkyvissä-Exchange yleinen osoite luettelo (GAL). Jos käyttäjä ei ole postilaatikon, minkä tahansa metsää voidaan edistää näiden määritteiden arvot.
4. Jos sinulla on linkitetty postilaatikon, valitse on myös toisen tilin eri metsää, käytetään kirjauduttaessa sisään.

Jos ympäristössä vastaa nämä perustiedot, tapahtuu seuraavasti:

- Jos sinulla on useampi kuin yksi aktiivinen tili tai useamman kuin yhden postilaatikon, sync engine valitsee yhden ja ohittaa toiseen.
- Linkitetyn postilaatikko, jossa ei ole aktiivinen tili on ei viedä Azure AD. Käyttäjätilin ei vastaa ryhmän jäsen. Linkitetyn DirSync-postilaatikossa voi esittää aina kuin tavallinen postilaatikkoon. Tämä muutos on tarkoituksella tukemaan paremmin usean metsää skenaariot toimii eri tavalla.

Lisätietoja löytyy [tietoa oletusarvo-configuation](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Useiden puuryhmien, useita synkronointi-palvelinten yhden Azure AD-vuokraajan
![Usean metsää usean synkronointi ei tueta](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Se ei ole tuettu on useampi kuin yksi Azure AD yhteyden synkronointi palvelin yhdistetty yksittäinen Azure AD-vuokraajan. Poikkeus on [väliaikainen palvelimen](#staging-server)käyttö.

### <a name="multiple-forests--separate-topologies"></a>Useiden puuryhmien – erillinen topologioissa
**Käyttäjien esitetään vain kerran kaikkien kansioiden välillä**

![Usean metsää käyttäjien kerran](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Usean metsää Seperate topologioissa](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

Tässä ympäristössä kaikki metsien paikallisen käsitellään erilliset yksiköt ja käyttäjä ei olisi paikalla olevat muiden metsää.
Kunkin metsää on oma Exchange-organisaation ja metsien välillä ei ole GALSync. Tämä topologian voi olla tilanteesta fuusion tai hankinnan jälkeen tai johon business kunkin yksikön toimii organisaation toisistaan erillään. Saatava on samassa organisaatiossa Azure AD- ja yhdistetty GAL esitetään.
Tässä kuvassa jokaisen metsää kullekin objektille on esitetään kerran metaverse ja koostetun kohdevuokraajassa Azure AD.

### <a name="multiple-forests--match-users"></a>Useiden puuryhmien – vastine-käyttäjät
**Käyttäjätietojen olemassa useita kansioiden välillä**

Yleisiä nämä skenaariot on jakelu ja käyttöoikeusryhmät voivat sisältää useita käyttäjiä, yhteystiedot ja FSPs (ulkoinen suojausasetukset ansaitun)

Lisää FSPs käytetään edustamaan käyttöoikeusryhmän muiden saatavista jäsenet. Kaikki FSPs on selvitetty todellisen objektin Azure AD.

### <a name="multiple-forests--full-mesh-with-optional-galsync"></a>Useiden puuryhmien – koko silmukan valinnainen GALSync kanssa
**Käyttäjätietojen olemassa useita kansioita yli. Vastaa käyttäminen: sähköpostin määrite**

![Usean metsää käyttäjien sähköpostin](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Usean metsää koko verkko](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Täysi verkkotopologia sallii käyttäjien ja resurssien sijaita minkä tahansa metsää ja yleisesti olisi kaksisuuntainen luottamussuhteet metsien välillä.

Jos Exchange on useampi kuin yksi metsää, voi myös olla paikallisen-GALSync ratkaisu. Jokaiselle käyttäjälle esitetään kaikki metsien yhteyshenkilöksi. GALSync on yleisesti toteutettu Forefront käyttäjätietojen hallinta 2010 tai Microsoft käyttäjätietojen hallinta 2016 avulla. Azure AD Connect ei voi käyttää paikallisen GALSync.

Tässä skenaariossa tunnistetietojen objektien liitettyinä posti-määritteen avulla. Käyttäjä, jolla yksi metsää postilaatikossa on liitetty muita metsien yhteyshenkilöiden kanssa.

### <a name="multiple-forests--account-resource-forest"></a>Useiden puuryhmien – tili-resurssi metsää
**Käyttäjätietojen olemassa useita kansioita yli. Vastaa käyttäminen: ObjectSID ja msExchMasterAccountSID määritteet**

![Usean metsää käyttäjien ObjectSID](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Usean metsää AccountResource](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

Sinulla tili resurssin metsää topologian yhden tai usean tilin metsien aktiivinen käyttäjätilien. Voit myös määrittää yhden tai useamman resurssin metsien ei käytössä-tilien kanssa.

Tässä skenaariossa (vähintään yksi) **resurssin metsää** luottaa kaikki **tilin metsien**. Resurssin metsää on yleensä laajennettu AD-rakenne, Exchangen ja Lyncin kanssa. Kaikki Exchange- ja Lync-palvelut sekä muita jaettujen palveluiden sijaitsevat tämän metsää. Käyttäjillä on poistettu käytöstä käyttäjätilille tämän metsää ja postilaatikko on linkitetty tilin metsää.

## <a name="office-365-and-topology-considerations"></a>Office 365- ja topologian huomioon otettavia seikkoja
Jotkin Office 365-toiminnoista on topologioista tietyt rajoitukset. Jos aiot käyttää mitä tahansa näistä lukea kuormituksen topologioista aihetta.

Kuormituksen |  
--------- | ---------
Exchange Online | Jos on useampi kuin yksi Exchange organisaation paikallisia (eli Exchange on otettu käyttöön useita metsää), sinun on käytettävä Exchange 2013 SP1 tai uudempi versio. Tiedot löytyvät seuraavassa: [yhdistelmäympäristöä useiden Active Directory-puuryhmien kanssa](https://technet.microsoft.com/library/jj873754.aspx)
Skype for Business | Jos käytät useita metsien paikallisen, tilin resurssin metsää topologian tuetaan. Tiedot-, Täältä löytyvät topologioista: [Skype for Business Server 2015 ympäristön vaatimukset](https://technet.microsoft.com/library/dn933910.aspx)

## <a name="staging-server"></a>Väliaikaisen palvelin
![Väliaikaisen Server](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD Connect tukee asentamista toinen palvelimen **väliaikaisen**tilassa. Tässä tilassa palvelimen lukee tietoja kaikista yhdistetyn kansioista, mutta ei tallenna mitään yhdistetyn kansioita. Se on käytössä normaali synkronoinnin kehä ja on siksi käyttäjätiedot päivitetty kopio. Tietojen, jossa ensisijaisen palvelimen epäonnistuu voit voi epäonnistua päälle väliaikaisen palvelimeen. Voit tehdä saman Azure AD Connect ohjatun toiminnon. Toisen palvelimen mieluiten voi sijaita eri joten, koska ei ole infrastruktuurin jaetaan ensisijaisen palvelimen kanssa. Kopioi toinen palvelimeen ensisijainen palvelimessa tehdyt muutokset, määritys manuaalisesti.

Väliaikaisen palvelin myös voidaan testata uusi mukautettu konfigurointi ja vaikutus, se on tietoja. Voit esikatsella muutoksia ja säädä määritykset. Kun olet tyytyväinen uudet määritykset, voit tehdä väliaikaisen palvelimen active server ja vanha active server määrittää väliaikaisen tilassa.

Tätä menetelmää voi käyttää myös korvaa ActiveSyncin palvelimen. Valmistele uusi palvelin ja aseta se väliaikaisen tilassa. Varmista, että se on hyvä tilassa väliaikaisen (jolloin aktiivinen)-tila käytöstä ja Sammuta aktiivisena palvelimeen.

Ei ole useita väliaikaisen-palvelinta, kun haluat tehdä useita varmuuskopioita eri tietojen keskikohdan mukaan.

## <a name="multiple-azure-ad-tenants"></a>Useita Azure AD-alihallinnat
Microsoft suosittelee yhtä vuokraajan tallentamisessa Azure AD organisaatiolle.
Ennen kuin aiot käyttää useita Azure AD-alihallinnat, Näissä ohjeaiheissa kansilehden Yleisiä tilanteita, joissa voit käyttää yhtä vuokraajan.

Aihe |  
--------- | ---------
Järjestelmänvalvojan yksikkö delegointi | [Azure AD hallinnollisten yksiköiden hallinta](active-directory-administrative-units-management.md)

![Usean metsää usean vuokraajan](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Tällä Azure AD Connect-synkronointi palvelimeen ja Azure AD-vuokraajan 1:1 suhteen. Kunkin Azure AD-vuokraajan sinun on yksi Azure AD Connect synkronointi palvelimen asennus. Azure AD-vuokraajan esiintymät on suunniteltu ominaisuus, eristää ja yhdessä käyttäjät eivät näe käyttäjien toisessa vuokraajassa. Jos tämä erottaminen on tarkoitettu, sitten tämä ei tueta, mutta muuten pitäisi käyttää yksi Azure AD-vuokraajan malli.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Vain kerran Azure AD-vuokraajan kunkin objektin
![Yhden metsää suodatettu](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

Tämä topologian yksi Azure AD Connect synkronointi palvelin on yhdistetty kunkin Azure AD-vuokraajan. Azure AD Connect synkronointi-palvelimet on määritettävä siten, että jokainen on toisensa poissulkevia objektijoukon toimimaan suodattamista varten. Voit esimerkiksi rajoittaa tietyn toimialueen tai OU kussakin palvelimessa. DNS-toimialueen voidaan rekisteröidä vain yhdessä Azure AD-vuokraajan. UPNs paikallista käyttäjille AD on käytettävä erillistä nimitilan. Esimerkiksi kolme erillistä UPN yllä olevassa kuvassa liitteet rekisteröidyt paikallisen AD: contoso.com, fabrikam.com ja lelut.fi. Kaikkien käyttäjien paikallisen AD toimialueen eri nimitilaa käyttämällä.

Ei ole GALsync Azure AD-vuokraajan esiintymien välillä. Osoitteisto Exchange Online- ja Skype yrityskäyttäjille ainoastaan näyttää samassa alihallinnassa.

Tämä topologian on seuraavat rajoitukset, joita ei ole tuettu skenaariot:

- Vain yksi Azure AD-alihallinnat ottaa Exchange hybrid paikallisen Active Directory-hakemistosta.
- Windows 10-laitteissa voi olla vain yksi Azure AD-vuokraajan liity.

Toisensa poissulkevia objektijoukon vaatimus koskee myös takaisinkirjoituksen. Takaisinkirjoituksen joitakin ominaisuuksia ei tueta tässä topologian koska nämä ominaisuudet oletetaan, että yhden määritysten paikallisen:

-   Ryhmän takaisinkirjoituksen kanssa oletusarvon määrittäminen
-   Laitteen takaisinkirjoituksen

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Kunkin objektin useita kertoja Azure AD-vuokraajan
![Yhden metsää usean vuokraajan, joita ei tueta](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Yhden metsää usean yhdistimiä, joita ei tueta](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

- Se ei tue synkronoimaan useita Azure AD-vuokraajiin sama käyttäjä.
- Ei tueta varmistaaksesi, että muuta varmistaaksesi, että käyttäjät yksi Azure AD näkyvät yhteystiedot toisesta Azure AD-vuokraajan määrityksen.
- Se ei tue Azure AD Connect Synkronoi yhdistää useita Azure AD-vuokraajiin muokkaamista.

### <a name="galsync-by-using-writeback"></a>Takaisinkirjoituksen käyttämällä GALsync
![MultiForestMultiDirectoryGALSync1Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![MultiForestMultiDirectoryGALSync2Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD alihallinnat, jotka ovat eristetty rakenne.

- Se ei tue voit muuttaa Azure AD Connect synkronointi voi lukea tietoja toiseen Azure AD-vuokraajan määrityksiä.
- Se ei tue voit viedä yhteystiedot käyttäjien toiseen paikallisen AD käyttämällä Azure AD Connect Synkronoi.

### <a name="galsync-with-on-premises-sync-server"></a>GALsync paikallisen synkronoiminen palvelimen kanssa
![MultiForestMultiDirectoryGALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

Se on tuettu käyttämään FIM2010/MIM2016 paikallisen Exchange kaksi organisaatioissa käyttäjien GALsync. Organisaation käyttäjien näyttää ulkomaan muiden organisaation käyttäjien/yhteyshenkilöiksi. Nämä eri paikallisen Active Directory voidaan synkronoida sitten oman Azure AD-vuokraajiin.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja Azure AD Connect näissä tilanteessa asentamisesta on artikkelissa [Azure AD Connect mukautettu asennus](./connect/active-directory-aadconnect-get-started-custom.md).

Lisätietoja [Azure AD Connect synkronointi](active-directory-aadconnectsync-whatis.md) -määritys.

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
