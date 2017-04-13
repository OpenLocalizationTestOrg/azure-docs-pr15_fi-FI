<properties 
   pageTitle="PowerShellin StorSimple hallinnan | Microsoft Azure"
   description="Opi käyttämään Windows PowerShell StorSimple voi hallita laitetta StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli@microsoft.com" />

# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>Windows PowerShell StorSimple avulla voit hallita laitteesi

## <a name="overview"></a>Yleiskatsaus

Windows PowerShell StorSimple on käyttöliittymä, joiden avulla voit hallita Microsoft Azure StorSimple laitetta. Koska nimi ehdottaa on Windows PowerShell-pohjainen-käyttöliittymä, joka on luotu rajoitettu runspace. Käyttäjän komentorivillä näkökulmasta rajoitettu runspace näkyy rajoitettu Windows PowerShell-versiota. Säilyttäen joitakin Windows PowerShellin perusominaisuudet liittymän on muita erillinen cmdlet-komennot, jotka ovat suunnattu hallinta Microsoft Azure StorSimple laitteen. 

Tässä artikkelissa kuvataan Windows PowerShellin StorSimple ominaisuudet, mukaan lukien, miten voit muodostaa yhteyden tähän liittymään ja sisältää linkkejä vaiheittaiset ohjeet tai työnkulut, joita voit suorittaa tämän käyttöliittymän avulla. Työnkulut sisältävät voit rekisteröidä laitteen, määrittää verkkoliittymän laitteen, asenna päivitykset, jotka edellyttävät laite-ylläpito-tilassa, Muuta laitteen tila ja vianmääritys, joita voi ilmetä ongelmia.

Luettuasi tämän artikkelin saa oikeuden:

- Yhdistä Windows PowerShellin avulla StorSimple StorSimple laitteen.

- Hallinta Windows PowerShellin avulla StorSimple StorSimple laitteen.

- Pyydä apua Windows PowerShellin StorSimple varten.

>[AZURE.NOTE]   

>- Windows PowerShell cmdlet-komentojen StorSimple avulla voit hallita StorSimple laitteen serial konsolissa tai etäyhteyden Windows PowerShellin Etäpalvelujen. Lisätietoja eri yksittäisiä cmdlet-komennot, joita voi käyttää tätä käyttöliittymän Siirry [Windows PowerShell StorSimple cmdlet-viittaus](https://technet.microsoft.com/library/dn688168.aspx).

>- Azure PowerShell StorSimple cmdlet-komennot ovat eri kokoelma cmdlet-komennot, joiden avulla voit automatisoida StorSimple palvelun tason ja siirtotehtävät komentoriviltä. Lisätietoja StorSimple Azure PowerShellin cmdlet-komennot Siirry [Azure StorSimple cmdlet-viittaus](https://msdn.microsoft.com/library/azure/dn920427.aspx).

Voit käyttää Windows PowerShellin StorSimple jollakin seuraavista tavoista:

- [Yhteyden muodostaminen StorSimple laitteen serial konsoli](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
- [Etäkäyttö StorSimple Windows PowerShellin avulla](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)
    

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Yhteyden Windows PowerShellin kautta laitteen serial konsolin StorSimple varten

Voit [ladata painovärit, muste](http://www.putty.org/) tai samanlaisia terminaalissa palvelinemulointiohjelmiston Windows PowerShellin muodostaa StorSimple. Sinun täytyy määrittää painovärit, muste erityisesti muodostamaan yhteys Microsoft Azure StorSimple laite. Seuraavissa ohjeaiheissa on lisätietoja kohdasta painovärit, muste määrittäminen ja yhteyden muodostaminen laitteen. Eri valikkovaihtoehdot serial konsolissa myös kuvaus.

### <a name="putty-settings"></a>Painovärit, muste asetukset

Varmista, että Käytä seuraavia painovärit, muste-asetuksia voit muodostaa yhteyden Windows PowerShell-liittymän serial konsolin.

#### <a name="to-configure-putty"></a>Voit määrittää painovärit, muste

1. Valitse painovärit, muste **kokoonpanon uudelleenmääritys** -valintaikkunassa **luokka** -ruudussa **Näppäimistö**.

2. Varmista, että seuraavat asetukset ovat valittuina (nämä ovat oletusasetukset käynnistyessä uuden istunnon). 

  	|Näppäimistön kohde|Valitse|
  	|---|---|
  	|ASKELPALAUTIN|Ohjausobjektin-? (127)|
  	|Aloitus-ja End|Vakio|
  	|Toimintonäppäimet ja näppäimistö|ESC [n ~|
  	|Nuolinäppäimet alkuvaihe|Normaali|
  	|Numeronäppäimistön alkuvaihe|Normaali|
  	|Näppäimistön ominaisuuksien käyttöönotto|Ohjausobjektin Alt eroaa AltGr|

    ![Tuetut painovärit, muste asetukset](./media/storsimple-windows-powershell-administration/IC740877.png)

3. Valitse **Käytä**.

4. Valitse **luokka** -ruudussa **käännös**.

5. Valitse **Remote merkistö** -luetteloruudussa **UTF-8**.

6. Valitse **käsittely viivapiirros merkkiä**, **Käytä Unicode viivapiirros koodin pistettä**. Seuraavassa kuvassa näkyy oikea painovärit, muste valinnat.

    ![UTF painovärit, muste asetukset](./media/storsimple-windows-powershell-administration/IC740878.png)

7. Valitse **Käytä**.


Voit nyt painovärit, muste muodostaa laitteen serial konsolin toimimalla seuraavien ohjeiden mukaisesti.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]


### <a name="about-the-serial-console"></a>Lisätietoja sarja konsoli

Kun käytät käyttöliittymän StorSimple serial nauha viestin-konsolin kautta laitteen esitetään Windows PowerShell-perään valikkovaihtoehdot. 

Ilmoituspalkin viesti sisältää basic StorSimple laitteen tietoja, kuten mallin nimi, asennettujen ohjelmistoversio ja ohjauskoneen, kun käytät tila. Seuraavassa kuvassa on esimerkki nauha viestin.

![Sarja nauha-viesti](./media/storsimple-windows-powershell-administration/IC741098.png)

>[AZURE.IMPORTANT] Voit tunnistaa, onko yhteys on muodostettu ohjauskoneen aktiivisen tai passiivisen nauha-viesti.

Seuraava kuva esittää runspace eri vaihtoehdoista, jotka ovat käytettävissä serial konsoli-valikosta.

![Rekisteröi laite 2](./media/storsimple-windows-powershell-administration/IC740906.png)

Voit valita seuraavat asetukset:

1. **Kirjaudu sisään täydet käyttöoikeudet** Tämän asetuksen avulla voit muodostaa yhteyden (myönnät käyttöoikeudet) **SSAdminConsole** runspace paikallisen ohjaimen. (Paikallisen ohjauskoneen on ohjauskoneen, kun käytät tällä hetkellä StorSimple laitteen sarja-konsolin kautta). Tämä vaihtoehto voidaan myös, jotta Microsoft Support käyttämään rajoittamaton runspace (tuki-istunnon) kaikki mahdolliset laite-ongelmien vianmääritystä. Kun käytät vaihtoehto 1 kirjautumaan sisään, voit sallia Microsoft Support lähdekoodiksi käyttämään rajoittamaton runspace suorittamalla tietyt cmdlet-komento. Lisätietoja [tuki-istunnon](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)aloittaminen.

2. **Kirjaudu sisään peer valvonta ja täydet käyttöoikeudet** Tämä asetus on sama kuin vaihtoehto 1, sillä erotuksella, että voit muodostaa (ja myönnät käyttöoikeudet) **SSAdminConsole** runspace peer ohjaimen. Koska StorSimple laite on suuri käytettävyys laitetta, jonka kaksi ohjaimet Aktiivinen-passiivinen-kokoonpanossa, peer viittaa muiden ohjaimen laitteen, kun käytät sarja-konsolin kautta).
Vaihtoehto 1 samalla, vaihtoehto voidaan myös sallia Rajoittamattomien runspace peer ohjaimen käyttämään Microsoft Support.

3. **Yhdistä rajoitettu käyttö** Tämä vaihtoehto on käyttää Windows PowerShell-liittymän rajoitettu-tilassa. Sinua pyydetään access tunnistetiedot. Tämä asetus muodostaa yhteyden tiukemmat runspace, verrattuna asetukset 1 ja 2.  Joitakin tehtäviä, jotka ovat käytettävissä vaihtoehto 1 kautta, **ei* voi suorittaa tämän runspace ovat:

    - Palauttaa factory-asetukset
    - Salasanan vaihtaminen
    - Tuen käytön salliminen ja estäminen
    - Päivitykset
    - Korjausten asentaminen 
                                                

    >[AZURE.NOTE] **Tämä on ensisijainen tapa, jos olet unohtanut laitteen järjestelmänvalvojan salasanan ja saa yhteyttä vaihtoehto 1 tai 2.**

4. **Vaihda kieli** Tämän asetuksen avulla voit Windows PowerShell-liittymän Näyttökielen vaihtaminen. Tukemat kielet ovat englanti, japani, venäjä, ranska, Etelä-Korea, espanja, italia, saksa, kiina ja Portugali (Brasilia).


## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>Etäkäyttö StorSimple StorSimple Windows PowerShellin avulla

Windows PowerShellin Etäpalvelujen avulla voit muodostaa yhteyden StorSimple laitteeseen. Kun muodostat yhteyden tällä tavalla, valikko, ei näytetä. (Näkyy valikon vain, jos käytät serial konsolin laitteeseen muodostaa. Yhteyden muodostaminen etäyhteyden siirryt suoraan "-vaihtoehto 1 – täydet" serial konsolin vastikkeen.) Etäpalvelujen Windows PowerShellin avulla voit muodostaa yhteyden tietyn runspace. Voit myös määrittää näyttökielen. 

Näyttökielen vaihtaminen ei vaikuta kieli, jolla voit määrittää serial konsoli-valikosta **Vaihda kieli** -asetuksen avulla. PowerShell-Etäistunto automaattisesti Nosta kielen laitteen yhdistäminen on Jos ei ole määritetty.

>[AZURE.NOTE] Jos käytät Microsoft Azure virtual isännät- ja StorSimple virtual laitteita, voit Windows PowerShellin Etäpalvelujen ja virtual isäntä muodostaa virtual laitteeseen. Jos olet määrittänyt Jaa sijainti, johon haluat tallentaa tiedot Windows PowerShell-istunnon isäntään, sinun olisi otettava huomioon, että kaikki tärkeimmät sisältää vain todennetut käyttäjät. Sen vuoksi, jos olet määrittänyt Jaa käyttää kaikki, kun yhdistät määrittämättä tunnistetiedot, Todentamattomille anonyymi lyhennys käytetään ja virhe tulee näkyviin. Voit korjata tämän ongelman, Jaa, voit isännöidä on Ota Vieras-tili ja anna sitten Vierailijan tilin täydet Jaa tai sinun on määritettävä kelvolliset tunnistetiedot sekä Windows PowerShellin cmdlet-komento.

Voit käyttää HTTP tai HTTPS Windows PowerShellin Etäpalvelujen välityksellä. Käytä opetusohjelmassa ohjeita:

- [Yhteys etäyhteyden http:n](storsimple-remote-connect.md#connect-through-http)
- [Muodostaa yhteyden etäyhteyden HTTPS](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Yhteyden suojausasiat

Kun ovat päätät StorSimple Windows PowerShellin muodostaminen, toimi seuraavasti:

- Laitteen serial konsolin liittämisestä on suojattu, mutta yhteyden muodostaminen verkon valitsimet serial konsoliin ei ole. Ole varovainen suojauksen, kun yhteyden muodostaminen verkon valitsimet laitteeseen sarja.

- Yhteyden kautta HTTP-istunnon saattaa tarjota paremman suojauksen kuin yhteyden kautta serial konsolin verkon kautta. Vaikka tämä ei ole turvallisin tapa on hyväksyttävä luotettujen verkoissa.

- Yhteyden kautta HTTPS-istunnon on turvallisin ja suositeltavaa vaihtoehtoa.


## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>Windows PowerShellin avulla StorSimple StorSimple laitteen hallinta
Seuraavassa taulukossa yleisiä hallintatehtäviä ja monimutkaisia työnkulkuja, jotka voivat tehdä StorSimple laitteesi Windows PowerShell-liittymää. Lisätietoja jokaiselle työnkululle valitsemalla haluamasi merkintä-taulukossa.

#### <a name="windows-powershell-for-storsimple-workflows"></a>Windows PowerShell StorSimple työnkulut

|Jos haluat tehdä tämä...|Näiden ohjeiden avulla.|
|---|---|
|Rekisteröi laite|[Määritä ja rekisteröi laite StorSimple Windows PowerShellin avulla](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
|Web-välityspalvelimen asetusten määrittäminen</br>Näytä web-välityspalvelimen|[Web-välityspalvelimen StorSimple-laitteen määrittäminen](storsimple-configure-web-proxy.md)|
|Muokkaa tietoja 0 verkon käyttöliittymän asetukset laitteen|[Muokkaa tietoja 0 verkkoliittymän StorSimple laitteeseesi](storsimple-modify-data-0.md)|
|Lopeta ohjain </br> Käynnistä tai Sammuta ohjain </br> Sulje laite</br>Laitteen factory oletusasetusten palauttaminen|[Hallitse laitteen ohjaimet](storsimple-manage-device-controller.md)|
|Asenna ylläpidon tila päivitykset ja korjausten|[Päivittää laitteen](storsimple-update-device.md)|
|Siirtyminen ylläpito </br>Lopeta ylläpidon tila|[StorSimple laitteen tilat](storsimple-device-modes.md)|
|Tuen paketin luominen</br>Salaus ja Muokkaa tuki-paketti|[Voit luoda ja hallita tuki-paketti](storsimple-create-manage-support-package.md)|
|Tuki-istunnon aloittaminen</br>|[Tuki-istunnon aloittaminen Windows PowerShellin StorSimple varten](/storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)
 

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Hanki ohjeita Windows PowerShellin StorSimple

StorSimple Windows PowerShell-cmdlet-komento ohje on käytettävissä. Tämän ohjeen online, ajan tasalla versio on myös käytettävissä, jonka avulla voit päivittää järjestelmän ohjeita.

Avun liittymän on samanlainen kuin Windows PowerShellin ja Ohje-ryhmän cmdlet-komennot yleensä ne toimivat. Voit tarkistaa Ohje Windows PowerShellin verkossa TechNet-kirjaston: [komentosarjat Windows PowerShellin avulla](http://go.microsoft.com/fwlink/?LinkID=108518).

Seuraavassa on lyhyt kuvaus Ohjeen lajit tämän Windows PowerShell-liittymän, mukaan lukien päivittämisestä on ohjeessa.

#### <a name="to-get-help-for-a-cmdlet"></a>Saat ohjeita cmdlet-komento

- Jos tarvitset apua cmdlet-komennon tai toiminnon, käytä seuraavaa komentoa:`Get-Help <cmdlet-name>`

- Hakeminen minkä tahansa cmdlet-komento online-ohje, käytä edellisen cmdlet-komennon avulla `-Online` parametri:`Get-Help <cmdlet-name> -Online`

- Koko ohjeita voit määrittää `–Full` parametrin ja esimerkkejä käyttäminen `–Examples` parametri.

#### <a name="to-update-help"></a>Jos haluat päivittää Ohje

Voit helposti päivittää Windows PowerShell-käyttöliittymän ohje. Seuraavien toimien päivittää järjestelmän ohjeita.

#### <a name="to-update-cmdlet-help"></a>Päivitä cmdlet-komento Ohje

1. Käynnistä Windows PowerShell **Suorita järjestelmänvalvojana** -vaihtoehto.

1. Kirjoita komentoriville seuraava komento: `Update-Help`

1. Päivitettyjä ohjeita tiedostoja asennetaan.

1. Kun Ohje-tiedostot on asennettu, kirjoittaa: `Get-Help Get-Command`. Tässä näkyvät cmdlet-komennot, jotka ovat saatavilla luettelo.


>[AZURE.NOTE] Saat kaikki käytettävissä olevat cmdlet luettelo runspace-Kirjaudu sisään vastaava vaihtoehto ja suorita `Get-Command` cmdlet-komento.

## <a name="next-steps"></a>Seuraavat vaiheet
Jos kohtaat ongelmia StorSimple-laitteen kanssa suoritettaessa yllä työnkulkujen, viitata [Työkalut StorSimple ominaisuuksissa vianmääritystä varten](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

