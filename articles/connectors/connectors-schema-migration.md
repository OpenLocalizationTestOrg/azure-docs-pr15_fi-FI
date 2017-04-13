<properties
    pageTitle="Logiikan sovellusten siirtäminen rakenteen versio 2015-08-01 – esikatselu | Sovelluksen Microsoft Azure-palvelu"
    description="Voit siirtää logiikan sovelluksia helposti rakenteen uusimpaan versioon. Noudata seuraavia ohjeita."
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="deonhe"/>

# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Logiikan sovellusten siirtäminen rakenteen versio 2015-08-01-esikatselu

Jos haluat siirtää aiemmin logiikan sovelluksia uuden rakenteen, toimi seuraavasti:  
1. Avaa sovellus logiikan Azure-portaalissa  
2. Valitse Päivitä rakenne:

 ![API-kuvake][step1]   
Päivitä rakenne-sivu näyttää ja linkki tiedostoon, joissa on määritelty uusi parannuksia tiedot: ![API-kuvake][step2]

>[AZURE.NOTE] Kun valitset **Päivityksen rakenteen**, syy suorittaa siirron vaiheiden automaattisesti ja antaa koodin tulos. Tämän avulla voit päivittää määrityksen, mutta, että noudatat hyvä coding käytäntöjä, kuten jäsennetty **parhaat käytännöt** -osan.

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Parhaat käytännöt siirrettäessä logiikan sovelluksia uusin rakenteen:  

- Kopioi siirretyt komentosarja uuden logiikan sovelluksen – ei korvaa vanhan jokin vasta, kun olet valmis oman testaus ja vahvistettu siirretyt sovellus toimii odotetulla tavalla.
- Testaa yhteyttä logiikan app **ennen** tuotannon sijoittaminen
- Kun siirto on valmis, Käynnistä päivitys käyttää [hallitun ohjelmointirajapinnan](./apis-list.md) mahdollisuuksien logiikan sovelluksia. Voit esimerkiksi aloittaa avulla Dropbox v2, käytössäsi on DropBox v1 jäljitettävä.


## <a name="whats-next"></a>Mitä seuraavaksi?
-  [Opi siirtämään logiikan-sovellukset](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






