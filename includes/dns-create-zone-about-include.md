DNS-vyöhyke käytetään isännöimiseen tietyn toimialueen DNS-tietueet. Jotta voit aloittaa isännöinnin toimialueen, voit on luotava DNS-vyöhyke. Kaikki tietyn toimialueen ohjelma luo DNS-tietue on toimialueen DNS-vyöhykkeen sisällä. 

Esimerkiksi "contoso.com-toimialueen voi sisältää useita DNS-tietueiden, kuten"mail.contoso.com"(for postin palvelin) ja"www.contoso.com"(sivustolle). 


## <a name="names"></a>Lisätietoja DNS-vyöhyke nimet
 
- Alueen nimen on oltava yksilöllinen resurssiryhmän ja vyöhykkeen ei saa olla jo. Muussa tapauksessa toiminto epäonnistuu.

- Alueen nimi voidaan käyttää uudelleen eri resurssiryhmä-tai eri Azure-tilauksen. 

- Jos useita alueita, joilla on sama nimi, jokaiselle esiintymälle määritetään eri nimipalvelimen osoitteet ja vain yksi on delegoinut ylemmän tason toimialue. Lisätietoja on artikkelissa [edustajan Azure DNS toimialue](../articles/dns/dns-domain-delegation.md).