<properties 
    pageTitle="Azure Government kehittäjät opas" 
    description="Tämä sisältää ominaisuuksia ja ohjeita vertailua Azure Government-sovellusten kehittäminen" 
    services="" 
    cloud="gov"
    documentationCenter="" 
    authors="Joharve2" 
    manager="Chrisnie" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="azure-government" 
    ms.date="10/29/2015" 
    ms.author="jharve"/>


#  <a name="microsoft-azure-government-developer-guide"></a>Microsoft Azure Government Sovelluskehittäjän opas 

<p> Microsoft Azure julkinen fyysisesti verkon Microsoft Azure erillään esiintymä.  Kehittäjät oppaan päivitystavasta tiedot erot, sovelluskehittäjät ja järjestelmänvalvojien on vuorovaikutuksessa ja käyttää näitä Azure eri alueissa.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## <a name="in-this-topic"></a>Tässä ohjeaiheessa


+ [Yleiskatsaus](#Overview)
+ [Ohjeet kehittäjille](#Guidance)
+ [Tällä hetkellä käytettävissä Microsoft Azure Government ominaisuudet](#Features)
+ [Päätepisteen yhdistäminen](#Endpoint)
+ [Seuraavat vaiheet](#next)


## <a name="Overview"></a>Yleiskatsaus

Microsoft Azure Government on Microsoft Azure-palvelu osoitteiden US federal virastoille, tila ja paikallisen viranomaisten ja ratkaisujen toimittajat tietosuojaan ja vaatimustenmukaisuuteen tarpeisiin erillisen esiintymän. Azure Government tarjoaa fyysinen ja verkko-US government ominaisuuksissa erillään ja on tarkistettu varmenne US henkilökunnan. 

Microsoft antaa käyttöösi useita työkaluja, joilla voit luoda ja ottaa käyttöön cloud sovellusten Microsoftin yleisen Microsoft Azure-palvelu ("Yleinen Service") ja Microsoft Azure Government-palveluihin.

Kun luominen ja käyttöönotto sovellusten Azure Government-palveluihin, eikä yleinen palvelun kehittäjät tarvitsee tietää kahden palvelun keskeisiä eroja.  Erityisesti ympärille ja määrittämisestä niiden monipuolisemman ympäristön, ja määritä päätepisteet-sovellusten kirjoittaminen ja käyttöönotto ne kuin Azure Government-palvelut.

Tässä asiakirjassa nämä erot on koottu ja täydentää [Azure Government](http://www.azure.com/gov "Azure julkinen") sivusto- ja [Microsoft Azure Technical Library](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") MSDN-sivuston käytettävissä olevat tiedot. Virallinen tietoja voi olla käytettävissä myös muihin sijainteihin kuten [Microsoft Azure Trust Centeriin] (https://azure.microsoft.com/support/trust-center/ "Microsoft Azure Trust Centeriin" /), [Azure dokumentaatio Center](https://azure.microsoft.com/documentation/) ja [Azure blogit] (https://azure.microsoft.com/blog/ "Azure blogit" /). 

Tämä sisältö on tarkoitettu kumppanit ja sovelluskehittäjät, jotka ovat käyttöönotossa Microsoft Azure Government.



## <a name="Guidance"></a>Ohjeita ohjelmistokehittäjille
Koska useimmat teknistä sisältöä, joka on tällä hetkellä käytettävissä oletetaan, että sovellukset ovat on kehitetty, yleinen-palvelun sijaan Microsoft Azure Government varten, on tärkeää voit varmistaa, että kehittäjät ovat tietoisia sovellusten kehittää isännöidään Azure Government keskeisiä eroja.

- Ensin on Servicesissä, ominaisuuksien erojen, tämä tarkoittaa, että tiettyjä ominaisuuksia, jotka ovat tiettyjä alueita yleinen palvelun eivät ole käytettävissä Azure Government.

- Seuraavaksi ominaisuudet, jotka tarjotaan Azure Government, eroja määritysten yleinen-palvelusta.  Tämän vuoksi Tarkista otoksen koodi, määritykset ja haluat varmistaa, että luominen ja suorittaminen Azure Government Cloud Services-ympäristössä.


## <a name="Features"></a>Tällä hetkellä käytettävissä Microsoft Azure Government ominaisuudet
Azure Government on tällä hetkellä käytettävissä seuraavat palvelut US gov – IOWA ja US gov – VIRGINIA alueilla:

- Näennäiskoneiden
- Pilvipalveluihin
- Tallennustilan
- Active Directory
- Ajoitus
- Virtuaalinen verkko
- SQL-tietokantaan
- Azure-tiedostot
- Media-palveluita
- Liikenteen hallinta
- Palvelun Bus
- StorSimple
- Redis välimuisti
- Azure varmuuskopiointi
- Automaatio
- ExpressRoute
- jne.

Muut palvelut ovat käytettävissä ja lisää palveluja lisätään jatkuvasti.  Katso uusimmat palveluluettelosta [alueiden sivun](https://azure.microsoft.com/regions/#services) , joka korostaa käytettävissä kunkin alueen ja niiden palveluiden.  

Tällä hetkellä US gov – Iowa ja US gov – Virginia ovat tukemaan Azure Government tietojen keskukset.  Tutustu alueiden sivun nykyinen tietojen keskikohdan mukaan-ja käytössä.

## <a name="Endpoint"></a>Päätepisteen yhdistäminen

Seuraavan taulukon avulla voit auttaa sinua yhdistämismäärityksiä julkinen ja Microsoft Azure SQL-tietokantaan päätepisteet Azure Government tietyn päätepisteet.


Palvelutyyppi|Azure julkisen|Azure Government
---|---|---
Hallinta-portaalissa|Manage.windowsazure.com|Manage.windowsazure.us
Yleiset|*. windows.net|*. usgovcloudapi.net
Core|*. core.windows.net|*. core.usgovcloudapi.net
Laske|*. cloudapp.net|*. usgovcloudapp.net
Blob-objektien tallennustilaan|*. blob.core.windows.net|   *. blob.core.usgovcloudapi.net
Jonon tallennustila|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Taulukkotallennus|*. table.core.windows.net|*. table.core.usgovcloudapi.net
Hallinta|Management.Core.Windows.NET|Management.Core.usgovcloudapi.NET
SQL-tietokantaan|*. database.windows.net|*. database.usgovcloudapi.net
ARM kuormituksen saapuva päätepiste|https://Management.Windows.NET|https://Management.usgovcloudapi.NET  

* ARM-todennuksen kautta Azure AD-viittaus [Todennustapa Azure Resurssienhallinta pyynnöt](https://msdn.microsoft.com/library/azure/dn790557.aspx)

## <a name="next"></a>Seuraavat vaiheet

Jos olet kiinnostunut oppiminen Lisää ja Azure Government hyödyntää Ota joitakin alla olevia linkkejä.

- **[Kokeiluversion rekisteröityminen](https://azuregov.microsoft.com/trial/azuregovtrial)**

- **[Hankkiminen ja Azure Government käyttäminen](http://azure.com/gov)**

- **[Azure Government yleiskatsaus](/azure-government-overview)**

- **[Azure Government-blogi](http://blogs.msdn.com/b/azuregov/)**

- **[Azure yhteensopivuus](https://azure.microsoft.com/support/trust-center/compliance/)**

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: storage-whatis-account.md
