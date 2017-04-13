- Virtuaalinen verkot voivat olla saman tai toisen Azure alueilla (sijainti).

- Pilvipalveluun tai kuormituksen päätepisteen ei kattavat näennäinen verkkojen, vaikka ne ovat yhteydessä toisiinsa.

- Useiden Azure virtual verkostojen yhdistäminen toisiinsa ei tarvita minkä tahansa paikallisen VPN-yhdyskäytäviä, ellei paikallisen-yhteys, jota tarvitaan.

- VNet VNet tukee virtual verkkojen yhdistämistä. Se ei tue yhdistäviä näennäiskoneiden tai cloud Services-palvelujen ei virtual verkon.

- VNet VNet vaatii Azure VPN yhdyskäytävien RouteBased (aiemmalta nimeltään dynaaminen reititys) VPN tyyppejä. 

- VPN-yhteyden kanssa voidaan käyttää samanaikaisesti usean sivuston VPN-yhteydet, jossa on enintään 10 (oletus/Standard yhdyskäytävät) tai 30 (suuri suorituskyvyn yhdyskäytävät) VPN tunneloi joko virtual verkkojen muodostaa VPN VPN yhdyskäytävän tai paikalliseen sivustot.

- Osoite-välilyöntejä virtual verkkojen ja paikallisen lähiverkon sivustojen on päällekkäin. Päällekkäiset osoite välilyöntejä aiheuttaa epäonnistuu VNet VNet yhteyksien luomista.

- Tarpeettomien tunneleita virtual verkkojen kahdet välillä ei tueta.

- Kaikki VPN-tunneleita virtual verkoston jakaa kaistanleveyttä Azure VPN-yhdyskäytävän ja saman VPN-yhdyskäytävän käytettävyyttä SLA Azure-tietokannassa.

- VNet VNet liikenne siirtyy Microsoft Network ei ole Internetin kautta.

- VNet VNet liikenne samalla alueella on ilmainen molempiin suuntiin; ristiviittauksen alueen VNet VNet juniin liikenne veloitetaan lähde-alueiden perusteella lähtevä muun-VNet tietojen siirto kurssit. Tutustu [hinnat sivun](https://azure.microsoft.com/pricing/details/vpn-gateway/) lisätietoja.