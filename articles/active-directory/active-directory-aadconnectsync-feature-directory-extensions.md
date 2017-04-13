<properties
   pageTitle="Azure AD Connect synkronointi: kansion extensions | Microsoft Azure"
   description="Tässä ohjeaiheessa kerrotaan Azure AD Connect directory tunnisteet-ominaisuutta."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/19/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect synkronointi: kansion tunnisteet
Hakemisto-laajennukset avulla voit laajentaa rakenteen Azure AD oman ominaisuuksilla paikallisen Active Directorysta. Tämän ominaisuuden avulla voit muodostaa LOB-sovellusten käyttö määritteitä, voit jatkaa paikallisen hallinta. Nämä määritteet on kulutettu [Azure AD Graph directory tunnisteet](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) tai [Microsoft Graph](https://graph.microsoft.io/)kautta. Voit tarkastella käytettävissä käyttämällä [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) ja [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) määritteet.

Tällä hetkellä ei ole Office 365-työmäärää siinä käytetään seuraavanlaisia määritteitä.

Voit määrittää mitä haluat synkronoida mukautettuja asetuksia polussa ohjatun asennuksen määritteitä.
![Rakenteen tunniste ohjatun](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png) asennuksen näyttää määritteet, jotka ovat kelvollisia ehdokkaiden:

- Käyttäjien ja ryhmien objektityypit
- Yksittäisen arvon määritteitä: merkkijono, totuusarvo, kokonaisluku, binaariluvuksi
- Moniarvoinen määritteitä: merkkijono, binaariluvuksi

Luettelo määritteistä luetaan Azure AD Connect asennuksen aikana luodun välimuistista. Jos on laajennettu ylimääräisiä määritteitä Active Directory-rakenne- [rakenne on päivitettävä](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) ennen uuden määritteitä ovat näkyvissä.

Objektin voi olla enintään 100 directory tunnisteet määritteet. Enimmäispituus on 250 merkkiä. Jos määrite-arvo on pidempi, se katkaistaan sync engine mukaan.

Azure AD Connect asennuksessa sovellus rekisteröidään missä nämä määritteet ovat käytettävissä. Näet tämän sovelluksen Azure-portaalissa.  
![Rakenteen tunniste-sovellus](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3.png)

Nämä määritteet ovat nyt käytettävissä Graph kautta:  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Määritteet ovat etuliite tunniste\_{AppClientId}\_. AppClientId on sama arvo kaikki määritteiden Azure AD-kansiossa.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [Azure AD Connect synkronointi](active-directory-aadconnectsync-whatis.md) -määritys.

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
