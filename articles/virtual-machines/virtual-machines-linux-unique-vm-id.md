<properties
   pageTitle="Käytettäessä AM tunnus"
   description="Tässä artikkelissa kuvataan siirtyminen ja niiden käyttäminen Azure AM yksilöllinen tunnus"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="accessing-and-using-azure-vm-unique-id"></a>Siirtyminen ja niiden käyttäminen Azure AM yksilöllinen tunnus

Azure AM yksilöllinen tunnus on 128 bittiä tunnus koodattu tallennetaan kaikki Azure IaaS AM 's SMBIOS ja tällä hetkellä voi lukea millä ympäristö BIOS komentoja.

Azure AM yksilöllinen tunnus on vain luku-ominaisuus. Azure AM TEKSTI1 ei muuta Sammuta uudelleenkäynnistyksen yhteydessä (joko suunniteltu suunnittelematon), Aloita/Lopeta varaustiedoista varata, palvelun kenties tai palauta paikassa. Kuitenkin jos AM on kopioitu voit luoda uuden esiintymän, uusi Azure AM tunnus on määritetty.

> [AZURE.NOTE] Jos sinulla on vanhempi VMs luodaan ja käytössä, koska tämä uusi ominaisuus käytössä esiin (syyskuussa 18 2014), kirjoita uudelleenkäynnistyksen oman AM automaattisesti saat Azure yksilöllinen tunnus.


Siirry Azure yksilöivä AM tunnus sisällä AM seuraavasti:


## <a name="create-a-vm"></a>Luo AM
 

Lisätietoja on artikkelissa [Virtual Machine luominen](virtual-machines-linux-creation-choices.md)


## <a name="connect-to-the-vm"></a>Yhteyden muodostaminen AM
 

Lisätietoja on artikkelissa [Linux-SSH](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="query-vm-unique-id"></a>Kyselyn AM yksilöllinen tunnus

Komennon (esimerkissä käytetään **Ubuntu**):

    sudo dmidecode | grep UUID
    
Esimerkki odottamasi tulokset:

    UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
    
Vuoksi Big Endian bitti järjestys todellinen AM yksilöllinen tunnus tässä tapauksessa on:

    DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
    
    
Azure AM yksilöllinen tunnus voidaan eri skenaarioissa, että AM suoritetaan Azure tai paikallisen ja auttavat käyttöoikeuksien myöntämistä, raportointi ja Yleiset seuranta tarpeen on ehkä Azure IaaS käyttöönoton. Monta riippumattomat toimittajat sovellusten ja josta käy ilmi Azure-saattaa edellyttää tunnistavan Azure-AM koko sen elinkaaren aikana ja määrittää, onko AM käynnissä Azure-paikallisten tai muita cloud tarjoajia. Ympäristön tunnus auttaa esimerkiksi tunnistaa, jos ohjelmiston käyttöoikeus oikein tai avulla voidaan yhdistää AM tietojen lähteeseen, kuten auttaminen määrittämisestä sopivan ympäristön oikean mittaukset ja voit seurata ja yhdistää nämä arvot muita käyttötapoja toiseen.
