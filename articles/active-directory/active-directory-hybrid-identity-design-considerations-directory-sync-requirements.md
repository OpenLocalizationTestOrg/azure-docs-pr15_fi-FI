<properties
    pageTitle="Azure Active Directoryn hybrid tunnistetietojen tyyliseikat - kansion synkronoinnin vaatimusten määrittäminen | Microsoft Azure"
    description="Määritä, mitä vaatimukset tarvitaan välinen käyttäjien synkronoiminen = edelleen paikallisen ja yrityksen Pilvipalvelun."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-directory-synchronization-requirements"></a>Kansion synkronoinnin vaatimusten määrittäminen
Synkronointi on tietoja antaa käyttäjille jäsenyyden pilveen paikallisen lisätoimenpiteisiin jäsenyyden perusteella. Onko ne käyttävät synkronoidaan tilin todennusta tai liitetyt todennusta varten, käyttäjät edelleen on pystyttävä muodostamaan jäsenyyden pilveen.  Tämä jäsenyys on voidaan ylläpitää ja päivitetään säännöllisesti.  Päivitykset voi kestää useita lomakkeita, muutokset salasanan muutokset.  

Käynnistä arvioimisen organisaatiot paikalliset tunnistetiedot ratkaisu ja käyttäjän vaatimukset. Arvioinnissa on tärkeää määrittää, miten käyttäjätietojen luodaan ja ylläpidetään pilveen tekniset vaatimukset.  Active Directory on paikallisen suurimmalla osalla organisaatioista, ja paikallisesta hakemistosta, jotka käyttäjät suoritetaan, synkronoidaan-, mutta joissakin tapauksissa tämä toiminto ei ole kirjainkokoa.  

Varmista, että seuraaviin kysymyksiin:


- Sinulla on yksi AD metsää, useita tai ei mitään?
 - Kuinka monta Azure AD-kansioita oltava synkronoit?
 
    1. Käytät suodatusta?
    2. Sinulla on useita suunniteltu Azure AD Connect-palvelimia?
  
- Tee asennetun synkronointi paikallisen korjaustyökalulla?
  - Jos Kyllä, ei käyttäjille, jos käyttäjillä virtual yhteystietojen/integrointi käyttäjätiedoista?
- Sinulla on kaikki muut directory paikallisen, jonka haluat synkronoida (esimerkiksi LDAP-hakemisto-HR tietokannan ja niin edelleen)?
  - Oletko tekeminen voidaan minkä tahansa GALSync?
  - Mikä on organisaation UPNs nykyinen tila? 
  - Onko sinulla toiseen kansioon, jotka käyttäjät todennetaan?
  - Yrityksesi käyttää Microsoft Exchangen?
    - Ne suunnitteleminen, ottaa exchange-yhdistelmäympäristössä

Nyt kun olet luonut verrata tietoja synkronoinnin tarpeen, sinun täytyy määrittää, mikä työkalu on oikea näitä vaatimuksia.  Microsoft tarjoaa useita työkaluja, suorittaa Directoryn synkronointia.  Katso lisätietoja [Hybrid tunnistetietojen hakemiston integrointi Työkalut taulukosta](active-directory-hybrid-identity-design-considerations-tools-comparison.md) . 
   
Nyt kun olet luonut synkronoinnin tarpeen ja työkalua, joka vastaanottajaksi yrityksesi, haluat arvioida näitä hakemistopalvelujen käyttävät sovellukset. Arvioinnissa on tärkeää määrittää nämä sovellukset pilveen integroiminen tekniset vaatimukset. Varmista, että seuraaviin kysymyksiin:

- Nämä sovellukset siirretään pilvipalveluun ja käyttää hakemiston?
- Onko määritteitä, jotka on synkronoitu pilveen, jotta nämä sovellukset voi käyttää niitä onnistuneesti?
- Tarvitseeko nämä sovellukset kirjoitetaan uudelleen hyödyntää cloud auth?
- Säilyvät nämä sovellukset live paikallisen, kun käyttäjät voivat käyttää niitä käyttämällä cloud-tunnusta?

Sinun on määritettävä suojauksen vaatimukset ja julkaisun rajoitukset hakemistosynkronoinnin myös. Arvioinnissa on tärkeää vaatimukset, jotta voit luoda ja ylläpitää käyttäjätietojen pilveen tarvitaan luettelo. Varmista, että seuraaviin kysymyksiin:

- Jos synkronointipalvelimen sijoitetaan?
- Se on liitetty?
- Sijoitetaan palvelimen rajoitettu verkon palomuurin, kuten kyseessä?
  - Voit voivat avata tarvittavat palomuurin porttien tukemaan synkronoinnin?
- Onko sinulla tietojen palauttaminen suunnitelma synkronointi palvelimen?
- Sinulla on kaikki metsätalousmaata varten oikea käyttöoikeuksin varustetun tilin haluat synkronoida kanssa?
 - Jos yrityksesi ei tiedä kysymyksen vastauksen, tarkista kohta "Käyttöoikeuksien salasanojen synkronoinnin" on artikkelissa [Azure Active Directory Sync-palvelun asentaa](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) ja määrittää, onko sinulla on jo nämä käyttöoikeuksin varustetun tilin tai jos haluat luoda.
- Jos sinulla on mutli metsää synkronointi on synkronointi-palvelimen pääsevät kunkin metsää?
 
>[AZURE.NOTE]
Varmista, että kunkin vastauksen kirjoittaa muistiinpanoja ja ymmärtää perusteet takana vastaus. [Selvittäminen tapauksen vastauksen vaatimukset](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) siirtyvät käytettävissä vaihtoehtojen päälle. Mukaan on vastannut näihin kysymyksiin valitaan sopii parhaiten sopivan vaihtoehdon yrityksesi on.

## <a name="next-steps"></a>Seuraavat vaiheet
[Monimenetelmäisen todentamisen vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Katso myös
[Rakenteen huomioon otettavia seikkoja yleiskatsaus](active-directory-hybrid-identity-design-considerations-overview.md)
