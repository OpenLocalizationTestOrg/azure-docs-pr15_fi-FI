<properties
    pageTitle="Tilanteita Azure Monimenetelmäisen todentamisen ja 3 osapuolen VPN-yhteydet"
    description="Tämä sivu sisältää tietoja vaiheittaisesta määrittämisestä määritykset Azure MFA 3 osapuolen prodcuts kanssa."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban" 
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-3rd-party-vpn"></a>Tilanteita Azure Monimenetelmäisen todentamisen ja 3 osapuolen VPN
Azure Monimenetelmäisen todentamisen avulla voidaan yhdistää saumattomasti eri 3 osapuolten VPN-ratkaisuja.  Tämä sisältää Cisco® ASA VPN-laitteen Citrix NetScaler SSL VPN-laitteen ja Juniper verkkojen suojatun Access/Pulse suojatun yhteyden suojattu SSL VPN-laitteen.

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Cisco ASA VPN-laite ja Azure Monimenetelmäisen todentamisen
Azure Monimenetelmäisen todentamisen integroituu saumattomasti Cisco® ASA VPN-laitteen lisäsuojauksen antamaan Cisco AnyConnect® VPN-kirjautumiset ja portaalin käyttöä.  Voit tehdä tämän joko LDAP- tai SÄDE-protokollan avulla.  Valitse jokin seuraavista vaiheittaiset määrityksistä latausohjeet.

Määritysten opas  | Kuvaus
------------- | ------------- |
[Cisco ASA Anyconnect VPN ja Azure LDAP MFA-asetuksissa](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Integroi saumattomasti Cisco ASA VPN-laitteen Azure MFA LDAP avulla|
[Cisco ASA Anyconnect VPN ja Azure RADIUS MFA-asetuksissa](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Integroi saumattomasti Cisco ASA VPN-laitteen käyttäminen RADIUS Azure MFA

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Citrix NetScaler SSL VPN ja Azure Monimenetelmäisen todentamisen
Azure Monimenetelmäisen todentamisen integroituu saumattomasti Citrix NetScaler SSL VPN-laitteen lisäsuojauksen antamaan Citrix NetScaler SSL VPN-kirjautumiset ja portaalin käyttöä.  Voit tehdä tämän joko LDAP- tai SÄDE-protokollan avulla.  Valitse jokin seuraavista vaiheittaiset määrityksistä latausohjeet.

Määritysten opas  | Kuvaus
------------- | ------------- |
[Citrix NetScaler SSL VPN ja Azure LDAP MFA-määritys](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Integroi saumattomasti Citrix NetScaler SSL VPN-verkon Azure MFA laitteen LDAP avulla|
[Citrix NetScaler SSL VPN ja Azure RADIUS MFA-määritys](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Integroi saumattomasti Citrix NetScaler SSL VPN-laitteen käyttäminen RADIUS Azure MFA

##<a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Juniper/Pulse suojattu SSL VPN-laite ja Azure Monimenetelmäisen todentamisen
Azure Monimenetelmäisen todentamisen integroituu saumattomasti Juniper/Pulse suojattu SSL VPN-laitteen lisäsuojauksen antamaan Juniper/Pulse suojattu SSL VPN-kirjautumiset ja portaalin käyttöä.  Voit tehdä tämän joko LDAP- tai SÄDE-protokollan avulla.  Valitse jokin seuraavista vaiheittaiset määrityksistä latausohjeet.

Määritysten opas  | Kuvaus
------------- | ------------- |
[Juniper/Pulse suojatun SSL VPN-yhteyden ja Azure LDAP MFA-määritys](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx)| Integroi saumattomasti oman Juniper/Pulse suojattu SSL VPN Azure MFA laitteen LDAP avulla|
[Juniper/Pulse suojatun SSL VPN-yhteyden ja Azure RADIUS MFA-määritys](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Integroi saumattomasti Juniper/Pulse suojattu SSL VPN-laitteen käyttäminen RADIUS Azure MFA
