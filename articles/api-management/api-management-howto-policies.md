<properties 
    pageTitle="Azure API hallinnan käytännöt | Microsoft Azure" 
    description="Lue, miten voit luoda, muokata ja määrittää käytäntöjä API hallinta." 
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


#<a name="policies-in-azure-api-management"></a>Azure API hallinnan määrittäminen

Azure-Ohjelmointirajapinnan hallinta-käytännöt ovat tehokas ominaisuuksien järjestelmän, joka mahdollistaa Publisherin määrittämisen jälkeen Ohjelmointirajapinnan toiminnan muuttaminen. Käytännöt ovat lausekkeita, jotka suoritetaan järjestyksessä pyynnön kokoelma tai Ohjelmointirajapinnan vastaus. Suositut lauseet ovat tiedostomuodon muuntaminen XML: stä JSON ja soita rajoittaa kehittäjä Saapuvien puhelujen määrän rajoittaminen korko. Monta käytäntöjä on käytettävissä oleva ruutu.

Hae [Käytännön viitata][] täydellinen luettelo käytännön lauseita ja niiden asetukset.

Käytäntöjä käytetään Gatewayn, joka on avattujen Ohjelmointirajapinta kuluttaja- ja hallitun Ohjelmointirajapinnan välillä. Yhdyskäytävän vastaanottaa kaikki pyynnöt ja välittää niitä sellaisenaan pohjana ohjelmointirajapinnan yleensä. Käytännön voit kuitenkin Ota muutokset käyttöön saapuva pyyntö ja lähtevien vastaus.

Käytännön lausekkeiden voidaan määritteiden arvot tai jokin API hallintakäytännöt tekstiarvoja, ellei käytäntö määrittää muuten. Joitakin käytäntöjä, kuten [Hallintavuo][] ja [Määritä muuttujan][] käytännöt perustuvat käytännön lausekkeita. Lisätietoja on artikkelissa [Advanced käytännöt][] ja [käytännön lausekkeet][].

## <a name="scopes"> </a>Käytäntöjen määrittäminen
Käytännöt voidaan määrittää yleisesti tai [tuotteen][], [API][] tai [toiminnon][]aluetta. Jos haluat määrittää käytännön, siirry käytännöt editorin publisher-portaalissa.

![Käytännöt valikko][policies-menu]

Käytännöt editorin koostuu kolme pääosaa: käytännön alue (päällimmäisenä), jossa käytännöt muokataan (vasen) ja lauseet luettelo (oikea) käytännön määritys:

![Käytännöt-editori][policies-editor]

Voit aloittaa, sinun on ensin valittava alue, johon käytäntö on voimassa käytännön määrittäminen. **Starter** alla olevassa näyttökuvassa tuote on valittuna. Huomaa, että käytännön nimen vieressä neliön muotoinen merkki ilmaisee, että käytäntö otetaan käyttöön tällä tasolla jo.

![Laajuus][policies-scope]

Koska käytäntö on jo käytössä, määritykset näkyy määritelmä-näkymässä.

![Asetusten määrittäminen][policies-configure]

Käytäntö tulee näkyviin vain luku-ensimmäinen. Jotta voit muokata määritys valitsemalla **Käytännön määrittäminen** -toiminto.

![Muokkaa][policies-edit]

Käytännön määritys on yksinkertainen XML-asiakirja, jossa kuvataan saapuvan ja lähtevän lauseet järjestyksessä. XML: voi muokata suoraan määritelmä-ikkuna. Luettelon väittämistä on annettu oikealle ja lauseiden koskevat vaikutusalueeseen käytössä ja korostettu; osoittanut yllä olevassa näyttökuvassa **Raja Soita korko** -lause.

On käytössä komento valitsemalla Lisää sopiva XML määritelmä-näkymässä kohdistimen kohtaan. 

>[AZURE.NOTE] Jos käytäntö, jonka haluat lisätä ei ole otettu käyttöön, varmista, että olet kyseisen käytännön oikea alue. Kunkin käytännön-lause on suunniteltu tiettyjen alueiden ja käytännön osat. Tarkistettava käytännön osat ja alueet käytännön Tarkista [Käytännön viitata][]käytännön **käyttö** -osassa.

Luettelon kaikista käytännön lauseita ja niiden asetukset ovat käytettävissä [Käytännön viitata][].

Jos haluat lisätä uuden ilmoituksen rajoittaa pyynnöt määritetty IP-osoitteet, Vie osoitin vain sisältö `inbound` XML-rakenne ja valitse **Rajoita soittajan IP-osoitteet** -lause.

![Rajoituksen käytännöt][policies-restrict]

Tämä lisää XML-koodikatkelman, `inbound` elementti, joka on ohjeita siitä, miten voit määrittää lause.

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

Jos haluat rajoittaa saapuvia pyyntöjä ja hyväksy vain ne IP-osoitteesta 1.2.3.4 Muokkaa XML seuraavasti:

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![Tallenna][policies-save]

Kun määrittäminen lauseet käytännön, valitse **Tallenna** ja muutokset otetaan käyttöön API Management Gateway heti.

##<a name="sections"> </a>Tietoja käytännön määritys

Käytäntö on väittämistä, joka suoritetaan, jotta pyynnön ja vastauksen sarja. Kokoonpano on jaettu asianmukaisesti `inbound`, `backend`, `outbound`, ja `on-error` osat seuraavassa määritysten mukaisesti.

    <policies>
      <inbound>
        <!-- statements to be applied to the request go here -->
      </inbound>
      <backend>
        <!-- statements to be applied before the request is forwarded to 
             the backend service go here -->
      </backend>
      <outbound>
        <!-- statements to be applied to the response go here -->
      </outbound>
      <on-error>
        <!-- statements to be applied if there is an error condition go here -->
      </on-error>
    </policies> 

Pyynnön käsittelemisen aikana tapahtuu virhe, jos kaikki jäljellä olevat vaiheet `inbound`, `backend`, tai `outbound` osien ohitetaan ja suorittamisen siirtyy lauseet `on-error` osa. Sijoittamalla käytännön lauseet `on-error` osan avulla voit tarkastella virheen `context.LastError` ominaisuutta, tarkastaa ja mukauttaa virhe vastauksen käyttämällä `set-body` käytännön, ja määritä, mitä tapahtuu, jos näyttöön tulee virhesanoma. Virhekoodit valmiin ohjeet ja virheet, joita voi ilmetä käsiteltäessä käytännön väittämistä on. Lisätietoja on artikkelissa [virheen käsittely API hallintakäytäntöjä](https://msdn.microsoft.com/library/azure/mt629506.aspx).

Koska käytännöt voidaan määrittää eri tasoilla (Yleinen, tuote, api ja toiminta) määritystä avulla voit määrittää järjestyksen, jossa käytännön määritys lauseet suorittaa ylemmän tason käytännön osalta. 

Käytännön käyttöalueen arvioidaan seuraavassa järjestyksessä.

1. Yleinen alue
2. Tuotteen laajuus
3. API laajuus
4. Toiminnon laajuus

Ne lauseita lasketaan mukaan sijaintia `base` elementti, jos se ei sisällä tietoja.

Esimerkiksi jos käytännön yleinen tasolla ja määritetty Ohjelmointirajapinnan käytännön, valitse aina, kun kyseinen tietyn API käytetään sekä käytännöt käytetään. API hallinnan avulla yhdistetyn käytännön lauseita kautta perus lisätä deterministic tilaamista varten. 

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

Esimerkki käytännön määritys-edellä `cross-domain` lauseen suoritetaan ennen kuin suurempi käytäntöjä, jotka haluat vuorostaan voi seurata `find-and-replace` käytännön.

Jos käytännön näkyy kahdesti tietojenhallintalausunnon, viimeksi arvioitu käytäntö otetaan käyttöön. Tämän avulla voit ohittaa käytännöt, jotka on määritetty suurempi alueessa. Saat käytännöt nykyisessä tilassa käytäntö-editori valitsemalla **uudelleenlaskenta käytännössä valitun alueen**.

Huomaa, että yleinen käytäntö on ylemmän tason ei käytäntöä ja käyttämisestä `<base>` osa ei ole mitään vaikutusta. 

## <a name="next-steps"></a>Seuraavat vaiheet

Tutustu seuraavan videon käytännön rajoituksista.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Käytännön viitata]: api-management-policy-reference.md
[Tuotteen]: api-management-howto-add-products.md
[OHJELMOINTIRAJAPINTA]: api-management-howto-add-products.md#add-apis 
[Toiminto]: api-management-howto-add-operations.md

[Lisäasetusten määrittäminen]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Hallintavuo]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Aseta muuttuja]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Käytännön lausekkeet]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
