<properties
   pageTitle="Usean Geo Ohje dokumentaation | Microsoft Azure"
   description="Lisätietoja työtilan luominen ja julkaiseminen web-palvelu eri kuin Etelä keskitetyn USA: ssa (SCUS) Azure alueen Azure alue."
   services="machine-learning"
   documentationCenter=""
   authors="tedway"
   manager="jhubbard"
   editor="rmca14"
   tags=""/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="tedway; neerajkh"/>

# <a name="multi-geo-help-documentation"></a>Usean Geo Ohje dokumentaatio

Tässä artikkelissa kuvataan, miten voit luoda työtilan ja julkaista verkkopalvelun Azure muilla alueilla.

## <a name="create-a-workspace"></a>Työtilan luominen

1. Kirjaudu Azure perinteinen-portaaliin.

2.  Valitse **+ Uusi** > **TIETOPALVELUJEN** > **MACHINE LEARNING** > **nopea luominen**.  Valitse **sijainti** Valitse toisen alueen, kuten **varaaja Aasian**.
![Usean Geo Ohje kuva 1][1]
3. Valitse työtila ja valitse sitten **Kirjaudu sisään ML Studio**.
![Usean Geo Ohje kuva 2][2]

4. Työtilan on nyt toisen alueen, joita voi käyttää aivan kuten muita työtila. Tutustu näytön oikeassa yläkulmassa siirtyminen työtilojen. Valitse avattavasta valikosta, alue ja valitse sitten työtila. Kaikki kohdat ovat paikallisia työtila-alueella. kaikkien palveluidesi web-työtila on luotu esimerkiksi on saman alueen työtilan sijaitsee.
![Usean Geo Ohje kuva 3][3]

## <a name="open-an-experiment-from-gallery"></a>Avaa kokeen valikoimasta

Jos avaat kokeen valikoimasta, voit valita myös mitä haluat kopioida voit kokeilla alue.

![Usean Geo Ohje kuva 4][4a]

## <a name="web-service-management"></a>Web-palvelun hallinta

Ohjelmallisesti hallintaa WWW-palvelut, kuten suorittavat Käytä aluekohtainen osoitetta:
- https://asiasoutheast.Management.azureml.NET
- https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Huomioita

1.  Voit kopioida vain kokeissa välillä työtilat, jotka kuuluvat saman alueen tällä tavalla. Jos haluat kopioida kokeen yli työtilat eri alueilla, voit käyttää [PowerShell](http://aka.ms/amlps) -komentosovelmalla [*Kopioi AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) voit tehdä tämän. Toisen Vaihtoehtoinen menetelmä on julkaista valikoimaan kokeen luettelon ulkopuoliset-tilassa, avaa se sitten muiden alueelta työtilassa.
2.  Alueen valitseminen näkyvät vain yhden alueen työtilat kerrallaan. Voit myöhemmin, voi nähdä luettelon kaikista työtilat, joihin sinulla on pääsy kaikkien alueiden kaikissa samanaikaisesti.  
3.  Vapaa työtilan tai vierailijan oikeudet (nimetön) työtilan luodaan ja ylläpidettävä Etelä keskitetyn USA Voit myöhemmin, voi luoda vapaa/vierailijan oikeudet työtiloja alueessa, joka on valittava.  
4.  Verkkopalvelut käyttöön varaaja Aasian työtilaan myös ylläpidettävä varaaja Aasian. Myöhemmin osaat halutessasi luoda kokeissa yhden alueen ja käyttöönotto luotu WWW-Palvelupäätepisteet eri alueisiin.  

## <a name="more-information"></a>Lisätietoja

Esitä kysymys [Azure koneen Learning keskustelupalstalle](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
