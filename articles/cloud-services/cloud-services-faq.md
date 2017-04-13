<properties
    pageTitle="Cloud Services-usein kysytyt kysymykset | Microsoft Azure"
    description="Usein kysyttyjä kysymyksiä pilvipalveluihin."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="adegeo"/>

# <a name="cloud-services-faq"></a>Cloud Services usein kysytyt kysymykset
Tässä artikkelissa vastataan Microsoft Azure pilvipalveluihin usein kysyttyihin kysymyksiin. Voit myös käydä Azure hinnoittelua ja tukea yleistiedot [Azure tukevat usein kysytyt kysymykset](http://go.microsoft.com/fwlink/?LinkID=185083) . Voit myös kuulla [Cloud Services AM koko sivun](cloud-services-sizes-specs.md) koon tietoja.

## <a name="certificates"></a>Varmenteet

### <a name="where-should-i-install-my-certificate"></a>Mistä varmennetta tulee asentaa?

- **Oma**  
Yksityinen avain sovelluksen varmenne (\*.pfx- \*.p12).

- **VARMENTEIDEN MYÖNTÄJÄ**  
Kaikki keskitason varmenteet Siirry myymälän (käytännön ja Sub CAs).

- **PÄÄKANSIO**  
Päämyöntäjä tallentaa, jotta tärkeimmät pääkansion CA-varmenne on täällä.

### <a name="i-cant-remove-expired-certificate"></a>En voi poistaa vanhentunut varmenne

Azure estää poistaminen varmennetta, kun se on käytössä. Sinun täytyy Poista käyttöönoton, joka käyttää varmenteen tai päivitä käyttöönoton eri tai uudistetun sertifikaatilla.

### <a name="delete-an-expired-certificate"></a>Poista varmenne on vanhentunut

Kunhan varmennetta ei ole käytössä, voit poistaa varmenteen [Poista AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell-cmdlet-komennon.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>Voin ovat vanhentuneet nimetyt Windows Azure hallinnan tunnisteesta varmenteet

Nämä varmenteet luodaan aina, kun tunniste on lisätty pilvipalveluun, kuten Etätyöpöytä-tunniste. Nämä varmenteet käytetään vain ja salauksen tunnisteen yksityinen määritys. Se ei ole merkitystä, jos nämä varmenteet päättyy. Vanhentumispäivä ei ole valittu.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Varmenteet on poistettu Poistamani

Nämä Poistamani todennäköisesti käytät, kuten Visual Studio työkalun vuoksi. Aina, kun olet taas työkalua, joka on sertifikaatilla, sitä uudelleen ladata Azure avulla.

### <a name="my-certificates-keep-disappearing"></a>Varmenteiden säilyttää katoaa

Kun virtuaalikoneen esiintymän kierrättää, kaikki paikalliset muutokset menetetään. [Käynnistyksen tehtävien](cloud-services-startup-tasks.md) avulla voit asentaa varmenteet virtuaalikoneen aina, kun tehtävä alkaa.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>En löydä varmenteiden hallinta-portaalissa

[Hallinnan varmenteet](..\azure-api-management-certs.md) ovat käytettävissä vain perinteinen Azure-portaalissa. Nykyinen Azure portaalin ei käytä hallinta varmenteet. 

### <a name="how-can-i-disable-a-management-certificate"></a>Miten voin vaimentaa hallinta-varmenteen?

[Varmenteiden hallinta](..\azure-api-management-certs.md) ei voi poistaa käytöstä. Voit poistaa ne palvelun perinteinen Azure-portaalissa, kun et halua niitä voidaan käyttää enää.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Miten voin luoda SSL-varmenteen tietyn IP-osoite?

[Luo sertifikaatti opetusohjelma](cloud-services-certs-create.md)ohjeiden mukaisesti. Määritä DNS-nimeksi IP-osoite.

## <a name="security"></a>Tietoturva

### <a name="disable-ssl-30"></a>SSL 3.0 poistaminen käytöstä

SSL 3.0 käytöstä ja TLS-suojausta, voit luoda käynnistys tehtävä, jota on kuvattu on blogikirjoituksessa: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

## <a name="scale-a-cloud-service"></a>Skaalaa pilvipalveluun

### <a name="i-cannot-scale-beyond-x-instances"></a>Minulla ei skaalaudu lisäksi X esiintymiä

Azure-tilauksesi on rajoitus sydämiä, voit käyttää määrän. Skaalaus eivät toimi, jos olet käyttänyt kaikki käytettävissä olevat sydämiä. Esimerkiksi jos sinulla on enintään 100 sydämiä, siis voi olla 100 A1 esitystapa virtuaalikoneen esiintymät cloud-palvelun tai 50 A2 esitystapa virtuaalikoneen esiintymät.

## <a name="troubleshooting"></a>Vianmääritys

### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Minulla ei voi varata IP usean VIP pilvipalvelussa

Varmista ensin, että virtuaalikoneen esiintymään, jota yrität varata IP varten on otettu käyttöön. Toiseksi Varmista, että käytät varattu IP-osoitteet, tekee testaus- ja -käyttöönotoissa. **Älä** Muuta asetuksia käyttöönoton päivittämisen aikana.

