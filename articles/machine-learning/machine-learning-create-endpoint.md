<properties
    pageTitle="Web-Palvelupäätepisteet luominen tietokoneen Learning | Microsoft Azure"
    description="Web-Palvelupäätepisteet luominen Azure koneen oppiminen"
    services="machine-learning"
    documentationCenter=""
    authors="hiteshmadan"
    manager="padou"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="10/04/2016"
    ms.author="himad"/>


# <a name="creating-endpoints"></a>Päätepisteet luominen

>[AZURE.NOTE] Tässä ohjeaiheessa kerrotaan koskevat perinteinen WWW-palvelun avulla.

Kun luot asiakkaillesi eteenpäin myydä Web-palveluja, sinun täytyy koulutetun mallien tarjota kunkin asiakkaan, jotka on linkitetty edelleen koe, josta Web-palvelu on luotu. Lisäksi voit kokeilla päivitykset kohdistetaan valikoivasti päätepisteen korvaamatta mukautukset.

Tämän vuoksi Azure koneen Learning avulla voit luoda useita päätepisteet käyttöön WWW-palvelun. Päätepisteiden-Web-palvelu on itsenäisesti osoitettu, rajoittanut ja hallitaan. Kunkin päätepiste on yksilöllinen URL-osoite ja todennus-näppäintä, voit jakaa asiakkaillesi.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Päätepisteet lisääminen Web-palvelu

Päätepisteen lisääminen Web-palvelu kolmella eri tavalla.

* Ohjelmallisesti
* Palvelun Azure koneen Learning Web Services-portaalissa
* Vaikka Azure perinteinen portal

Kun päätepiste on luotu, voit tarjoaman kautta synkronisen ohjelmointirajapinnan erä API- ja excel-laskentataulukot. Lisäksi lisääminen päätepisteet tämän Käyttöliittymän välityksellä, voit käyttää myös päätepisteen hallinnan API Lisää ohjelmallisesti päätepisteet.

 >[AZURE.NOTE] Jos olet lisännyt muita päätepisteet Web-palveluun, et voi poistaa oletusarvoinen päätepiste.

## <a name="adding-an-endpoint-programmatically"></a>Lisäämällä päätepisteen ohjelmallisesti

Voit lisätä päätepisteen käyttäminen ohjelmallisesti [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) sample code Web-palvelu.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Lisäämällä päätepisteen Azure koneen Learning Web Services-portaalissa

1. Valitse vasemmanpuoleisessa sarakkeessa koneen Learning Studiossa verkkopalvelut.
2. Web-palvelu Raporttinäkymät-ikkunan alareunassa valitsemalla **Hallitse päätepisteet**. Azure koneen Learning Web Services-portaalin avautuu WWW-palvelun päätepisteet-sivulla.
3. Valitse **Uusi**.
4. Kirjoita nimi ja kuvaus uusi päätepiste. Päätepisteen nimet on oltava 24 merkki tai pienempi pituus ja on tehtävä pienet aakkosia tai numeroiden. Valitse haluamasi kirjaamistaso ja onko mallitiedot käytössä. Saat lisätietoja kirjaamisen [ottaa lokiin kirjaamisen käyttöön kone Learning Web services](machine-learning-web-services-logging.md).

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Päätepisteen Azure perinteinen portaalissa lisääminen


1. Kirjautuminen [Azure perinteinen portal](http://manage.windowsazure.com)-kohdasta **Tietokoneen Learning** vasemmassa sarakkeessa. Valitse työtila, joka sisältää Web-palvelu, johon olet kiinnostunut.

    ![Siirry työtilaan](./media/machine-learning-create-endpoint/figure-1.png)

2. Valitse **Web-palvelut**.

    ![Siirry verkkopalvelut](./media/machine-learning-create-endpoint/figure-2.png)

3. Valitse Web-palvelu, olet kiinnostunut luettelo käytettävissä päätepisteet.

    ![Siirry päätepiste](./media/machine-learning-create-endpoint/figure-3.png)

4. Valitse **Lisää päätepisteen**sivun alareunaan. Kirjoita nimi ja kuvaus, ei ole samannimistä verkkosovelluksen päätepisteet. Jätä rajoituksen tason sen oletusarvo ellei sinulla ole erityisvaatimuksista. Lisätietoja rajoitus on artikkelissa [Skaalaus API päätepisteet](machine-learning-scaling-webservice.md).

    ![Päätepisteen luominen](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Seuraavat vaiheet

[Miten tarjoaman julkaistun Azure koneen Learning Web-palvelu](machine-learning-consume-web-services.md).
