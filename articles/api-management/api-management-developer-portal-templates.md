<properties 
    pageTitle="Mallien käyttäminen Azure API hallinta developer-portaalissa mukauttamisesta | Microsoft Azure" 
    description="Opettele mukauttamaan malleja Azuren API hallinnan developer-portaalissa." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Voit mukauttaa malleja Azuren API hallinnan developer-portaalissa

Azure API hallinta sisältää useita mukauttaminen-toimintoja, joiden avulla järjestelmänvalvojat voivat [mukauttaa developer-portaalissa ulkoasu](api-management-customize-portal.md)sekä mukauttaa käyttämällä mallia, joka määrittää itse sivujen sisällön developer portaalin sivujen sisällön. Käytä [DotLiquid](http://dotliquidmarkup.org/) syntaksista ja määritetty lokalisoitu merkkijonon resursseja, kuvakkeet ja sivun ohjausobjektit, sinulla joustavasti määrittäminen sivujen sisällön tarpeen mukaan mallien avulla.

## <a name="developer-portal-templates-overview"></a>Kehitystyökalut-portaalin mallien yleiskatsaus

Kehitystyökalut-portaalin malleja hallitaan developer-portaalissa API Management-palvelun esiintymän järjestelmänvalvojat. Kehitystyökalut-mallien hallinta API hallinta palvelun-esiintymän Azure perinteinen portaalissa Siirry ja valitse **Selaa**.

![Developer-portaalissa][api-management-browse]

Jos olet jo publisher-portaalissa, voit avata developer-portaalissa valitsemalla **Developer-portaalissa**.

![Kehittäjän portal-valikko][api-management-developer-portal-menu]

Kehitystyökalut-portaalin mallit käyttöön vasemmalle, jos haluat näyttää mukauttaminen-valikon Mukauta-kuvaketta ja valitse **malleja**.

![Kehitystyökalut-portaalin mallit][api-management-customize-menu]

Mallit-luettelo sisältää useita luokkia mallia, joka kattaa eri sivuilla developer-portaalissa. Jokainen malli on erilainen, mutta voit muokata niitä ja Julkaise muutokset vaiheet ovat samat. Muokkaa mallia, napsauttamalla mallin nimeä.

![Kehitystyökalut-portaalin mallit][api-management-templates-menu]

Mallin valitsemalla Avaa developer portaalisivu, joka on mukautettavan mallin mukaan. Tässä esimerkissä **tuoteluettelon** näkyy mallia. **Tuoteluettelon** mallia ohjaa punaiseen suorakulmioon merkitty näytön alue. 

![Tuotteiden tehtäväluettelo-malliin][api-management-developer-portal-templates-overview]

Joitakin malleja, kuten **Käyttäjäprofiili** -mallit, voit mukauttaa eri osissa samalla sivulla. 

![Profiilin mallit][api-management-user-profile-templates]

Kunkin developer julkaisuportaalimalliin-editori on kaksi osaa näkyvät sivun alareunassa. Vasemmalla puolella näkyvät mallin kieleksi-ruudussa, ja oikealla puolella näkyvät mallin tietomalliin. 

Muokkaaminen ruudussa malli sisältää markup, joka ohjaa ulkoasua ja toimintaa vastaavaan sivuun developer-portaalissa. Mallin markup käyttää [DotLiquid](http://dotliquidmarkup.org/) syntaksia. Yksi suosittu DotLiquid-editori on [suunnittelijat DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Mallin muokkaamisen aikana tehdyt muutokset näkyvät reaaliaikainen selaimessa, mutta eivät näy asiakkaille, ennen kuin voit [tallentaa](#to-save-a-template) ja [julkaista](#to-publish-a-template) mallia.

![Mallin merkintä][api-management-template]

**Mallitiedot** -ruutu on tietomallin opas objektit, jotka ovat käytettävissä tiettyyn mallina. Se on tämän oppaan näyttämällä reaaliaikaiset tiedot, jotka ovat näkyvissä developer-portaalissa. Voit laajentaa mallia ruudut valitsemalla **Mallitiedot** -ruudun oikeassa yläkulmassa suorakulmio.

![Malli-tietomalli][api-management-template-data]

Edellisessä esimerkissä on kaksi developer-portaalissa näyttöön tuotteiden, jotka on haettu **Mallitiedot** -ruudussa näkyvät tiedot, kuten seuraavassa esimerkissä.

    {
        "Paging": {
            "Page": 1,
            "PageSize": 10,
            "TotalItemCount": 2,
            "ShowAll": false,
            "PageCount": 1
        },
        "Filtering": {
            "Pattern": null,
            "Placeholder": "Search products"
        },
        "Products": [
            {
                "Id": "56ec64c380ed850042060001",
                "Title": "Starter",
                "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
                "Terms": "",
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            },
            {
                "Id": "56ec64c380ed850042060002",
                "Title": "Unlimited",
                "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
                "Terms": null,
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            }
        ]
    }

**Tuoteluettelon** mallin merkinnät käsittelee tarjota haluttu tulos näyttää kunkin yksittäisen tuotteen tiedot ja linkki tuotteiden sivustokokoelman läpikäyminen tiedot. Huomautus `<search-control>` ja `<page-control>` elementit merkinnät. Nämä hallita etsimällä ja sivutus sivulla olevat ohjausobjektit. `ProductsStrings|PageTitleProducts`on lokalisoitu merkkijonon soluviittaus, joka sisältää `h2` sivun otsikkoteksti. Merkkijono luettelo resursseja, sivun ohjausobjekteja ja kuvakkeet, jotka ovat käytettävissä developer portaalin mallien käyttäminen on artikkelissa [API hallinta sovelluskehittäjän portaalin mallien opas](https://msdn.microsoft.com/library/azure/mt697540.aspx).

    <search-control></search-control>
    <div class="row">
        <div class="col-md-9">
            <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
        {% if products.size > 0 %}
        <ul class="list-unstyled">
        {% for product in products %}
            <li>
                <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
                {{product.description}}
            </li>   
        {% endfor %}
        </ul>
        <paging-control></paging-control>
        {% else %}
        {% localized "CommonResources|NoItemsToDisplay" %}
        {% endif %}
        </div>
    </div>

## <a name="to-save-a-template"></a>Jos haluat tallentaa mallina

Jos haluat tallentaa mallina, valitse Tallenna malli-editorissa.

![Mallin tallentaminen][api-management-save-template]

Tallennetut muutokset eivät ole suoraa developer-portaalissa, kunnes ne on julkaistu.

## <a name="to-publish-a-template"></a>Jos haluat julkaista mallina

Tallennettujen mallien voi julkaista yksitellen tai ollenkaan. Voit julkaista yksittäisiä malli, valitse Julkaise malli-editorissa.

![Julkaise mallia][api-management-publish-template]

Valitsemalla **Kyllä** ja vahvista mallia live developer-portaalissa.

![Vahvista julkaiseminen][api-management-publish-template-confirm]

Jos haluat julkaista kaikki tällä hetkellä julkaisemattomien mallia versiot, valitse **Julkaise** mallit-luettelosta. Julkaisemattomien malleja on nimetty tähti seuraa mallin nimi. Tässä esimerkissä julkaistaan **tuoteluettelon** ja **tuotteen** mallit.

![Mallien julkaiseminen][api-management-publish-templates]

Valitse Vahvista **Julkaise mukautukset** .

![Vahvista julkaiseminen][api-management-publish-customizations]

Äskettäin julkaistuun mallit ovat tehokkaita heti developer-portaalissa.

## <a name="to-revert-a-template-to-the-previous-version"></a>Mallin edellisen version palauttaminen

Voit palata edelliseen julkaistu versio mallia, valitse palautetaan malli-editorissa.

![Palauta mallia][api-management-revert-template]

Vahvista valitsemalla **Kyllä** .

![Vahvista][api-management-revert-template-confirm]

Mallin aiemmin julkaistu versio on julkaistu developer-portaalissa, kun palautustoiminto on valmis.

## <a name="to-restore-a-template-to-the-default-version"></a>Voit palauttaa oletusversio mallia

Mallien palauttaminen niiden oletusarvoisen-versioon on kaksivaiheinen prosessi. Ensin malleja voi palauttaa ja sitten palautettu versio on oltava julkaistu.

Voit palauttaa yhden mallin oletusversio valitsemalla Palauta malli-editorissa.

![Palauta mallia][api-management-reset-template]

Vahvista valitsemalla **Kyllä** .

![Vahvista][api-management-reset-template-confirm]

Palauttaa niiden oletusarvoisen versiot kaikki mallit, valitse malli-luettelosta **palauttaa oletusmalleissa** .

![Palauttaa mallit][api-management-restore-templates]

Palautetun malleja on sitten julkaistava yksitellen tai kaikki kerralla noudattamalla seuraavia ohjeita [Voit julkaista mallin](#to-publish-a-template).

## <a name="developer-portal-templates-reference"></a>Sovelluskehittäjän portaalin mallien opas

Viittaus developer portaalin mallit, merkkijonon resursseja, kuvakkeet ja sivun ohjausobjektit lisätietoja [API hallinta sovelluskehittäjän portaalin mallien opas](https://msdn.microsoft.com/library/azure/mt697540.aspx).

## <a name="watch-a-video-overview"></a>Katso video yleiskatsaus

Seuraavassa videossa esitellään keskustelualueen ja luokitusten lisäämisestä Ohjelmointirajapinta ja toiminta sivut developer-portaalissa mallien avulla.

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







