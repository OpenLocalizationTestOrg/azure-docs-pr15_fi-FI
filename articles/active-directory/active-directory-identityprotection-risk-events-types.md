<properties
    pageTitle="Riskin tapahtumien Azure Active Directory tunnistetietojen suojauksen havaitsemien | Microsoft Azure"
    description="Tässä ohjeaiheessa tutustutaan yksityiskohtaiset yleiskatsaus käytettävissä tyyppisiä riskin tapahtumia Azure Active Directory tunnistetietojen suojaus"
    services="active-directory"
    keywords="Azure active Directoryn tunnistetietojen suojaus-cloud app etsiminen-sovellukset, suojaus, riski, riskin taso, heikkous, suojauskäytäntö hallinta"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="markvi"/>

#<a name="types-of-risk-events-detected-by-azure-active-directory-identity-protection"></a>Riskin tapahtumien havaitsemien Azure Active Directory tunnistetietojen suojaus 

Azure Active Directory tunnistetietojen suojaus-riski tapahtumat ovat tapahtumia, jotka:

- on merkitty epäilyttäväksi

- Ilmaisee, että jäsenyyden saattaa olla on käsiin. 

Tässä ohjeaiheessa tutustutaan yksityiskohtaiset yleiskatsaus käytettävissä tyyppisiä riskin tapahtumia.


## <a name="leaked-credentials"></a>Vuodetun tunnistetiedot

Vuodetun tunnistetiedot löytyy lähettänyt julkisesti Tumma web Microsoftin security tutkijoiden. Näitä tunnistetietoja sijaitsevat yleensä vain teksti. Tarkistetaan Azure AD-tunnistetiedot, ja jos vastine löytyy, ne ovat valmiiksi "Leaked tunnistetietoja-tunnistetiedot.

Vuotamaan riskin tapahtumat luokitellaan "Suuri" vakavuus riskin tapahtuman tunnistetiedot, koska ne on, että käyttäjänimi ja salasana ovat käytettävissä luvaton Poista merkintä.

## <a name="impossible-travel-to-atypical-locations"></a>Mahdotonta matka, jotka sijainteihin

Tämä riski tapahtumalaji tunnistaa kaksi kirjautumiset peräisin maantieteellisesti kaukana sijainneista, jossa on vähintään yksi sijainnit voidaan myös jotka käyttäjälle, annettu aiempia toiminta. Lisäksi kaksi merkki-laajennukset välisen ajan on lyhyempi kuin se otettava matkustaa ensimmäisen sijainnista, joka ilmaisee, että toinen käyttäjä käyttää samoja tunnistetietoja toisen käyttäjän. 

Tämän tietokoneen learning algoritmin, joka ohittaa ilmeisimmät "*tunnistettujen*" osallistuvat mahdotonta matka-ehtoa, kuten VPN-yhteydet ja sijainnit säännöllisesti käyttämä organisaation muille käyttäjille.  Järjestelmässä on 14 päivän ajalta se oppii uuden käyttäjän kirjautuminen toiminta ensimmäisen learning-ajanjakson.

Mahdotonta matka on yleensä hyvä ilmaisin, joka hakkeri voitiin kirjautuminen onnistuneesti. EPÄTOSI positiivisten voi kuitenkin ilmetä käyttäjän matkoilla käyttämällä uutta laitetta tai käyttämällä VPN-yhteyttä, joka ei yleensä käytetä organisaation muiden käyttäjien. EPÄTOSI-positiivisten toisesta lähteestä on sovellukset, jotka siirtävät virheellisesti asiakkaaksi IP-osoitteet, jotka voivat ulkoasun palvelimen IP-osoitteet, kirjaudu apuohjelmien ottaen paikka, jossa sovellus on taustatietokantaan tietojen Centeristä nykyisessä (usein nämä ovat Microsoft palvelinkeskusten, jotka voivat Kirjaudu apuohjelmien ottaen ulkoasua sijoittaa Microsoftilta, jotka omistan IP-osoitteet). Nämä false-positiivisten riskien tapahtuman riski-tason on "**Normaali**".

##<a name="sign-ins-from-infected-devices"></a>Kirjaudu apuohjelmien tartunnan laitteilla

Tämä riski tapahtumalaji tunnistaa Kirjaudu apuohjelmien haittaohjelman, joissa laitteilla, joiden tiedetään vaihtamaan aktiivisesti robotti-palvelimen kanssa. Tämä määräytyy hajautettuna IP-osoitteiden käyttäjän laitteen vastaan IP-osoitteet, jotka olivat robotti palvelimen kanssa. 

Riskin tapahtuman tunnistaa IP-osoitteet, ei Käyttäjälaitteet. Jos useita laitteita ovat takana yksittäinen IP-osoite ja jotkin ovat hallitsemat robotti verkkoon, kirjaudu apuohjelmien muilla laitteilla Omat käynnistimen tapahtuman tarpeettoman, joka on syytä luokittelussa riskin tapahtuman nimellä "**Pieni**".  

Suosittelemme, että Ota käyttäjä ja tarkista käyttäjän kaikkia laitteita. On myös mahdollista, että käyttäjän Omat laitteen tartunnan tai kuten edellä mainittiin, että joku muu käytti tartunnan laite, sama IP-osoite-käyttäjänä. Tartunnan laitteita ovat usein tartunnan haittaohjelman, joka ei ole vielä määritetty Anti-Virus-ohjelmisto ja voi myös ilmaista kuin virheelliset käyttäjän käyttöä, jotka saattavat aiheuttaa laitteen muuttuvat tartunnan mukaan.

Saat lisätietoja osoite haittaohjelman ehkäiseminen [Haittaohjelman Protection Center](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409).


## <a name="sign-ins-from-anonymous-ip-addresses"></a>Kirjaudu apuohjelmien anonyymi IP-osoitteista

Riskin tapahtuman tällaista määrittää käyttäjät, jotka olet kirjautunut sisään-IP-osoite, joka on määritetty anonyymi välityspalvelimen IP-osoite. Nämä välityspalvelimet käyttävät henkilöt, jotka haluat piilottaa niiden laitteen IP-osoite ja voidaan käyttää väärinkäyttömahdollisuuksien takia.

On suositeltavaa, että käyttäjän vahvistamiseksi, jos he käyttäisivät anonyymien IP-osoitteiden heti yhteyttä. Tämä riski tapahtumalaji riski-tason ei "**Medium**", koska yksinään anonyymi IP ei ole vahva merkintä tilin vuotamiseen.

## <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Kirjaudu apuohjelmien IP-osoitteista epäilyttävistä aktiviteettiin

Tämä riski tapahtumalaji tunnistaa IP-osoitteet, josta on suuri määrä epäonnistui-kirjautuminen yritykset on nähnyt, yli useita käyttäjätilejä lyhyen ajan kuluessa. Tämä vastaa liikenne kuviot hyökkääjät käyttämä IP-osoitteiden ja on vahva ilmaisin, että tilit on jo tai aiot olla käsiin. Tämä on konepohjaisten oppimistekniikoiden algoritmin, joka ohittaa ilmeisimmät "*false positiivisten*", kuten IP-osoite, joita muut käyttäjät organisaatiossa käytetään säännöllisesti.  Järjestelmässä on 14 päivän alkuperäinen learning kauden oppii kohtaa, johon uusi käyttäjä ja uuden vuokraajan kirjautumisen toiminnan.

Suosittelemme, että Ota käyttäjä vahvistamiseksi, jos ne todella kirjautunut IP-osoitteesta, jotka on merkitty epäilyttäväksi. Tämä tapahtumalaji riski-tason on "**Medium**", koska on useita, mikä saattaa olla aikataulusta sama IP-osoite joidenkin saattaa olla vastuussa epäilyttävistä tehtävästä. 


## <a name="sign-in-from-unfamiliar-locations"></a>Kirjaudu sisään tuntemattomia sijainneista

Tämä riski tapahtumatyyppi on reaaliaikainen kirjautumisen arviointi-toimintoa, joka katsoo aiempia sijainnit-kirjautuminen (IP, leveyttä / pituusasteet ja ASN) määrittää uuden / tuntemattomia sijainnit. Järjestelmä käyttää käyttäjän kansioista tietoja ja katsoo "tuttuja" paikoista. Riski käynnistyy silloinkin, kun kirjautuminen-ilmenee sijainnista, joka ei ole jo tuttuja luettelosta. Järjestelmässä on alkuperäinen learning-ajan 14 päivän ajan, jolloin se ei merkitse uusia sijainteja tuntemattomia sijaintiin. Järjestelmä ohittaa myös Kirjaudu apuohjelmien tuttuja laitteet ja sijainneista, jotka ovat maantieteellisesti lähellä tuttuja sijainti. <br>
Tuntemattomia sijainnit tarjota vahva osoittaa, että luvaton pystyy yrittää käyttää sitä tunnistetiedot. EPÄTOSI-positiivisten voi ilmetä, kun käyttäjä on matkalla, kokeilla uutta laitetta tai käyttää uuden VPN-yhteyttä. Nämä tunnistettujen tämän tapahtumalaji riski-tason on "**Normaali**".


## <a name="azure-ad-anomalous-activity-reports"></a>Azure AD erheellisiin tehtävän raportit

Jotkin riskin näitä tapahtumia on saatavana Azure AD erheellisiin käyttöraporttien Azure-portaalissa. Seuraavassa taulukossa on lueteltu riskin tapahtuman eri ja vastaavan **Azure AD erheellisiin toimintojen** -raportti. Microsoft jatketaan sijoittamaan tämän tilan ja suunnitelmien jatkuvasti parantaa tunnistuksen tarkkuutta, riskien olemassa olevat tapahtumat ja Lisää uusi riski tapahtumatyypit kanssa jatkuvasti. 



| Käyttäjätietojen suojauksen riskin tapahtumalaji | Vastaavat Azure AD erheellisiin toimintojen raportti |
| :-- | :-- |
| Vuodetun tunnistetiedot    | Käyttäjät, joilla on vuodetun tunnistetiedot |
| Mahdotonta matka, jotka sijainteihin | Epäsäännöllinen kirjautumisen tehtävä |
| Kirjaudu apuohjelmien tartunnan laitteilla    | Kirjaudu apuohjelmien mahdollisesti tartunnan laitteilla |
| Kirjaudu apuohjelmien anonyymi IP-osoitteista  | Kirjaudu apuohjelmien tuntemattomista lähteistä |
| Kirjaudu apuohjelmien IP-osoitteista epäilyttävistä aktiviteettiin | Kirjaudu apuohjelmien IP-osoitteista epäilyttävistä aktiviteettiin |
| Etumerkki-tuntemattomia sijainneista    | - |
| Lukituksen tapahtumat    | - |

Seuraavat Azure AD erheellisiin tehtävän raportit eivät sisälly kuin Azure AD-tunnistetiedot tapahtumat riski ja ei tästä syystä ole käytettävissä – tunnistetiedot. Näiden raporttien ovat edelleen käytettävissä Azure-portaalissa kuitenkin ne on poistettu jonkin aikaa myöhemmin, kun ne ovat riskin tapahtumat tunnistetiedot korvattu.

- Kirjaudu apuohjelmien useita virheitä jälkeen
- Kirjaudu apuohjelmien useita paikkojen kohteesta


## <a name="see-also"></a>Katso myös

- [Azure Active Directory-tunnistetietojen suojaus](active-directory-identityprotection.md)


