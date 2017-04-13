<properties
   pageTitle="Tietojen suojaus ja salaus parhaat käytännöt | Microsoft Azure"
   description="Tässä artikkelissa on joukko tietojen suojauksen parhaat käytännöt ja salauksen käyttämällä valmista Azure ominaisuuksia."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yuridio"/>

#<a name="azure-data-security-and-encryption-best-practices"></a>Azure tietojen suojaus ja salaus parhaista käytännöistä

Yksi tietosuojaa pilveen näppäimet accounting, jossa tietoja voi ilmetä, ja mitä valvontatoimia on saatavilla valtion mahdolliset tilat. Azure tietojen varten suojaus ja salaus parhaita käytäntöjä suositukset on ympärille hyötyä seuraavat tiedot:

- Milloin kuin rest: Tämä sisältää kaikki tiedot tallennustilan objekteja, säilöjä ja tyypit, jotka ovat olemassa nimipalvelimet fyysinen tietovälineessä, on se Magneettinen tai optisen levy.

- Valitse siirrossa: Kun tietoja siirretään välillä osia, sijainteja tai ohjelmat, kuten verkon yli service-bus (mistä paikalliseen cloud ja päinvastoin, mukaan lukien hybrid yhteyksiä, kuten ExpressRoute) tai i /-aikana, se on ajatella siten, että liikkeessä.

Tässä artikkelissa käsittelemme Azure tietojen suojaus ja salaus toimintaohjeita kokoelma. Näiden vinkkien johdettuja Microsoftin kokemus Azure tietojen suojaus ja salaus ja asiakkaat itse kokemukset.

Parhaita käytäntöjä, kerrotaan:

- Mikä on paras käytäntö
- Miksi haluat ottaa käyttöön kyseisen parhaiden käytäntöjen mukaisesti
- Mitä voi olla tulos, jos et ota parhaita käytäntöjä
- Paras käytäntö mahdollista vaihtoehtoja
- Miten voit lukea käyttöön parhaat käytännöt

Tässä artikkelissa Azure tietojen suojaus ja salaus parhaat käytännöt perustuu yksimielisyyttä-mielestä ja Azure ympäristö ominaisuudet ja määrittää ominaisuus, tässä artikkelissa on kirjoitettu milloin olemassa. Lausuntoja ja -tekniikoiden muuttuvat ajan kuluessa ja sisältö päivitetään säännöllisesti muutosten mukaisiksi.

Azure tietojen suojaus ja salaus parhaita käytäntöjä tässä artikkelissa kuvatut ovat seuraavat:

- Pakottaa monimenetelmäisen todentamisen
- Käytä Roolipohjainen käyttöoikeuksien valvonta (RBAC)
- Salaa Azure-virtuaalikoneissa
- Laitteiston suojauksen mallien käyttäminen
- Suojatun työasemien ja hallinta
- SQL-salauksen ottaminen käyttöön
- Siirron tietojen suojaaminen
- Säilytä tiedoston tason tietojen salaus


## <a name="enforce-multi-factor-authentication"></a>Pakottaa monimenetelmäisen todentamisen

Tietojen käyttö ensimmäisessä vaiheessa ja Microsoft Azure-asetus on todennetaan. [Azure multi-factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md) on menetelmä käyttäjän tunnistetietojen vahvistamisen jollakin muulla tavalla kuin vain käyttäjätunnusta ja salasanaa. Tämän todennus menetelmä auttaa suojaavat käyttämään tietoja ja sovelluksia käyttäjän tarve-kirjautuminen yksinkertaista kokouksen aikana.

Ottamalla Azure MFA käyttäjiä lisätään toisen layer Security käyttäjän kirjautumiset ja tapahtumia. Tässä tapauksessa tapahtuman voi käyttää asiakirjan, joka sijaitsee tiedostopalvelimessa tai SharePoint-Online. Azure MFA avulla voit pienentää todennäköisyys, että vaarantuneen tunnistetieto on organisaation tietojen käytön IT.

Esimerkki: Jos viite Azure MFA-käyttäjien ja määritä se käyttää tekstiviesti tai puhelu tarkistus, jos käyttäjän tunnistetietojen käsiin, hän voi käyttää mitä tahansa resursseja, koska hän ei voi käyttää käyttäjän puhelimeen. Organisaatioissa, joissa ei lisätä ylimääräisen kerroksen tunnistetietojen suojauksen on helpompi tunnistetiedon varkaus hyökätä, mikä voi aiheuttaa tietojen haavoittuvan.

Organisaatioissa, joissa haluat säilyttää käyttöoikeuksien hallinta paikallisen yksi vaihtoehto on käyttää [Azure multi-factor Authentication Server](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), kutsutaan myös MFA paikallisen. Tällä menetelmällä voit silti saa oikeuden pakottaa monimenetelmäisen todentamisen ja säilyttää MFA palvelimen paikallisen.

Lisätietoja Azure MFA Lue artikkeli, [Azure Monimenetelmäisen todentamisen pilveen käytön aloittaminen](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Käytä Roolipohjainen käyttöoikeuksien valvonta (RBAC)
Rajoita käyttöoikeuksia [on tunnettava](https://en.wikipedia.org/wiki/Need_to_know) ja [käyttöoikeuksien myöntäminen](https://en.wikipedia.org/wiki/Principle_of_least_privilege) suojauksen periaatteiden perusteella. Tämä on välttämättömien organisaatioissa, joissa haluat käyttää tietojen käytön suojauskäytäntöjä. Azure Roolipohjainen käytön hallinta (RBAC) voidaan määrittää käyttäjät, ryhmät ja tiettyjen käyttöalue sovellusten käyttöoikeudet. Roolien osoitus laajuus voi olla tilauksen, resurssiryhmä tai yksittäinen resurssi.

Määritä oikeuksia käyttäjille Azure [valmiin RBAC roolien](../active-directory/role-based-access-built-in-roles.md) avulla voidaan hyödyntää. Kannattaa käyttää *Tallennustilan tilin avustaja* cloud operaattoreita, jotka on hallittava tallennustilan asiakkaat ja *Perinteinen tallennustilan tilin osallistujan* rooli perinteinen tallennustilan tilien hallinta. Harkitse cloud operaattoreita, joka on hallittava VMs ja tallennustilaa tilin lisääminen *Virtuaalikoneen osallistujan* rooli.

Organisaatioissa, joissa Älä pakota tietojen käyttöoikeuksien valvonta mukaan hyödyntäminen ominaisuudet, kuten RBAC saattaa antaa enemmän kuin on tarpeen niiden käyttäjien oikeuksia. Tämä voi aiheuttaa tietojen haavoittuvan että joillakin käyttäjillä kirjautumisoikeuden tiedot, jotka heillä ei kannata ensiksi.

Voit lukea lisää Azure RBAC lukemalla artikkelin [Azure Role-Based käyttöoikeuksien hallinta](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Salaa Azure-Virtuaalikoneissa
Monissa organisaatioissa [salauksen rest-palvelussa](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) on pakollinen askel tietojen tietosuoja ja vaatimustenmukaisuus tietoja paikallisen tietosuojan. Azure salauksen avulla järjestelmänvalvojat voivat salata Windows ja Linux IaaS virtuaalikoneen (AM) levyjä. Azure salauksen hyödyntää alan BitLocker-standardin ominaisuus, Windowsin ja Linux säätää äänenvoimakkuutta salaus käyttöjärjestelmän ja tietoja-levyjä DM Crypt-ominaisuus.

Voit hyödyntää Azure levyn salausta suojaaminen ja organisaation suojaus ja vaatimukset täyttävän tietoja. Organisaatioiden huomioon myös minimoida riskejä luvattoman liittyvien tietojen käytön salauksen avulla. On myös suositeltavaa salata asemat ennen kirjoittaminen luottamuksellisia tietoja niihin.

Varmista, että salaamiseen oman AM tietomääriä ja käynnistyksen voimakkuutta haluat suojata loput Azure-tallennustilan tilin tiedot. Suojata tietoja ja salausavaimet mukaan hyödyntäminen [Azure avaimen säilö](../key-vault/key-vault-whatis.md).

Paikallisen Windows-palvelinten huomioon seuraavat salaus parhaat käytännöt:

- [BitLockerin salauksen](https://technet.microsoft.com/library/dn306081.aspx)
- Tallenna tiedot AD DS: ään.
- Jos kaikki huolta, että BitLocker näppäimet on ollut käsiin, on suositeltavaa asema, voit poistaa kaikki esiintymät BitLocker metatietojen asemasta joko muotoileminen tai salauksen ja salata koko asemaan uudelleen.

Organisaatioissa, joissa Älä pakota salauksen on todennäköisesti voi luovuttaa tietojen eheyteen liittyvien ongelmien, kuten haittaohjelmien tai päivittämättömien käyttäjän varastamasta tietoja ja poistaa muotoilun tietojen luvattoman pääsyn tilit käsiin. Lisäksi näitä riskejä yrityksille, jotka on alan sääntöjen on osoitettava, että ne ovat turvallisemman ja käytät oikeaa turvaominaisuudet parantaa tietojen suojaaminen.

Voit lukea lisää Azure salauksen lukemalla artikkelin [Azure levyn salaus for Windowsista tai Linux IaaS VMs](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Käytä laitteiston suojauksen moduulit

Teollisuuden salausratkaisuja käyttämällä salainen näppäimiä salaamaan tiedot. Tämän vuoksi on ehdottoman painettavat näppäimet turvallisesti tallennettuna. Hallintaa on olennainen osa tietojen suojaaminen, koska se hyödyntää tallentamiseen salainen näppäimet, joita käytetään salaamaan tiedot.

Azure salauksen käyttää [Azure avaimen säilö](https://azure.microsoft.com/services/key-vault/) avulla voit hallita ja levyn salausavaimet ja tietoja avaimen säilö tilaukseesi, ja varmistaa, että, että kaikki tiedot virtuaalikoneen kohtauksen salataan loput Azure-tallennustilan hallinta. Voit valvoa näppäimet ja käytännön käyttö käytettävä Azure avaimen säilö.

On monia luontaisen riskejä suojaaminen salainen näppäimet, joita on käytetty tietojen salaamiseen paikallaan ilman tarvittavat turvaominaisuudet. Jos hyökkääjät on salainen näppäimet, hän saa oikeuden salaus tietoja ja käyttää mahdollisesti luottamuksellisten tietojen.

Lue lisää yleisiä suosituksia varmenteen hallinta Azure-tietokannassa lukemalla artikkelin [varmenteen hallinta Azure-tietokannassa: tehtävät ja vältettävät asiat](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Lisätietoja Azure avaimen säilö lukea [Azure avaimen säilö käytön aloittaminen](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Suojatun työasemien ja hallinta

Suurin osa kalastelu kohteen peruskäyttäjän, koska päätepisteen tulee jokin hyökkäyksen ensisijainen pisteitä. Jos luvaton hakujärjestelmän päätepisteen, hän voidaan hyödyntää käyttäjän tunnistetiedot saat käyttöösi organisaation tiedot. Useimmat päätepisteen kalastelu voivat hyödyntää siitä, että käyttäjät ovat niiden paikallisen työasemien järjestelmänvalvojat.

Voit pienentää suojatun hallinta työasemassa käyttämällä näitä riskejä. On suositeltavaa, että voit pienentää-työasemien [Sellaisten Access työasemien (PAW)](https://technet.microsoft.com/library/mt634654.aspx) avulla. Nämä suojatun hallinta-työasemien voi auttaa pienentämään joitakin näistä kalastelu auttaa varmistamaan, että tiedot ovat suojaaminen. Varmista, että tiukentamista ja työaseman lukita PAW avulla. Tämä on tärkeä vaihe antamaan osallistujista vahvistukset luottamuksellisia tilien, tehtävien ja tietojen suojaaminen.

Endpoint protection puuttuminen voi tietojen vastuullasi, varmista, että Pakota suojauskäytännöt kaikissa laitteissa, joita käytetään tarjoaman tietoja, vaikka tietojen sijainti (cloud tai paikalliseen) kautta.

Voit lukea lisätietoja luottamuksellisten käyttäminen työasemassa lukemalla artikkelin [Suojaaminen sellaisten Access](https://technet.microsoft.com/library/mt631194.aspx).

## <a name="enable-sql-data-encryption"></a>SQL-salauksen ottaminen käyttöön

[Läpinäkyvä salauksen Azure SQL-tietokanta](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) auttaa suojaamaan tietokonetta haittaohjelmien toiminnan suorittamalla tarvitsematta sovelluksen muutokset reaaliaikainen salausta ja salauksen tietokannan, liitetty varmuuskopiot ja tapahtuman lokitiedostojen rest-palvelussa.  TDE salaa koko tietokannan tallennustilaan kutsutaan tietokannan salausavaimen symmetrisen avaimen avulla.

Vaikka koko tallennustila on salattu, on erittäin tärkeää myös salata tietokannan itse. Tämä on toteutus linnaa syvyys vaiheelta tietojen suojaamisesta. Jos käytät [Azure SQL-tietokantaan](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) ja haluat suojata luottamuksellisia tietoja, kuten luottokorttia tai henkilötunnukset, voit salata tietokantojen FIPS 140-2 vahvistettu 256 bittistä AES-salausta joka monta yleisesti käytettyjen (esimerkiksi HIPAA, PCI) vaatimusten mukainen.

On tärkeää ymmärtää [puskurin resurssivarantoon tunniste](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) liittyviä tiedostoja ei ole salattu tietokanta on salattu TDE. Sinun on käytettävä tiedoston järjestelmän tason salaus työkaluja, kuten BitLocker tai [Salaaminen File System](https://technet.microsoft.com/library/cc700811.aspx) (EFS) BPE liittyviä tiedostoja.

Valtuutettu käyttäjä jälkeen voit käyttää suojaus-järjestelmänvalvojaan tai tietokannan ylläpitäjältä tietoja vaikka tietokanta on salattu TDE, kuten myös noudattamalla seuraavia suosituksia:

- Tietokannan tasolla SQL-todennus
- Azure AD-todennuksen avulla RBAC roolit
- Käyttäjien ja sovellusten tulisi käyttää eri tilien todennusta. Näin voit rajoittaa käyttäjien ja sovellusten käyttöoikeudet ja pienentää haittaohjelmien toiminnan
- Ota käyttöön tietokannan suojauksen käyttämällä pysyvät tietokannan roolit (esimerkiksi db_datareader tai db_datawriter) tai voit luoda mukautettuja rooleja sovelluksesi valittujen tietokantaobjektien eksplisiittinen käyttöoikeuksia

Organisaatioissa, joissa ei käytetä tietokannan tason salaus voi olla johtuu usein kalastelu, jotka voivat saada tiedot sijaitsevat SQL-tietokantoja varten.

Voit lukea lisää SQL TDE salaus lukemalla artikkelin [Läpinäkyvä salauksen Azure SQL-tietokantaan](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Siirron tietojen suojaaminen

Siirron tietojen suojaaminen pitäisi olla olennainen osa yrityksen tietojen suojauksen määrittäminen. Tiedot siirtäminen edestakaisin monta sijainneista, Yleiset suositus on, että käytät SSL/TLS-protokollat aina vaihtaa tietoja eri sijainneissa yli. Joissakin tilanteissa haluat ehkä eristää koko tietoliikenneyhteyden paikallisen ja cloud välillä infrastruktuurin näennäisen yksityisverkon (VPN) avulla.

Tietojen siirtäminen paikalliseen infrastruktuuri ja Azure välillä kannattaa harkita tarvittavat valvontaa, kuten HTTPS tai VPN-yhteyttä.

Organisaatioissa, joissa on useita työasemia käytöltä suojaamiseen sijaitsevat paikallisen Azure, voit käyttää [Azure sivusto sivusto VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Organisaatioissa, joissa on suojattu käytöltä yhden työasema, joka sijaitsee paikallisen Azure, käytä [piste-,-sivusto VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Kuten [ExpressRoute](https://azure.microsoft.com/services/expressroute/)linkki suurempi tietojen joukot voidaan siirtää erillinen nopea WAN päälle. Jos haluat käyttää ExpressRoute, voi myös salata tiedot sovelluksen-tasolla [SSL/TLS](https://support.microsoft.com/kb/257591) -tai muut protokollat lisätty suojaus.

Jos ovat käyttäminen Azuren tallennustilaan palvelun Azure-portaalissa, kaikki tapahtumat toteutuvat HTTPS. [Tallennustilan REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx) HTTPS-protokollan välityksellä voidaan myös toimia [Azuren tallennustilaan](https://azure.microsoft.com/services/storage/) ja [Azure SQL-tietokantaan](https://azure.microsoft.com/services/sql-database/).

Organisaatiot, jotka eivät läpäise tietojen siirrossa on helpompi [- tietojenvaihto](https://technet.microsoft.com/library/gg195821.aspx), [salakuuntelulta](https://technet.microsoft.com/library/gg195641.aspx) ja istunnon kaappaaminen. Nämä kalastelu voi olla luottamuksellisia tietoja päässyt ensimmäisessä vaiheessa.

Voit lukea lisää Azure VPN-vaihtoehto lukemalla artikkelin [suunnittelu- ja VPN-yhdyskäytävän rakenne](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Säilytä tiedoston tason tietojen salaus

Suojaus, jotka voivat lisätä suojaustasoa tiedoillesi uuden kerroksen salaa tiedosto itse riippumatta tiedostosijainti.

[Azure RIGHTS](https://technet.microsoft.com/library/jj585026.aspx) käytetään salausta, käyttäjätiedot ja luvan suojata tiedostojasi ja sähköpostin. Azure RMS toimii eri useita laitteita, puhelimissa, tableteissa ja PC suojaamalla organisaatiossa sekä oman organisaation ulkopuolelle. Tämä ominaisuus on mahdollista, koska Azure RIGHTS Lisää suojan, joka sisältää kopioitavat tiedot pysyy myös silloin, kun se jättää organisaation rajat.

Azure RIGHTS avulla voit suojata tiedostojasi, kun käytät yleisesti salaus [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html)täyden tuen kanssa. Azure RIGHTS hyödyntää tietojen suojaamisesta, kun sinulla on varmistaa, että suojauksen pysyy tiedostoon, vaikka se kopioidaan tallennustilan, jota ei ole valvonnassa IT, kuten pilvitallennuspalveluun. Sama toteutuu tiedosto on suojattu liitteenä sähköpostiviestin, jossa on ohjeet sähköpostitse, jaettujen tiedostojen avaaminen suojatun liitettä.

Kun suunnittelet Azure RIGHTS hyväksyttäväksi Suosittelemme seuraavasti:

- Asenna [RMS jakaminen sovelluksen](https://technet.microsoft.com/library/dn339006.aspx). Sovelluksen integroitu Office-sovellusten asentamalla Office-apuohjelma niin, että käyttäjät voivat helposti suojata tiedostoja suoraan.
- Sovellusten ja palveluiden tukemaan Azure RIGHTS asetuksia
- Voit luoda [mukautettuja malleja](https://technet.microsoft.com/library/dn642472.aspx) , jotka vastaavat oman yrityksesi tarpeita. Esimerkki: yläreunan salainen tiedot, joita käytetään kaikkien yläreunan salaisuus mallin liittyvät sähköpostit.

Organisaatioissa, joissa on heikko [tietojen luokittelu](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) ja tiedoston suojaus voi olla tietoja vuoto johtuu usein. Ilman oikea tiedostonsuojaus-organisaatiot voi hankkia business tiedot, valvoa väärinkäyttö ja estää haittaohjelmien tiedostot.

Lue lisätietoja Azure RIGHTS lukemalla on artikkelissa [Azure Rights Management käytön aloittaminen](https://technet.microsoft.com/library/jj585016.aspx).
