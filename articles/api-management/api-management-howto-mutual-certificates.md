<properties 
    pageTitle="Suojaamisesta taustatietokantaan-palveluita käyttämällä asiakkaan todennus Azure API hallinta" 
    description="Tietoja suojatun taustatietokantaan-palveluita käyttämällä Asiakasvarmenteen todennus Azure API hallinnassa." 
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

# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Suojaamisesta taustatietokantaan-palveluita käyttämällä asiakkaan todennus Azure API hallinta

API hallinta tarjoaa mahdollisuuden suojata access Ohjelmointirajapinnan taustatietokantaan-palvelun avulla asiakkaan varmenteet. Tämän oppaan näyttää varmenteet API publisher-portaalin hallinta ja käyttää varmenteen taustatietokantaan palvelussa Ohjelmointirajapinnan määrittämisestä.

Lisätietoja varmenteiden API hallinta REST-Ohjelmointirajapinnan käyttäminen hallinta artikkelissa [Azure API hallinta REST API varmenteen kohteen][].

## <a name="prerequisites"> </a>Edellytykset

Tämän oppaan avulla voit Määritä API Management service-esiintymä käyttää Asiakasvarmenteen todennus Ohjelmointirajapinnan taustatietokantaan palvelu. Ennen noudattamalla tämän ohjeaiheen, olisi on määritetty Asiakasvarmenteen todennus ([määrittäminen Azure sivustot-todennus viittaa tämän artikkelin][]) taustatietokantaan palvelun ja voivat käyttää varmenteen ja salasanan varmenteen ladataan API hallinta publisher-portaalissa.

## <a name="step1"> </a>Lataa Asiakasvarmenne

Aloita valitsemalla Azure perinteinen portaalissa API Management-palvelun **hallinta** . Tämä avaa API hallinta publisher-portaalin.

![API Publisher-portaalissa][api-management-management-console]

>Jos et ole luonut erillisen service API hallinta-kohdassa [Azure API hallinnan käytön aloittaminen][] -opetusohjelma [Luo erillisen service API hallinta][] .

Valitse **Security** vasemmalla **API hallinta** -valikosta ja valitse **asiakkaan varmenteet**.

![Asiakkaan varmenteet][api-management-security-client-certificates]

Voit ladata uutta varmennetta valitsemalla **Lataa sertifikaatti**.

![Lataa sertifikaatti][api-management-upload-certificate]

Siirry varmennetta ja kirjoita salasana varmenne.

>Varmenne on oltava **.pfx** -muodossa. Itse allekirjoitetun varmenteet on sallittua.

![Lataa sertifikaatti][api-management-upload-certificate-form]

Valitse **Lataa** Lataa sertifikaatti.

>Sertifikaatin salasana tarkistetaan tällä hetkellä. Jos se on väärä virhesanoma näkyy.

![Varmenne on ladattu][api-management-certificate-uploaded]

Kun varmenne on ladattu palvelimeen, se näkyy **asiakkaan varmenteet** -välilehti. Jos sinulla on useita sertifikaatteja, tee muistiinpanon aiheessa tai joita käytetään Valitse varmenne, määritettäessä Ohjelmointirajapinnan käyttäminen todistuksista, kuin seuraavassa [määrittäminen Ohjelmointirajapinnan käyttäminen Asiakasvarmenne yhdyskäytävän todennusta varten][] -kohdassa allekirjoitus, viimeiset neljä merkkiä.

>Käytöstä varmenteen ketju vahvistamista kun käytät, esimerkiksi itse allekirjoitetun varmenteen on kuvattu tämän usein kysytyt kysymykset- [kohteen](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)noudattamalla.

## <a name="step1a"> </a>Asiakkaan varmenteen poistaminen

Varmenteen poistaminen valitsemalla haluamasi varmenne vieressä **Poista** .

![Varmenteen poistaminen][api-management-certificate-delete]

Valitse Vahvista **Kyllä, poista se** .

![Vahvista poistaminen][api-management-confirm-delete]

Jos varmennetta ei käytössä Ohjelmointirajapinta, Varoitus-näyttö tulee näkyviin. Varmenteen poistaminen sinun on ensin poistettava varmenteen API, joille on määritetty käyttämään sitä.

![Vahvista poistaminen][api-management-confirm-delete-policy]

## <a name="step2"> </a>Ohjelmointirajapinnan Asiakasvarmenne käytettävät yhdyskäytävän käyttöoikeuksien määrittäminen

Valitse **ohjelmointirajapinnan** vasemmalla **API hallinta** -valikosta haluamasi Ohjelmointirajapinnan nimeä ja **Suojaus** -välilehti.

![API-suojaus][api-management-api-security]

Valitse **asiakkaan varmenteet** **tunnuksilla** -avattavasta luettelosta.

![Asiakkaan varmenteet][api-management-mutual-certificates]

Valitse haluamasi varmenne **Asiakasvarmenne** avattavasta luettelosta. Jos määritettynä on useita sertifikaatteja voi tarkastella kohdetta tai viimeiset neljä merkkiä, kuten edellisessä osassa allekirjoitus oikea sertifikaatti.

![Valitse varmenne][api-management-select-certificate]

Valitse Tallenna määritysten muutos ohjelmointirajapinnan **Tallenna** .

>On tämän muutoksen heti ja puhelut, API toimintoja käyttämällä varmenteen todennetaan taustatietokantaan palvelimessa.

![Ohjelmointirajapinnan muutosten tallentaminen][api-management-save-api]

>Kun varmenne on määritetty yhdyskäytävän todennus Ohjelmointirajapinnan taustatietokantaan-palveluun, se tulee osaksi käytännön kyseisen API ja voidaan tarkastella käytäntö-editorissa.

![Sertifikaatin käytäntö][api-management-certificate-policy]

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja muita tapoja suojata Taustajärjestelmä-palvelun HTTP basic tai jaettu salainen todennusta, kuten seuraavassa videossa.

> [AZURE.VIDEO last-mile-security]

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Azure API hallinnan käytön aloittaminen]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance

[Azure API hallinta REST API varmenteen kohde]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Voit määrittää todennus-Azure sivustojen viitata tässä artikkelissa]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Määritä Ohjelmointirajapinnan käyttäminen Asiakasvarmenne yhdyskäytävän todennusta varten]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps


 
