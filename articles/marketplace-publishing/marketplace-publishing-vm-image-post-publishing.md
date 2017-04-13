<properties
   pageTitle="Hallinta virtuaalikoneen kuvan Azure Marketplace | Microsoft Azure"
   description="Yksityiskohtainen opas hallinnasta Azure Marketplace-virtuaalikoneen kuvan alkuperäisen julkaisun jälkeen."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Azure"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="hascipio;"/>

# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Jälkeinen tuotannon oppaan virtuaalikoneen tarjouksia Azure Marketplacesta

Tässä artikkelissa kerrotaan, miten voit päivittää live virtuaalikoneen tarjouksen Azure Marketplacesta. Se myös antaa ohjeet-luettelokohteiden lisääminen vähintään yksi uusi tuotteissa aiemmin tarjouksen ja poista live virtuaalikoneen tarjous tai SKU Azure Marketplacesta.

Kun tarjous/tuote on vaiheistettu [Azure-portaalissa](http://portal.azure.com), et voi muuttaa alla kentät:

- **Tarjota tunnus:** [Julkaiseminen portal -> näennäiskoneiden -> Valitse tarjous -> AM kuvat-välilehti -> tarjota tunnus]
- **SKU tunnus:** [Julkaiseminen portal -> näennäiskoneiden -> Valitse tarjous -> tuotteissa välilehti -> Lisää SKU: ta]
- **Publisher Namespace:** [Julkaiseminen portal -> näennäiskoneiden -> ongelmatilanteita-välilehti > Kerro meille tietoja yrityksesi (löytyy kohdasta "Vaiheessa 2 rekisteröidä yrityksen") -> Publisher Namespace -> Namespace]

Kun tarjous/tuote on lueteltu [Azure Marketplacesta](http://azure.microsoft.com/marketplace), et voi muuttaa alla kentät:

- **Tarjota tunnus:** [Julkaiseminen portal -> näennäiskoneiden -> Valitse tarjous -> AM kuvat-välilehti -> tarjota tunnus]
- **SKU tunnus:** [Julkaiseminen portal -> näennäiskoneiden -> Valitse tarjous -> tuotteissa välilehti -> Lisää SKU: ta]
- **Publisher Namespace:** [Julkaiseminen portal -> näennäiskoneiden -> välilehti -> ongelmatilanteita kertoa meille tietoja yrityksen (löytyy kohdasta vaiheessa 2 rekisteröidä) Publisher Namespace -> Namespace]
- **Portit** [Julkaiseminen portal -> näennäiskoneiden -> Valitse tarjous -> AM kuvat-välilehti -> Avaa portit]
- **Hinnat luettelossa SKU(s) muuttaminen**
- **Mallin muuttaminen luettelossa SKU(s) Laskutus**
- **Laskutus luettelossa SKU(s) alueiden poistaminen**
- **Luettelossa SKU(s) tietojen levyn määrän muuttaminen**



## <a name="1-how-to-update-the-technical-details-of-a-sku"></a>1. Voit päivittää SKU teknisistä tiedoista

Voit lisätä uuden version luettelossa olevat versiot ja uudelleenjulkaista tarjous jäljempänä annettujen ohjeiden mukaisesti:

1. Kirjaudu [julkaiseminen portal](https://publish.windowsazure.com).
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla puolella-valikossa **AM kuvat** -välilehti.
4. **Tuotteissa** -osiosta **AM kuvat** -välilehti Etsi tuote, jonka haluat päivittää.
5. Tämän jälkeen voit lisätä uuden version joukon olevat versiot ja valitse sitten **"+"** -painiketta. Uuden version on oltava X.Y.Z muodon jossa X-Y, Z ovat kokonaislukuja. Muutokset vain pitäisi olla vaiheittainen.
6. URI luotu Näennäiskiintolevyn-käyttöjärjestelmän jaettuun käyttöön allekirjoituksen lisääminen **OS Näennäiskiintolevyn URL** -ruutuun ja Tallenna muutokset.

    >[AZURE.IMPORTANT] Sinun ei lisäys/vähennys luettelossa SKU tietojen levyn määrää. Sinun on luotava uusi tuote tällöin. Katso kohta [3. Voit lisätä uusi tuote, valitse luettelossa tarjouksen](#3-how-to-add-a-new-sku-under-a-live-offer) yksityiskohtaisia ohjeita.

7. Kun olet tehnyt haluamasi muutokset, siirry **JULKAISE** -välilehti ja valitse **PUSH väliaikaisen**-painiketta. Yksityiskohtaisia ohjeita siitä, että testaus tarjous väliaikaisen ympäristössä Lue tämä [linkki](marketplace-publishing-vm-image-test-in-staging.md)
8. Kun olet testannut tarjous väliaikaisen, siirry julkaisu **JULKAISE** -välilehti portaaliin ja valitse **PYYDÄ hyväksynnän, PUSH, tuotannon** uudelleenjulkaista Azure Marketplacesta tarjous-painikkeen.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku"></a>2. Voit päivittää tarjouksen tai SKU: ta tehtävistä selainpohjaisista tiedot

Voit päivittää tehtävistä selainpohjaisista (markkinointi, oikeudellisten, tuki, luokat) live tarjous tai SKU Azure Marketplace-tiedot.

### <a name="21-update-the-offer-description-and-logos"></a>2.1 tilauksen kuvaus tai logot päivittäminen

Voit päivittää tarjouksen tiedot ja julkaista uudelleen tarjous noudattamalla seuraavia ohjeita:

1. Kirjaudu [julkaiseminen portal](https://publish.windowsazure.com).
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla puolella-valikosta **markkinointi** -välilehti.
4. Valitse **ENGLANTI (Yhdysvallat)** -painiketta.
5. Valitse vasemmalla puolella-valikosta **tiedot** -välilehdessä. **Tiedot** -välilehden *kuvaus* -kohdassa voit päivittää tarjouksen otsikko, tarjota yhteenveto, tarjota pitkä yhteenveto ja Tallenna muutokset.

    >[AZURE.NOTE] Ota huolehtia seuraavat samalla, kun päivität tuote-tiedot.
    **Älä kirjoita tekstin kaksoiskappaleen tilauksen kuvaus ja tuote-kuvaus. Älä kirjoita tuote-otsikko ja pitkä yhteenveto tarjous tekstin kaksoiskappaleen. Älä kirjoita tuote-otsikko ja tarjouksen yhteenveto tekstin kaksoiskappaleen.**

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)

6. **Tiedot** -välilehden *logot* -osassa voit päivittää logoja. Mutta taata logoja noudattamalla [ohjeita Azure Marketplace](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (viittaa vaihe 1: Anna Marketplace markkinoinnin sisältö -> tiedot -> Azure Marketplace-Logo-ohjeista).

    >[AZURE.NOTE] Pääkuva-kuvake on valinnainen. Voit valita ei, jos haluat ladata Pääkuva-kuvaketta. Kuitenkin kun Pääkuva-kuvake on ladattu palvelimeen, valitse ei ole mahdollista poistaminen julkaisu portal. Siinä tapauksessa sinun on noudatettava [Pääkuva kuvake ohjeita](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (viittaa vaihe 1: Anna Marketplace markkinoinnin sisältö -> tiedot -> Pääkuva logo-nauha lisäohjeita).

7. Kun olet tehnyt haluamasi muutokset, siirry **JULKAISE** -välilehti ja valitse **PUSH väliaikaisen**-painiketta. Yksityiskohtaisia ohjeita testaaminen tarjous väliaikaisen ympäristössä tutustumaan tätä [linkkiä](marketplace-publishing-vm-image-test-in-staging.md).
8. Kun olet testannut tarjous väliaikaisen, siirry julkaisu **JULKAISE** -välilehti portaaliin ja valitse **PYYDÄ hyväksynnän, PUSH, tuotannon** uudelleenjulkaista Azure Marketplacesta tarjous-painikkeen.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="22-update-the-sku-description"></a>2.2. Päivitä tuote kuvaus

Voit päivittää tuote-tiedot ja julkaista uudelleen tarjous noudattamalla seuraavia ohjeita:

1. Kirjautuminen [julkaiseminen portal](https://publish.windowsazure.com)
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla puolella-valikosta **markkinointi** -välilehti.
4. Valitse **ENGLANTI (Yhdysvallat)** -painiketta.
5. Valitse vasemmalla puolella-valikosta **suunnitelmien** -välilehdessä. **Suunnitelmien** -välilehden *tuotteissa* -osassa voit päivittää SKU otsikko, yhteenveto tuote ja SKU kuvaus tiedot ja Tallenna muutokset.

    >[AZURE.NOTE] Ota huolehtia seuraavat samalla, kun päivität tuote-tiedot. **Älä kirjoita tekstin kaksoiskappaleen tilauksen kuvaus ja tuote-kuvaus. Älä kirjoita kaksoiskappaleiden teksti tuote otsikko ja pitkä yhteenveto tarjous. Älä kirjoita tuote-otsikko ja tarjouksen yhteenveto tekstin kaksoiskappaleen.**

6. Kun olet tehnyt haluamasi muutokset, siirry **JULKAISE** -välilehti ja valitse **PUSH väliaikaisen**-painiketta. Yksityiskohtaisia ohjeita siitä, että testaus tarjous väliaikaisen ympäristössä Lue tämä linkki
7. Kun olet testannut tarjous väliaikaisen, siirry julkaisu **JULKAISE** -välilehti portaaliin ja valitse **PYYDÄ hyväksynnän, PUSH, tuotannon** uudelleenjulkaista Azure Marketplacesta tarjous-painikkeen.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="23-change-the-existing-links-or-add-new-links"></a>2.3 muuttaa olemassa olevat linkit tai lisätä uusia linkkejä

Voit muuttaa olemassa olevat linkit tai lisätä uusia linkkejä ja sitten julkaista uudelleen tarjous noudattamalla seuraavia ohjeita:

1. Kirjautuminen [julkaiseminen portal](https://publish.windowsazure.com)
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla puolella-valikosta **markkinointi** -välilehti.
4. Valitse **ENGLANTI (Yhdysvallat)** -painiketta.
5. Valitse vasemmalla puolella-valikosta **linkit** -välilehdessä.
6. Jos haluat lisätä uuden linkin, valitse *linkkejä* osan kohdasta **Lisää LINKKI** -painiketta. *"Lisää linkki-* valintaikkuna avautuu. Tässä valintaikkunassa voit lisätä linkin otsikko ja URL-osoite-kentät ja Tallenna muutokset. Voit kirjoittaa linkki, joka sisältää tiedot, jotka voivat auttaa asiakkaita.
7. Jos haluat päivittää tai poistaa aiemmin luodun linkin, valitse haluamasi linkki ja valitse Muokkaa tai poista-painiketta vastaavasti.

    >[AZURE.NOTE] Varmista, että, linkkejä, jotka olet syöttänyt sisältö toimivat oikein, kun nämä linkit Hae vahvistettu pyyntö tuotantoprosessin aikana.

8. Kun olet tehnyt haluamasi muutokset, siirry **JULKAISE** -välilehti ja valitse **PUSH väliaikaisen**-painiketta. Yksityiskohtaisia ohjeita siitä, että testaus tarjous väliaikaisen ympäristössä tutustumaan tätä [linkkiä](marketplace-publishing-vm-image-test-in-staging.md).
9. Kun olet testannut tarjous väliaikaisen, siirry julkaisu **JULKAISE** -välilehti portaaliin ja valitse **PYYDÄ hyväksynnän, PUSH, tuotannon** uudelleenjulkaista Azure Marketplacesta tarjous-painikkeen.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="24-change-an-existing-sample-image-or-add-a-new-sample-image"></a>2.4 muuttaa olemassa olevan mallikuva tai lisätä uuden mallikuva

Voit muuttaa olemassa olevan otoksen-kuvia tai uusi malli-kuvien ja sitten julkaista uudelleen tarjous noudattamalla seuraavia ohjeita:

>[AZURE.NOTE] Vain yksi mallikuva näkyy [https://portal.azure.com](https://portal.azure.com).

1. Kirjautuminen [julkaiseminen portal](https://publish.windowsazure.com)
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla puolella-valikosta **markkinointi** -välilehti.
4. Valitse **ENGLANTI (Yhdysvallat)** -painiketta.
5. Valitse vasemmalla puolella-valikossa **Esimerkki kuvat** -välilehti.
6. Jos haluat lisätä uuden mallikuva, valitse *Otoksen kuvia* kohdasta **Lataa A uusi KUVA** -painiketta ja Tallenna muutokset.

    >[AZURE.NOTE] Esimerkkikuva mukaan lukien on valinnainen vaihe.

7. Jos haluat päivittää tai poistaa aiemmin mallikuva, Etsi haluamasi mallikuva ja valitse sitten **Vaihda KUVA** -painike tai poista-painiketta vastaavasti.

8. Kun olet tehnyt haluamasi muutokset, siirry **JULKAISE** -välilehti ja valitse **PUSH väliaikaisen**-painiketta. Yksityiskohtaisia ohjeita siitä, että testaus tarjous väliaikaisen ympäristössä tutustumaan tätä [linkkiä](marketplace-publishing-vm-image-test-in-staging.md).
9. Kun olet testannut tarjous väliaikaisen, siirry julkaisu **JULKAISE** -välilehti portaaliin ja valitse **PYYDÄ hyväksynnän, PUSH, tuotannon** uudelleenjulkaista Azure Marketplacesta tarjous-painikkeen.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="25-update-the-legal-content"></a>2.5 Päivitä lakisääteisen sisällön

Voit päivittää lakisääteisen sisällön ja julkaista uudelleen tarjous noudattamalla seuraavia ohjeita:

1. Kirjautuminen [julkaiseminen portal](https://publish.windowsazure.com)
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla puolella-valikosta **markkinointi** -välilehti.
4. Valitse **ENGLANTI (Yhdysvallat)** -painiketta.
5. Valitse vasemmalla puolella-valikosta **oikeudellinen** -välilehti. *Oikeudellinen* -osassa voit päivittää oman käytännöt/käyttöehdot. Kirjoita tai liitä käytännöt/ehdot *Käyttöehdot* -tekstiruutuun ja Tallenna muutokset.
6. Oikeudellinen käyttöehdot merkkien enimmäismäärä on 1,000,000 merkkiä.
7. Kun olet tehnyt haluamasi muutokset, siirry **JULKAISE** -välilehti ja valitse **PUSH väliaikaisen**-painiketta. Yksityiskohtaisia ohjeita siitä, että testaus tarjous väliaikaisen ympäristössä Lue tämä [linkki](marketplace-publishing-vm-image-test-in-staging.md)
8. Kun olet testannut tarjous väliaikaisen, siirry julkaisu **JULKAISE** -välilehti portaaliin ja valitse **PYYDÄ hyväksynnän, PUSH, tuotannon** uudelleenjulkaista Azure Marketplacesta tarjous-painikkeen.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="26-update-the-support-information"></a>2.6 tukitietojen päivittäminen

Voit päivittää tukitietoja ja julkaista uudelleen tarjous noudattamalla seuraavia ohjeita:

1. Kirjautuminen [julkaiseminen portal](https://publish.windowsazure.com)
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla puolella-valikossa **tuki** -välilehti.
4. **Tuki** -välilehdessä *Suunnitteluryhmät yhteyttä* -osassa voit päivittää yhteystiedot. Näitä tietoja käytetään: n sisäisen tietoliikenteen kumppanin ja Microsoftin välillä vain.
5. **Tuki** -välilehdessä *Asiakastukea* -kohdassa voit päivittää tuki yhteystietoja, kuten **nimi, sähköpostiosoite, puhelin** - ja **Tuen URL-osoite**. Näitä tietoja käytetään: n sisäisen tietoliikenteen kumppanin ja Microsoftin välillä vain.

    >[AZURE.NOTE] Jos haluat antaa vain sähköpostituki, antaa **Asiakastuki** -kohdassa tyhjä puhelinnumero. Tässä tapauksessa annettu sähköpostin käytetään sen sijaan.

6. Kun olet tehnyt haluamasi muutokset, siirry **JULKAISE** -välilehti ja valitse **PUSH väliaikaisen**-painiketta. Yksityiskohtaisia ohjeita siitä, että testaus tarjous väliaikaisen ympäristössä Lue tämä [linkki](marketplace-publishing-vm-image-test-in-staging.md)
7. Kun olet testannut tarjous väliaikaisen, siirry julkaisu **JULKAISE** -välilehti portaaliin ja valitse **PYYDÄ hyväksynnän, PUSH, tuotannon** uudelleenjulkaista Azure Marketplacesta tarjous-painikkeen.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="27-update-the-categories"></a>2.7 Päivitä luokat

Voit päivittää luokat-osassa tarjous ja julkaista uudelleen tarjous noudattamalla seuraavia ohjeita:

1. Kirjautuminen [julkaiseminen portal](https://publish.windowsazure.com)
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla puolella-valikossa **Luokat** -välilehti.
4. *Luokat* -kohdassa voit päivittää luokat tarjous ja Tallenna muutokset. Voit valita enintään viiteen Azure Marketplace-valikoiman luokista.
5. Kun olet tehnyt haluamasi muutokset, siirry **JULKAISE** -välilehti ja valitse **PUSH väliaikaisen**-painiketta. Yksityiskohtaisia ohjeita siitä, että testaus tarjous väliaikaisen ympäristössä Lue tämä [linkki](marketplace-publishing-vm-image-test-in-staging.md)
6. Kun olet testannut tarjous väliaikaisen, siirry julkaisu **JULKAISE** -välilehti portaaliin ja valitse **PYYDÄ hyväksynnän, PUSH, tuotannon** uudelleenjulkaista Azure Marketplacesta tarjous-painikkeen.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="3-how-to-add-a-new-sku-under-a-listed-offer"></a>3. Voit lisätä luettelossa tarjous-kohdassa uusi tuote

Voit lisätä uusi tuote kohdassa live tarjous jäljempänä annettujen ohjeiden mukaisesti:

1. Kirjaudu [julkaiseminen portal](https://publish.windowsazure.com).
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla puolella-valikosta **tuotteissa** -välilehdessä. Sen jälkeen, valitse **ADD A tuote**-painiketta.  Uusi valintaikkuna avautuu. Kirjoita tuote-tunniste pieniksi kirjaimiksi. Valitse Tuo-your-omista laskutuksen model(BYOL) valintaruudun valinta, jos haluat julkaista uusi tuote, joka BYOL laskutuksen mallin. BYOL varten, poista valintaruudun valinta. Sen jälkeen, napsauta Luo uusi tuote valintaikkunassa merkki. Jos olet ei BYOL laskutuksen mallin for uusi tuote, valitse laskutuksen mallin automaattisesti asetetaan Hourly for uusi tuote. Jos haluat ottaa käyttöön 30days maksuttoman kokeiluversion tunnin välein laskutuksen mallin, valitse "Kuukausi"-vaihtoehtoa, napsauta "on maksuttoman kokeiluversion käytettävissä?". Valitse muussa "Ei-KOKEILUVERSIO". [Huomautus: asetus "on käytettävissä maksuttoman kokeiluversion käyttäjäksi?" näkyy vain, jos et ole valinnut BYOL-valintaikkunassa uusi tuote luotaessa.]

    >[AZURE.IMPORTANT] Vaihtoehdon "Piilottaa Marketplacesta Tämä tuote, koska se on aina ostanut kautta ratkaisu mallin" pitäisi olla merkitty "Kyllä" vain, jos on hyväksytty julkaisemisen ratkaisu-mallin tarjouksen Azure Marketplacesta. Muussa tapauksessa asetus aina merkitään "Ei".

4. Nyt vasemmalla puolella-valikosta **AM kuvat** -välilehdessä ja selvitä, jotka olet luonut uuden tuote.
5. Jos haluat määrittää uusi tuote, viitata vaiheen 5 ohjeita tämän [linkin](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) .
6. Lisää uusi tuote markkinoinnin aineiston viittaa vaihe 1: Anna Marketplace markkinoinnin sisältö -> tiedot -> tämän [linkin](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content)lukujen 2 – 5.
7. Lisää uusi tuote hinnoittelutiedot viitata 2.1-kohdassa. Määritä AM-hinnat tämän [linkin](marketplace-publishing-push-to-staging.md#step-2-set-your-prices)
8. Kun olet tehnyt haluamasi muutokset, siirry **JULKAISE** -välilehti ja valitse **PUSH väliaikaisen**-painiketta. Yksityiskohtaisia ohjeita siitä, että testaus tarjous väliaikaisen ympäristössä Lue tämä [linkki](marketplace-publishing-vm-image-test-in-staging.md)
9. Kun olet testannut tarjous väliaikaisen, siirry julkaisu **JULKAISE** -välilehti portaaliin ja valitse **PYYDÄ hyväksynnän, PUSH, tuotannon** uudelleenjulkaista Azure Marketplacesta tarjous-painikkeen.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="4-how-to-change-the-data-disk-count-for-a-listed-sku"></a>4. Voit muuttaa tietojen levyjen määrä luettelossa SKU

Sinun ei lisäys/vähennys luettelossa SKU tietojen levyn määrää. Luo uusi tuote on tässä tapauksessa. Katso kohta [3. Voit lisätä live tarjous-kohdassa uusi tuote](#3-how-to-add-a-new-sku-under-a-live-offer) yksityiskohtaisia ohjeita.

## <a name="5---how-to-delete-a-listed-offer-from-the-azure-marketplace"></a>5. Voit poistaa luettelossa tarjouksen Azure Marketplacesta

Useilla eri ominaisuuksia, jotka voidaan ottaa huolellisesti Jos pyyntö poistaa live tarjous on. Saat ohjeet luettelossa tarjouksen poistaminen Azure Marketplace-tukitiimiltä ohjeita noudattamalla:

1.  Korottaa tuki lippu tämän [linkin](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) avulla
2.  Valitse ongelmatyyppi **"Hallinta tarjouksia"** ja valitse sitten luokka nimellä **"Muokkaaminen tarjous ja/tai jo tuotannon-tuote"**
3.  Lähetä pyyntö

Tukiryhmän opastaa sinua tarjouksen/SKU poistoprosessi.

>[AZURE.NOTE] Voit poistaa tarjous aina kun se on Luonnos-tilaan (eli ei ole väliaikaisen tai tuotannon) valitsemalla **HYLKÄÄ luonnos** -painike, valitse **HISTORIA** -välilehteen.

## <a name="6-how-to-delete-a-listed-sku-from-the-azure-marketplace"></a>6. Voit poistaa luettelossa SKU Azure Marketplacesta

Voit poistaa luettelossa SKU Azure Marketplacesta jäljempänä annettujen ohjeiden mukaisesti:

1. Kirjaudu [julkaiseminen portal](https://publish.windowsazure.com).
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla olevasta reunaruudun **tuotteissa** -välilehdessä.
4. Valitse tuote, jonka haluat poistaa ja valitse Poista-painikkeen vastaan, tuote.
5. Kun olet valmis, siirry julkaisu JULKAISE-välilehti portaaliin ja valitse **PYYDÄ hyväksynnän, PUSH, tuotannon** uudelleenjulkaista Azure Marketplacesta tarjous-painikkeen.
6. Kun tarjous saa uudelleen julkaistu Azure Marketplace-olevat versiot poistetaan Azure Marketplacesta ja Azure-portaalin.

## <a name="7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace"></a>7. Voit poistaa luettelossa SKU nykyisen version Azure Marketplacesta

Voit poistaa luettelossa SKU nykyinen versio Azure Marketplacesta jäljempänä annettujen ohjeiden mukaisesti. Kun prosessi on valmis, olevat versiot peruutetaan edellisen version.

1. Kirjaudu [julkaiseminen portal](https://publish.windowsazure.com).
2.  Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3.  Valitse vasemmalla olevasta reunaruudun **AM kuvat** -välilehti.
4.  Valitse tuote, jonka haluat poistaa ja valitse Poista-painikkeen vastaan versiossa nykyinen versio.
5.  Kun olet valmis, siirry julkaisu **JULKAISE** -välilehti portaaliin ja valitse **PYYDÄ hyväksynnän, PUSH, tuotannon** uudelleenjulkaista Azure Marketplacesta tarjous-painikkeen.
6.  Kun tarjous saa uudelleen julkaistu Azure Marketplace-luettelossa SKU nykyinen versio poistetaan Azure Marketplacesta ja Azure-portaalin. Olevat versiot peruutetaan edellisen version.

## <a name="8-how-to-revert-listing-price-to-production-values"></a>8. Voit palata tuotannon arvojen luettelon hinta
Olen vaihtanut luettelossa SKU hinnat (tai luettelossa SKU laskutuksen alueissa poistanut). Koska se ei tue Azure Marketplacesta, haluan palauttaa tuotannon arvot tehdyt muutokset eivät näy. Miten voin saavuttamiseksi?

Alla ohjeiden:

1. Kirjaudu [julkaiseminen portal](https://publish.windowsazure.com).
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla puolella-valikossa **HINNOITTELU** -välilehti.
4. Valitse alue, jonka hinnat, jonka haluat palauttaa hinnoittelu-välilehdessä.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)

5. Palauttaa tunnin välein laskutuksen malli tuotteissa, jos kaikki sydämiä hinnat kuin tuotannon valitun alueen. Varastointiyksikköjen BYOL laskutuksen mallin kanssa käytettäväksi olevat versiot alueen valitsemalla valintaruutu kohdassa EXTERNALLY-LICENSED (BYOL) SKU KÄYTETTÄVYYS SKU vastaan (Katso alla olevassa näyttökuvassa).

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)

6. Napsauta **AUTOPRICE muut markkinoilla perusteella edelleen hinnat IN UNITED tilat**-painiketta.

    >[AZURE.NOTE] Painikkeen selitettä saattavat olla erilaiset sen mukaan, alue, jossa on valittuna. Koska on valinnut tämän asiakirjan luotaessa Yhdysvalloissa, niin painikkeen on merkitty "Automaattinen hinnan muilla markkinoilla hintojen Yhdysvalloissa perusteella" alla olevassa näyttökuvassa.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)

7. Automaattinen hinta ohjattu toiminto avautuu. Ensimmäinen sivu näyttää perus market valintaa. Varmista, että osa ja siirtyy seuraavalle sivulle napsauttamalla **"->"** -painiketta.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)

8. Vaihtoehto Sydämiä ja suunnitelmat näkyvät sivulla 2. Valitse haluamasi palvelupaketeista ja Sydämiä ja valitse "->-painiketta.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)

9. Sivu 3 näyttää markkina-alueet/alueiden tietueet. Napsauta Valitse kaikki alueet, tai valitse manuaalisesti alueen Näytä tai Piilota kaikki-painiketta. Napsauta "->"-painiketta voit siirtyä seuraavalle sivulle.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)

10. Sivu 4 näkyy exchange-kurssit. Valitse seuraavien vaiheiden suorittamiseen Valmis-painiketta. Ohjatun toiminnon palauttaa hinnat valintasi mukaan.

11. Nyt siirtyä hinnoittelu-välilehteen ja napsauta "Näytä yhteenveto ja muutoksia"-painiketta.
Valitse "Tekstin" "Näytä versio" ja "Tuotannon" "Vertailu"-osassa (Katso alla olevassa näyttökuvassa). Jos hinnoittelu ero ei ole näkyvissä, se tarkoittaa sitä hinnat on ollut peruuttanut tuotannon arvoihin onnistuneesti.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)

12. Kun olet tehnyt haluamasi muutokset, siirry JULKAISE-välilehti ja valitse **PUSH väliaikaisen**-painiketta. Yksityiskohtaisia ohjeita siitä, että testaus tarjous väliaikaisen ympäristössä Lue tämä [linkki](marketplace-publishing-vm-image-test-in-staging.md)
13. Kun olet testannut tarjous väliaikaisen, siirry julkaisu JULKAISE-välilehti portaaliin ja valitse **PYYDÄ hyväksynnän, PUSH, tuotannon** uudelleenjulkaista Azure Marketplacesta tarjous-painikkeen.

## <a name="9-how-to-revert-billing-model-to-production-values"></a>9. Voit palata laskutuksen mallin tuotannon arvoihin
Voin muuttanut luettelossa SKU laskutuksen malli. Koska se ei tue Azure Marketplacesta, haluan palauttaa tuotannon arvot tehdyt muutokset eivät näy. Miten voin saavuttamiseksi?

Noudata seuraavia ohjeita:

1. Kirjaudu [julkaiseminen portal](https://publish.windowsazure.com).
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla puolella-valikossa **tuotteissa** -välilehti.
4. Napsauta Muokkaa-painiketta, palata laskutuksen malli. Ikkuna avautuu. Valintaruutu tai poista vastaavasti **'laskutus- ja käyttöoikeudet tehdään ulkoisesti Azure (eli tuoda Omat oman käyttöoikeus)-** valintaruutu.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)

5. Kerran Lue kysymyksen 8 tässä asiakirjassa, jos haluat palata takaisin hinnoittelua vastaus.
6. Kun julkaisu **JULKAISE** -välilehti, siirry portaalin ja push väliaikaisen tarjous testaa se. Kerran on tehty testaaminen tarjous ja valitse sitten painikkeen **PYYTÄÄ hyväksyntää, PUSH, tuotannon** uudelleenjulkaista tarjous Azure Marketplacesta.

## <a name="10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value"></a>10. miten palata näkyvyysasetus luettelossa SKU tuotannon-arvoa.

Noudata seuraavia ohjeita:

1. Kirjaudu [julkaiseminen portal](https://publish.windowsazure.com).
2. Siirry **NÄENNÄISKONEIDEN** -välilehti ja valitse tarjous.
3. Valitse vasemmalla puolella-valikossa **tuotteissa** -välilehti.
4. Valitse oman tuote ja palauttaa tuotannon arvoon SKU näkyvyysasetus.

    ![piirustuksen](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)

5. Kun olet tehnyt haluamasi muutokset ja valitse sitten painikkeen **PYYTÄÄ hyväksyntää, PUSH, tuotannon** uudelleenjulkaista tarjous Azure Marketplacesta.

## <a name="see-also"></a>Katso myös
- [Aloittaminen: Miten voit julkaista tarjouksen Azure Marketplacesta](marketplace-publishing-getting-started.md)
- [Tietoja myyjän tietoja raportointia varten](marketplace-publishing-report-seller-insights.md)
- [Tietoja maksu raportointi](marketplace-publishing-report-payout.md)
- [Voit muuttaa Cloud Solution Provider-jälleenmyyjältä kannustaa](marketplace-publishing-csp-incentive.md)
- [Yleiset Marketplacesta julkaisun ongelmien vianmääritys](marketplace-publishing-support-common-issues.md)
- [Käytä tukipalveluita julkaisijan nimellä](marketplace-publishing-get-publisher-support.md)
- [Paikallisen AM näköistiedoston luominen](marketplace-publishing-vm-image-creation-on-premise.md)
- [Windows Azure esikatselu-portaalissa on virtual koneen luominen](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
