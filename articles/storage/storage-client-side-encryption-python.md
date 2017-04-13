<properties
    pageTitle="Asiakkaan salaus ja Python Microsoft Azuren tallennustilaan | Microsoft Azure"
    description="Azure tallennustilan asiakkaan kirjasto Python tukee parhaan mahdollisen suojauksen Azuren tallennustilaan sovellusten asiakaspuolen salausta."
    services="storage"
    documentationCenter="python"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Asiakkaan salaus ja Python Microsoft Azuren tallennustilaan

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Yleiskatsaus

[Azure tallennustilan asiakkaan kirjasto Python](https://pypi.python.org/pypi/azure-storage) tukee salaamaan tiedot ‑asiakassovelluksissa ennen Azuren tallennustilaan lataaminen ja salauksen tietojen ladattaessa asiakkaalle.

>[AZURE.NOTE] Azure-tallennustilan Python kirjasto on esikatselussa.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Salaaminen ja salauksen purkaminen kirjekuori-tekniikka kautta
Salaaminen ja salauksen purkaminen prosesseja noudattamalla kirjekuori-tekniikka.

### <a name="encryption-via-the-envelope-technique"></a>Kirjekuori-tekniikka kautta salaus
Salauksen kautta kirjekuori-tekniikka toimii seuraavasti:

1.  Azure-tallennustilan asiakkaan kirjaston Luo sisällön salauksen näppäintä (CEK), joka on yhteen-aikainen käyttöön symmetrisen.

2.  Käyttäjätiedot salataan käyttämällä tämän CEK.

3.  CEK sitten rivittyy (salattu) avaimen salausavaimen (KEK). KEK tunnistetaan avaimen tunnisteen, ja se voi olla julkiseen avaimen pari tai symmetrisen-näppäintä, joka on hallitaan paikallisesti.
Itse tallennustilan asiakkaan kirjasto on aina KEK käyttöoikeus. Kirjaston käynnistää avaimen rivitystä algoritmin, joka tarjoaa KEK. Käyttäjät voivat valita käytettävät mukautetut tarjoajat avaimen rivitys/purkaminen tarvittaessa.

4.  Salatut tiedot tuodaan sitten Azure tallennuspalvelu. Rivitetty näppäintä sekä salauksen joitakin metatietoja on tallennettu metatiedot (-Blob-objektien) tai interpoloitu arvo salattuja tiedoilla (sanomien ja taulukon yksiköt).

### <a name="decryption-via-the-envelope-technique"></a>Salauksen kirjekuori-tekniikka kautta
Salauksen kautta kirjekuori-tekniikka toimii seuraavasti:

1.  Asiakirjakirjaston oletetaan, että käyttäjällä on avaimen Salausavaimen hallinta (KEK) paikallisesti. Käyttäjän ei tarvitse tietää, jota käytettiin salauksen avain. Sen sijaan avaimen tulkintatoiminnon, joka korjaa avaimen tunnisteina näppäimet, voit määrittää ja käyttää.

2.  Asiakirjakirjaston lataaminen salattuja tiedot minkä tahansa salaus materiaali, joka on tallennettu palvelun kanssa.

3.  Sitoa (purettu) käyttämällä avaimen salausta key (KEK) on rivitetty sisällön salausavaimen (CEK). Tähän uudelleen asiakas-kirjasto ei ole KEK käyttöoikeus. Tuo näyttöön yksinkertaisesti mukautetun palvelun avaamista algoritmin.

4.  Sisällön salausavaimen (CEK) käytetään salausta salattuja käyttäjätiedot.

## <a name="encryption-mechanism"></a>Salauksen järjestelmä  
Tallennustilan asiakas-kirjasto käyttää [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) jotta salaa käyttäjätiedot. Tarkemmin sanottuna [Salaus estä ketjutus (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) tilaa AES. Jokaisen palvelun toimii hieman eri tavalla, jotta käsiteltävät aiheet ne tähän.

### <a name="blobs"></a>BLOB-objektit
Asiakirjakirjaston tukee tällä hetkellä vain koko BLOB salausta. Tarkemmin sanottuna salausta tuetaan, kun käyttäjät käyttävät **luominen*** tavoista. Lataamisen, molemmat valmiiksi ja alueen lataukset tuetaan ja lataa ja lataa parallelization on käytettävissä.

Salauksen, aikana asiakas-kirjasto satunnainen alustus Vector (IV) 16 tavua 32 tavua satunnaisia sisällön salauksen avaimen (CEK) ja luo ja suorita kirjekuori salauksen blob-tietojen avulla nämä tiedot. Rivitetty CEK ja salauksen joitakin metatietoja tallennetaan sen jälkeen, kun blob-metatiedot sekä salattu blob-palvelusta.

>[AZURE.WARNING] Jos muokkaat tai ladataan oman metatiedot blob-haluat varmistaa, että metatiedot säilytetään. Jos lataat uudet metatiedot ilman metatietoja, rivitetty CEK, IV ja muita metatietoja menetetään ja blob-sisältöä voi koskaan noudatettavan uudelleen.

Salattu blob lataaminen kuuluu koko Blob-objektien käyttäminen **Hae**sisällön noutaminen * helppokäyttöisyys tavoista. Rivitetty CEK kääreestä ja käyttää IV (tallennettu tällöin Blob-objektien metatietojen) ja palauttaa purettu tietoja käyttäjille.

Lataaminen haluamaansa alueen (**Hae*** menetelmiä alueen parametreilla välitetty) liittyy salattu blob säätäminen asteikkoon, käyttäjät, jotta saat pienen osan tiedot, joita voi käyttää purkaa onnistuneesti pyydetty alue.

Estä BLOB-objektit ja sivun BLOB vain voi olla salattu/salauksen tämän mallin avulla. Ei tällä hetkellä ei tue, salaaminen liittäminen BLOB-objektit.

### <a name="queues"></a>Olevien
Sanomien voi olla minkä tahansa muodon, koska asiakirjakirjaston määrittää mukautetun muodon, joka sisältää viestin tekstin alustus Vector (IV) ja salatun sisällön salausavaimen (CEK).

Salauksen, aikana asiakirjakirjaston Luo satunnaisen IV 16 tavua sekä 32 tavun satunnaisia CEK ja suorittaa kirjekuori salaus jonon viestin tekstin näiden tietojen avulla. Rivitetty CEK ja salauksen joitakin metatietoja lisätään sitten jonon salatun viestin. Muokattu viestiä (kuten alla) on tallennettu palveluun.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Salauksen, aikana rivitetty avainta poimittujen jonon viestin ja kääreestä. IV on myös jonon viestin poimittujen ja käyttää sitoa näppäintä sekä purkaa jonon viestitietoja. Huomaa, että salaus metatiedot on pieni (kohdassa 500 tavua), niin kun laskea kohti jonon viestin 64 Kilotavun rajoitus, vaikutus voi hallita.

### <a name="tables"></a>Taulukot
Asiakirjakirjaston tukee salausta kohteen ominaisuuksien lisäämistä ja korvaa-toimintoja.

>[AZURE.NOTE] Yhdistämistä ei tueta. Koska alijoukkoa ominaisuudet voivat on salattu aiemmin eri avaimen avulla, yksinkertaisesti yhdistäminen uudet ominaisuudet ja metatietojen päivitys aiheuttaa tietojen menettämisen. Yhdistäminen joko edellyttää ylimääräisiä palvelun soittaminen lukemaan olemassa kohde-palvelusta tai uusi näppäimellä ominaisuutta kohti, jotka eivät sovellu suorituskyvyn parantamiseksi.

Taulukon tietojen salauksen toimii seuraavasti:

1.  Käyttäjien määrittää salattuja ominaisuudet.

2.  Asiakirjakirjaston Luo satunnainen alustus Vector (IV) sekä satunnainen sisällön salausavaimen (CEK) 32 tavun jokaisen kohteen 16 tavua ja suorittaa kirjekuori salaus todistuksista uusi IV ominaisuutta kohti salataan yksittäisiä ominaisuudet. Salatun ominaisuus on tallennettu binaarimuodossa.

3.  Rivitetty CEK ja salauksen joitakin metatietoja tallennetaan sitten kaksi varattu lisäominaisuuksia. Ensimmäinen varattu ominaisuuden (\_ClientEncryptionMetadata1)-ominaisuus on merkkijono, joka sisältää tietoja IV, versio ja rivitetty avain. Toisen varatut ominaisuuden (\_ClientEncryptionMetadata2) on binaarinen ominaisuus, joka sisältää ominaisuuksia, jotka ovat salattuja tietoja. Toisen ominaisuuden tiedot (\_ClientEncryptionMetadata2) on itse salattu.

4.  Vuoksi nämä varattu lisäominaisuuksia pakollinen salauksen käyttäjät voivat nyt ehkä vain 250 mukautettujen ominaisuuksien 252 sijaan. Kohteen kokonaiskoko on oltava pienempi kuin 1 Megatavu.

    Huomaa, että vain merkkijonon ominaisuudet salattu. Jos ominaisuudet muuntyyppiset salata, ne on muunnettava merkkijonoja. Salatun merkkijonot tallennetaan palvelun binaarinen ominaisuuksina ja ne muunnetaan takaisin merkkijonot (raaka merkkijonot, ei EntityProperties sisältötyyppi EdmType.STRING) salauksen jälkeen.

    Taulukoiden lisäksi salaus-käytäntö käyttäjien on määritettävä salattuja ominaisuudet. Tämä on mahdollista tallentamalla näiden ominaisuuksien TableEntity objektien EdmType.STRING tyyppi, määritetty ja salata asetus on TOSI tai käyttöoikeuksien määrittäminen encryption_resolver_function tableservice objekti. Salaus-tulkintatoiminnon on toiminto, joka hakee osio-näppäintä, rivi-näppäintä ja ominaisuuden nimi ja palauttaa totuusarvon, joka ilmaisee, onko kyseisestä ominaisuudesta salattu. Salauksen, aikana asiakas-kirjastossa käyttämällä näitä tietoja päättää, onko ominaisuus salattu kirjoitettaessa koneisiin. Edustajan nimen on myös ympärille miten ominaisuudet ovat salattuja logiikka vahinkojen. (Esimerkiksi jos X-, sitten salaa ominaisuus A; salaa muussa ominaisuudet A ja b) Huomaa, että se ei tarvitse lisätä luettaessa tai kyselyt kohteiden tiedot.

### <a name="batch-operations"></a>Erän toiminnot
Yksi salaus käytäntö koskee kaikkien rivien osalta. Asiakirjakirjaston tuottaa sisäisesti uuden satunnaisia IV ja satunnaisia CEK riviä kohden osalta. Käyttäjät voivat valita myös salata eri ominaisuuksia jokaisen toiminnolle erän määrittämällä ongelman salaus tulkintatoiminnon.
Jos erän luodaan kontekstin valvoja tableservice batch() menetelmä kautta, tableservice salauksen käytännön automaattisesti käyttöön erään. Jos erän luodaan erikseen soittamalla konstruktoria, salaus-käytäntö on välitetty ja vasemmalle muuttamattomia erän elinkaaren varten.
Huomautus kohteiden salataan, kun ne on lisätty erä haluamasi erä salaus käytännön (kohteiden salataan ei ajankohtana tableservice salauksen käytännön avulla erä vahvistetaan) avulla.

### <a name="queries"></a>Kyselyt
Jos haluat suorittaa kyselyn, sinun on määritettävä avaimen tulkintatoiminnon, joka on yrittää ratkaista kaikkien tulosjoukkoa näppäimet. Jos kyselyn tulokseen sisältyvät kohde ei voi selvittää palveluntarjoaja, asiakirjakirjaston palauttaa virheen. Kyselyn, joka suorittaa palvelimen puoli ennusteiden, asiakirjakirjaston lisääminen salauksen erityisiä metatieto-ominaisuudet (\_ClientEncryptionMetadata1 ja \_ClientEncryptionMetadata2) oletusarvoisesti valitut sarakkeet.

>[AZURE.IMPORTANT] Ota huomioon seuraavat tärkeät seikat asiakaspuolen salausta käytettäessä:
>
>- Luettaessa tai kirjoitettaessa salattu blob, käytä koko blob Lataa komennot ja alue/kokonaisuuteen blob Lataa komennot. Vältä salattu blob käyttämällä protokolla toimintoja, kuten valitseminen lohko, valitseminen ruutuluettelo, kirjoittaa sivut tai poista sivut, kirjoittaminen saattaa muuten vioittunut salattu blob ja tekemällä siitä voi lukea.
>
>- Taulukoiden samalla rajoitus on olemassa. Älä päivitä salattuja ominaisuudet päivittämättä salaus metatiedot.
>
>- Jos olet määrittänyt metatietojen salattu blob, voi korvata vaaditaan salauksen, koska metatietojen määrittäminen ei ole toisiinsa salaus liittyviä metatietoja. Tämä koskee myös tilannevedoksia; Vältä määrittäminen metatietojen tilannevedoksen salattu blob luomisen aikana. Jos metatietojen on määritettävä ensin nykyinen salaus metatiedot pääset **get_blob_metadata** -menetelmää, että ja välttää samanaikainen kirjoituksia, kun metatiedot määritetään.
>
>- Ota käyttöön palvelun objektin käyttäjille, jotka kannattaa käsitellä vain salattujen tietojen **require_encryption** -merkinnän. Noudata seuraavia ohjeita, katso lisätietoja.

Tallennustilan asiakkaan kirjaston odottaa annettu KEK ja avaimen tulkintatoiminnon toteuttamisesta liittymästä. [Azure avaimen säilö](https://azure.microsoft.com/services/key-vault/) tuki Python KEK hallinta odottaa ja integroidaan tämän kirjaston, kun olet valmis.


## <a name="client-api--interface"></a>Asiakkaan API / liittymän
Kun palvelun tallennuspaikka (eli blockblobservice) on luotu, käyttäjä voi määrittää arvot kentät, jotka muodostavat salaus-käytäntö: key_encryption_key, key_resolver_function, ja require_encryption. Käyttäjät voivat antaa vain KEK vain tulkintatoiminnon tai molemmat. key_encryption_key on basic avaimen tyyppi, joka on määritetty käyttämällä avaimen tunnisteen sekä, joka annetaan logiikan rivitys/purkaminen. key_resolver_function käytetään ratkaista avaimen salauksen aikana. Se palauttaa kelvollinen KEK, joka on antanut avaimen tunnisteen. Tämä on käyttäjien mahdollisuus valita useita näppäimiä, joita hallitaan useisiin sijainteihin.

KEK on pantava täytäntöön salaamaan tiedot onnistuneesti seuraavista tavoista:
- wrap_key(cek): rivittyy algoritmin käyttäjän määrittämä avulla määritetyn CEK (tavua). Palauttaa rivitetty avain.
- get_key_wrap_algorithm(): palauttaa algoritmin rivitys avaimet.
- get_kid(): palauttaa merkkijonon avaimen tunnus tämän KEK.
KEK on pantava täytäntöön purkaa onnistuneesti tietoja seuraavista tavoista:
- unwrap_key (cek, algoritmin): palauttaa merkkijonon määrittämän-algoritmin avulla määritetyn CEK sitoa lomake.
- get_kid(): palauttaa merkkijonon avaimen tunnus tämän KEK.

Avaimen tulkintatoiminnon vähintään on pantava täytäntöön menetelmän, joka, key-tunnus annetaan palauttaa vastaavan KEK soveltamisesta yllä käyttöliittymän. Tämä menetelmä on key_resolver_function ominaisuutta service-objekti liitetään.

- Salauksen avainta käytetään aina ja näppäintä puuttuminen aiheuttaa virheen.
- Saat salauksen:
    - Avaimen tulkintatoiminnon käynnistyy, jos määritetty hankkia avain. Jos tulkintatoiminnon on määritetty, mutta se ei ole yhdistämismäärityksen avaimen tunnus, ilmenee virhe.
    - Jos Ratkaisin ei ole määritetty mutta näppäintä on määritetty, avainta käytetään, jos tunnisteen vastaa tarvittavat avaimen tunniste. Jos tunnus ei täsmää, ilmenee virhe.

      Salauksen mallit-azure.storage.samples <fix URL>kuvataan yksityiskohtaisempia lopusta loppuun-skenaario BLOB-objektit, olevien ja taulukot.
        Esimerkki käyttöotot KEK ja avaimen tulkintatoiminnon on esitetty esimerkki tiedostojen KeyWrapper ja KeyResolver tarpeen mukaan.

### <a name="requireencryption-mode"></a>RequireEncryption tila
Käyttäjät voivat ottaa käyttöön myös moodilla-toiminto, jossa kaikki keskeytetyt lataukset on salattu. Tässä tilassa asiakkaan epäonnistuu yrittää ladata tiedot ilman salaus-käytäntö tai ladata tietoja, joita ei ole salattu-palvelusta. Palvelun objektin **require_encryption** -merkinnän ohjaa tämän ongelman.

### <a name="blob-service-encryption"></a>BLOB-palvelun salaus
Määrittää salaus kentät blockblobservice objekti. Monipuolisista käsitellään mukaan asiakirjakirjaston sisäisesti.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_block_blob_service.key_encryption_key = kek
    my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

    # Upload the encrypted contents to the blob.
    my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

    # Download and decrypt the encrypted contents from the blob.
    blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)

### <a name="queue-service-encryption"></a>Jonon palvelun salaus
Määrittää salaus kentät queueservice objekti. Monipuolisista käsitellään mukaan asiakirjakirjaston sisäisesti.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_queue_service.key_encryption_key = kek
    my_queue_service.key_resolver_funcion = key_resolver.resolve_key

    # Add message
    my_queue_service.put_message(queue_name, content)

    # Retrieve message
    retrieved_message_list = my_queue_service.get_messages(queue_name)

### <a name="table-service-encryption"></a>Taulukon palvelun salaus
Salaus-käytännön luominen ja määrittäminen pyynnön asetukset-kohdan lisäksi voit täytyy määrittää **encryption_resolver_function** **tableservice**tai määrittää salaa-määrite EntityProperty.

### <a name="using-the-resolver"></a>Tulkintatoiminnon käyttäminen

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Define the encryption resolver_function.
    def my_encryption_resolver(pk, rk, property_name):
            if property_name == 'foo':
                    return True
            return False

    # Set the KEK and key resolver on the service object.
    my_table_service.key_encryption_key = kek
    my_table_service.key_resolver_funcion = key_resolver.resolve_key
    my_table_service.encryption_resolver_function = my_encryption_resolver

    # Insert Entity
    my_table_service.insert_entity(table_name, entity)

    # Retrieve Entity
    # Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
    my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])

### <a name="using-attributes"></a>Käyttämällä määritteet
Edellä mainittua ominaisuus voi merkitä salauksen tallennetaan EntityProperty objektin ja määrittämällä salaa-kentässä.

    encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)

## <a name="encryption-and-performance"></a>Salaus ja suorituskyky
Huomaa, että salaaminen suorituskykyä katseltavan tallennustilan tietojen tulokset. Sisällön avain ja IV on luotava, sisällön itse täytyy olla salattu ja muotoiltu metatietoja ja ladattu. Tämä katseltavan vaihtelevat sen mukaan, salauksen tietojen määrän mukaan. On suositeltavaa, että asiakkaiden testata verkkosovelluksistaan suorituskyvyn kehityksen aikana.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure tallennustilan asiakkaan kirjaston Java PyPi paketin](https://pypi.python.org/pypi/azure-storage) lataaminen
- Lataa [Python Azure tallennustilan asiakkaan kirjaston tietolähteen GitHub koodilla](https://github.com/Azure/azure-storage-python)
