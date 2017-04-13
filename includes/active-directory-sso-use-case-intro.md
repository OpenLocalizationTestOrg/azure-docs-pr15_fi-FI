Organisaatioiden on käytössä [ohjelmiston serviceksi (hyväksymislautakunnan)](https://azure.microsoft.com/overview/what-is-saas/) sovelluksiin tuottavuutta koska cloud tekniikan ja -työkalujen on tulossa useammin. Kun SaaS sovellusten määrä kasvaa, on hankalaa järjestelmänvalvojien tilien hallinta ja käyttöoikeudet ja niiden eri salasanojen käyttäjille. Näiden sovellusten hallinta yksitellen Luo ylimääräisiä työmäärän ja ei ole yhtä turvallinen.


- Työntekijät, jotka on useita salasanoja seurantaan yleensä menetelmillä pienempi suojatun muista ne joko kirjoittaa salasanat alaspäin tai salasanojen saman monta tilien välillä.

- Kun uuden työntekijän saapuu tai jokin jättää, niiden tilien valmisteltu yksitellen tai poistaa valmistelun yhteydessä.

- Lisäksi työntekijät voivat käytön aloittaminen SaaS sovellusten työssään avaamatta IT, mikä tarkoittaa, että he luovat oman tilit järjestelmiin, joissa IT-järjestelmänvalvojille on vielä hyväksytty ja eivät ole seuranta.  

Kaikki näihin haasteisiin voi kertakirjautuminen (SSO). Hallitse useita sovelluksia ja tarjota käyttäjille yhdenmukaisia Sign käytettävyyttä helpoin tapa on. Azure Active Directory (Azure AD) tarjoaa tehokkaat SSO-ratkaisun ja on monta käytettävissä olevat ennalta integroitujen sovellusten, opetusohjelmat järjestelmänvalvojille nopeasti uuden sovelluksen määrittäminen ja Käynnistä valmistelu käyttäjien kanssa.


## <a name="how-does-azure-active-directory-integrate-apps"></a>Miten Azure Active Directory integroida sovelluksia?  

Azure AD voit integroida sovellukset ja valmistellun tilit. Tämä voidaan tehdä jommankumman kahdesta tavoista kautta.

- Jos sovellus on valmiiksi integroitu valikoima-sovelluksessa, voit siirtyä, jota portaalin asetuksia sallimaan SSO ja sovellusten kautta. Minkä tahansa valikoima-sovelluksen, voit aloittaa mukaan noudattamalla yksinkertainen vaiheittaisia ohjeita noudattaen esittää sovelluksen valikoiman ja Azure-portaalissa voit ottaa käyttöön kertakirjautumisen.

- Jos sovellusta ei ole valikoimassa, voit silti määrittää useimpien sovellusten Azure AD mukautetun sovelluksen nimellä. Tämä edellyttää hieman enemmän tekniset osaamisalueet määrittämiseen. Voit lisätä jokin sovellus, joka tukee SAML 2.0 kuin liitetyt app tai sovellus, jossa on HTML-pohjaisia kirjautumissivu kuin salasana SSO-sovellus.

Tapauksessa, jossa käyttäjät luonut SaaS sovellukset, jotka eivät ole hallitsee Omat tilit IT-ratkaisu [Cloud App etsintä](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) -työkalu. Tämä työkalu valvoo web-liikenne paikalliseen tunnistaa, mitkä sovellukset ovat käytössä koko organisaation sekä kunkin niiden käyttäjien lukumäärä. SE käyttää näitä tietoja kerrotaan, mitä sovelluksia käyttäjille mieluummin ja mitkä integroida Azure AD SSO varten.  

Kun integroit sovelluksen Azure AD, voit yhdistää käyttäjien aiemmin luotuja sovelluksen käyttäjätiedoista niiden vastaaviin Azure AD-käyttäjätietoja.  
