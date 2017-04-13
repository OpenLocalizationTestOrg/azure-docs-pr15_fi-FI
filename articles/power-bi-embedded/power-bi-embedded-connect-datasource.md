<properties
   pageTitle="Microsoft Power BI-upotettu - yhteyden muodostaminen tietolähteen"
   description="Power BI upotetun, muodosta yhteys tietolähteisiin"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="connect-to-a-data-source"></a>Yhteyden muodostaminen tietolähteen

**Power BI upotetun**, jossa voit upottaa raportit itse sovelluksen. Kun Power BI-taulukkoraportin upotat sovelluksen raportin muodostaa pohjana olevat tiedot **tuomalla** tiedot tai **DirectQuery**tietolähteen **yhteyden suoraan** .

Seuraavassa on erot käytettäessä **tuonti** ja **DirectQuery**.

|Tuo | DirectQuery
|---|---
|Taulukot, sarakkeet *ja tiedot* tuodaan tai raportin tietojoukko kopioidaan. Voit tarkastella muutoksia on tapahtunut pohjana olevat tiedot, Päivitä tai tuo, valmis, nykyisen tietojoukko uudelleen.|Vain *taulukoiden ja sarakkeiden* tuotu tai raportin tietojoukko kopioidaan. Voit tarkastella aina uusimmat tiedot.
Power BI upotetun voit käyttää DirectQuery pilvitietolähteet mutta ei paikallisen tietolähteiden tällä hetkellä.

## <a name="benefits-of-using-directquery"></a>DirectQuery edut

On kaksi ensisijaista edut **DirectQuery**käytettäessä:

   -    **DirectQuery** avulla voit täydentää visualisoinnit päälle hyvin suuria tietojoukkoja, muuten olisi unfeasible ensimmäisen Tuo kaikki tiedot.
   -    Pohjana olevat tiedot muuttuvat edellyttää, että tietojen päivityksen ja joissakin raporteissa tarvitse näyttävät ajantasaiset tiedot, voit määrittää suurien tietomäärien siirrot, tekeminen uudelleen tietoja unfeasible. Sen sijaan **DirectQuery** raportit aina käyttöön nykyiset tiedot.

## <a name="limitations-of-directquery"></a>DirectQuery rajoitukset

   Käytä **DirectQuery**joitakin rajoituksia:

   -    Kaikki taulukot on annettava yhden tietokannasta.
   -    Jos kysely on monimutkaisesta, ilmenee virhe. Virheen korjaamiseksi refactor kysely on monimutkaisuutta. Quuery on oltava monimutkaisia, jos haluat tuoda tiedot **DirectQuery**sijaan.
   -    Yhteyden suodatus on rajoitettu yhteen suuntaan molempiin suuntiin sijaan.
   -    Et voi muuttaa sarakkeen tietotyypin.
   -    Oletusarvon mukaan rajoitukset sijoitetaan sallittu toimenpiteiden DAX-lausekkeet. Katso [DirectQuery ja mitat](#measures).

<a name="measures"/>
## <a name="directquery-and-measures"></a>DirectQuery ja toimenpiteet

Kyselyt lähetetään pohjana olevassa tietolähteessä on hyväksyttävä suorituskyvyn varmistamiseksi rajoitukset otetaan käyttöön toimenpiteitä. Kun **Power BI Desktopin**edistyneille käyttäjille avulla voit valita valitsemalla Ohita tämä rajoitus **Tiedosto > Asetukset ja asetukset > Asetukset**. Valitse **asetukset** -valintaikkunan **DirectQuery**ja valitse vaihtoehto **sallia Rajoittamattomien toimenpiteiden DirectQuery-tilassa**. Kun asetus on valittuna, mikä tahansa DAX-lauseke, joka on voimassa mitta voidaan. Käyttäjien on otettava huomioon; kuitenkin, että joitakin lausekkeita, jotka suorittavat hyvin kun tiedot on tuotu voi johtaa hyvin hidasta kyselyjen Taustajärjestelmä lähde- **DirectQuery** -tilassa. 

## <a name="see-also"></a>Katso myös
- [Aloita Microsoft Power BI upotetun](power-bi-embedded-get-started.md)
- [Power BI Desktopin](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
