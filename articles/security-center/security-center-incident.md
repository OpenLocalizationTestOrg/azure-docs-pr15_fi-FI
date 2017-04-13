<properties
   pageTitle="Suojauksen tapahtumaa Azure Tietoturvakeskuksessa käsittely | Microsoft Azure"
   description="Tämän asiakirjan auttaa Azure Tietoturvakeskus ominaisuuksien avulla voit käsitellä suojauksen tapaukset."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="handling-security-incident-in-azure-security-center"></a>Suojauksen tapahtumaa Azure Tietoturvakeskuksessa käsittely 
Triaging ja tutkiminen suojausvaroitusten voi kestää kauan, vaikka useimmat taitoja suojauksen analyytikot ja monien on vaikea tietää, missä voit aloittaa. [Analyysin](security-center-detection-capabilities.md) avulla voit yhdistää eri [suojausvaroitusten](security-center-managing-and-responding-alerts.md)välillä, Tietoturvakeskus voi antaa sinulle samaa näkymää hyökkäyksen markkinointikampanjan ja kaikki liittyvät ilmoitukset – ymmärrät nopeasti hän noudatit toiminnot ja mitä resursseja on vaikuttaa.

Tässä asiakirjassa kerrotaan, miten helpottavat käsittely suojausasetukset tapaukset Tietoturvakeskus suojauksen ilmoitusten ominaisuuksien avulla.


## <a name="what-is-a-security-incident"></a>Mikä on jokin?

Tietoturvakeskus jokin on kooste resurssin kaikki ilmoitukset, joka [lopettaa ketju](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) kuvioilla tasata. Tapahtumat näkyvät [Suojausvaroitusten](security-center-managing-and-responding-alerts.md) -ruutu ja sivu. Tapahtuma paljastaa liittyvät ilmoitukset-luettelo, jonka avulla voit saada lisätietoja jokaiselle esiintymälle.

## <a name="managing-security-incidents"></a>Suojauksen tapaukset hallinta

Voit tarkastella nykyistä suojaus-tapaukset katsomalla suojauksen ilmoitukset-ruutu. Azure-portaalin ja noudata seuraavia ohjeita voit tarkastella tarkempia tietoja kunkin suojaus-tapahtumaa:

1. Tietoturvakeskus koontinäytössä näet **suojausvaroitusten** -ruutu.

    ![Suojausvaroitusten vierekkäin Tietoturvakeskus](./media/security-center-incident/security-center-incident-fig1.png)

2.  Valitse tämä ruutu, laajenna, ja jos jokin havaitaan, se näkyy kohdassa suojaus ilmoitukset-kaavion alla kuvatulla tavalla:

    ![Turvallisuutta](./media/security-center-incident/security-center-incident-fig2.png)

3.  Huomaa, että suojauksen tapauksen kuvaus on eri kuvake verrattuna muut ilmoitukset. Valitse voit tarkastella tarkempia tietoja tätä tapahtumaa.

    ![Turvallisuutta](./media/security-center-incident/security-center-incident-fig3.png)

4.  **Tapauksen** -sivu tulee näkyviin lisää tarkat tiedot tämän suojauksen tapahtumaa, joka sisältää sen koko kuvauksen sen vakavuus (joka tässä tapauksessa on suuri), nykyisestä tilasta (Tässä tapauksessa on edelleen *aktiivinen*, joka viittaa käyttäjä ei ole ottanut toiminnon *Hylkää* sen – voit tehdä tämän napsauttamalla hiiren kakkospainikkeella **suojausvaroitusten** sivu tapauksen) , (Valitse tämä palvelupyynnön *VM1*) hyökkäyksen kohteeksi joutuneen resurssin korjaus tapauksen ohjeet ja alaruudun sinulla on ilmoituksia, joka sisältää tätä tapahtumaa. Jos haluat saada lisätietoja kunkin ilmoituksen, valitse se ja toinen sivu avautuu, alla kuvatulla tavalla:

    ![Turvallisuutta](./media/security-center-incident/security-center-incident-fig4.png)

Tämä sivu tiedot vaihtelevat ilmoituksen mukaan. Lue lisätietoja siitä, miten voit hallita äänimerkkien [hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) . Joitakin tärkeitä asioita koskevat tätä ominaisuutta:

- Uuden suodattimen avulla voit mukauttaa näkymän vain tapahtumaa tai vain ilmoitukset. 
- Sama ilmoitus voi olla tapahtuma (jos saatavilla) sekä olevan näkyvissä erillinen ilmoituksesta osana. 
- Sulkemisessa tapahtuma ole hylkää liittyvät ilmoitukset.

## <a name="see-also"></a>Katso myös

Tässä asiakirjassa opit suojauksen tapauksen ominaisuuksien käyttämisestä Tietoturvakeskuksessa. Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Hallinta ja Azure Tietoturvakeskuksessa suojausvaroitusten vastaaminen](security-center-managing-and-responding-alerts.md)
- [Azure Tietoturvakeskus tunnistusominaisuudet](security-center-detection-capabilities.md)
- [Azure Tietoturvakeskus suunnittelu- ja käyttöoppaan](security-center-planning-and-operations-guide.md)
- [Hallinta ja Azure Tietoturvakeskuksessa suojausvaroitusten vastaaminen](security-center-managing-and-responding-alerts.md)
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md)– Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/)– Etsi blogi kirjaa tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen.
