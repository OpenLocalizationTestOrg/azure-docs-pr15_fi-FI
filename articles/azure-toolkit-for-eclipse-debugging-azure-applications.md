<properties
    pageTitle="Virheenkorjaus Pimennys Azure sovellukset"
    description="Lisätietoja virheenkorjaus-Azure sovellusten Pimennys Azure-työkalujen avulla."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690949.aspx -->

# <a name="debugging-azure-applications-in-eclipse"></a>Virheenkorjaus Pimennys Azure sovellukset #

Pimennys-Azure-työkalujen avulla voit korjata sovelluksia, ne ovat käytössä Azure tai Laske emulaattori Jos käytössäsi on Windows käyttöjärjestelmää. Seuraava kuva esittää valintaikkunan avulla käyttöön remote virheenkorjaus **Virheenkorjaus** ominaisuudet:

![][ic719504]

Tässä opetusohjelmassa oletetaan, että sinulla on jo sovellus, joka on luotu ja ovat tuttuja Laske emulaattori ja käyttöönottaminen Azure.

Käytetään [käyttämällä Azure palvelun suorituksenaikainen kirjastossa JSP][] opetusohjelman sovelluksen alkupisteeksi tämän ohjeaiheen. Ennen jatkamista, luo sovellus, jos et ole vielä tehnyt niin.

## <a name="to-debug-your-application-while-running-in-azure"></a>Jos haluat korjata sovelluksen ohjaamiseen Azure ##

>[AZURE.WARNING] Työkalujen nykyisen tuki remote Java virheenkorjaus on tarkoitettu lähinnä ominaisuuksissa Azure Laske emulaattori käynnissä. Muistin yhteyttä ei ole suojattu, ei ota remote virheenkorjaus tuotannon käyttöönotoissa. Jos haluat korjata sovelluksen Azure pilveen, väliaikaisen käyttöönoton, mutta huomaat, että luvattomasti virheenkorjaus-istunto on mahdollista, jos joku tietää cloud-käyttöönoton IP-osoite, vaikka on väliaikainen käyttöönotto.

1. Luominen testikäyttöön emulaattori projektin: Valitse Pimennys 's Project Explorer **MyAzureProject**hiiren kakkospainikkeella, valitse **Ominaisuudet**, valitse **Azure**ja asettaminen **käyttöönoton cloud** **luominen** .
1. Muodosta uudelleen projektin: Pimennys-valikosta **Project**ja valitse sitten **Luo kaikki**.
1. Ottaa käyttöön sovelluksen *väliaikaisen* Azure-tietokannassa.
    >[AZURE.IMPORTANT] Kuten edellä mainittiin, on erittäin suositeltavaa useimmissa tapauksissa Laske-emulaattorin virheenkorjaus sitten virheenkorjaus väliaikaisen ympäristössä vain, jos muut virheenkorjaus tarvita. On suositeltavaa tuotantoympäristössä ei korjata.
1. Kun käyttöönoton on valmis Azure-tietokannassa, hanki [Azure hallinta-portaalin][]käyttöönoton DNS-nimi. Väliaikaisen käyttöönoton on DNS-nimen muodossa http://*&lt;guid&gt;*. cloudapp.net, jossa * &lt;GUID-tunnus&gt; * on määrittänyt Azure GUID-arvon.
1. Pimennys 's Project Explorer- **WorkerRole1**hiiren kakkospainikkeella, valitse **Azure**ja valitse sitten **Virheenkorjaus**.
1. **WorkerRole1 virheenkorjaus ominaisuudet** -valintaikkunassa:
    1. Tarkista **käyttöön remote virheenkorjaus tämän roolin.**
    1. **Syötteen päätepisteen käyttämään**Käytä **Virheenkorjaus (julkinen: 8090, yksityinen: 8090)**.
    1. Varmista **Käynnistä JVM-keskeytystilaan, odotetaan virheenkorjaus-yhteyttä** ei ole valittuna.
        >[AZURE.IMPORTANT] Laajennettu virheenkorjaus Laske emulaattori vain skenaariot on tarkoitettu **JVM Käynnistä virheenkorjaus yhteyden odotetaan keskeytystilaan-** vaihtoehto (ei cloud käyttöönottoa varten). Jos **JVM Käynnistä virheenkorjaus yhteyden odotetaan keskeytystilaan-** vaihtoehto on käytössä, se keskeyttää palvelimen käynnistysprosessin kunnes Pimennys virheenkorjaus on yhdistetty sen JVM. Kun voi käyttää tätä asetusta käyttämällä Laske-emulaattorin muistin istuntoa, älä käytä sen muistin istunnon cloud-yhdistelmäympäristössä. Palvelimen alustus tapahtuu Azure käynnistys-tehtävässä ja Azure pilveen Soita julkisen päätepisteet käytettävissä ennen kuin käynnistys-tehtävä on valmis. Näin ollen käynnistysprosessin ei pääty onnistuneesti Jos tämä asetus on käytössä cloud-yhdistelmäympäristössä, koska se voi tulla yhteyden ulkoisen Pimennys-asiakasohjelmassa.
1. Valitse **Luo virheenkorjaus määrityksiä**.
1. **Azure virheenkorjaus määritys** -valintaikkunassa:
    1. **Java-projekti, jonka virheenkorjaus**Valitse **MyHelloWorld** projekti.
    1. **Määritä virheenkorjaus**Tarkista **Azure cloud (väliaikaisen)**.
    1. Varmista **Azure Laske emulaattorin** ei ole valittuna.
    1. **Host (isäntä)**Kirjoita DNS-nimi vaiheistettu käyttöönoton, mutta edellisen **http://**. Esimerkiksi (käytä omaa GUID sijaan kuvassa GUID-tunnus): **4e616d65-6f6e-6 d 65-6973-526f62657274.cloudapp.net**
1. Valitse Sulje **Azure virheenkorjaus Configuration** -valintaikkunassa **OK** .
1. Valitse **OK** ja sulje **WorkerRole1 virheenkorjaus ominaisuudet** -valintaikkunan.
1. Jos sinulla ei ole keskeytyskohdasta, joka on jo määritetty index.jsp, määrittää sen:
    1. Pimennys 's Project Explorer sisällä Laajenna **MyHelloWorld**, laajenna **WebContent**ja kaksoisnapsauta **index.jsp**.
    1. Sisällä index.jsp sininen palkki vasemmalla puolella Java-koodia hiiren kakkospainikkeella ja valitsemalla **Näytä tai piilota keskeytyskohdat**, seuraavassa esitetyllä tavalla: ![][ic551537]
1. Valitse **Suorita** ja napsauta **Virheenkorjaus käyttömahdollisuudet**Pimennys 's-valikossa.
1. **Virheenkorjaus määritykset** -valintaikkunassa Laajenna **Remote Java-sovelluksen** vasemmassa ruudussa, valitse **Azure Cloud (WorkerRole1)**ja valitse **Virheenkorjaus**.
1. Selaimestasi, suorita vaiheistettu sovelluksen **http://***&lt;guid&gt;***.cloudapp.net/MyHelloWorld**, korvaaminen GUID-tunnus-kohdassa DNS-nimi * &lt;guid&gt;*. Jos pyytää **Vahvista perspektiivi valitsin** -valintaikkuna, valitse **Kyllä**. Virheenkorjausistunnon suoritetaan nyt koodi, joissa keskeytyskohdasta on määritetty riville.

>[AZURE.NOTE] Jos yrität käynnistää etäkohteeseen virheenkorjaus käyttöönoton, jossa on useita roolin esiintymät, jossa on käytössä, ei voi tällä hetkellä hallita mikä yhteyden virheenkorjaus alun perin yhdistetään, kuten Azure kuormituksen Valitse erillisen satunnaisesti. Kun yhteys on muodostettu kyseisen esiintymän, vaikka, saat jatkossakin virheenkorjaus samassa esiintymässä. Huomaa myös, jos näkyvissä on oltava käyttämättömänä yli 4 minuuttia (esimerkiksi silloin, kun lopetit on liian kauan keskeytyskohdasta), Azure sulkea yhteys.

## <a name="debugging-a-specific-role-instance-in-a-multi-instance-deployment"></a>Virheenkorjaus rooli-esiintymän usean esiintymä-yhdistelmäympäristössä ##

Kun käyttöönoton toimii pilveen, todennäköisesti esität sen useita Laske- tai rooli, esiintymät. Näin voit hyödyntää Azure 99.95 % käytettävyys on voimassa ja skaalata sovelluksen ulos.

Näiden tilanteissa joudut ehkä virheenkorjaus etäyhteyden Java-sovelluksen rooli esiintymän. Kuitenkin vain tavallisen syötteen päätepiste virheenkorjaus käyttöön Azure kuormituksen mahdotonta paikasta, joihin voit muodostaa virheenkorjaus rooli-esiintymässä. Sen sijaan se muodostaa yhteyden, valitsee satunnaisesti rooli-esiintymässä.

Tämä on tilanne, jossa hyödyntämistä esiintymän syötteen päätepisteet helpottaa voit korjata rooli esiintymän tyyppi.

Oletetaan, että aiot käyttöönoton enintään 5 roolin esiintymät. Käytä **päätepisteet** ominaisuuden sivulla roolin ominaisuudet-valintaikkunassa, esiintymän syötteen päätepisteen luominen ja sen julkisen portit solualueen yhden porttinumero sijaan. Määritä esimerkiksi **Julkinen portti** tekstiruutu **81 85**.

Kun sovelluksen esiintymää tässä päätepisteissä otetaan käyttöön, Azure määrittää porttinumeroa tämän alueen kullekin roolin esiintymät. Valitse, jotta voit selvittää, mitkä esiintymän käytössä määritetty porttinumeroa, voit käyttää *InstanceEndpointName***_PUBLICPORT** ympäristömuuttuja (Jos *InstanceEndpointName* on sinulle määritetty luodessasi esiintymän päätepisteen nimi) automaattisesti määritetyn käyttöönoton (esimerkiksi palauttamalla arvonsa verkkosivulla alatunnisteeseen siten voi lukea sen, kun sen Siirry)-työkalujen avulla.

Kun tiedät, mitä julkisen porttinumero kyseisen esiintymän on määritetty, voidaan viitata se virheenkorjaus-kokoonpanoa Pimennys, kiinnittäminen palvelun isäntänimi. Tämä ottaa käyttöön Pimennys virheenkorjaus muodostaa yhteyden tietyn tekstikohdan ja jokin muut esiintymät.

## <a name="windows-only-to-debug-your-application-while-running-in-the-compute-emulator"></a>Vain Windows: korjaamisessa sovelluksesi ohjaamiseen Laske emulaattori ##

>[AZURE.NOTE] Azure emulaattorin on käytettävissä vain Windows. Ohittaa tämän osan, jos käytät kuin Windows käyttöjärjestelmä. 

1. Luominen testikäyttöön emulaattori projektin: Valitse Pimennys 's Project Explorer **MyAzureProject**hiiren kakkospainikkeella, valitse **Ominaisuudet**, valitse **Azure**ja asettaminen **emulaattorin Testing** **luominen** .
1. Muodosta uudelleen projektin: Pimennys-valikosta **Project**ja valitse sitten **Luo kaikki**.
1. Pimennys 's Project Explorer- **WorkerRole1**hiiren kakkospainikkeella, valitse **Azure**ja valitse sitten **Virheenkorjaus**.
1. **WorkerRole1 virheenkorjaus ominaisuudet** -valintaikkunassa:
    1. Tarkista **käyttöön remote virheenkorjaus tämän roolin.**
    1. **Syötteen päätepisteen käyttämään**Käytä oletusarvoinen päätepiste automaattisesti luomat työkalujen peruutuksesta **Virheenkorjaus (julkinen: 8090, yksityinen: 8090)**.
    1. Varmista **JVM Käynnistä virheenkorjaus yhteyden odotetaan keskeytystilaan-** vaihtoehto ei ole valittuna.
        >[AZURE.IMPORTANT] Laajennettu virheenkorjaus Laske emulaattori vain skenaariot on tarkoitettu **JVM Käynnistä virheenkorjaus yhteyden odotetaan keskeytystilaan-** vaihtoehto (ei cloud käyttöönottoa varten). Jos **JVM Käynnistä virheenkorjaus yhteyden odotetaan keskeytystilaan-** vaihtoehto on käytössä, se keskeyttää palvelimen käynnistysprosessin kunnes Pimennys virheenkorjaus on yhdistetty sen JVM. Kun voi käyttää tätä asetusta käyttämällä Laske-emulaattorin muistin istuntoa, älä käytä sen muistin istunnon cloud-yhdistelmäympäristössä. Palvelimen alustus tapahtuu Azure käynnistys-tehtävässä ja Azure pilveen Soita julkisen päätepisteet käytettävissä ennen kuin käynnistys-tehtävä on valmis. Näin ollen käynnistysprosessin ei pääty onnistuneesti Jos tämä asetus on käytössä cloud-yhdistelmäympäristössä, koska se voi tulla yhteyden ulkoisen Pimennys-asiakasohjelmassa.
1. Valitse **Luo virheenkorjaus määrityksiä**.
1. **Azure virheenkorjaus määritys** -valintaikkunassa:
    1. **Java-projekti, jonka virheenkorjaus**Valitse **MyHelloWorld** projekti.
    1. **Määritä virheenkorjaus**Tarkista **Azure Laske emulaattorin**.
1. Valitse Sulje **Azure virheenkorjaus Configuration** -valintaikkunassa **OK** .
1. Valitse **OK** ja sulje **WorkerRole1 virheenkorjaus ominaisuudet** -valintaikkunan.
1. Määrittää keskeytyskohdasta index.jsp:
    1. Pimennys 's Project Explorer sisällä Laajenna **MyHelloWorld**, laajenna **WebContent**ja kaksoisnapsauta **index.jsp**.
    1. Sisällä index.jsp sininen palkki vasemmalla puolella Java-koodia hiiren kakkospainikkeella ja valitsemalla **Näytä tai piilota keskeytyskohdat**, seuraavassa esitetyllä tavalla: ![][ic551537]

       Keskeytyskohdasta määritetään, jos keskeytyskohdasta kuvake, sininen palkissa vasemmalla puolella Java-koodia.
1. Käynnistä sovellus Laske emulaattori napsauttamalla Azure-työkalurivin **Azure emulaattorin Suorita** -painiketta.
1. Valitse **Suorita** ja napsauta **Virheenkorjaus käyttömahdollisuudet**Pimennys 's-valikossa.
1. **Virheenkorjaus määritykset** -valintaikkunassa Laajenna **Remote Java-sovelluksen** vasemmassa ruudussa, valitse **Azure emulaattorin (WorkerRole1)**ja valitse **Virheenkorjaus**.
1. Kun Laske emulaattori ilmaisee, että sovellus on käynnissä, selaimella, suorita **8080/MyHelloWorld**.
    Jos pyytää **Vahvista perspektiivi valitsin** -valintaikkuna, valitse **Kyllä**.
    Virheenkorjausistunnon suoritetaan nyt koodi, joissa keskeytyskohdasta on määritetty riville.

Tämän vuoksi voit miten Laske; emulaattorin korjaaminen seuraavan osan avulla voit korjata sovelluksen Azure käyttöön.

## <a name="debugging-notes"></a>Virheenkorjaus muistiinpanot ##

* Sen jälkeen virheenkorjaus, voit siirtyä perspektiivi- **Virheenkorjaus** **Javan** kautta valitsemalla Pimennys päivän napsauttamalla **ikkunan**, **Avaa perspektiivi**ja valitsemalla perspektiivin, jota haluat käyttää.
* Jotta remote virheenkorjaus GlassFish, älä käytä remote Azure-Työkalut, muistin kokoonpanoa Pimennys. Määrittää sen sijaan GlassFish manuaalisesti. GlassFish käsittelee Java asetukset ennalta ympäristömuuttujat tavasta työkalujen remote muistin määritys toiminto ei toimi oikein GlassFish kanssa. Jos työkalujen remote muistin määritys ominaisuus on käytössä, se estää GlassFish aloittamasta.

## <a name="see-also"></a>Katso myös ##

[Azure työkalujen Pimennys varten][]

[Pimennys Azure Hei maailma-sovelluksen luominen][]

[Azure-työkalut asennetaan Pimennys][] 

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure hallinta-portaalissa]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure työkalujen Pimennys varten]: http://go.microsoft.com/fwlink/?LinkID=699529
[Pimennys Azure Hei maailma-sovelluksen luominen]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure-työkalut asennetaan Pimennys]: http://go.microsoft.com/fwlink/?LinkId=699546
[Käyttämällä JSP Azure palvelun suorituksenaikainen-kirjasto]: http://go.microsoft.com/fwlink/?LinkID=699551

<!-- IMG List -->

[ic719504]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic719504.png
[ic551537]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic551537.png
