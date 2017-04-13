<properties
    pageTitle="Azure Active Directoryn hybrid tunnistetietojen tyyliseikat - tietojen suojauksen vaatimusten määrittäminen | Microsoft Azure"
    description="Kun suunnittelet yhdistelmäympäristön identity-ratkaisun, Määritä tietojen suojausvaatimukset yrityksesi ja mitä asetuksia on käytettävissä parhaiten täytä näitä vaatimuksia."
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

#<a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Suunnitelman vahva identity-ratkaisussa tietojen suojauksen parantaminen

Tietojen suojaaminen ensimmäiseksi on tunnistaa, kuka voi käyttää tietoja ja tässä yhteydessä, tarvitset jäsenyyden ratkaisu, joka voi integroituu järjestelmä antaa todennus- ja ominaisuuksia. Todennus- ja usein sekoittaa keskenään ja roolien ymmärtää väärin. Todellisuudessa ne aivan eri alla olevassa kuvassa osoitetulla tavalla:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)
 
**Mobiililaitteen hallinnan elinkaari vaiheet**

Kun suunnittelet yhdistelmäympäristön identity-ratkaisun tietojen suojausvaatimukset on hyvä tietää tä ja mitkä asetukset ovat käytettävissä täyttävät parhaiten näitä vaatimuksia.
 
>[AZURE.NOTE]
Kun lopetat tietojen suojauksen suunnittelu, tarkista varmistamiseksi valintasi koskevat vaatimukset monimenetelmäisen todentamisen on vaikuta tässä kohdassa tehdyt päätökset [vaatimusten monimenetelmäisen todentamisen määrittäminen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) .

## <a name="determine-data-protection-requirements"></a>Tietojen suojauksen vaatimusten määrittäminen
Mobility iän useimmat yritykset on yleisiä tavoite: niiden käyttäjät voivat olla tuottelias niiden mobiililaitteet paikallisen aikana tai etäyhteyden välityksellä missä tahansa, jotta tuottavuuden. Vaikka tämä saattaa johtua yleisiä tavoite, yrityksille, jotka on tällainen vaatimus voi myös huolta koskeva uhkien, joka on joiden lievity suojata yrityksen tiedot ja säilyttää käyttäjän tietosuoja määrää. Jokaisen yrityksen voi olla erilaisia vaatimuksia tältä; eri yhteensopivuuden säännöt, jotka vaihtelevat mitä teollisuuden mukaan yrityksen parhaillaan johtavat eri suunnitteluun. 

On kuitenkin suojaus tietyiltä, joka olisi tarkasteltavia ja vahvistaa, alaan riippumatta siitä, jossa on esitelty seuraavan osion.

## <a name="data-protection-paths"></a>Tietojen suojauksen polut

![](./media/hybrid-id-design-considerations/data-protection-paths.png)
 
**Tietojen suojauksen polut**

Yllä olevassa kaaviossa identity-osa on ensimmäinen tarkistettavaksi, ennen kuin tiedot ovat käytössä. Näitä tietoja voi kuitenkin eri aikana, se on käytetty. Tässä kaaviossa kaikkien lukujen edustaa polku, johon tiedot voivat sijaita joskus samanaikaisesti. Nämä numerot on selitetty alla:

1. Tietojen suojaaminen laitteen tasolla.
2. Tietojen suojaaminen aikana.
3. Milloin muiden paikallisen tietosuojan.
4. Tietojen suojaaminen osoitteessa muiden pilveen.

Vaikka tekniset ohjaa on Ota IT itse tietojen suojaaminen niitä yksitellen näiden vaiheiden ei suoraan tarjoamia hybrid identity-ratkaisu on tarpeen, että hybrid identity-ratkaisun pystyy hyödyntäminen sekä paikallisesti että cloud käyttäjätietojen hallinta resurssien ennen käyttäjälle käyttöoikeus tiedot. Kun hybrid tunnistetietojen suunnitteluratkaisun varmistaa organisaation vaatimusten mukainen vastataan seuraaviin kysymyksiin:

## <a name="data-protection-at-rest"></a>Tietojen suojaaminen rest-palvelussa
Riippumatta siitä, jossa tiedot on muiden (laitteessa, cloud tai paikalliseen) on tärkeää ymmärtää organisaation tarpeiden tältä arvioiminen. -Alueelle varmista seuraavat seikat pyydetään:

- Tarvitseeko yrityksen tietojen, loput?
 - Kyllä, jos on hybrid identity-ratkaisun voi integroida nykyisen paikallisen-infrastruktuurin?
 - Kyllä, jos on hybrid identity-ratkaisun voi integroida oman toiminnoista, jotka sijaitsevat pilvessä?
- Cloud jäsenyyksien hallinta suojaa käyttäjän tunnistetiedot ja muiden tietojen pilveen onnistuu?

## <a name="data-protection-in-transit"></a>Tietojen suojaaminen siirrettäessä
Tietojen siirron laitteen ja sen palvelinkeskuksen tai laite ja pilveen välillä on suojattava. Kuitenkin käytössä-siirrossa ei välttämättä tarkoita viestintä-prosessi, jonka osan ulkopuolella pilvipalvelussa; se siirtyy sisäisesti, myös näiden kahden virtual verkon välillä. -Alueelle varmista seuraavat seikat pyydetään:

- Tarvitseeko yrityksen tietojen siirron?
 - Kyllä, jos on hybrid identity-ratkaisun voi integroida suojatun ohjausobjekteja, kuten SSL/TLS?
- Cloud jäsenyyksien hallinta säilyttää tietoliikennettä hakemiston säilössä (ja niiden välillä palvelinkeskusten) allekirjoitettu?


## <a name="compliance"></a>Yhteensopivuus
Asetukset, lakeja ja lakisääteisten määräysten vaatimukset vaihtelevat alan, joka kuuluu yrityksen mukaan. Suuri säännellyillä teollisuuden yritysten on osoitteen jäsenyyksien hallinta koskee liittyvät mukaisuus. Asetusten, kuten Sarbanes tavalla (SOX), sairausvakuutus siirrettävyys ja vastuut Act (HIPAA), Gramm-Leach-Bliley Act (GLBA) ja maksu kortin alan tietojen suojauksen Vakio (PCI DSS) on erittäin tarkka koskeva tunnistetietojen ja käytön. Hybrid identity-ratkaisun, joka hyväksyy yrityksesi on oltava core ominaisuuksista, joita täyttävät yhden tai useamman nämä asetukset. -Alueelle varmista seuraavat seikat pyydetään:

- On hybrid identity-ratkaisun standardien kanssa yrityksesi lainmukaisia?
- Hybrid identity-ratkaisu on muodostettu ominaisuuksia, jotka mahdollistavat yrityksesi on yhteensopiva lainmukaisia? 
 
>[AZURE.NOTE]
Varmista, että kunkin vastauksen kirjoittaa muistiinpanoja ja ymmärtää perusteet takana vastaus. [Määritä tietojen suojauksen strategia](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) siirtyvät vaihtoehtoja ja kunkin asetuksen eduista/Haittapuolia.  Mukaan on vastannut näihin kysymyksiin valitaan sopii parhaiten sopivan vaihtoehdon yrityksesi on.

## <a name="next-steps"></a>Seuraavat vaiheet
 [Sisällön hallinta vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)


## <a name="see-also"></a>Katso myös
[Rakenteen huomioon otettavia seikkoja yleiskatsaus](active-directory-hybrid-identity-design-considerations-overview.md)
