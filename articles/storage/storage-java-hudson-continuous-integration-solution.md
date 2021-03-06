<properties
    pageTitle="Voit käyttää Hudson Blob-objektien tallennustilaan | Microsoft Azure"
    description="Tässä artikkelissa käsitellään käyttäminen Hudson kanssa Azure-Blob-säiliö säilön muodosta palvelutiedot."
    services="storage"
    documentationCenter="java"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016" 
    ms.author="dinesh"/>

# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Azuren tallennustilaan käyttäminen Hudson jatkuva integrointi ratkaisu

## <a name="overview"></a>Yleiskatsaus

Seuraavassa on käyttämisestä Blob-objektien tallennustilaan muodosta on luotu Hudson jatkuva integrointi (CI)-ratkaisun säilön tai ladattavia tiedostoja lähteen, jota käytetään prosessin muodosta. Jokin skenaariot, jolta hakea tämä hyötyä on, kun olet koodauksen (joko Java tai muiden kielten) joustava kehitysympäristö versiot jatkuva integrointi on käytössä perusteella ja tarvitset säilön muodosta-palvelutiedot, niin, että voit voitu, esimerkiksi jakaa niitä organisaation muiden jäsenten asiakkaille, tai ylläpitää arkistoon.  Toinen tilanne on, kun muodosta työtäsi itse edellyttää muita tiedostoja, esimerkiksi riippuvuudet, lataa Lisää Muodosta osana.

Tässä opetusohjelmassa käytät Azuren tallennustilaan laajennus Hudson CI saataville Microsoft varten.

## <a name="introduction-to-hudson"></a>Johdanto Hudson ##

Hudson mahdollistaa jatkuva integrointi ohjelmiston projektin sallimalla kehittäjät voivat helposti integroida muutoksensa koodi ja on versiot valmistettu, automaattisesti ja usein, jolloin lisääntyvien kehittäjät tuottavuutta. Versiot ovat versiotietoja ja muodosta palvelutiedot voi ladata eri säilöjen tietoihin. Tässä artikkelissa kerrotaan käyttämisestä Azure-Blob-säiliö kuin muodosta palvelutiedot säilöön. Se näkyy myös riippuvuudet lataamisesta Azure-Blob-säiliö.

Lisätietoja Hudson on [Täytettävä Hudson](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson)osoitteessa.

## <a name="benefits-of-using-the-blob-service"></a>Blob-palvelun eduista ##

Joustava kehittäminen muodosta-palvelutiedot isännöimiseen Blob-palvelun käytön etuja ovat:

- Suuren käytettävyyden muodosta palvelutiedot ja/tai ladattavan riippuvuudet.
- Suorituskyvyn, kun Hudson CI-ratkaisun latauksia muodosta palvelutiedot.
- Suorituskyvyn, kun asiakkaat ja kumppanit Lataa muodosta palvelutiedot.
- Hallita käyttäjän käyttöoikeuskäytäntöjä-vaihtoehtoa anonyymin käytön, vanheneminen perustuvan jaettujen käyttöoikeuksien allekirjoituksen access, yksityinen access jne.

## <a name="prerequisites"></a>Edellytykset ##

Tarvitset Blob-palvelun käyttäminen Hudson CI-ratkaisun seuraavasti:

- Hudson jatkuva integrointi ratkaisu.

    Jos ei ole tällä hetkellä ratkaisua Hudson CI, voit suorittaa Hudson CI ratkaista seuraavassa kuvatulla tavalla:

    1. Java on otettu käyttöön tietokoneessa Lataa Hudson SODAN <http://hudson-ci.org/>.
    2. Komentokehotteeseen, jotka on avattu Hudson SOTA, suorita Hudson SODAN sisältävä kansio. Jos olet ladannut version 3.1.2 esimerkiksi:

        `java -jar hudson-3.1.2.war`

    3. Avaa selaimessa, `http://localhost:8080/`. Tämä avaa Hudson Raporttinäkymät-ikkunan.

    4. Hudson ensimmäisen käyttökerran yhteydessä Täytä ensimmäisen määrityskerran osoitteessa `http://localhost:8080/`.

    5. Kun ensimmäisen asennuksen, Peruuta Hudson SODAN suoritettavan, Aloita Hudson SODAN uudelleen ja avaa uudelleen Hudson Raporttinäkymät-ikkunan `http://localhost:8080/`, jota käytät asennetaan ja määritetään Azuren tallennustilaan laajennus.

        Kun tyypillinen Hudson CI ratkaisu voidaan määrittää toimimaan palveluna käynnissä Hudson sodan komentorivillä on riittävä Tässä opetusohjelmassa.

- Azure tili. Voit rekisteröidä Azure-tilille osoitteessa <http://www.azure.com>.

- Azure-tallennustilan tilin. Jos sinulla ei vielä ole tallennustilan tilin, voit luoda sellaisen noudattamalla ohjeita, [Luo tallennustilan tili](storage-create-storage-account.md#create-a-storage-account).

- Hudson CI-ratkaisun tuntemusta suositellaan, mutta sitä ei tarvita, kuten seuraavan sisällön käytetään basic Esimerkki Näytä vaiheet, kun säilö Blob-palvelun käyttäminen Hudson CI luominen palvelutiedot.

## <a name="how-to-use-the-blob-service-with-hudson-ci"></a>Voit käyttää Hudson CI Blob-palvelu ##

Jos haluat käyttää Blob-palvelun Hudson, sinun on, asenna laajennus Azuren tallennustilaan laajennus tallennustilan tilin käyttöä varten ja luo sitten jälkeinen muodosta-toiminto, joka lataa muodosta palvelutiedot tallennustilan-tiliisi. Nämä vaiheet on kuvattu seuraavissa osissa.

## <a name="how-to-install-the-azure-storage-plugin"></a>Azuren tallennustilaan-laajennuksen asentaminen ##

1. Olet Hudson Raporttinäkymät-ikkunan Napsauta **Hallinta Hudson**.
2. Valitse **Hallitse laajennukset** **Hudson hallinta** -sivulla.
3. Valitse **käytettävissä olevat** -välilehti.
4. Valitse **muita**.
5. Valitse **Microsoft Azuren tallennustilaan laajennuksen** **Palvelutietojen Uploaders** -osiossa.
6. Valitse **Asenna**.
7. Kun asennus on valmis, Käynnistä uudelleen Hudson.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Azuren tallennustilaan laajennus käyttämään tallennustilan tilin määrittäminen ##

1. Olet Hudson Raporttinäkymät-ikkunan Napsauta **Hallinta Hudson**.
2. Valitse **Määrittää järjestelmän** **Hudson hallinta** -sivulla.
3. **Microsoft Azure tallennustilan tilin määrittäminen** -osassa:

    a. Kirjoita tallennustilan tilin nimen, jonka voit hankkia [Azure-portaalissa](https://portal.azure.com).

    b. Tiliavain tallennustilaa, myös vähennettävä [Azure-portaalissa](https://portal.azure.com).

    c-näppäinyhdistelmää. Käytä oletusarvoa **Blob palvelun päätepisteen** URL-osoitteen, jos käytössäsi on julkinen Azure pilveen. Jos käytössäsi on eri Azure pilvestä, käytä päätepisteen määritettyinä [Azure Portal](https://portal.azure.com) -tallennustilan tilin.

    d. Valitse **Vahvista tallennustilan tunnistetietojen** avulla voit vahvistaa tallennustilan tilisi.

    e. [Valinnainen] Jos sinulla on lisätallennustilaa tilit, jotka haluat saataville Hudson CI, valitse **Lisää tallennustilan tilejä**.

    f. Valitse Tallenna asetukset **Tallenna** .

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Jälkeinen muodosta-toiminto, joka lataa muodosta palvelutiedot tallennustilan tilin luominen ##

Ohjeen tarkoituksiin ensin on on luoda työn, joka luo useita tiedostoja, ja sitten Lisää tiedostoja ladataan tallennustilan tilin jälkeinen muodosta-toiminto.

1. Olet Hudson Raporttinäkymät-ikkunan Napsauta **Uusi projekti**.
2. Työn **MyJob**nimi, valitse **Muodosta - tyyppiseksi ohjelmiston työn**ja valitse sitten **OK**.
3. Töiden määritysten **luominen** -osassa **Lisää Muodosta vaiheessa** ja valitse **Suorita Windows erän komento**.
4. **Komento**Käytä seuraavia komentoja:

        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt

5. Valitse **Lataa palvelutiedot Microsoft Azure-Blob-säiliö**töiden määritysten **Jälkeen muodosta toiminnot** -osassa.
6. **Tallennustilan tilin nimi**Valitse tallennustilan-tili, jota käytetään.
7. **Säilön nimi**Määritä säilö nimi. (Säilö luodaan, jos se ei jo ole kun muodosta palvelutiedot ladataan.) Voit käyttää ympäristömuuttujia, kirjoita niin **${JOB_NAME}** tässä esimerkissä säilön nimeksi.

    **Vihje**

    **Komennon** alapuolella osa, johon lisäsit komentosarjan, **Suorita Windows erän komento** on Hudson tunnista ympäristömuuttujat linkki. Lisätietoja ympäristön muuttujan nimistä ja kuvauksista linkkiä napsauttamalla. Huomaa, että ympäristömuuttujat erikoismerkkejä, kuten **BUILD_URL** -ympäristömuuttuja sisältävät eivät ole sallittuja säilön nimi tai yleisen virtual polun.

8. Valitse tässä esimerkissä **uuteen säilöön julkistaminen oletusarvoisesti** . (Jos haluat käyttää yksityinen säilön, tarvitset käyttää jaettua käyttöä allekirjoituksen luomiseen. Tämä on tämän artikkelin piiriin. Voit lukea lisää jaettu käyttö allekirjoitukset etsiminen [Käyttämällä jaetun Access allekirjoitukset (SAS)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Valinnainen] Valitse **Puhdista säilö ennen lataamista** , jos haluat tyhjentää sisällön ennen muodosta palvelutiedot ladataan säilö (jätä se ole valittuna, jos et halua Puhdista säilön sisältöä).
10. Anna **Luettelon ja palvelutiedot lataaminen** * *tekstin /*.txt**.
11. **Yleisiä ladatut palvelutiedot virtual polun**, kirjoita **${luominen\_tunnus} / ${luominen\_numero}**.
12. Valitse Tallenna asetukset **Tallenna** .
13. Valitse **Muodosta nyt** suorittaa **MyJob**Hudson Raporttinäkymät-ikkunan. Tarkastele konsolin tulosteen tila. Azuren tallennustilaan tilasanomat sisällytetään konsolin tuloste jälkeinen muodosta toiminnon käynnistyessä lataaminen muodosta palvelutiedot.
14. Yhteydessä, projekti suoritetaan valmiiksi voit tutkia muodosta palvelutiedot avaamalla julkisen Blob-objektien.

    a. Kirjautuminen [Azure Portal](https://portal.azure.com).

    b. Valitse **tallennustilan**.

    c-näppäinyhdistelmää. Valitse tallennustilan tilin nimi, jota käytit Hudson.

    d. Valitse **säilöjen**.

    e. Valitse säilö **myjob**, joka on projektin nimi, jonka määritit, kun olet luonut Hudson työn pienikirjaiminen versio. Säilön ja Blob-objektien nimet ovat pieniä (ja kirjainkoko on merkitsevä) Azuren tallennustilaan. **Myjob** säilöä BLOB luettelossa pitäisi näkyä **hello.txt** ja **date.txt**. Kopioi URL-osoite jommankumman kohteita ja avaa se selaimessa. Näet tekstitiedosto, joka on ladattu muodosta Palvelutietojen.

Vain yksi jälkeinen muodosta-toiminto, joka lataa palvelutiedot Azure-Blob-säiliö voi luoda projektia kohti. Huomaa, että yksi jälkeinen muodosta toiminto, voit ladata palvelutiedot Azure-Blob-säiliö voit määrittää eri tiedostoja (mukaan lukien yleismerkkejä) ja polkujen **Luettelosta, palvelutiedot lataaminen** käyttäen erottimen puolipiste tiedostoihin. Esimerkiksi jos Hudson muodosta tuottaa PURKKI ja parantaa työtilan **luominen** kansiossa TXT-tiedostot ja haluat ladata sekä Azure-Blob-säiliö, käytä **Luettelon ja palvelutiedot lataaminen** -arvon seuraavasti: **luominen /\*.jar; muodosta /\*.txt**. Voit käyttää myös double kaksoispiste syntaksi polun käytetään blob-nimi. Esimerkiksi tölkki Hae ladattu Hae ladattu **ilmoitukset** käyttäminen blob-polku **binaaritiedostot** TXT-tiedostot ja blob-polku avulla voit halutessasi käyttää **Luettelon ja palvelutiedot lataaminen** -arvon seuraavasti: **luominen /\*. jar::binaries, muodosta tai\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Miten voit luoda muodosta vaihetta, jonka lataamisen Azure-Blob-säiliö ##

Seuraavissa vaiheissa kuvataan muodosta vaiheen kohteet ladataan Azure-Blob-säiliö määrittämisestä. Tämä on kätevä, jos haluat sisällyttää kohteita oman muodosta, kuten tölkki, voit pitää Azure-Blob-objektien tallennustilaan.

1. Töiden määritysten **luominen** -osassa **Lisää Muodosta vaiheessa** ja valitse **Lataa Azure-Blob-säiliö**.
2. **Tallennustilan tilin nimi**Valitse tallennustilan-tili, jota käytetään.
3. **Säilön nimi**Määritä säilö, joka on BLOB-objektit, jotka haluat ladata nimi. Voit käyttää ympäristömuuttujat.
4. Määritä blob **Blob nimi**. Voit käyttää ympäristömuuttujat. Lisäksi voit yleismerkkinä blob-nimen ensimmäinen antavan määrittämisen jälkeen on tähti. Esimerkiksi **projektin\* ** määrittää kaikki BLOB-objektit, joiden nimet alkavat **projektin**.
5. [Valinnainen] **Lataa polku**määrittää polku kohtaa, johon haluat ladata tiedostoja Azure-Blob-säiliö Hudson tietokoneeseen. Ympäristön muuttujaa voidaan myös. (Jos et anna arvo, **Lataa polku**, Azure-Blob-säiliö tiedostot ladataan projektin työtila.)

Jos haluat ladata Azure-Blob-säiliö muut kohteet, voit luoda uusia muodosta vaiheet.

Kun olet suorittanut muodosta, tarkista muodosta historia konsolin tulosteen tai tarkastella Lataa sijaintisi, jos haluat nähdä, onko odottamaasi BLOB-objektit on onnistuneesti ladattu.

## <a name="components-used-by-the-blob-service"></a>Osien Blob-palvelu ##

Seuraavassa on esitetty yleiskatsaus Blob-palvelun osat.

- **Tallennustilan tilin**: kaikki pääsy Azuren tallennustilaan tehdään tallennustilan tilin kautta. Tämä on ylimmän tason nimitilaa käyttäminen BLOB-objektit. Tilin voi olla säilöjen, rajoittamaton määrä, kunhan niiden yhteenlaskettu koko on alle 100 Teratavua.
- **Säilön**: säilön on joukko BLOB ryhmittely. Kaikki Blob-objektien on oltava säilö. Tilin voi olla säilöjen rajoittamaton määrä. Säilön voi tallentaa BLOB rajoittamaton määrä.
- **BLOB**: tiedosto, jonka kaikki tyyppi ja koko. On kahdentyyppisiä BLOB-objektit, jotka voidaan tallentaa Azuren tallennustilaan: lohko ja sivun BLOB-objektit. Useimmat tiedostot ovat estä BLOB-objektit. Yksittäisen estä Blob-objektien voi olla enintään 200 gt kokoisia. Tässä opetusohjelmassa käyttää estä BLOB-objektit. Sivun BLOB-Blob-objektien muu voi olla enintään 1 TT koko ja ovat tehokkaampaa, kun alueiden tavua tiedostossa on muokattu usein. Saat lisätietoja BLOB [tietoja estä BLOB-objektit, Liitä BLOB ja sivun BLOB](http://msdn.microsoft.com/library/azure/ee691964.aspx).
- **URL-muoto**: BLOB ovat käytettävissä seuraavassa URL-osoite muodossa:

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`

    (Edellä Muotoile koskee julkisia Azure pilveen. Jos käytössäsi on eri Azure pilvestä, päätepisteen [Azure-portaalin](https://portal.azure.com) määrittämiseen käyttää URL-Osoitteen päätepiste.)

    Yllä olevassa muodossa `storageaccount` tallennustilan tilin nimi `container_name` lisääminen säilöön, nimi ja `blob_name` edustaa nimeä, että Blob-objektien vastaavasti. Sisällä säilössä, voi olla useita polut-, vinoviivalla erotettua **/**. Tässä opetusohjelmassa säilö esimerkkinimi on **MyJob**ja **${luominen\_tunnus} / ${luominen\_numero}** käytettiin yleisen virtual polun, tuloksena on seuraavassa lomakkeen URL-Osoitteen blob:

    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Seuraavat vaiheet

- [Täytä Hudson](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
- [Azure-tallennustilan SDK Java](https://github.com/azure/azure-storage-java)
- [Azure-tallennustilan asiakkaan SDK-ohje](http://dl.windowsazure.com/storage/javadoc/)
- [Azure liittyviä palveluja REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dd179355.aspx )
- [Azure-tallennustilan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/)

Lisätietoja on Katso myös [Java Developer Center](https://azure.microsoft.com/develop/java/).