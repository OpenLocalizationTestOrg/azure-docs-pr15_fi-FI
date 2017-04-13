<properties 
   pageTitle="Laitteiston StorSimple 10 GbE liittymät | Microsoft Azure"
   description="Kuvataan tuetut pieni lomake-kerroin vaihdettava (SFP) enintään, kaapeli ja 10 GbE verkkoliittymät valitsimet StorSimple laitteen."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>Tuetut laitteet 10 GbE-liittymät StorSimple laitteen

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa on tietoja myös muita laitteita, jotka Microsoft Azure StorSimple laite toimii.

## <a name="list-of-devices-tested-by-microsoft"></a>Luettelo Microsoftin testattu

Microsoft on testannut seuraavat pieni lomakkeen kertoimen vaihdettava (SFP) enintään, kaapeli ja siirtyy varmistaa, että ne toimivat optimaalisesti laitteiden kanssa. (Seuraavissa taulukoissa päivitetään, kun uusi laite on testannut.)

### <a name="sfp-transceivers"></a>SFP + enintään

|Tee|Malli|
|---|---|
|Cisco|SFP-10G-SR|

### <a name="cables"></a>Kaapeli

|S-KIRJAINTA. Ei. |Tee|Malli|
|---|---|---|
| 1.|Cisco|SFP H10GB-CU1M|
| 2.|Cisco|SFP H10GB-CU2M|
| 3.|Cisco|SFP H10GB-CU3M|
| 4.|Tripp kertomasta Lite|N820 05M (OM3)|

### <a name="switches"></a>Valitsimet

|S-KIRJAINTA. Ei.|Tee|Malli|
|---|---|---|
| 1. |Cisco|N3K C3172PQ-10GE|
| 2. |Cisco|N3K-C3048-ZM-F|
| 3. |Cisco|N5K C5596UP-FA|

## <a name="list-of-devices-tested-in-the-field"></a>Luettelo testattu kenttään

Tässä osassa on luettelo laitteista, jotka on otettu käyttöön kenttään StorSimple asiakkaiden. Nämä ei ole testattu Microsoft, mutta todennäköisesti StorSimple laitteen käyttöä varten.
 
| Parametri                         | Arvo                                    |
|-----------------------------------|------------------------------------------|
| Tee vaihtaminen                     | Juniper                                  |
| Vaihda-malli                    | ex4550 32F                               |
| Vaihda käyttöjärjestelmäversio | JunOS 12.3R9.4                           |
| Sivu-malli                     | Portit määrän (PIC 0)                    |
| Vastaanotin merkki                  | Juniper                                  |
| Vastaanotin malli               | Osan numero 740 021308 <br></br> Osan numero 740 030658                   |
| Vastaanotin laitteisto-versio    | REV 01 versio 0,0 (ilmoitettu)            |
| Kaapeli malli                     | Kaksisuuntainen hyppykytkin LC/LC 50/125µ, OM3, LSZH |
| StorSimple malli                | 8600                                     |
| StorSimple versio     | 6.3.9600.17491                           |


## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>Luettelo testattu OEM-palveluntarjoaja (Mellanox)  

Mellanox on testannut seuraavat pieni lomakkeen kertoimen vaihdettava (SFP) enintään, kaapeli ja varmistaa, että ne toimivat optimaalisesti Mellanox verkon liittymät 10 GbE-liittymät StorSimple laitteen esimerkiksi valitsimet.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kaapeli ja moduulit tukemat Mellanox 

Seuraavassa taulukossa on lueteltu kaapeli ja moduulit tukemat Mellanox. Nämä ei ole testattu Microsoft, mutta todennäköisesti StorSimple laitteen käyttöä varten.

| S-KIRJAINTA. Ei. | Nopeus | Malli                 | Kuvaus                                            | Tee                |
|--------|-------|-----------------------|--------------------------------------------------------|-----------------------|
| 1.     | 10 GbE| CAB SFP-SFP - 1M        | Passiivinen kupari kaapeli SFP + 10 gt/s 1m                   | Arista                |
| 2.     | 10 GbE| CAB SFP-SFP - 2M        | Passiivinen kupari kaapeli SFP + 10 gt/s 2m                   | Arista                |
| 3.     | 10 GbE| CAB SFP-SFP - 3M        | Passiivinen kupari kaapeli SFP + 10 gt/s 3m                   | Arista                |
| 4.     | 10 GbE| CAB SFP-SFP - 5M        | Passiivinen kupari kaapeli SFP + 10 gt/s 5m                   | Arista                |
| 5.     | 10 GbE| Cisco SFP-H10GBCU1M   | Cisco SFP + kaapeli                                       | Cisco                 |
| 6.     | 10 GbE| Cisco SFP-H10GBCU3M   | Cisco SFP + kaapeli                                       | Cisco                 |
| 7.     |10 GbE | Cisco SFP-H10GBCU5M   | Cisco SFP + kaapeli                                       | Cisco                 |
| 8.     | 10 GbE| J9281B HP X242 10 G    | SFP + SFP + 1m liittäminen suoraan kupari kaapeli             | HP                    |
| 9.     | 10 GbE| 455883 B21 HP BLc     | 10 gigatavua SR SFP + osallistua                                       | HP                    |
| 10.    | 10 GbE| 455886 B21 HP BLc     | 10 gigatavua LR SFP + osallistua                                       | HP                    |
| 11.    |10 GbE | 487649 B21 HP BLc     | SFP + 0,5 m 10GbE kuparia kaapeli                           | HP                    |
| 12.    |10 GbE | 487652 B21 HP BLc     | SFP + 1m 10GbE kuparia kaapeli                             | HP                    |
| 13.    |10 GbE | 487655 B21 HP BLc     | SFP + 3m 10GbE kuparia kaapeli                             | HP                    |
| 14.    |10 GbE | 487658 B21 HP BLc     | SFP + 7m 10GbE kuparia kaapeli                             | HP                    |
| 15.    |10 GbE | 537963 B21 HP BLc     | SFP + 5m 10GbE kuparia kaapeli                             | HP                    |
| 16.    |10 GbE | AP784A HP             | 3m C-sarjan passiivinen kupari SFP + kaapeli                  | HP                    |
| 17.    |10 GbE | AP785A HP             | 5m C-sarjan passiivinen kupari SFP + kaapeli                  | HP                    |
| 18.    |10 GbE | AP818A HP             | 1m B sarjan aktiivinen kupari SFP + kaapeli                   | HP                    |
| 19.    |10 GbE | AP819A HP             | 3m B sarjan aktiivinen kupari SFP + kaapeli                   | HP                    |
| 20.    |10 GbE | J9150A HP             | X132 10 G SFP + LC SR vastaanotin                        | HP                    |
| 21.    |10 GbE | J9151A HP             | X132 10 G SFP + LC LR vastaanotin                        | HP                    |
| 22.    |10 GbE | J9283B HP             | X242 10 G SFP + SFP + 3 m DAC kaapeli                        | HP                    |
| 23.    |10 GbE | J9285B HP             | X242 10 G SFP + SFP + 7 m DAC kaapeli                        | HP                    |
| 24.    | 10 GbE| JD095B HP             | X240 10 G SFP + SFP + 0.65 m DAC kaapeli                     | HP                    |
| 25.    | 10 GbE| JD096B HP             | X240 10 G SFP + SFP + 1,2 m DAC kaapeli                      | HP                    |
| 26.    | 10 GbE| JD097B HP             | X240 10 G SFP + SFP + 3 m ISÄ kaapeli                        | HP                    |
| 27.    | 10 GbE| MAM1Q00A QSA Mellanox | QSFP SFP + sovittimen                                   | Mellanox tekniikat |
| 28.    | 10 GbE| MC2309124 006 Mt      | Passiivinen kupari kaapeli 1 x SFP+, QSFP 10 gt/s 24awg 7 m   | Mellanox tekniikat |
| 29.    | 10 GbE| MC2309124 007 Mt      | Passiivinen kupari kaapeli 1 x SFP+, QSFP 10 gt/s 24awg 7 m   | Mellanox tekniikat |
| 30.    | 10 GbE| MC2309130 003 Mt      | Passiivinen kupari kaapeli 1 x SFP+ QSFP 10 gt/s 30awg, 3 m   | Mellanox tekniikat |
| 31.    | 10 GbE| MC2309130 00A Mt      | Passiivinen kupari kaapeli 1 x SFP+, QSFP 10 gt/s 30awg 0,5 m | Mellanox tekniikat |
| 32.    | 10 GbE| MC3309124-005 Mt      | Passiivinen kupari kaapeli 1 x SFP+ 10 gt/s 24awg 5 m           | Mellanox tekniikat |
| 33.    | 10 GbE| MC3309124 007 Mt      | Passiivinen kupari kaapeli 1 x SFP+ 10 gt/s 24awg 7 m           | Mellanox tekniikat |
| 34.    | 10 GbE| MC3309130 003 Mt      | Passiivinen kupari kaapeli 1 x SFP+ 10 gt/s 30awg 3 m           | Mellanox tekniikat |
| 35.    | 10 GbE| MC3309130 00A Mt      | Passiivinen kupari kaapeli 1 x SFP+ 10 gt/s 30awg 0,5 m         | Mellanox tekniikat |

### <a name="switches-supported-by-mellanox"></a>Tukemat Mellanox valitsimet 

Seuraavassa taulukossa on luettelo valitsimista tukemat Mellanox. Nämä ei ole testattu Microsoft, mutta todennäköisesti StorSimple laitteen käyttöä varten.

| S-KIRJAINTA. Ei. | Nopeus | Malli | Kuvaus                                                         | Tee              |
|--------|-------|-------------|---------------------------------------------------------------------|-------------|
| 1.     | 10GbE | 516733 B21  | HP ProCurve 6120XG 10GbE Ethernet sivu valitsin                      | HP    |
| 2.     | 10GbE | 538113 B21  | HP 10GbE läpivienti moduuli (PTM)                                  | HP    |
| 3.     | 10GbE | EN4093      | IBM PureFlex kangasta EN4093 10 Gigabit skaalattava valitsin moduuli | IBM   |
| 4.     | 1GbE  | 3020        | Cisco Catalyst 3020 1GbE valitsin-sivu                               | Cisco |
| 5.     | 1GbE  | 3020 X       | Cisco Catalyst 3020 X 1GbE valitsin-sivu                              | Cisco |
| 6.     | 1GbE  | 438030 B21  | HP 1GbE valitsin moduuli - GbE2c kerros 2/3 Ethernet sivu vaihtaminen       | HP    |
| 7.     | 1GbE  | 6120G       | HP ProCurve 6120G/XG 1GbE valitsin-sivu                              | HP    |

## <a name="next-steps"></a>Seuraavat vaiheet

[Lue lisää siitä, StorSimple laitteet ja tila](storsimple-monitor-hardware-status.md).
