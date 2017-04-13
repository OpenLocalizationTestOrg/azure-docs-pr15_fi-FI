<properties
pageTitle="Oracle AM kuvia Huomioitavaa | Microsoft Azure"
description="Tietoja ennen niiden käyttöönottoa tuetut määritykset ja rajoitukset Oracle-AM Windows Server Azure-tietokannassa."
services="virtual-machines-windows"
documentationCenter=""
manager="timlt"
authors="rickstercdn"
tags="azure-service-management"/>

<tags
ms.service="virtual-machines-windows"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-windows"
ms.workload="infrastructure-services"
ms.date="09/06/2016"
ms.author="rclaus" />

#<a name="miscellaneous-considerations-for-oracle-virtual-machine-images"></a>Muita huomioon otettavia seikkoja Oracle virtuaalikoneen kuvat



Tässä artikkelissa käsitellään Huomioitavaa Oracle näennäiskoneiden Azure-tietokannassa, jotka perustuvat Oracle-ohjelmiston kuvista Oracle myöntämä.  

-  Oracle-tietokantaan virtuaalikoneen kuvat
-  Oracle Serverin WebLogic virtuaalikoneen kuvat
-  Oracle JDK virtuaalikoneen kuvat

##<a name="oracle-database-virtual-machine-images"></a>Oracle-tietokantaan virtuaalikoneen kuvat
### <a name="clustering-rac-is-not-supported"></a>Klusteroinnin (RAC) ei tueta

Azure tällä hetkellä tue Oracle reaali sovelluksen varausyksiköt (RAC) Oracle-tietokantaan. Vain erillinen esiintymät Oracle-tietokanta on mahdollista. Tämä johtuu siitä Azure ei tue virtual levyn jakaminen luku-/ kirjoitusoikeudet tavalla kesken virtuaalikoneen useita kertoja. Moni UDP myös ei tueta.

### <a name="no-static-internal-ip"></a>Ei ole staattinen sisäinen IP

Azure määrittää kunkin virtual tietokoneen sisäinen IP-osoite. Ellei virtuaalikoneen kuuluu virtual verkkoon, virtuaalikoneen IP-osoite on dynaaminen ja voivat muuttua virtuaalikoneen uudelleenkäynnistyksen jälkeen. Tämä voi aiheuttaa ongelmia, koska Oracle-tietokantaan odottaa on staattinen IP-osoite. Voit välttää ongelman, lisäämällä virtuaalikoneen Virtual Azure-verkkoon. Lisätietoja on kohdassa [VPN](https://azure.microsoft.com/documentation/services/virtual-network/) ja [Luo virtuaalisia verkon Azure-tietokannassa](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="attached-disk-configuration-options"></a>Liitetty levyn kokoonpanoasetusten määrittäminen

Voit sijoittaa datatiedostojen joko käyttöjärjestelmän levyn, virtuaalikoneen tai levyjen, kutsutaan myös tietojen levyjä. Levyjen saattaa tarjota parempi suorituskyky ja koko joustavuutta kuin käyttöjärjestelmän levy. Käyttöjärjestelmän levy saattaa olla parempi vain alle 10 gigatavua (gt)-tietokantoja varten.

Azure Blob storage-palvelu luottaa levyjen. Kunkin levyn pystyy teoreettinen enimmäismäärä on noin 500 i/toimintoja sekunnissa (IOPS). Levyjen suorituskyvyn eivät ehkä ole optimaalista aluksi ja IOPS suorituskykyä voi parantaa huomattavasti noin 60 90 minuuttia jatkuvan toiminnan "tallennus-kohdassa" ajan kuluttua. Jos käyttämättömänä pysyy myöhemmin DVD-levyllä, IOPS suorituskyky saattaa vaikuta toiseen tallennus-ajan jatkuvan toiminnan asti. Lyhyesti sanottuna levyn a lisää aktiivinen, Lisää todennäköisesti on lähestymistapa IOPS optimaalisen.

Helpoin tapa on voit liittää yhteen virtuaalikoneen ja siirtää tietokantatiedostoja kyseisellä levyllä, mutta tämä vaihtoehto on myös eniten rajoittaminen suorituskykyä. Sen sijaan voit usein nopeuttaa tehokas IOPS Jos käytät useita levyjen, tietokannan tietojen levittäminen kattavia ja käytä sitten Oracle automaattinen tallennustilan hallinta (ASM). Lisätietoja on kohdassa [Yleistä Oracle automaattinen tallennuksesta](http://www.oracle.com/technetwork/database/index-100339.html) . Se on mahdollista käyttää useita levyjä raitasarjoittamista käyttöjärjestelmän tasolla, mutta lähestymistapaan ei suositella, koska se ei ole tiedossa IOPS suorituskyvyn parantamiseksi.

Voit liittää useita levyjä mukaan, haluatko priorisoida luettujen suorituskykyä tai kirjoittaa tietokannalle toimintojen kahdella eri tavalla:

- **Oracle ASM omalla** todennäköisesti johtaa suorituskyvyn parantamiseksi kirjoitus-toimintoa, mutta worse IOPS luku toimille verrattuna lähestymistapa levyn matriisien avulla. Seuraava kuva esittää loogisesti Tässä vaihtoehdossa.  
    ![](media/virtual-machines-windows-classic-oracle-considerations/image2.png)

>[AZURE.IMPORTANT] Arvioi trade-off kirjoittamisen suorituskykyä ja lue suorituskyvyn tapauksen mukaan. Todellinen hakutulokset voi vaihdella, kun käytät tätä.

### <a name="high-availability-and-disaster-recovery-considerations"></a>Suuri käytettävyys ja tietojen palauttaminen huomioon otettavia seikkoja

Kun käytät Azuren näennäiskoneiden Oracle-tietokantaan, olet toteuttamisesta suuri käytettävyys ja tietojen palauttaminen ratkaista välttämiseksi minkä tahansa käyttökatkot. Voit myös ovat vastuussa Omat tiedot ja sovelluksen varmuuskopioiminen.

Suuri käytettävyys ja tietojen palauttaminen Oracle tietokannan Enterprise Edition (ilman RAC) Azure-saavutetaan käyttämällä [tietojen suojaa, aktiivinen tietojen suojaa](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html)tai [Oracle Golden portti](http://www.oracle.com/technetwork/middleware/goldengate)-ja kaksi erillistä näennäiskoneiden tietokannoista. Molemmat näennäiskoneiden on oltava sama [pilvipalvelussa](virtual-machines-linux-classic-connect-vms.md) ja samassa [VPN](https://azure.microsoft.com/documentation/services/virtual-network/) jotta he voivat käyttää toistensa päälle yksityinen pysyvä IP-osoite.  Lisäksi on suositeltavaa näennäiskoneiden siten saman [saatavuus](virtual-machines-windows-manage-availability.md) sallimaan Azure sijoittaa ne erillisessä vika toimialueille ja päivittää toimialueet. Vain saman pilvipalvelussa näennäiskoneiden voivat osallistua käytettävyys samat. Kunkin virtuaalikoneen on oltava vähintään 2 Gigatavua muistia ja 5 gt levytilaa.

Oracle tietojen suojaa kanssa suuren käytettävyyden onnistuu yhden virtual machine virtual käyttäjäprofiilista toissijainen (valmiustilassa) tietokannan ensisijaisen tietokannan kanssa ja niiden välillä määrittäminen yksisuuntainen replikoinnin. Tulos on lukuoikeudet kopio tietokannasta. Oracle GoldenGate voit määrittää kaksisuuntaisen replikointi tietokannoista välillä. Opit määrittämään suuren käytettävyyden ratkaisua näillä työkaluilla tietokantoja, on [Aktiivinen tietojen suojaa](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) ja [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) ohjeissa Oracle-sivustosta. Jos sinun on luku-ja kirjoitusoikeudet access tietokannan kopio, voit käyttää [Oracle aktiivinen tietojen suojaa](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

##<a name="oracle-weblogic-server-virtual-machine-images"></a>Oracle Serverin WebLogic virtuaalikoneen kuvat

-  **Klusterointi tuetaan Enterprise Edition vain.** Asiakkaalla on oikeus käyttää WebLogic klusterointi vain silloin, kun yrityksen Edition, WebLogic palvelimen avulla. Älä käytä klusterointi WebLogic Server Standard-version mukana.

-  **Yhteyden aikakatkaisu:** Jos sovellus on riippuvainen yhteydet julkisen päätepisteiden toisessa Azure pilvipalvelussa (kuten tietokannan taso palvelu)-Azure ehkä Sulje Avaa asiakasyhteydet neljä minuutin ajan kuluttua. Tämä voi olla vaikutusta ominaisuudet ja sovellukset käyttäisit jakavat yhteys, koska yhteydet, jotka ovat yli rajoituksen poistaminen käytöstä voi enää voimassa. Jos tämä vaikuttaa sovelluksesi, mieti, kannattaisiko ottaa käyttöön yhteyden jakavat "säilyttämisen" logiikan.

    Jos päätepisteen on *sisäinen* Azure cloud palvelun käyttöönoton (kuten erillinen tietokannan virtual kone sisällä kuin WebLogic näennäiskoneiden *saman* pilvipalvelussa), yhteys on suora ja ei riippuvaisia Azure kuormituksen ja näin ollen eivät koske yhteyden aikakatkaisu.

-  **UDP moni ei tueta.** Azure tukee UDP unicasting, mutta ei monilähetystä tai lähettämistä. WebLogic palvelin ei voi luottaa Azure UDP unicast ominaisuuksia. Varten parhaiten käyttäisit UDP unicast tuloksia, on suositeltavaa WebLogic klusteri säilytettävä staattinen, eli enintään 10 hallitun palvelimissa, jotka sisältyvät klusterin säilyttää.

-  **WebLogic palvelimen odottaa julkisista ja yksityisistä portit sama T3 käytön (esimerkiksi yrityksen JavaBeans käytettäessä).** Harkitse monitasoisten tilanne, jossa palvelun kerroksen (EJB)-sovellus on käynnissä WebLogic palvelinklusterin, jossa on vähintään kaksi hallitun palvelimet-niminen **SLWLS**pilvipalvelussa. Asiakas-taso on eri pilvipalvelussa, yrittää soittaa EJB palvelukerros yksinkertainen Java-ohjelmaa. Koska tarvittaessa lataamaan saldo palvelukerros julkisen kuormituksen päätepisteen on luotava WebLogic palvelinklusterin näennäiskoneiden. Jos yksityinen portin, voit määrittää, että päätepisteen eroaa Julkinen portti (esimerkiksi 7006:7008), tapahtuu virhe, esimerkiksi seuraavat:

        [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

        Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

    Tämä johtuu siitä T3 etäkäyttö, WebLogic palvelimen odottaa kuormituksen tasauspalvelun portti ja hallittujen WebLogic palvelimen portti on sama. Yllä tapauksessa asiakas käyttää portin 7006 (kuormituksen tasauspalvelun-portti) ja hallittujen palvelin Kuuntele 7008 (yksityinen portti). Tämä rajoitus koskee vain T3 access ei HTTP.

    Voit välttää tämän ongelman jollakin seuraavilla tavoilla:

    -  Käytä samaa yksityisten ja julkisten porttinumerot kuormitus tasataan päätepisteet omistautunut T3 access.

    -  Sisällytä seuraava JVM parametri WebLogic palvelimen käynnistettäessä:

            -Dweblogic.rjvm.enableprotocolswitch=true

Aiheeseen liittyviä tietoja on kt artikkelissa **860340.1** osoitteessa <http://support.oracle.com>.

-  **Dynaaminen klusterointi ja kuormituksen rajoitukset.** Oletetaan, että dynaaminen klusterin WebLogic Serverin avulla ja näyttää – Luo yksittäinen, julkisen kuormituksen päätepisteen Azure-tietokannassa. Tämä onnistuu, kunhan, kun käytät kiinteä porttinumero kunkin hallitun palvelimet (ei dynaamisesti varattu alueelta) ja lisää hallitun palvelimet kuin järjestelmänvalvojan seurataan koneet eivät ala (eli enemmän kuin yksi hallitun palvelimen virtuaalikoneen kohden). Jos kokoonpanoa käynnistetään kuin näennäiskoneiden WebLogic palvelimien (, jossa useita WebLogic palvelimen esiintymiä jakaa saman virtuaalikoneen), valitse ei voida useammasta kuin yhdestä tilanteita, WebLogic Server-palvelimien sitoa annetun porttinumero – muut kyseisen virtuaalikoneen epäonnistuu.

    Toisaalta, jos olet määrittänyt järjestelmänvalvoja, johon haluat määrittää automaattisesti yksilöllisen porttinumerot sen hallitun palvelimet, valitse kuormituksen tasaamisen ei ole mahdollista koska Azure ei tue yhdistäminen yhteen julkisen porttiin useita yksityinen portit, kuten olisi määritysten varten tarvittavat.

-  **Weblogic palvelimen virtual tietokoneeseen useita kertoja.** Sen mukaan, koko käyttöönotto vaatimukset kannattaa harkita mahdollisuuden suorittaa useita kertoja WebLogic palvelimen virtual samaan tietokoneeseen, jos virtuaalikoneen on riittävän suuri. Esimerkiksi Normaali koko virtual tietokoneessa, jossa on kaksi sydämiä voitu haluat suorittaa kaksi esiintymää WebLogic palvelimeen. Huomaa kuitenkin, että edelleen kannattaa välttää esittely pisteet virheen arkkitehtuuri joka olisi kirjainkokoa, jos olet käyttänyt yhden virtual tietokoneeseen, jossa on käytössä useita kertoja WebLogic palvelimen kyselyjä. Vähintään kaksi näennäiskoneiden avulla voi olla vaihtoehto ja kaikkien näiden näennäiskoneiden sitten suorittaa WebLogic palvelimen useita kertoja. Kunkin esiintymän WebLogic palvelimen voi olla edelleen samaa klusterin osa. Huomautus, sitä ei kuitenkaan tällä hetkellä voi käyttää Azure koska Azure kuormituksen edellyttää voi jakaa yksilöllinen näennäiskoneiden kuormituksen-palvelinten kuormituksen päätepisteet, käyttämiä WebLogic palvelinta, esimerkiksi saman virtuaalikoneen sisällä.

##<a name="oracle-jdk-virtual-machine-images"></a>Oracle JDK virtuaalikoneen kuvat

-  **JDK 6 ja 7 uusimmat päivitykset.** Suosittelemme, että uusin julkinen, tuettujen versiota Java (tällä hetkellä Java 8), kun Azure myös vapauttaa JDK 6 ja 7 kuvia. Tämä on tarkoitettu vanhojen sovellusten, jotka eivät ole vielä valmis päivitettäväksi JDK 8. Päivitykset edellisen JDK kuvat voi olla käytettävissä yleiseen Microsoft partnership Oracle, jossa on annettu JDK 6 ja 7 kuvia myöntämä Azure on tarkoitus sisältää uudemman yksityinen-päivityksen, joka on tavallisesti tarjoamia Oracle vain käyttäjäryhmän Oracle on tuettu asiakkaille. JDK kuvat uusia versioita on saatavana ajan myötä päivitettyjen versioiden JDK 6 ja 7.

    Käytettävissä olevat JDK-6 ja 7 kuvia ja näennäiskoneiden ja kuvia, johdettu JDK voidaan käyttää vain Azure kuluessa.

-  **64-bittinen JDK.** Oracle WebLogic Serverin virtuaalikoneen kuvat ja Azure myöntämä Oracle JDK virtuaalikoneen kuvat sisältävät sekä Windows Server JDK 64-bittiset versiot.

##<a name="additional-resources"></a>Lisäresursseja
[Azure Oracle virtuaalikoneen kuvat](virtual-machines-linux-classic-oracle-images.md)
