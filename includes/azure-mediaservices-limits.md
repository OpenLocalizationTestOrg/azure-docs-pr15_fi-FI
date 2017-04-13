Resurssi|Oletusarvoinen enimmäismäärä|Enimmäismäärä
---|---|---
Yhden tilauksen Azure Media Services (AMS)-tilit||25
Resurssien AMS tiliä kohti||1,000,000<sup>1</sup>
Ketjutetut tehtäviä projektia kohti||30
Resurssien tehtävää kohden||50
Resursseja projektia kohti||100
Töiden AMS tiliä kohti ||50 000<sup>2</sup>
Yksilöivä locators liittyvä sijoituksen samalla kertaa||5<sup>4</sup>
Live kanavien AMS tili </p></td>|5</p></td>|Puuttuu-<sup>1</sup>
Sovellus lakkasi tilaan kanavaa kohden </p></td>|50</p></td>|Puuttuu-<sup>1</sup>
Ohjelmien käytössä tilan kanavaa kohden </p></td>|3</p></td>|3
Streaming päätepisteet käynnissä tilan AMS tiliä kohti</p></td>|2</p></td>|Puuttuu-<sup>1</sup>
Streaming streaming päätepisteen yksiköt </p></td>|10 </p></td>|Puuttuu-<sup>1</sup>
Koodauksen yksiköt AMS tiliä kohti </p></td>|25</p></td>|Puuttuu-<sup>1</sup>
Tallennustilan tilit | |1 000<sup>5</sup>
Käytännöt || 1,000,000<sup>6</sup>

<sup>1</sup> voit pyytää päivittämään tämän kiintiön rajat avaamalla tuki lippu. Älä luo Suurenna rajoitukset, lähettää sen sijaan tuki lippu AMS tilejä.

<sup>2</sup> numerossa jonossa, valmis, aktiivinen ja peruutettuja töitä. Se ei ole poistettu työt. Voit poistaa vanhan työt **IJob.Delete** tai **poistaa** HTTP-pyynnön.

<sup>3</sup> tehtäessä pyyntö luettelon työn kohteiden enimmäismäärä on 1 000 palautetaan pyynnön kohden. Jos tarvitset kaikki lähetetyn työt seurantaan, voit käyttää ylä-tai Ohita kuvatulla tavalla [OData järjestelmän Kyselyasetukset](http://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> locators ei ole suunniteltu hallintaan käyttäjäkohtainen käyttöoikeuksien hallinta. Jos haluat antaa eri yksittäisten käyttäjien käyttöoikeuksia, käyttää ratkaisuja, digitaalisten oikeuksien hallinta (DRM).

<sup>5</sup> tallennustilan tilit on oltava sama Azure tilauksesta.

<sup>6</sup> ei rajoitettu 1,000,000 käytäntöjen eri AMS käytäntöjen (esimerkiksi Locator käytännön tai ContentKeyAuthorizationPolicy). Käytä samaa käytännön tunnus, jos aina käyttävät samaa päivää / käyttöoikeudet / jne.
