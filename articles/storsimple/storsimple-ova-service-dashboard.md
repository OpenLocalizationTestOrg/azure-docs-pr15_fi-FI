<properties 
   pageTitle="StorSimple Managerin Raporttinäkymät-ikkunan service - Virtual matriisin | Microsoft Azure"
   description="Tässä artikkelissa kuvataan StorSimple hallinnan palvelun Raporttinäkymät-ikkunan ja kerrotaan, miten voit käyttää sitä StorSimple Virtual matriisin kunnon valvonta."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-dashboard-for-the-storsimple-virtual-array"></a>StorSimple Virtual matriisin StorSimple hallinnan palvelun Raporttinäkymät-ikkunan käyttäminen

## <a name="overview"></a>Yleiskatsaus

StorSimple hallinta palvelun dashboard-sivulla on yhteenvetonäkymän StorSimple Virtual matriisin (tunnetaan myös nimellä StorSimple paikallisen virtual tai virtual laitteet), jotka ovat yhteydessä StorSimple hallintapalvelu korostaminen ne, jotka järjestelmänvalvojan huomiota. Tässä opetusohjelmassa esitellään dashboard-sivu, kerrotaan Raporttinäkymät-ikkunan sisällön ja -funktio ja kuvataan tehtävät, joita voit suorittaa tältä sivulta.

![Palvelun koontinäyttö](./media/storsimple-ova-service-dashboard/dashboard1.png)

StorSimple hallinta palvelun Raporttinäkymät-ikkunan näkyvät seuraavat tiedot:

- Virtuaalinen laitteet haluamasi arvot näkyvät **sivun yläreunassa kaavioalue** . Voit tarkastella kaikkien virtual laitteiden välillä ensisijainen tallennustilaa sekä kulutettu virtual laitteiden ajan kuluessa pilvitallennustilaa. Käyttämällä kaavion oikeassa yläkulmassa ohjausobjektit, Määritä suhteellinen tai absoluuttinen käyttö ja tarkistaa 1 viikko, 1 kuukausi, 3 kuukauden tai vuoden 1 aika-asteikon. Päivitä-toiminnolla ![päivitysasetukset](./media/storsimple-ova-service-dashboard/refresh-control.png) päivittää kaavion.

- **Käyttö yleiskatsaus** näkyy ensisijainen tallennustila on valmisteltu ja kulutettu kaikki virtual laitteiden käytettävissä kokonaistallennustila suhteessa kaikkien virtual laitteiden välillä. **Provisioned** viittaa tallennustilan on valmisteltu ja varattu **käytetty** viittaa käyttö osakkeet tai asemat kuin tarkastella laitteistokäynnistäjät, jotka ovat yhteydessä virtual laitteet ja **enintään kapasiteetti** näkyy kaikki virtual laitteet suurin kapasiteetti.

- **Ilmoitukset** on aktiivinen ilmoitusten tilannevedoksen yli virtual laitteet-ilmoitusten vakavuus Ryhmittelyperuste. Vakavuus-tason valitseminen avaa suodatetut hälytykset Näytä **ilmoitukset** -sivulta. **Ilmoitukset** -sivulla voit valita yksittäisiä ilmoituksen Saat lisätietoja ilmoituksen, mukaan lukien kaikki suositeltavat toimenpiteet. Voit myös poistaa ilmoituksen, jos ongelma on ratkaistu.

- **Projektit** -alueelle mahdollistaa tilannevedoksen viimeisimmät työt virtual palveluun liitettyjen laitteiden välillä. On linkkejä, joiden avulla voit tarkastella työt, jotka ovat parhaillaan käynnissä ja mitkä onnistui tai epäonnistui viimeisen 24 tunnin aikana. 

- Valitse sivun oikeassa **quick glance** -alueella on hyödyllisiä tietoja, kuten palvelutila-virtual palvelu, -palvelun sijainti ja tilauksen, joka on liitetty palvelun tiedot liitettyjen laitteiden kokonaismäärän. On myös lokiin toiminnot linkki. Valitse luettelo kaikki valmiit StorSimple hallinta-palvelutoiminnot linkki. 

Voit käyttää StorSimple hallinta-palvelun raporttinäkymäsivu aloittaa seuraavat toimet:

- Pyydä palvelun rekisteröinti-näppäintä.
- Näytä toiminnon lokit.

## <a name="get-the-service-registration-key"></a>Palvelun rekisteröinti Key-tunnuksen hankkiminen

Palvelun rekisteröinti avainta käytetään Rekisteröi StorSimple virtual laitteen StorSimple hallintapalvelu niin, että laitteen näkyy edelleen hallinnan toiminnot Azure perinteinen-portaalissa. Avain luodaan ensimmäisen virtual laitteessa ja jaettu virtual muissa laitteissa. 

**Palvelun rekisteröinti avainta** -valintaikkuna, missä voit nykyisen palvelun rekisteröinti avaimen Kopioi Leikepöydälle tai palvelun rekisteröinti avain uudelleen valitsemalla **Rekisteröinti Key** (sivun alareunassa) avautuu.

![rekisteröinti avain](./media/storsimple-ova-service-dashboard/service-dashboard3.png)

Avain uudelleen ei vaikuta aiemmin rekisteröidyn virtual laitteet: se vaikuttaa vain virtual laitteita, jotka on rekisteröity-palvelussa, kun näppäintä uudelleen.

Lisätietoja käytön palvelun rekisteröinti-näppäintä Siirry kohtaan [Hae palvelun rekisteröinti-näppäintä](storsimple-ova-manage-service.md#get-the-service-registration-key).

## <a name="view-the-operations-logs"></a>Näytä toiminnot-lokit

Voit tarkastella toimintojen lokit napsauttamalla käytettävissä koontinäytön **quick glance** -ruudussa toiminnon lokit-linkkiä. Tällöin näyttöön hallinta palveluita-sivulle, jossa voit suodattaa ja nähdä tietyn lokit StorSimple hallintapalveluun.

![Kirjaudu toiminnot](./media/storsimple-ova-service-dashboard/ops-log.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Tietoja [paikallisen sivuston Käyttöliittymä ammattimainen StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md)käyttämisestä.