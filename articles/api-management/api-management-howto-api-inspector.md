<properties 
    pageTitle="Jäljittää API tarkastaminen-toiminnolla voit soittaa Azure API hallinta" 
    description="Opettele jäljittää puhelujen soittaminen API tarkastaminen Azure API hallinta." 
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

# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Jäljittää API tarkastaminen-toiminnolla voit soittaa Azure API hallinta

API hallinta sisältää API tarkastaminen-työkalu auttaa sinua virheenkorjaus ja että ohjelmointirajapinnan vianmääritys. API tarkastaminen-toiminnon avulla voidaan ohjelmallisesti ja voidaan myös suoraan developer-portaalissa. 

Jäljitys-toimintojen lisäksi Ohjelmointirajapinta tarkastaminen jäljittää [käytännön lauseketta](https://msdn.microsoft.com/library/azure/dn910913.aspx) arvioinnit. Katso esittely, [Cloud kattavat jakson 177: Lisää API hallintaominaisuudet](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ja eteenpäin siirtyminen 21:00.

Tässä oppaassa on hallintapaketteihin käytön API tarkastaminen.

>[AZURE.NOTE] API tarkastaminen jäljittää vain luodaan ja saataville pyynnöt, jotka sisältävät tilauksen näppäimet, jotka kuuluvat [järjestelmänvalvojan](api-management-howto-create-groups.md) tilille.

## <a name="trace-call"></a> Käyttö API tarkistaminen Jäljitä puhelun

Käyttämällä API tarkastamista, Lisää **ocp apim-jäljitys: TOSI** pyytää otsikon toiminto-puheluun, lataa ja tarkistaminen Jäljitä tekstiarvona ilmaistun **ocp-apim-jäljitys-sijaintiin** vastauksen otsikon URL-Osoitteen avulla. Tämän voi tehdä ohjelmallisesti ja voi tehdä myös suoraan developer-portaalissa.

Tässä opetusohjelmassa kerrotaan, miten jäljitys toiminnot, joka on määritetty [Hallitse ensimmäisen API](api-management-get-started.md) hakeminen aloittaminen opetusohjelmassa Basic laskuri-Ohjelmointirajapinnan käyttäminen API tarkastaminen-toiminnolla. Jos et ole suorittanut kyseisen opetusohjelma kestää vain hetkeksi tuo Basic Laskimen Ohjelmointirajapinnan tai voit käyttää toisen API kuten Kaiku Ohjelmointirajapinnan sähköpostikansioon. Kunkin API Management-palvelun esiintymän tulee esimääritettyjä Kaiku-ohjelmointirajapinnalla, jonka avulla voidaan kokeilla ja tutustu API hallinta. Päivitä-Ohjelmointirajapinnan palauttaa takaisin lähetetään jostakin syöte. Sitä käytetään voit käynnistää minkä tahansa HTTP-toiminnon ja palautusarvon yksinkertaisesti päivitetään, voit lähettää. 



Aloita valitsemalla Azure perinteinen Portal-Ohjelmointirajapinnan hallintapalvelun **developer-portaalissa** . Toimintoja voidaan kutsua suoraan developer-portaalissa, joiden avulla voit tarkastella ja testaa Ohjelmointirajapinnan toimintojen kätevästi.

>Jos et ole vielä luonut erillisen service API hallinta-kohdassa [Azure API hallinnan käytön aloittaminen][] -opetusohjelma [Luo erillisen service API hallinta][] .

![API hallinta developer-portaalissa][api-management-developer-portal-menu]

Valitse **ohjelmointirajapinnan** yläreunan valikosta ja valitse sitten **Basic Laskimen**.

![Päivitä Ohjelmointirajapinta][api-management-api]

Valitse **Kokeile** , yritä **Lisää kahden kokonaisluvun** .

![Kokeile][api-management-open-console]

Oletusasetus parametriarvot ja valitse sitten tilauksen avainta tuotteen, jota haluat käyttää **tilauksen avainta** avattavan luettelon.

Oletusarvoisesti developer-portaalissa **Ocp Apim-jäljitys** otsikon jo on määritetty **Tosi**. Tämä ylätunniste määrittää, onko jäljityksen luodaan.

![Lähetä][api-management-http-get]

Valitse **Lähetä** käynnistää toiminnon.

![Lähetä][api-management-send-results]

Vastauksen otsikot ovat **ocp-apim-jäljitys-sijaintiin** kuin seuraavassa esimerkissä arvolla.

    ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742

Jäljitys voi ladata määritettyyn sijaintiin ja tarkistanut osoittanut seuraavassa vaiheessa.

## <a name="inspect-trace"> </a>Tarkasta jäljitys

Tarkastele seurannasta arvot, lataa jäljitystiedoston **ocp-apim-jäljitys-sijainnin** URL-osoite. Se on tekstitiedostoon JSON-muodossa ja sisältää merkintöjä samalla tavalla kuin seuraavassa esimerkissä.

    {
        "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
        "traceEntries": {
            "inbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0725926",
                    "data": {
                        "request": {
                            "method": "GET",
                            "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "Connection",
                                    "value": "Keep-Alive"
                                },
                                {
                                    "name": "Host",
                                    "value": "contoso5.azure-api.net"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "mapper",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0726213",
                    "data": {
                        "configuration": {
                            "api": {
                                "from": "/calc",
                                "to": {
                                    "scheme": "http",
                                    "host": "calcapi.cloudapp.net",
                                    "port": 80,
                                    "path": "/api",
                                    "queryString": "",
                                    "query": {},
                                    "isDefaultPort": true
                                }
                            },
                            "operation": {
                                "method": "GET",
                                "uriTemplate": "/add?a={a}&b={b}"
                            },
                            "user": {
                                "id": 1,
                                "groups": [
                                    "Administrators",
                                    "Developers"
                                ]
                            },
                            "product": {
                                "id": 1
                            }
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0727522",
                    "data": {
                        "message": "Request is being forwarded to the backend service.",
                        "request": {
                            "method": "GET",
                            "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "X-Forwarded-For",
                                    "value": "33.52.215.35"
                                }
                            ]
                        }
                    }
                }
            ],
            "outbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1960601",
                    "data": {
                        "response": {
                            "status": {
                                "code": 200,
                                "reason": "OK"
                            },
                            "headers": [
                                {
                                    "name": "Pragma",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Length",
                                    "value": "124"
                                },
                                {
                                    "name": "Cache-Control",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Type",
                                    "value": "application/xml; charset=utf-8"
                                },
                                {
                                    "name": "Date",
                                    "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                                },
                                {
                                    "name": "Expires",
                                    "value": "-1"
                                },
                                {
                                    "name": "Server",
                                    "value": "Microsoft-IIS/8.5"
                                },
                                {
                                    "name": "X-AspNet-Version",
                                    "value": "4.0.30319"
                                },
                                {
                                    "name": "X-Powered-By",
                                    "value": "ASP.NET"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1961112",
                    "data": {
                        "message": "Response headers have been sent to the caller. Starting to stream the response body."
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1963155",
                    "data": {
                        "message": "Response body streaming to the caller is complete."
                    }
                }
            ]
        }
    }

## <a name="next-steps"> </a>Seuraavat vaiheet

-   Katsele esittely jäljittää käytännön lausekkeiden [Cloud kattavat jakson 177: Lisää API hallintaominaisuudet](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Kelata 21:00, katso esittely.

>[AZURE.VIDEO episode-177-more-api-management-features-with-vlad-vinogradsky]

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Azure API hallinnan käytön aloittaminen]: api-management-get-started.md
[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




 