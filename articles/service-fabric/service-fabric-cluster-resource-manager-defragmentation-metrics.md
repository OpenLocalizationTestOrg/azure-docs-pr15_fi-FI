<properties
   pageTitle="Azure palvelun kangasta arvot kohteen | Microsoft Azure"
   description="Yleiskatsaus käyttämällä eheytys tai palvelun kangasta arvot strategia kuin pakkaaminen"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Arvot ja lataa-palvelun kangasta kohteen
Palvelun kangasta klusterin Resurssienhallinta pääasiassa koskee tasaamisen kannalta kuormituksen jakaminen – varmistamalla, että kaikki klusterin solmujen tasaisesti käytössä. Tämä on yleensä turvallinen ja Edistyksellinen käyttöjärjestelmä asettelun kannalta elossa epäonnistuu, koska Näin varmistat, että minkä tahansa määritetyn virheen ei poista joitakin annetun työmäärää suuri prosentti. Palvelun kangasta klusterin Resurssienhallinta tue eri strategia sekä, joka on eheytys. Eheytys tarkoittaa yleensä, että sen sijaan, että yrität jakaa klusterin mittarin käyttö-olisi todella Yritämme koottavat sitä. Tämä on Microsoftin Normaali strategia – sijaan optimointi klusterin perusteella pienentäminen pysyykö metrisillä kuormituksen keskimääräinen keskihajonnan, ILO etumerkin, aluksi optimoiminen keskihajonta kasvaa. Mutta miksi haluat tämän toimintamallin?

Jos sinun on levittäminen kuormituksen tasaisesti klusterin solmujen sitten voit olet syödään joitakin resurssit, joiden solmut on tarjolla. Tavallisesti tämä ei ole ongelma, mutta joskus joitakin työmääriä luominen palveluja, jotka ovat poikkeuksellisen suuria ja tarjoaman solmun suurin osa – vaikkapa 75 95 % solmun resurssien sinut erillinen palvelun yksittäisen esiintymän tai replika. Ei ole ongelma, klusterin Resurssienhallinta tunnistaa palvelun luominen aikaa, jotka tarvitaan tilaa tämä suuren kuormituksen ja siitä, että se tapahtuu klusterin järjestäminen uudelleen, mutta sillä välin kyseisen työmäärää on odottamaan voidaan ajoittaa klusterin.

Koska, että uusi työmääriä ajoitus on yleensä ainakin pieni viive luottamuksellisia, jos olemme et tee mitään eri tavalla emme voi joskus bEi tärkeä oikeuden näiden palvelutasosopimuksia sen jälkeen, jos on paljon services ja tila siirtyminen, etenkin jos työmääriä klusterin ole yleensä suuri (ja näin ollen kestää odotettua klusterin siirtyminen). Myös luomisen ajat simulaatioita reaali klusterin tietojen perusteella mitattuna on olemme tuli, jos services on riittävän suuri ja klusterin varsin käytössä, suuri palvelujen luominen hidastua sekä että saattaa parantamiseen tämä tuomalla eheytys arvot käytännön.

Tavoin tiedoston luomis- tai access saattaa Hae slowed Jos tietokoneen kiintolevylle on pirstoutuneet ja voitu nopeutuu eheytyksellä asema, niin, että käytettävissä on suurempi jatkuvat lohkot, voit määrittää eheytys mittarit on klusterin Resurssienhallinta yrittää itse tiivistää palvelut kuormituksen vähemmän solmujen, niin, että (lähes) aina on myös suuri palvelujen tilaa , ne voidaan luoda nopeasti. Useimmat käyttäjät ei tarvita, koska palvelut yleensä pitäisi olla pieni ja näin ollen ei ole vaikeaa, voit etsiä ryhmän niiden, mutta jos käytössäsi on suuri services ja tarvitset niitä luodaan nopeasti (ja parantavat impactfulness virheiden kuten kompromissien ja parhaillaan vasemmalle unutilized samalla, kun ne Odota työmääriä voidaan ajoittaa resurssien tiedostokoolla ole), valitse eheytys strategia on tarkoitettu sinulle.

Seuraavassa kuvassa antaa kaksi eri klustereiden, yksi, jossa on eheytetty ja yksi, joka ei ole visuaalisessa muodossa. Tasatut tapauksessa kannattaa liikkeitä, joka olisi tarvittavat kohtaan suurin service-objektia, uuden on luotu, verrattuna defragmented klusterin, se saattaa olla heti sisältävään solmuissa 4 ja 5.

![Saapuva ja eheytetty klustereiden vertailu][Image1]

## <a name="defragmentation-pros-and-cons"></a>Eheytys etuja sekä haittoja
Mitkä ovat niin näiden käsitteellinen kompromissien? On suositeltavaa perusteellisesti mittaaminen oman työmääriä, ennen kuin otat käyttöön eheytys arvot. Seuraavassa on lyhyt taulukko kannattaa ottaa huomioon:

| Eheytys asiantuntijoille  | Eheytys kuvakkeet |
|----------------------|----------------------|
|Luomisen nopeus suuri palvelujen avulla | Rikasteet ladata vähemmän solmuissa ristiriita lisääminen sivulle
|Ottaa käyttöön Pienennä tietojen siirto luonnin aikana    | Virheet voivat vaikuttaa Lisää palveluja ja lisää churn aiheuttaa
|Sallii monipuolisia kuvaus vaatimuksista ja regeneroitaviksi tilaa | Monimutkaisempia yleinen Resurssienhallinta-määritys

Voit käyttää samaa klusterin eheytetty ja normaalin arvot ja Resurssienhallinta hyötyä sen parhaiten jotta saat asettelua, joka kokoaa mahdollisimman paljon eheytys mittarit, kun se aikana yritetään muiden jaettuina. Saat tarkat tulokset riippuu määrän tasaamisen arvot verrattuna määrään eheytys arvot ja niiden leveydet, nykyisen latautuu, jne.

## <a name="configuring-defragmentation-metrics"></a>Eheytys arvot määrittäminen
Määrittäminen eheytys arvot on yleinen päätös klusterin ja eheyttäminen voi valita yksittäisiä arvot:

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Disk" Value="true" />
    <Parameter Name="CPU" Value="false" />
</Section>
```

## <a name="next-steps"></a>Seuraavat vaiheet
- Klusterin Resurssienhallinta on paljon kuvaava klusterin asetukset. Selvitä lisätietoja niitä Katso tässä artikkelissa [kerrotaan palvelun kangasta klusterin](service-fabric-cluster-resource-manager-cluster-description.md)
- Arvot ovat, miten palvelun kangasta klusterin resurssin Manager hallitsee kulutus ja kapasiteettiin klusterin. Saat lisätietoja niitä ja miten ne määritetään Tutustu [tämän artikkelin](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
