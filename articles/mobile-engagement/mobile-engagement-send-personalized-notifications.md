<properties 
    pageTitle="Lähetä mukautettu ilmoitus kanssa Azure Mobile välitys" 
    description="Lähettäminen Omat ilmoitukset ilmoitukset, kuten heidän nimensä käyttäjäprofiilin tietoja lisäämällä"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="all" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="personalize-notifications-by-including-user-name"></a>Mukauta ilmoitukset sisällyttämällä käyttäjänimi

Varmista ilmoitukset näköisen sovelluksen käyttäjille tekemällä Yleishaku huomioon mukauttaminen niitä. Yksi tehokas tapa käsittää käytön valikoivasti sähköpostiosoite, ilmoitukset, jotta ne uusia henkilökohtaisia sovelluksen käyttäjien nimet. Varoitus tähän - sanan sinun olisi lähestymistapa lisääminen käyttäjänimet ilmoitukset huolellisesti koska Jos tämä strategia liikakäytöksi sitten se voi tulla kuin epäilyttävältä app joidenkin käyttäjien. Sinun tulee myös varmistaa, että käyttäjän osallistua henkilökohtaiset tiedot antaa sinulle täysi suostumus mobiilisovelluksessa ennen kuin aloitat, käytät tätä toimintoa on auttaa muita. 

Tarkalleen ottaen Azure Mobile varauksia, jossa voit tehdä mukauttaminen ilmoitukset, jossa Käytämme skenaariota, mukaan lukien käyttäjänimi ilmoituksiin ohjeita noudattamalla. Haluat käyttää sovelluksen tiedot käsite tai tunnisteita, jonka arvot joko voi välittää mukaan SDK: T integroitu Mobile-sovelluksesta tai API. Nämä sovelluksen Infos tai tunnisteita, valitse käytettävä:

1. Saat kohdistamisen ilmoitukset tietyille käyttäjille sovelluksen-tiedot arvojen perusteella tai 
2. ilmoitukset, joiden lähetettäessä ilmoitukset laitteen korvaa arvot tietyn laitteen/käyttäjä-sovelluksessa paikkamerkkeinä. 

> [AZURE.IMPORTANT] Huomaa, että nopeuden ilmoitusten lähettäminen nähdä vähennys vuoksi tämän lisäkäsittelyä, sovelluksen tiedot-arvojen korvaaminen kunkin ilmoitukset. 

##<a name="register-app-info-in-the-mobile-engagement-portal"></a>Rekisteröi sovelluksen tiedot Mobile välitys-portaalissa

1) Aloitetaan rekisteröinti sovelluksen tiedot tai tunnisteita Azure-portaalissa. Valitse **asetukset** -> tämän**tunnisteen (tiedot sovelluksen)** .  

![][1]  

2) Valitse **Uusi tunniste (tiedot sovelluksen)** ja *käyttäjätunnus* muodossa nimi ja tyyppi on kuin *merkkijono* ja valitse **Lähetä**. 

![][2]

3) Lopuksi näet tämän sovelluksen-info rekisteröity seuraavalla tavalla:

![][3]

##<a name="send-app-info-from-the-client-sdk"></a>Sovelluksen tiedot lähettäminen asiakkaan SDK

Tähän on käytössä Windows yleinen app esimerkissä, mutta vastaavat menetelmiä olemassa myös sekä muita SDK: T. 

Jos sinulla on menetelmän mobiilisovelluksen, johon sinulla on profiilitiedot käyttäjän, kuten heidän nimensä todennäköisesti ne tarkistamisen jälkeen, voit soittaa `SendAppInfo` tätä menetelmää ja täytä arvo `user_name` sovelluksen tiedot, jotka kyselyjä Mobile välitys palvelun Taustajärjestelmä aikaisemmin rekisteröinyt. 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

##<a name="send-personalized-notifications"></a>Mukautetut ilmoitukset lähetetään

Nyt voit on määritetty, käyttämällä tämän **käyttäjätunnus**ilmoitukset lähetetään. 

1) Siirry Mobile välitys Portal **yhteys** -välilehdessä voit luoda ilmoituksen ja voit käyttää mitä tahansa ilmoituksen otsikko ja teksti-muotoa tämän paikkamerkin. 

![][4]  

> [AZURE.NOTE] Käyttäjätunnus-sovelluksen tiedot ei määritetä, jonka, käyttäjät saavat ei ilmoitusta. Jos suoritat ilmoituksen markkinointikampanjan testi-tilassa, ja jos sinulla ei ole sovelluksen tiedot sitten lähetämme määrittäminen '?-merkin korvaa paikkamerkki. 

2) Kun Mobile välitys valitsee laite, kun haluat lähettää ilmoituksen, valitse se katsomalla tämän sovelluksen-tiedot ja korvaa paikkamerkin arvo.  
Jos Microsoft on esimerkiksi `str = "Scott"` käyttäjän kuin laitteen rekisteröinti saavat sovelluksen tiedot liitetty **käyttäjätunnus = SCOTT** kyseisen käyttäjän sekä tämän käyttäjän artikkelissa sovelluksen push ilmoituksella muodossa poissa. 

![][5]  

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png

