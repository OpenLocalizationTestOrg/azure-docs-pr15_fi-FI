<properties
   pageTitle="Voit käyttää Azure Redis välimuistin Java | Microsoft Azure"
    description="Azure Redis välimuistin käyttämällä Java käytön aloittaminen"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-java"></a>Voit käyttää Azure Redis välimuistin Java

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis välimuistin pääset erillinen Redis välimuisti-hallinnassa on käytössä Microsoft. Välimuistin on käytettävissä mistä tahansa sovelluksesta Microsoft Azure kuluessa.

Tässä ohjeaiheessa kerrotaan, miten Aloita Azure Redis välimuistin käyttämällä Java.

## <a name="prerequisites"></a>Edellytykset

[Jedis](https://github.com/xetorthio/jedis) - Java asiakkaan Redis.txt

Tässä opetusohjelmassa käyttää Jedis, mutta voit määrittää minkä tahansa Java-asiakasohjelman tarkoitettuihin [http://redis.io/clients](http://redis.io/clients).

## <a name="create-a-redis-cache-on-azure"></a>Luo Redis.txt välimuistin Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Noutaa host nimet ja avaimet

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Ottaa käyttöön-SSL päätepisteen

Redis.txt Joissakin palveluissa ei tue SSL-Suojausta ja oletusarvon mukaan [- SSL portti ei ole käytettävissä Azure Redis välimuistin uusia esiintymiä](cache-configure.md#access-ports). Milloin tämä kirjoittaminen [Jedis](https://github.com/xetorthio/jedis) asiakas ei tue SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## <a name="add-something-to-the-cache-and-retrieve-it"></a>Lisää jotain välimuistin ja hakea sitä

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Seuraavat vaiheet

- [Käyttöön välimuistin diagnostiikka](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) , niin voit [näytön](https://msdn.microsoft.com/library/azure/dn763945.aspx) välimuistin kunto.
- Lue virallinen [Redis ohjeissa](http://redis.io/documentation).

