<properties 
   pageTitle="Lokitiedoston Analytics Syslog viestien | Microsoft Azure"
   description="Syslog on tapahtuman kirjaaminen protokolla, joka on yhteinen Linux.   Tässä artikkelissa kuvataan, miten määritetään Syslog viestijoukkoa lokin analyysin ja he luovat OMS säilössä tietueiden tietoja."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />


# <a name="syslog-data-sources-in-log-analytics"></a>Lokitiedoston Analytics Syslog tietolähteet

Syslog on tapahtuman kirjaaminen protokolla, joka on yhteinen Linux.  Sovellusten lähettää paikalliseen tietokoneeseen tallennettu tai toimitettu Syslog kerääminen viesteissä.  Kun Linux OMS-agentti on asennettu, määrittää paikallisen Syslog daemon, voit lähettää viestejä edelleen agentti.  Agentti lähettää viestin sitten Log Analytics missä vastaavaa tietuetta luodaan OMS säilössä.  

> [AZURE.NOTE]Lokitiedoston Analytics tukee viestijoukkoa rsyslog tai syslog maa lähettää. Oletusarvon syslog daemon punainen on Enterprise Linux CentOS ja Oracle Linux-versio (sysklog) 5 versiota ei tueta syslog tapahtuman keräämistä varten. Syslog tietojen keräämiseen nämä jaot tässä versiossa, [rsyslog daemon](http://rsyslog.com) olisi asennettu ja määritetty korvaa sysklog.

![Syslog sivustokokoelman](media/log-analytics-data-sources-syslog/overview.png)


## <a name="configuring-syslog"></a>Syslog määrittäminen
Linux OMS-agentti kerää vain tapahtumat, joiden tilat ja severities, jotka on määritetty sen asetukset.  Voit määrittää Syslog palvelun OMS-portaalissa tai Linux edustajasi tiedostojen määritysten hallinta.


### <a name="configure-syslog-in-the-oms-portal"></a>Määritä Syslog OMS-portaalissa

Määritä Syslog [Lokiasetukset Analytics-tiedot-valikko](log-analytics-data-sources.md#configuring-data-sources).  Tässä määrityksessä toimitetaan kunkin Linux-agentti määritystiedoston.

Voit lisätä uuden tilan kirjoittamalla sen nimi ja valitsemalla **+**.  Kunkin tilan kerätään viesteihin, joissa on valittu severities.  Tarkista severities varten tietyn tilan, jotka haluat kerätä.  Et voi määrittää muut ehdot suodatin viesteihin.

![Määritä Syslog](media/log-analytics-data-sources-syslog/configure.png)


Oletusarvon mukaan kaikki määritysmuutoksia automaattisesti siirretty, kaikki agenttien vuoksi.  Jos haluat määrittää Syslog manuaalisesti jokaisen Linux-agentti, poista sitten valinta *Käytä alla määritysten Linux-tietokoneissa*.


### <a name="configure-syslog-on-linux-agent"></a>Syslog määrittämistä Linux-agentti

Kun se [OMS agentti on asennettu asiakastietokoneeseen Linux](log-analytics-linux-agents.md)asentaa oletusarvoisesti syslog määritystiedoston, joka määrittää tilojen ja viestit, jotka kerätään vakavuus.  Voit muokata tämän tiedoston, voit muuttaa määrityksiä.  Määritystiedosto on vaihtelevat Syslog daemon asiakas on asennettu.

> [AZURE.NOTE] Jos muokkaat syslog määritystä, sinun on käynnistettävä syslog daemon, jotta muutokset tulevat voimaan.

#### <a name="rsyslog"></a>rsyslog

Rsyslog määritystiedosto sijaitsee **/etc/rsyslog.d/95-omsagent.conf**.  Oletussisällön näkyvät jäljempänä.  Tämä kerää syslog toimialueeltasi paikallisen agentti, kaikki tilat viereiset varoitus tai uudempi versio.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

Voit poistaa toiminnon poistamalla sen määritystiedosto-osa.  Voit rajoittaa severities, jotka ovat kerätä tietyn tilan muokkaamalla, että tila-merkintä.  Voit rajoittaa käyttäjän tilan viestien kanssa vakavuus virheen tai uudempi Muokkaa kyseisen rivin määritystiedoston seuraavankaltaiselta:

    user.error  @127.0.0.1:25224


#### <a name="syslog-ng"></a>syslog maa

Rsyslog määritystiedosto on osoitteessa **/etc/syslog-ng/syslog-ng.conf**sijainti.  Oletussisällön näkyvät jäljempänä.  Tämä kerää syslog toimialueeltasi paikallisen agentti kaikki tilat ja kaikki severities.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };
    
    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };
    
    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };
    
    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };
    
    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };
    
    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };
    
    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Voit poistaa toiminnon poistamalla sen määritystiedosto-osa.  Voit rajoittaa severities, jotka ovat kerätä tietyn tilan poistamalla sen luettelosta.  Jos haluat rajoittaa käyttäjän tilan vain ilmoituksen ja kriittinen viesteihin, muokata määritystiedoston seuraavan osan seuraavasti:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="changing-the-syslog-port"></a>Syslog portin muuttaminen

OMS-agentti seuraa Syslog viestien paikalliseen asiakastietokoneeseen portin 25224 osoitteessa.  Voit muuttaa portin lisäämällä OMS agentti kokoonpanotiedosto **/etc/opt/microsoft/omsagent/conf/omsagent.conf**sijaitsee seuraavassa osassa.  Korvaa **port** -merkinnän 25224 porttinumero, jonka haluat.  Huomaa, että sinun myös on muokattava Syslog daemon lähettää viestejä tähän porttiin määritystiedosto.

    <source>
      type syslog
      port 25224
      bind 127.0.0.1
      protocol_type udp
      tag oms.syslog
    </source>


## <a name="data-collection"></a>Tietojen kerääminen

OMS-agentti seuraa Syslog viestien paikalliseen asiakastietokoneeseen portin 25224 osoitteessa. Syslog daemon määritystiedosto välittää Syslog tämän portin, johon ne kootaan Log Analytics-sovelluksen lähettämät viestit.


## <a name="syslog-record-properties"></a>Syslog tietueen ominaisuudet-valintaikkunassa

Syslog tietueiden tyyppi on **Syslog** ja sen ominaisuudet ovat seuraavassa taulukossa.

| Ominaisuus | Kuvaus |
|:--|:--|
| Tietokoneen | Tietokone, jossa tapahtuma on kerätty. |
| Tilojen | Määrittää järjestelmän, joka on luotu viestin osa. |
| HostIP | Viestin lähettäminen järjestelmän IP-osoite.  |
| Isäntänimi | Viestin lähettäminen järjestelmän nimi. |
| SeverityLevel | Tapahtuman vakavuustaso. |
| SyslogMessage | Viestin tekstiosassa. |
| Prosessin tunnus | Viestin luoneen prosessin tunnus. |
| EventTime | Päivämäärä ja kellonaika, jolloin tapahtuma on luotu.



## <a name="log-queries-with-syslog-records"></a>Log-kyselyjen Syslog tietueisiin

Seuraavassa taulukossa on eri esimerkkejä log kyselyitä, jotka hakevat Syslog tietueet.

| Kyselyn | Kuvaus |
|:--|:--|
| Tyyppi = Syslog | Kaikki Syslogs. |
| Tyyppi = Syslog SeverityLevel = virhe | Kaikki Syslog tietueet virheen vakavuus. |
| Tyyppi = Syslog & #124; mittaa count() tietokoneella | Syslog Laske tietueet tietokoneella. |
| Tyyppi = Syslog & #124; mittaa count() tilan mukaan | Syslog Laske tietueet tilan mukaan. |

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [lokin haut](log-analytics-log-searches.md) tietolähteet ja ratkaisujen kerättyjen tietojen analysointia varten. 
- [Mukautettujen kenttien](log-analytics-custom-fields.md) avulla voit syslog tietueista jäsentää yksittäisiä kenttiä.
- [Määritä Linux tekijöiden](log-analytics-linux-agents.md) muunlaisten tietojen keräämiseen. 
