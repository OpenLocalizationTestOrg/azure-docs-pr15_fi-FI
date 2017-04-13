<properties
    pageTitle="Voit käyttää Azure Redis välimuistin Python | Microsoft Azure"
    description="Azure Redis välimuistin käyttämällä Python käytön aloittaminen"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-python"></a>Voit käyttää Azure Redis välimuistin Python

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Tässä ohjeaiheessa kerrotaan, miten Aloita Azure Redis välimuistin Python avulla.


## <a name="prerequisites"></a>Edellytykset

Asenna [Redis.txt kopioi](https://github.com/andymccurdy/redis-py).


## <a name="create-a-redis-cache-on-azure"></a>Luo Redis.txt välimuistin Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Noutaa host nimet ja avaimet

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Ottaa käyttöön-SSL päätepisteen

Redis.txt Joissakin palveluissa ei tue SSL-Suojausta ja oletusarvon mukaan [- SSL portti ei ole käytettävissä Azure Redis välimuistin uusia esiintymiä](cache-configure.md#access-ports). Milloin tämä kirjoittaminen [Redis.txt kopioi](https://github.com/andymccurdy/redis-py) asiakas ei tue SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Lisää jotain välimuistin ja hakea sitä


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Korvaa `<name>` välimuistin nimesi ja `key` access-näppäimen kanssa.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
