<properties 
    pageTitle="Tietoja Factory - nimeäminen säännöt | Microsoft Azure" 
    description="Tässä artikkelissa kuvataan Data Factory kohteiden nimeämiskäytäntö säännöt." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---naming-rules"></a>Azure tietojen Factory - säännöt nimeäminen 
Seuraavassa taulukossa on tietoja Factory palvelutiedot nimeämiskäytäntö säännöt.



Nimi | Nimen yksilöllisyyden | Kelpoisuuden tarkistus
:--- | :-------------- | :----------------
Tietoja Factory | Yksilöivä Microsoft Azure yli. Nimet ovat asennustavan, eli MyDF ja mydf viittaavat samaan tietojen factory. |<ul><li>Tietoja kunkin factory on yhdistetty täsmälleen yhden Azure-tilaukseen.</li><li>Objektinimet on alettava kirjaimella tai luku ja voivat sisältää vain kirjaimia, numeroita ja yhdysmerkin (-)-merkki.</li><li>Jokainen yhdysmerkin (-)-merkki on edessä välittömästi ja perään kirjain tai numero. Peräkkäiset katkoviivat eivät ole sallittuja säilö nimet.</li><li>Nimi voi olla 3 63 merkin pituinen.</li></ul>
Linkitetyn Services/taulukot/putkistot | Yksilöllinen, jossa tiedot factory. Nimet ovat asennustavan. | <ul><li>Taulukon nimen merkkien enimmäismäärä: 260.</li><li>Objektinimien on alettava kirjain luvun tai alaviivaa (_).</li><li>Seuraavat merkit eivät ole sallittuja: ".", "+", "?", "/", "<", ">";"*", "%", "&", ":","\\"</li></ul>
Resurssiryhmä | Yksilöivä Microsoft Azure yli. Nimet ovat asennustavan. | <ul><li>Merkkien enimmäismäärä: 1 000.</li><li>Nimessä voi olla kirjaimia, numeroita ja seuraavia merkkejä: "-", "_",","ja".".</li></ul>