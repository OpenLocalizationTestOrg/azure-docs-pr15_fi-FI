<properties
    pageTitle="Ota Azure pino uudelleen | Microsoft Azure"
    description="Ota Azure pino uudelleen."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="erikje"/>

# <a name="redeploy-azure-stack"></a>Ota Azure pino uudelleen

Ota Azure pino uudelleen, sinun on käynnistettävä päälle alusta alkaen seuraavalla tavalla.

## <a name="steps-to-redeploy-azure-stack"></a>Ota uudelleen Azure pinon vaiheet

1. Uudelleen isäntä alkuperäinen-käyttöjärjestelmään, (asennettuna ajoneuvon metalli). Tämä ei käynnistysvalikkoon, oletusasetus, joten sinun on käytettävä KVM tai paikallisessa konsoli napsauttamalla sitä uudelleenkäynnistyksen aikana (asennuksen aikana nimeltä "Käynnistys-Näennäiskiintolevyn" OS "AzureStack TP2" avulla, tämä auttaa selville, mitä OS on).

    Sinun ei tarvitse poistaa aiemmin määritettä (uusi tuki komentosarja "PrepareBootFromVHD.ps1" kestää huolellisesti, puolestasi.)

2. Jos ei ole KVM tai haluat valita käynnistyksen OS ennen uudelleenkäynnistämistä:
    
    1. Etsi komentosarjan.\BootMenuNoKVM.ps1. Tiedosto on käytettävissä muiden tuki komentosarjat tämän koontiversion mukana.
    
    2. Suorita komentosarja oikeuksin. Valitse alkuperäinen Host OS nimi. Tämä käynnistää isäntä alkuperäisen vastaanottavaan OS ilman KVM yhteyttä.
    
    3. Kun komentosarja on valmis, sinua pyydetään vahvistamaan, että käynnistetään uudelleen.

    - Jos määritettynä on muita käyttäjiä on kirjautunut sisään, tämä komento epäonnistuu.

    - Suorita seuraava komento vain: Käynnistä tietokone-pakottaminen 
 
3. Poista CloudBuilder.vhdx-tiedosto, jota käytettiin edellisen käyttöönoton osana.

    Ei tarvitse poistaa olemassa olevan tallennustilan resurssivarantoon edellisen TP2 käyttöönotto. Käyttöönoton komentosarja tunnistaa ja tyhjentää nykyisen ja valitse Luo uusi.

5. Ota uudelleen kopioimasta uuden CloudBuilder.vhdx käynnistyksen se jne.

## <a name="next-steps"></a>Seuraavat vaiheet

[Yhteyden muodostaminen Azure pino](azure-stack-connect-azure-stack.md)
