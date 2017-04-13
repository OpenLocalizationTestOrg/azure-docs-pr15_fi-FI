<properties
   pageTitle="Yksityinen mallien käytön aloittaminen | Microsoft Azure"
   description="Lisätä, hallita ja jakaa yksityistä mallisi Azure portaalin, Azure CLI tai PowerShellin avulla."
   services="marketplace-customer"
   documentationCenter=""
   authors="VybavaRamadoss"
   manager="asimm"
   editor=""
   tags="marketplace, azure-resource-manager"
   keywords=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/18/2016"
   ms.author="vybavar"/>

# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Yksityinen malleja Azure-portaalin käytön aloittaminen

[Azure Resurssienhallinta](../resource-group-authoring-templates.md) -malli on määritettäviä mallin avulla määritetään käyttöönoton. Voit määrittää resurssien ratkaisun käyttöönotto ja Määritä parametrit ja muuttujat, joiden avulla voit kirjoittaa arvoja eri ympäristöissä. Mallin koostuu JSON ja lausekkeita, jonka avulla voit muodostaa käyttöönoton arvot.

Voit käyttää uuden **Mallit** -ominaisuutta [Azure Portal](https://portal.azure.com) **Microsoft.Gallery** resurssin toimittajan kanssa samalla tunnisteena [Azure Marketplacesta](https://azure.microsoft.com/marketplace/) , jotta käyttäjät voivat luoda, hallita ja ottaa käyttöön yksityinen malleja Omat kirjastosta.

Tämän asiakirjan opastaa lisäämistä, hallinta ja jakaminen Azure-portaalissa yksityinen **malli** .

## <a name="guidance"></a>Ohjeet

Seuraavat ehdotukset avulla voit hyödyntää **mallia** ratkaisujen käsitellessäsi:

- **Malli** on encapsulating resurssi, joka sisältää mallin Resurssienhallinta ja metatietoja. Se toimii hyvin tapaan kohteen Marketplacesta. Tärkein ero on yksityisen kohteen verrattuna julkisen Marketplace-kohteita.
- **Mallit** -kirjaston toimii myös käyttäjille, jotka tarvitsevat mukauttamiseen niiden käyttöönottoa varten.
- **Mallien** sovi hyvin käyttäjät, joilla on yksinkertainen säilön Azure kuluessa.
- Aloita aiemmin luotua Resurssienhallinta-mallia. Etsi [github](https://github.com/Azure/azure-quickstart-templates) tai [Vie malli](../resource-manager-export-template.md) mallien resurssi-ryhmästä.
- **Malleja** on liitetty käyttäjälle, joka julkaisee ne. Julkaisijan nimi näkyy kaikille käyttäjille, joilla on lukuoikeudet.
- **Mallien** Resurssienhallinta resursseja ja ei voi nimetä kerran julkaistu.

## <a name="add-a-template-resource"></a>Lisää mallin resurssi

Voit luoda **mallin** resurssin Azure-portaalissa kahdella eri tavalla.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>Tapa 1: Luo uusi malli resurssi käynnissä resurssiryhmä

1. Siirry Azure-portaalin aiemmin luotu resurssiryhmä. Valitse **vietävä malli** **asetukset**.
2. Kun Resurssienhallinta-malli on viety, tallentaa sen **mallien** säilöön **Tallenna malli** -painikkeella. Etsi kaikki tiedot Vie mallin [tähän](../resource-manager-export-template.md).
<br /><br />
![Resurssien ryhmä-vienti](media/rg-export-portal1.PNG)  <br />

3. Valitse **Tallenna malli** -komentopainiketta.
<br /><br />

4. Anna seuraavat tiedot:

    - Nimi – mallin objektin nimi (Huomautus: Tämä on Azure Resurssienhallinta mukaan nimi. Kaikki nimeämiseen liittyviä rajoituksia eikä niitä voi muuttaa kerran luotu).
    - Kuvaus – nopean yhteenvedon tietoja mallista.

    ![Mallin tallentaminen](media/save-template-portal1.PNG)  <br />

5. Valitse **Tallenna**.

    > [AZURE.NOTE] Vie malli-sivu näyttää ilmoitukset, kun viedyn Resurssienhallinta-mallissa on virheitä, mutta silti voi tallentaa Resurssienhallinta tämän mallin mallit. Varmista, että tarkistaminen ja korjaaminen jokin Resurssienhallinta mallin ennen mallirakenteeseen viedyn Resurssienhallinta-malli.

### <a name="b-method-2--add-a-new-template-resource-from-browse"></a>B. Tapa 2: Lisää uuden mallin resurssin Selaa

Voit myös lisätä uuden **mallin** taittopöydällä käyttämästä + Lisää komentopainike **Selaa > mallit**. Sinun on annettava nimi, kuvaus ja Resurssienhallinta-mallin JSON.

![Lisää malli](media/add-template-portal1.PNG)  <br />

> [AZURE.NOTE] Microsoft.Gallery perustuu palvelutili Azure resurssin toimittaja. Mallin resurssi on yhdistetty luonut käyttäjä. Se ei ole sidottu tietyn-tilaukseen. Tilaus on valittu, vain, kun otat mallin käyttöön.

## <a name="view-template-resources"></a>Näytä mallin resurssit

Kaikki **mallien** käytettävissä näkevät, milloin **Selaa > mallit**. Tämä sisältää sekä lähimpään kokonaislukuun, jotka on jaettu kanssasi erilaisten määritetyt käyttöoikeudet luomasi **Mallit** . Lisätietoja [käyttöoikeuksien hallinta](#access-control-for-a-tenant-resource-provider) -osassa.

![Näytä malli](media/view-template-portal1.PNG)  <br />

Voit tarkastella tietoja **malli** napsauttamalla kohteeseen-luettelossa.

![Näytä malli](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Muokkaa mallia resurssi

Voit aloittaa **mallin** Muokkaa-työnkulku Selaa luettelon kohtaan napsauttaminen hiiren kakkospainikkeella tai valitsemalla Muokkaa-komento-painiketta.

![Mallin muokkaaminen](media/edit-template-portal1a.PNG)  <br />

Voit muokata Resurssienhallinta mallin tekstit tai kuvaus. Nimeä ei voi muokata, koska se on Resurssienhallinta-resurssinimi. Kun muokkaat Resurssienhallinta-mallin JSON on vahvistaa varmistaa, että on kelvollinen JSON. Valitse **OK** ja sitten **Tallenna** päivitetyn mallin tallentaminen.

![Mallin muokkaaminen](media/edit-template-portal2a.PNG)  <br />

Kun **malli** tallennetaan vahvistus-ilmoitus tulee näkyviin.

![Mallin muokkaaminen](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Mallin resurssin käyttöönotto

Voit ottaa **mallin** , jotka sinulla **lukuoikeudet** . Käyttöönotto-työnkulku käynnistyy vakio Azure mallin käyttöönottoa sivu. Täytä jatkaa käyttöönottoa Resurssienhallinta Malliparametrien arvot.

![Mallin ottaminen käyttöön](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Mallin resurssien jakaminen

**Mallin** resurssi voidaan jakaa kollegoiden kanssa. [Azure-resurssin roolimääritys](../active-directory/role-based-access-control-configure.md)jakaminen toimii samalla tavalla. **Mallin** omistaja on käyttöoikeudet käyttäjille, joilla voi olla vuorovaikutuksessa mallin resurssiin liittyvien. Henkilön tai henkilöryhmän kanssa jaat **mallin** voivat Resurssienhallinta-malli ja sen valikoima ominaisuuksien tarkasteleminen.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Microsoft.Gallery-resurssien käyttöoikeuksien valvonta

Rooli | Käyttöoikeudet
---|----
Omistaja | Täydet oikeudet, mukaan lukien Jaa mallin resurssin avulla
Lukija | Sallii luku- ja Execute(Deploy) mallin resurssi
Avustaja | Sallii mallin resurssin Muokkaa ja poista käyttöoikeudet. Käyttäjä voi jakaa mallin muiden kanssa

Valitse **Jaa** selata kohteessa, napsauttamalla hiiren kakkospainikkeella tai tietyn kohteen näkymä-sivu. Tämä käynnistää Jaa-toiminto.

![Jaa-malli](media/share-template-portal1a.png)  <br />

 Voit nyt rooli ja käyttäjän tai ryhmän käsitellä tietty **malli**. Käytettävissä olevat roolit ovat omistaja, lukija ja avustaja. Lisätietoja yllä olevien [käyttöoikeuksien hallinta](#access-control-for-a-tenant-resource-provider) .

![Jaa-malli](media/share-template-portal2b.png)  <br />

![Jaa-malli](media/share-template-portal3b.png)  <br />

Valitse **Ok**ja **Valitse** . Nyt näet niiden käyttäjien tai ryhmien resurssi on lisätty.

![Jaa-malli](media/share-template-portal4b.png)  <br />

> [AZURE.NOTE] Mallin voi jakaa vain käyttäjien ja ryhmien samassa Azure Active Directory-alihallinnassa. Jos jaat mallin sähköpostiosoite, joka ei ole vuokraajan, kutsun lähetetään kysyy käyttäjältä liittyä vieraana alihallintaan.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Resurssienhallinta-mallien luomisesta on artikkelissa [julkaisu-mallit](../resource-group-authoring-templates.md)
- Funktiot, voit käyttää Resurssienhallinta-mallissa on artikkelissa [mallin Funktiot](../resource-group-template-functions.md)
- Ohjeita mallisi suunnittelemisesta on artikkelissa [parhaita käytäntöjä suunnitteluun Azure Resurssienhallinta-mallit](../best-practices-resource-manager-design-templates.md)
