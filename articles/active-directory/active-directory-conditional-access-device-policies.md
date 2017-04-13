<properties
    pageTitle="Ehdollisen käyttöoikeuden laitteen käytännöt Office 365-palveluissa | Microsoft Azure"
    description="Laitteen perusteella ehtojen tiedot hallita Office 365-palveluihin. Kun tietotyöntekijät (IWs) haluat käyttää Office 365-palvelujen, kuten Exchange ja SharePoint Onlinen työpaikalla tai oppilaitoksessa niiden Omat laitteilla, niiden IT-järjestelmänvalvoja haluaa käyttöoikeus on secure.IT järjestelmänvalvojat voivat valmistella ehdollisen käyttöoikeuden laitteen käytännöt suojaamiseen yritysresurssit, kun samanaikaisesti salliminen IWs yhteensopiva laitteissa voit käyttää palveluita."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>
# <a name="conditional-access-device-policies-for-office-365-services"></a>Ehdollisen käyttöoikeuden laitteen käytännöt Office 365-palveluissa

Termi, "ehdollinen käyttää" on useita ehtoja, kuten multi-factor todennetun käyttäjän, todennus liittyy laitteen ja yhteensopiva laite jne. Tässä ohjeaiheessa focusses ensisijaisesti laitteen perustuva ehtojen hallita käyttöoikeuksia Office 365-palveluissa. Kun tietotyöntekijät (IWs) haluat käyttää Office 365-palvelujen, kuten Exchange ja SharePoint Onlinen työpaikalla tai oppilaitoksessa niiden Omat laitteilla, niiden IT-järjestelmänvalvoja haluaa käyttöoikeus on suojattu. IT-järjestelmänvalvojat voivat valmistella ehdollisen käyttöoikeuden laitteen käytännöt suojaamiseen yritysresurssit, kun samanaikaisesti salliminen IWs yhteensopiva laitteissa voit käyttää palveluita. Office 365: een ehdollisen käyttöoikeuden käytännöt voidaan määrittää Microsoft Intune ehdollinen access-portaalista.

Azure Active Directory pakottaa ehdollisen käyttöoikeuden käytännöt suojaamiseen Office 365-palveluiden käyttäminen. Vuokraajan järjestelmänvalvoja, voit luoda ehdollisen käyttöoikeuden käytännön, joka estää käyttäjän käyttämästä O365-palvelu ei ole yhteensopiva laitteessa. Käyttäjän on oltava yrityksen laitteen suojauskäytäntöjen, ennen kuin voit olla käytettävyys-palveluun. Voit vaihtoehtoisesti järjestelmänvalvojan voit myös luoda käytännön, joka käyttäjien on vain rekisteröidä laitteensa, saat käyttöösi O365-palvelu. Käytäntöjä voi käyttöön kaikille organisaation käyttäjille tai vain muutaman kohde-ryhmän ja parannetun ajan kuluessa, voit sisällyttää muita kohteena ryhmät.

Pakotetut laitteen käytännöt edellyttää on tarkoitettu käyttäjille, voit rekisteröidä laitteensa Azure Active Directory laitteen rekisteröinti-palveluun. Voit valita käyttöön monimenetelmäisen todentamisen (MFA) rekisteröitymisestä laitteiden Azure Active Directory laitteen rekisteröinti-palvelun kanssa. MFA suositellaan Azure Active Directory laitteen rekisteröinti-palveluun. Kun MFA on käytössä, käyttäjien rekisteröinti laitteensa Azure Active Directory laitteen rekisteröinti-palvelussa liittyy toinen tekijä todennusta varten.

##<a name="how-does-conditional-access-policy-work"></a>Miten ehdollinen käyttöoikeuskäytäntö toimii?

Kun pyynnöt käyttöoikeudet O365-palvelu-ympäristö tuetulla laitteella, Azure Active Directory todentaa käyttäjään ja laitteeseen, josta käyttäjä avaa pyyntö; sekä myöntää käyttää palveluun vain silloin, kun käyttäjä mukainen käytännön määrittäminen palvelulle. Käyttäjät, joilla ei ole rekisteröity laitteen annetaan korjaavat ohjeita siitä, miten voit kirjoittamistasi yhteensopiva yrityksen O365-palvelujen ja rekisteröi. Käyttäjien iOS- ja Android-laitteille edellyttää rekisteröidä laitteensa yritysportaali-sovelluksesta. Kun käyttäjä on rekisteröity kyseistä laitteen, laitteen Azure Active Directoryyn ja rekisteröity laitteen hallintaa ja yhteensopivuusominaisuuksia varten. Asiakkaiden on käytettävä Azure Active Directory laitteen rekisteröinti-palvelua yhdessä Microsoft Intune mobiililaitteiden hallinta Office 365-palvelun käyttöön. Laitteen rekisteröinti on vanhat tarvittavat käyttäjät voivat käyttää Office 365-palveluja, kun laite käytännöt ovat voimassa.

Kun käyttäjä on rekisteröity kyseistä laitteen onnistuneesti, laite on luotettu. Azure Active Directory tarjoaa Single-Sign käyttää yrityksen sovelluksia ja pakottaa ehdollinen käyttöoikeuskäytäntö palvelu ei ole vain ensimmäistä kertaa käyttäjä pyytää access, mutta aina, kun käyttäjä pyytää uusi access käyttöoikeus. Käyttäjän hylätään access Services kun kirjautumisen tunnistetiedot ovat muuttuneet, laite on hävitty/varastamiselta tai käytäntöä ei täyty milloin pyynnön uusimista varten.

## <a name="deployment-considerations"></a>Käyttöönottoon liittyviä huomioita:
Laitteen rekisteröinnin Azure Active Directory laitteen rekisteröinti-palvelu on käytettävä.

Kun käyttäjät on paikallinen todennus, Active Directory Federation Services (AD FS) (1.0 ja yllä) on pakollinen. Monimenetelmäisen todentamisen saat työpaikkasi liittyä epäonnistuu, kun tunnistetietojen toimittaja ei voi monimenetelmäisen todentamisen. Esimerkiksi AD FS 2.0 ei ole multi-factor authentication-yhteyttä hyödyntäviin. Vuokraajan järjestelmänvalvoja on varmistettava, että paikallista AD FS on MFA pystyvät ja kelvollinen MFA-menetelmän on käytössä, ennen kuin otat käyttöön MFA Azure Active Directory laitteen rekisteröinti-palvelusta. Esimerkiksi AD FS-Windows Server 2012 R2 on MFA ominaisuuksia. Lisää kelvollinen todentaminen (MFA)-menetelmä ADFS-palvelimeen on otettava myös ennen käyttöönottoa MFA Azure Active Directory laitteen rekisteröinti-palvelusta. Lisätietoja tuetuista MFA esitetyssä AD FS saat AD FS määrittäminen muita todennustavat.

## <a name="frequently-asked-questions-faq"></a>Usein kysyttyjä kysymyksiä

K: mikä on pois jätettyjen oletuskäytäntö ympäristöjen, joita ei tueta laitteen?

V: tällä hetkellä ehdollisen käyttöoikeuden käytännöt ovat valikoivasti pakotetun käyttäjille iOS- ja Android-laitteisiin. Laitteen muiden ympäristöjen sovellukset ovat oletusarvoisesti vaikuta ehdollinen käyttöoikeuskäytäntö iOS-ja Android-laitteille. Vuokraajan järjestelmänvalvoja voi kuitenkin valita yleistä käytäntöä estääksesi käyttäjille, joita ei tueta ympäristöissä access ohittaa.
Se on ohjeet siirtämisestä ehdollinen käyttöoikeuskäytäntö muiden ympäristöjen, mukaan lukien käyttäjille.

K: kun Office 365: n ehdollinen käyttöoikeuskäytäntö laajennetaan selainpohjainen apps (esimerkiksi OWA, selainpohjaisia SharePoint).

V: tällä hetkellä ehdollisia käyttöoikeuksia Office 365-palvelut on rajoitettu laitteeseen RTF-sovellukset. Se on ohjeet siirtämisestä ehdollinen käyttöoikeuskäytäntö palvelujen avaaminen selaimet käyttäjille.
