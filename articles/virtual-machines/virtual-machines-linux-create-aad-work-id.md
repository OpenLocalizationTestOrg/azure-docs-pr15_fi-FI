<properties
   pageTitle="Työpaikan tai oppilaitoksen käyttäjätietojen luominen AAD | Microsoft Azure"
   description="Opettele työpaikan tai oppilaitoksen käyttäjätietojen luominen Azure Active Directoryn Linux näennäiskoneiden käytettäväksi."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-linux-vms"></a>Työpaikan tai oppilaitoksen käyttäjätietojen luominen Azure Active Directory Linux VMs käyttäminen

Jos olet luonut omat Azure-tili tai on henkilökohtainen MSDN-tilaus ja luotu Azure-tili, voit hyödyntää MSDN Azure--hyvitykset luomiseen käytetyn *Microsoft-tilin* tunnistetiedot. Monia hyvien ominaisuuksia, Azure-- [resurssin ryhmän mallit](../azure-resource-manager/resource-group-overview.md) on esimerkki--tarvitse toimimaan (jäsenyyden hallitsee Azure Active Directory) työpaikan tai oppilaitoksen tiliä. Voit noudattaa alla olevia ohjeita voit luoda uudelle työpaikan tai oppilaitoksen tilin, koska lähettäjä-paras toimea tietoja Omat Azure-tili on, että se sisältää oletusarvoista Azure Active Directory-toimialueen, joiden avulla voit luoda uudelle työpaikan tai koulun tili, jota voit käyttää Azure ominaisuuksia, jotka edellyttävät sitä.

Kuitenkin viimeisimmät muutokset mahdollista Hallitse tilaustasi minkä tahansa Azure-tiliin `azure login` vuorovaikutteinen kirjautuminen menetelmä on kuvattu [seuraavassa](../xplat-cli-connect.md). Voit käyttää järjestelmään tai voit seurata, noudata ohjeita. Voit myös [luoda on työpaikan tai oppilaitoksen Azure Active Directoryn Windows VMs kanssa käytettävät tunnistetiedot](virtual-machines-windows-create-aad-work-id.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]
