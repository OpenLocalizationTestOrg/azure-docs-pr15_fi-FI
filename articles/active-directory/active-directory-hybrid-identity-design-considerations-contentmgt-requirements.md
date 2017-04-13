<properties
    pageTitle="Azure Active Directoryn hybrid tunnistetietojen tyyliseikat - sisällönhallinta vaatimusten määrittäminen | Microsoft Azure"
    description="Sisältää huomioon tietoja sen selvittämisestä sisällönhallinta tarpeisiin. Kun käyttäjä on omalla laitteen hän voi on yleensä myös useita tunnistetiedot, jotka vaihteleva sovellus, jossa hän käyttää mukaan. On tärkeää erottaa poistetusta sisällöstä on luotu ja yrityksen tunnistetiedot-sovelluksella luodut niistä Omat tunnistetiedot. Identity-ratkaisun pitäisi olla vuorovaikutuksessa cloud services sujuvan kokemuksen tarjota samalla, kun käyttäjä varmistaa hänen tietosuoja ja parantaa tietojen vuoto suojautumista."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Sisällön hallinta vaatimusten hybrid identity-ratkaisun määrittäminen

Yrityksen sisällönhallinta-vaatimukset tietoja suoraan voi vaikuttaa päätöksesi käyttämään mitä hybrid identity-ratkaisussa. Kun useilla eri laitteilla koskevia ja käyttäjien mahdollisuus tuoda oman laitteiden ([BYOD](http://aka.ms/byodcg)), yrityksen on oma tietojen suojaaminen mutta myös on muuttumattomana käyttäjän tietosuoja. Kun käyttäjä on omalla laitteen hän voi on yleensä myös useita tunnistetiedot, jotka vaihteleva sovellus, jossa hän käyttää mukaan. On tärkeää erottaa poistetusta sisällöstä on luotu yrityksen tunnistetiedot luotuja kommentteja ja henkilökohtainen tunnistetiedoilla. Identity-ratkaisun pitäisi olla vuorovaikutuksessa cloud services sujuvan kokemuksen tarjota samalla, kun käyttäjä varmistaa hänen tietosuoja ja parantaa tietojen vuoto suojautumista. 

Identity-ratkaisun voi hyödyntää eri tekniset ohjausobjekteja jotta sisällönhallinta alla olevassa kuvassa osoitetulla tavalla:
 
![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Turvaominaisuudet, joka hyödyntäminen käyttäjätietojen hallintajärjestelmän**

Sisällön hallinta-vaatimukset yleensä hyödyntää että käyttäjätietojen hallintajärjestelmän seuraavilla alueilla:

- Tietosuoja: tunnistaminen resurssin omistavan käyttäjän ja säilyttää eheys hallintatoiminnot käyttämällä.
- Tietojen luokittelu: Määritä käyttäjän tai ryhmän ja käyttöoikeustasot objektin mukaan sen luokitus. 
- Vuoto tietosuojaa: turvaominaisuudet vastuussa välttämiseksi vuoto tietojen suojaaminen on vuorovaikutuksessa tunnistetietojen järjestelmä tarkistaa käyttäjän tunnistetiedot. Tämä on myös tärkeää tarkistaminen kirjausketju tarkoitus.

>[AZURE.NOTE]
Lue lisätietoja parhaita käytäntöjä [tietojen luokitus cloud valmiuden](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) ja ohjeet tietojen luokittelu.

Kun hybrid tunnistetietojen suunnitteluratkaisun varmistaa organisaation vaatimusten mukainen vastataan seuraaviin kysymyksiin:

- Onko yrityksesi turvaominaisuudet voit pakottaa tietojen suojauksen?
 - Jos Kyllä, suojaus-ohjausobjektit voivat integroida hybrid identity-ratkaisun, joka aiot hyväksyy?
- Yrityksesi käyttää tietojen luokittelu
 - Kyllä, jos on nykyistä ratkaisua voi integroida hybrid identity-ratkaisun, joka aiot hyväksyy?
- Onko yrityksesi tällä hetkellä kaikki tiedot vuoto ratkaisu? 
 - Kyllä, jos on nykyistä ratkaisua voi integroida hybrid identity-ratkaisun, joka aiot hyväksyy?
- Tarvitseeko yrityksen resurssien valvoa?
 - Jos Kyllä, minkä tyyppisiä resursseja?
 - Jos Kyllä, mitkä tiedot on tarpeen?
 - Jos Kyllä, jos valvontaloki on sijaittava? Paikalliseen tai pilvessä?
- Täytyykö yrityksesi salata kaikki sähköpostit, jotka sisältävät luottamuksellisia tietoja (SSNs, luottokortin numeroista jne.)?
- Täytyykö yrityksesi salata kaikki asiakirjat/sisältöä ulkoisten liikekumppanien jaettu?
- Tarvitse yrityksesi voidaan valvoa yrityksen käytäntöjä, valitse tietyntyyppiset sähköpostit (Älä ei vastaa kaikille, älä siirrä)?
 
>[AZURE.NOTE]
Varmista, että kunkin vastauksen kirjoittaa muistiinpanoja ja ymmärtää perusteet takana vastaus. [Määritä tietojen suojauksen strategia](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) siirtyvät vaihtoehtoja ja kunkin asetuksen eduista/Haittapuolia.  Mukaan on vastannut näihin kysymyksiin valitaan sopii parhaiten sopivan vaihtoehdon yrityksesi on.


## <a name="next-steps"></a>Seuraavat vaiheet
[Access-ohjausobjektin vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Katso myös
[Rakenteen huomioon otettavia seikkoja yleiskatsaus](active-directory-hybrid-identity-design-considerations-overview.md)
