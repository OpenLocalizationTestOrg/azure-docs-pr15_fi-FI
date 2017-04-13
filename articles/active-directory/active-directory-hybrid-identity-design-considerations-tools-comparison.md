<properties
    pageTitle="Hybrid tunnistetietojen: Yhteystietojen integrointi Työkalut vertailu | Microsoft Azure"
    description="Tämä on sivulla on täydellinen taulukon, joka vertaa eri hakemistointegrointityökalut, joka voi käyttää hakemistointegrointi."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Hybrid tunnistetietojen yhteystietojen integrointi Työkalut vertailu

Vuosien hakemistointegrointityökalut on kasvanut ja kehittynyt.  Tämä asiakirja on parantaa kootut näkymän työkaluja ja ominaisuuksia, jotka ovat käytettävissä kaikissa vertailu.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

>[AZURE.NOTE] Azure AD Connect kattaa osat ja toiminnot, jotka on aiemmin julkaistu Dirsync-työkalun ja AAD Sync. Työkaluja julkaistaan enää yksitellen ja kaikissa tulevissa parannukset sisällytetään Azure AD Connect päivitykset niin, että tiedät aina mistä sellaisen saat uusimmat toimintoja.
>
>Dirsync-työkalun ja Azure AD-synkronoinnista on poistettu. Lisätietoja löytyvät [tähän](active-directory-aadconnect-dirsync-deprecated.md).


Käytä seuraava rekisteriavain taulukot.

● = Nyt  
F = versioon  
PP = julkisen esikatselu  

## <a name="on-premises-to-cloud-synchronization"></a>Paikalliseen synkronointi pilveen

| Toiminto  | Azure Active Directory-muodosta  | Azure Active Directory-synkronointipalvelut (AAD Sync) | Azure Active Directory-Synkronointityökalu (DirSync)| Forefront käyttäjätietojen hallinta 2010 R2 (FIM) |Microsoftin Käyttäjätietojen hallinta 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Yhteyden muodostaminen yhden paikallisen AD metsää | ● | ● | ● | ● |● |
| Yhteyden muodostaminen useita paikallisen AD metsien |●  | ● |  | ● |● |
| Yhteyden muodostaminen useisiin paikallisen Exchange organisaatioiden | ● |  |  |  | |
| Yhteyden muodostaminen yhden paikallisen LDAP-hakemisto | F |  |  | ● |● |
| Yhteyden muodostaminen useita paikallisen LDAP-hakemistoja |F  |  |  | ● |● |
| Yhteyden muodostaminen paikalliseen AD ja paikallisten LDAP-hakemistoja |F  |  |  | ● |● |
| Yhteyden muodostaminen mukautetun järjestelmien (eli SQL, Oracle, MySQL jne.) | F |  |  | ● |● |
| Synkronoi asiakkaan määrittämä määritteet (directory extensions) | ● |  |  |  |  |
|Yhteyden muodostaminen paikalliseen HR (eli SAP-Oracle verkkoliiketoiminnan PeopleSoft)| F |  |  | ● |● |
|Tukee FIM synkronoinnin säännöt ja yhdistimet valmisteluun paikalliseen järjestelmään.|  |  |  | ● |● |

## <a name="cloud-to-on-premises-synchronization"></a>Paikallisen synkronointi pilveen

| Toiminto  | Azure Active Directory-muodosta  | Azure Active Directory-synkronointipalvelut | Azure Active Directory-Synkronointityökalu (DirSync) | Forefront käyttäjätietojen hallinta 2010 R2 (FIM) |Microsoftin Käyttäjätietojen hallinta 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Takaisinkirjoituksen laitteiden | ● |  | ● |  ||
| Määritteen takaisinkirjoituksen (for Exchange-yhdistelmäratkaisu) | ● | ● | ● | ● |● |
| Takaisinkirjoituksen käyttäjät ja ryhmät-objektit |  ●|  | |  ||
| Takaisinkirjoituksen salasanojen (Omatoiminen salasanan (Vaihtaminen) tai salasanan vaihtaminen) |  ● | ● |  |  ||



## <a name="authentication-feature-support"></a>Todennus tukemista

| Toiminto  | Azure Active Directory-muodosta | Azure Active Directory-synkronointipalvelut | Azure Active Directory-Synkronointityökalu (DirSync) | Forefront käyttäjätietojen hallinta 2010 R2 (FIM) |Microsoftin Käyttäjätietojen hallinta 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Yhden salasanan synkronointi paikallisen AD metsää | ● | ● | ● |  ||
| Useiden salasanan synkronointi paikallisen AD metsien |  ●| ● |  |  ||
| Kertakirjautumisen liittoutumistoiminnossa | ● | ● | ● | ● |● |
| Takaisinkirjoituksen salasanojen (mistä Vaihtaminen ja salasanan muuttaminen) |●  | ● |  |  ||



## <a name="set-up-and-installation"></a>Määritetään ja asentaminen

| Toiminto  | Azure Active Directory-muodosta  | Azure Active Directory-synkronointipalvelut | Azure Active Directory-Synkronointityökalu (DirSync) | Microsoftin Käyttäjätietojen hallinta 2016 (MIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Toimialueen ohjauskoneen tukee asennus | ● | ● | ● |  |
| Tukee asennuksen SQL Express | ● | ● | ● |  |
| DirSync-asetusten päivittäminen |● |  |  |  |
| Windows Server kieliä lokalisoinnin järjestelmänvalvojan UX. | ● | ● | ● |  |
|Sovellus on lokalisoitu käyttäjää UX Windows Server kielet| |  |  |● |
| Windows Server 2008: n ja Windows Server 2008 R2: n tuki | ●, synkronoinnin, ei ole liitetty viestintä| ● | ●  | ● |
| Windows Server 2012: n ja Windows Server 2012 R2-tuki | ● | ● | ● | ● |

## <a name="filtering-and-configuration"></a>Suodattaminen ja määrittäminen

Toiminto  | Azure Active Directory-muodosta | Azure Active Directory-synkronointipalvelut | Azure Active Directory-Synkronointityökalu (DirSync) | Forefront käyttäjätietojen hallinta 2010 R2 (FIM)| Microsoftin Käyttäjätietojen hallinta 2016 (MIM)
:-------- |:--------:|:--------:|:--------:|:--------:|:--------:|
Valitse toimialueet ja organisaatioyksiköiden suodattaminen | ● | ● | ● | ●  | ●
Objektien määrite arvojen suodattaminen | ● | ● | ● | ●| ●
Salli vähimmäismäärää määritteet, joita voidaan synkronoida (MinSync) | ● | ● |  ||
Salli eri malleja voi suojata määrite kulkee |●  | ● |  ||
Salli juoksutus AD-Azure AD määritteet poistaminen | ● | ● |  |  |
Salli enemmän määrite kulkee | ● | ● |  | ●  | ●

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
