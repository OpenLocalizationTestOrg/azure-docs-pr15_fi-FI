<properties
   pageTitle="Luo FQDN AM Azure-portaalissa | Microsoft Azure"
   description="Opettele luomaan täydellistä toimialuenimeä tai FQDN Resurssienhallinta,-pohjaiseen virtual machine Azure-portaalissa."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Luoda täysin täydellinen toimialuenimi Azure-portaalissa
Kun luot virtual machine (AM) [Azure portal](https://portal.azure.com) Resurssienhallinta käyttöönoton mallin, virtuaalikoneen julkiseen IP-resurssin luodaan automaattisesti. IP-osoitteen avulla voit käyttää etäyhteyden välityksellä AM. Vaikka portaalin ei luoda [täydellinen toimialuenimi](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), tai täydellinen toimialuenimi, oletusarvoisesti, voit luoda sen AM luomisen jälkeen. Tässä artikkelissa kerrotaan vaihe vaiheelta, miten DNS-nimen tai FQDN luominen.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Voit muodostaa etäyhteyden välityksellä, AM DNS-nimeä esimerkiksi varten työpöydän RDP (Remote Protocol) avulla.

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt, että AM on julkinen IP-osoite ja DNS-nimi, voit ottaa käyttöön yleisiä sovelluksen kehysten tai palveluihin IIS-, SQL- tai SharePoint.

Voit myös lukea lisää vihjeitä luominen Azure käyttöönoton [resurssien hallinnan avulla](../azure-resource-manager/resource-group-overview.md) .