<properties
    pageTitle="Palvelun hallinta API (Python) - toiminto oppaan käyttäminen"
    description="Opettele suorittamaan ohjelmallisesti Python service management perustoiminnot."
    services="cloud-services"
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="how-to-use-service-management-from-python"></a>Palvelun hallinta-Python käyttäminen

> [AZURE.NOTE] Service Management API korvataan uusi resurssi hallinta Ohjelmointirajapinnan kanssa, tällä hetkellä käytettävissä preview-versio.  Artikkelissa on [Azure Resurssienhallinta dokumentaatio](http://azure-sdk-for-python.readthedocs.org/) uusi Python-resurssien hallinnan Ohjelmointirajapinnan käyttäminen.

Tämän oppaan avulla voit suorittaa ohjelmallisesti Python service management perustoiminnot. [Azure SDK Python](https://github.com/Azure/azure-sdk-for-python) **ServiceManagementService** -luokan tukee ohjelmoitua pääsyä paljon palvelun hallintaan liittyviä toimintoja, jotka on käytettävissä [Azure perinteinen portal] [ management-portal] (kuten **luominen, päivittäminen, ja poistaminen pilvipalveluihin, käyttöönotoissa, data management services ja näennäiskoneiden**). Tätä toimintoa voi olla hyötyä sovellusten, jotka tarvitsevat ohjelmallisesti hallinta.

## <a name="WhatIs"> </a>Hallinnan ominaisuudet
Service Management Ohjelmointirajapinnan tarjoaa paljon käytettävissä palvelun [Azure perinteinen portal]-palvelun hallinta-toiminnon ohjelmallisesti pääsyn[management-portal]. Azure SDK Python avulla voit hallita pilvipalveluihin ja tallennustilaa tilit.

Käyttämään Service Management Ohjelmointirajapinnan haluat [luoda Azure-tili](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"> </a>Käsitteitä
Azure-SDK Python rivittyy [Azure Service Management API][svc-mgmt-rest-api], joka on REST-Ohjelmointirajapinnalla. Kaikki API toiminnot suoritetaan SSL: n ja todennettu toisensa X.509 v3 varmenteiden käyttäminen. Management-palvelu saattaa voidaan käyttää palvelua Azure-tietokannassa tai suoraan Internetissä mistä tahansa sovelluksesta, HTTPS-pyynnön lähettää ja vastaanottaa HTTPS-vastauksen.

## <a name="Installation"> </a>Asennus

Tässä artikkelissa kuvattuja ominaisuuksia on käytettävissä `azure-servicemanagement-legacy` paketti, josta voit asentaa pip. Katso lisätietoja asennuksen (esimerkiksi, jos ole aiemmin käyttänyt Python), katso tässä artikkelissa: [asentaminen Python ja Azure SDK-paketissa](../python-how-to-install.md)

## <a name="Connect"> </a>Toimintaohje: service management yhdistäminen
Muodosta yhteys hallinnan päätepisteen tarvitset Azure Tilaustunnus ja kelvollinen hallinta-varmenteen. Voit hankkia palvelun [Azure perinteinen portal]-Tunnuksesi Tilauksen[management-portal].

> [AZURE.NOTE] Python v0.8.0 Azure SDK-paketissa, koska on nyt mahdollista luoduissa OpenSSL, kun Windows-käyttöjärjestelmässä varmenteiden käytöstä.  Se vaatii Python 2.7.4 tai uudempi versio. Suosittelemme, että käyttäjät voivat käyttää sen sijaan, että .pfx-OpenSSL jälkeen tuki .pfx varmenteet todennäköisesti ole enää myöhemmin.

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Hallinnan varmenteet Windows/Mac tai Linux (OpenSSL)
Voit käyttää [OpenSSL](http://www.openssl.org/) hallinta-varmenteen.  On todella luotava todistuksista, yksi palvelimen ( `.cer` tiedosto) ja toinen asiakkaan ( `.pem` tiedosto). Voit luoda `.pem` tiedosto, suorittaa:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Voit luoda `.cer` varmenne, suorittaa:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Lisätietoja Azure varmenteet ohjeaiheessa [Azure pilvipalveluihin yleistä varmenteet](./cloud-services-certs-create.md). Kuvaus OpenSSL parametrit on osoitteessa [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html)ohjeissa.

Kun olet luonut nämä tiedostot, sinun on ladattava `.cer` tiedosto "Asetukset"-välilehden [Azure perinteinen portal]"Lataa"-toiminnon kautta Azure[management-portal], eikä sinun tarvitse tallennuspaikan muistiin `.pem` tiedosto.

Kun olet hankkinut tilauksen tunnus, luotu varmenne ja ladata `.cer` tiedoston Azure-, voit muodostaa yhteyden Azure hallinta päätepisteen välittämällä tilauksen tunnus ja polku `.pem` **ServiceManagementService**tiedoston:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

Edellisessä esimerkissä `sms` on **ServiceManagementService** objekti. **ServiceManagementService** luokka on ensisijainen luokka, jota käytetään Azure palveluiden hallinta.

### <a name="management-certificates-on-windows-makecert"></a>Hallinnan varmenteet Windows (MakeCert)

Voit luoda itse allekirjoitettua hallinta-varmenteen tietokoneen käyttämisestä `makecert.exe`.  Avaa **Visual Studio komentokehote** **järjestelmänvalvoja** ja käytä seuraavaa komentoa, *AzureCertificate* tilalle haluat käyttää varmenteen nimi.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Luo komento `.cer` tiedoston ja asentaa sen **Henkilökohtainen** varmenteen säilö. Lisätietoja on artikkelissa [Azure pilvipalveluihin yleistä varmenteet](./cloud-services-certs-create.md).

Kun olet luonut varmennetta, sinun on ladattava `.cer` tiedosto "Asetukset"-välilehden [Azure perinteinen portal]"Lataa"-toiminnon kautta Azure[management-portal].

Kun olet hankkinut tilauksen tunnus, luotu varmenne ja ladata `.cer` tiedoston Azure-avulla voit muodostaa yhteyden Azure hallinta päätepisteen välittämällä tilauksen tunnus ja varmennetta sijainnin **omasta** varmenteen säilöstä **ServiceManagementService** (uudelleen korvaava *AzureCertificate* sertifikaatin nimi):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

Edellisessä esimerkissä `sms` on **ServiceManagementService** objekti. **ServiceManagementService** luokka on ensisijainen luokka, jota käytetään Azure palveluiden hallinta.

## <a name="ListAvailableLocations"> </a>Toimintaohje: käytettävissä olevat sijainnit-luettelo

Luettelo sijainneista, jotka ovat käytettävissä isännöintipalveluina, käytä **luettelon\_sijainnit** menetelmää:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Kun luot pilvipalveluun tai tallennuspalvelu, sinun on annettava kelvollinen sijainti. **Luettelon\_sijainnit** menetelmä palauttaa aina ajan tasalla tällä hetkellä käytettävissä olevat sijainnit-luettelo. Tämä kirjoittaminen saavuttaman käytettävissä olevat sijainnit ovat seuraavat:

- Länsi Europe
- Pohjois-Eurooppa
- Kaakkoisaasialaiset Aasian
- Itä-Aasian
- Keskitetyn USA
- Pohjois-keskitetyn USA
- Etelä keskitetyn USA
- Länsi USA
- Yhdysvaltojen Itä
- Japani Itä
- Japani Länsi
- Brasilia Etelä
- Australia Itä
- Australia varaaja

## <a name="CreateCloudService"> </a>Toimintaohje: luominen pilvipalveluun

Kun sovelluksen luominen ja suorittaminen Azure-tietokannassa, koodi- ja määritystietoja yhdessä kutsutaan Azure [pilvipalvelussa][] (nimeltään *nykyisessä palvelun* Azure aiempien). **Luominen\_isännöidään\_palvelun** menetelmän avulla voit luoda uuden isännöityä palvelun antamalla isännöityä palvelunimi (joka on oltava yksilöllinen Azure-tietokannassa), (automaattisesti koodattu base64) otsikko, kuvaus ja sijainti.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Voit lisätä isännöityjen palveluiden tilauksen kanssa **luettelon\_isännöidään\_services** menetelmää:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Jos haluat tietyn isännöityä palvelun tietoja, voit tehdä välittämällä isännöityä palvelunimi **Hae\_isännöidään\_palvelun\_ominaisuudet** menetelmää:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Kun olet luonut pilvipalveluun, voit ottaa käyttöön koodin palvelua **luominen\_käyttöönoton** menetelmää.

## <a name="DeleteCloudService"> </a>Toimintaohje: Poista pilvipalveluun

Voit poistaa pilvipalveluun välittämällä palvelunimi **poistaminen\_isännöidään\_palvelun** menetelmää:

    sms.delete_hosted_service('myhostedservice')

Ennen kuin voit poistaa palvelu, kaikki ominaisuuksissa palvelun on ensin poistettava. (Katso [Toimintaohje: Poista käyttöönoton](#DeleteDeployment) lisätietoja.)

## <a name="DeleteDeployment"> </a>Toimintaohje: käyttöönoton poistaminen

Voit poistaa käyttöönoton **poistaminen\_käyttöönoton** menetelmää. Seuraavassa esimerkissä poistamisesta nimeltä käyttöönoton `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Toimintaohje: Luo tallennustilan palvelu

[Tallennuspalvelu](../storage/storage-create-storage-account.md) kautta pääset käsiksi Azure- [BLOB-objektit](../storage/storage-python-how-to-use-blob-storage.md), [taulukoita](../storage/storage-python-how-to-use-table-storage.md)ja [olevien](../storage/storage-python-how-to-use-queue-storage.md). Luo tallennustilan palvelu edellyttää nimi (välillä 3 – 24 pieniä kirjaimia ja Azure yksilöivä) palvelun, kuvaus, otsikko (enintään 100 merkkiä, koodattu automaattisesti Base64-muotoon) ja sijainti. Seuraavassa esimerkissä esitetään luomisesta tallennuspalvelu määrittämällä sijainnin.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Huomautus edellisessä esimerkissä, tila **luominen\_tallennustilan\_tilin** toiminto voi hakea välittämällä palauttama tulos **luominen\_tallennustilan\_tilin** , **Hae\_toiminnon\_tila** menetelmää.  

Voit lisätä tallennustilaa asiakkaiden ja niiden ominaisuudet ja **luettelon\_tallennustilan\_tilit** menetelmää:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Toimintaohje: tallennustilan-palvelun poistaminen

Voit poistaa tallennustilaa palvelu välittämällä tallennustilan palvelunimi **poistaminen\_tallennustilan\_tilin** menetelmää. Tallennustilan palvelu poistetaan rajoitukseen (BLOB-objektit, taulukoita ja olevien)-palvelussa.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Toimintaohje: käyttöjärjestelmien luettelo

Luettelo käyttöjärjestelmistä, jotka ovat käytettävissä isännöintipalveluina, käytä **luettelon\_toimivien\_järjestelmien** menetelmää:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Vaihtoehtoisesti voit käyttää **luettelon\_toimivien\_järjestelmän\_perheille** menetelmä, joka ryhmittelee perheen käyttöjärjestelmät:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Toimintaohje: Luo käyttöjärjestelmä-kuva

Käyttöjärjestelmä kuva Lisää kuva-säilöön **Lisää\_os\_kuva** menetelmää:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Luettelon käyttöjärjestelmän kuvat, jotka ovat käytettävissä, käytä **luettelon\_os\_kuvia** menetelmää. Se sisältää kaikki ympäristö kuvia ja käyttäjän kuvat:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Toimintaohje: Poista käyttöjärjestelmä-kuva

Voit poistaa käyttäjän kuva **poistaminen\_os\_kuva** menetelmää:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Toimintaohje: Luo virtual machine

Luo virtual machine, sinun on luominen [pilvipalvelussa](#CreateCloudService).  Luo virtuaalikoneen käyttöönoton avulla **luominen\_virtual\_tietokoneen\_käyttöönoton** menetelmää:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Toimintaohje: Poista virtual machine

Virtual machine poistamalla ensin poistat käyttöönoton avulla **poistaminen\_käyttöönoton** menetelmää:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Pilvipalvelussa sijaitsevaan sitten voi poistaa **poistaminen\_isännöidään\_palvelun** menetelmää:

    sms.delete_hosted_service(service_name='myvm')

##<a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Toimintaohje: Luo Virtual Machine siepatun virtuaalikoneen kuvasta

Ottaa AM kuva kutsu ensin **siepata\_AM\_kuva** menetelmää:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Seuraavaksi voit varmistaa, että kuva on tallennetaan onnistuneesti, käytä **luettelon\_AM\_kuvia** api, ja varmista, että kuva näkyy tulokset:

    images = sms.list_vm_images()

Voit luoda käyttämällä siepatun kuvan virtuaalikoneen lopuksi **luominen\_virtual\_tietokoneen\_käyttöönoton** ennen, mutta tällä hetkellä välittää, vm_image_name-menetelmällä

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Lisätietoja siitä, miten voit siepata Linux-Virtual Machine-artikkelissa [sieppaaminen Linux-Virtual Machine.](../virtual-machines/virtual-machines-linux-classic-capture-image.md)

Lisätietoja siitä, miten voit siepata Windows-Virtual Machine-artikkelissa [sieppaaminen Windows-Virtual Machine.](../virtual-machines/virtual-machines-windows-classic-capture-image.md)

## <a name="What's Next"> </a>Seuraavat vaiheet

Nyt kun olet tutustunut hallinnan perusteet, voit käyttää [Azure Python SDK täydellisen API ohjeet](http://azure-sdk-for-python.readthedocs.org/) ja monimutkaisia tehtäviä helposti, jos haluat hallita python sovelluksen.

Lisätietoja on artikkelissa [Python Developer Center](/develop/python/).

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[pilvipalvelussa]:/services/cloud-services/

