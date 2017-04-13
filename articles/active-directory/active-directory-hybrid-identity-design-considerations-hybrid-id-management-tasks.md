<properties
    pageTitle="Azure Active Directory-hybrid tunnistetietojen rakenteeseen - määrittää hybrid tunnistetietojen hallintatehtäviä | Microsoft Azure"
    description="Ohjausobjektin ehdollisen käyttöoikeuden kanssa Azure Active Directory tarkistaa tiettyjen ehtojen, voit valita, kun käyttäjän todentamista ja ennen kuin sallit sovelluksen käyttöä. Ehtojen täyttyessä, kun käyttäjä on todennus ja sovelluksen käyttöoikeus."
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

# <a name="plan-for-hybrid-identity-lifecycle"></a>Hybrid tunnistetietojen elinkaari suunnitteleminen 

Käyttäjätietojen on yrityksen yrityksen mobility ja sovelluksen käytön määrittäminen perusteiden. Olet kirjautumassa mobiililaitteen tai SaaS app, henkilöllisyytesi on päässyt kaikki-näppäintä. Milloin sen ylimmän tason identity management-ratkaisun kattaa verkkoympäristöjen ja synkronoiminen oman tunnistetietojen säilöjen tietoihin, joka sisältää automatisointi ja keskittäminen prosessi, jossa valmistellaan resurssien välillä. Identity-ratkaisun olisi olevan keskitetystä tunnistetietojen koko paikallisen ja pilvi ja säilyttää Keskitetty todennus ja suojatusti jakaminen ja yhteistyö ulkoisten käyttäjien ja yritysten kanssa myös joitakin tunnistetietojen yhdistämisessä lomakkeen avulla. Resurssien väliltä käyttöjärjestelmiä ja sovelluksia henkilöille tai liittyvän organisaatiossa. Voit muuttaa organisaation rakenne valmistelu käytäntöjä ja ohjeita.

On myös tärkeää on tarkoitettu voit antaa käyttäjien antamalla ne Omatoiminen kokemukset pitämään tehokkaasti identity-ratkaisun. Käyttäjätietojen ratkaisu on tehokkaamman Jos sen avulla kertakirjautumisen käyttäjille kaikki tarvitsemansa käyttää järjestelmänvalvojat lainkaan resurssien välillä tasot käyttää standardoitu menettelyt hallintaan käyttäjän tunnistetietoja. Jotkin tasot hallinta voit pienentää tai on poistettu mukaan valmistelu management-ratkaisun ulkopuolella. Lisäksi voit turvallisesti jakaa hallinta-ominaisuuksia, manuaalisesti tai automaattisesti kesken eri organisaatioissa. Esimerkiksi toimialueen järjestelmänvalvoja voi toimia vain henkilöt ja resurssit toimialueen. Tämän käyttäjän tehdä järjestelmänvalvojan ja valmistelu tehtävät, mutta ei ole oikeutta määritysten tehtävät, kuten työnkulkujen luominen.


## <a name="determine-hybrid-identity-management-tasks"></a>Määrittää Hybrid käyttäjätietojen hallintatehtävät
Organisaation hallinnollisten tehtävien jakaminen parantaa tarkkuutta ja tehokkuutta hallinta ja parantaa organisaation työmäärää saldo. Seuraavassa on pivot-taulukot, jotka määrittävät tehokkaat käyttäjätietojen hallintajärjestelmän.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)


Voit määrittää hybrid tunnistetietojen hallintatehtäviä, on hyvä tietää joitakin organisaatiossa, jossa antamista hybrid tunnistetietojen olennaiset ominaisuudet. On tärkeää ymmärtää käytössä tunnistetietojen lähteiden nykyisen säilöjen tietoihin. Tietää core kyseisiä osia, on foundational vaatimukset ja perusteella, että sinun on eritellympiä kysymyksiä, joka johtaa olet paremman rakenne päätös Identity-ratkaisun.  

Kun määrität näitä vaatimuksia, että vähintään seuraavat seikat vastataan

- Valmistelu asetukset: 
 - Tukeeko hybrid identity-ratkaisun tehokas käyttöoikeushallinta ja valmistelujärjestelmän?
 - Miten ovat käyttäjät, ryhmät ja siirtymällä voi hallita salasanat?
 - Vastaa tunnistetietojen elinkaaren hallinta? 
      - Kuinka kauan salasanan päivitykset tilin keskeyttäminen kestää?
      
- Käyttöoikeuksien hallinta: 
 - Onko hybrid tunnistetietojen ratkaisu kahvoja käyttöoikeuksien hallinta?
     - Jos Kyllä, mitä ominaisuuksia on käytettävissä?
- Ratkaisun Käsittele ryhmän perustuva käyttöoikeuksien hallinta 
      – Jos Kyllä, onko se mahdollista käyttöoikeusryhmän määrittämiseen? 
       – Jos Kyllä, se cloud hakemiston määrittää käyttöoikeuksia kaikki ryhmän jäsenet? 
        -Mitä tapahtuu, jos käyttäjä on myöhemmin lisätään tai poistetaan ryhmästä, on käyttöoikeuden automaattisesti määritetty tai poistaa tarvittaessa? 

- Muiden kolmansien osapuolten tunnistetietojen toimittajat integrointi:
- Kolmannen osapuolen tunnistetietojen toimittajat ottaa käyttöön kertakirjautumisen integroitu hybrid ratkaisu?
- Onko se mahdollista selkeyttää kaikki eri tunnistetietojen toimittajat koossa pysyviin identity-järjestelmään?
- Jos Kyllä, miten ja jotka ovat ne ja mitä ominaisuuksia on käytettävissä?

## <a name="synchronization-management"></a>Synkronoinnin hallinta
Yksi tavoitteisiin käyttäjätietojen hallinta-tunnistetietojen toimittajat ja pitämään ne voivat synkronoida. Voit pitää tietojen synkronoiminen tärkeimpien perusmuodon tunnistetietojen toimittaja perusteella. Hybrid tunnistetietojen tilanne, joka synkronoidun hallintamallin kaikki paikallisen palvelimen käyttäjien ja laitteiden jäsenyyksiä hallitaan ja synkronoi-tilit ja halutessasi salasanat pilveen. Käyttäjä lisää kuin hän saman salasanan paikallisen hän tekee pilveen ja kirjauduttaessa sisään, salasana on tarkistanut identity-ratkaisun. Tämä malli käyttää directory-synkronointityökalu.
 
![](./media/hybrid-id-design-considerations/Directory_synchronization.png)Oikea suunnitteluun hybrid identity-ratkaisun synkronointi varmistaa seuraaviin kysymyksiin vastataan: • mitkä ovat käytettävissä hybrid identity-ratkaisun synkronointi ratkaisuja?
• Mitä kertakirjautuminen toiminnot ovat käytettävissä?
• Mitä asetukset tunnistetietojen yhdistämisessä B2B ja B2C välillä?

## <a name="next-steps"></a>Seuraavat vaiheet
[Hybrid käyttäjätietojen hallinta käyttöönoton määrittäminen](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)


## <a name="see-also"></a>Katso myös
[Rakenteen huomioon otettavia seikkoja yleiskatsaus](active-directory-hybrid-identity-design-considerations-overview.md)

