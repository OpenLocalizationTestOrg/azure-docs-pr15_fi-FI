## <a name="what-is-reverse-dns"></a>Käänteinen DNS-ominaisuudet

Tavanomaisen DNS-tietueiden käyttöön yhdistämismäärityksen DNS-nimi (kuten www.contoso.com) IP-osoite (esimerkiksi 64.4.6.100).  Käänteinen DNS-palvelun avulla takaisin nimi ('www.contoso.com') IP-osoite (64.4.6.100) käännös.

Käänteinen DNS-tietueet voi käyttää erilaisia tilanteita. Käänteinen DNS-tietueet käytetään esimerkiksi laajasti ensimmäisessä Roskaposti vahvistamalla sähköpostiviestin lähettäjälle.  Sähköpostin vastaanottava palvelin noutaa käänteinen DNS-tietueen lähettävä palvelin IP-osoite ja varmista Jos isännöivien on oikeus lähettää sähköpostia alkuperäisen toimialueen. (Huomaa kuitenkin, että [Azure Laske palvelut eivät tue lähettämisen sähköpostit lähetetään ulkoisista toimialueista](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).)

## <a name="how-reverse-dns-works"></a>Miten käänteinen DNS toimii

Käänteinen DNS-tietueet isännöidään määräten DNS-alueet, nimeltään 'ARPA' vyöhykkeisiin.  Vyöhykkeiden lomakkeen eri DNS-hierarkiassa rinnakkain isännöinnin toimialueet, kuten "contoso.com" Normaali hierarkiaan.

Esimerkiksi DNS record 'www.contoso.com' on toteutettu käyttämällä "A" DNS-tietuetta, jonka nimi "www" alue "contoso.com".  A-tietue osoittaa vastaava IP-osoite, tässä tapauksessa 64.4.6.100.  Käänteinen haku on toteutettu erikseen käyttämällä 'PTR-tietue nimeltä "100" vyöhykkeellä "6.4.64.in-addr.arpa" (Huomaa, että IP-osoitteiden palautetaan ARPA vyöhykkeisiin.)  Tämä PTR-tietue, jos se on määritetty oikein, osoittaa nimi "www.contoso.com".

Kun organisaatio on määritetty IP-Osoitelohko, ne on hankittava oikeuden hallita vastaavan ARPA vyöhykkeen. Vastaavat IP-osoitelohkot Azure käyttämä ARPA vyöhykkeet isännöidään ja niitä hallitsee Microsoft. Palveluntarjoajalta voi sisältää ARPA vyöhykkeen IP-osoitteiden tai ehkä salli isännöi DNS-palvelun valittua, kuten Azure DNS ARPA vyöhykkeelle.

>[AZURE.NOTE] DNS-nimihaut ja käänteinen DNS-hakuja otettu käyttöön erillinen, rinnakkainen DNS hierarkioita. Käänteinen haku, "www.contoso.com" **ei** hallinnoida "contoso.com"-vyöhykkeessä, mieluummin sitä isännöidään vastaavan IP-Osoitelohko ARPA-vyöhykkeellä.

Saat lisätietoja käänteinen DNS [Käänteinen DNS-haku](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).

## <a name="azure-support-for-reverse-dns"></a>Azure käänteinen DNS-tuki

Azure tukee kaksi eri skenaarioita koskevat käänteinen DNS:

1. Isännöidä ARPA vyöhykkeen vastaavat IP-Osoitelohko.
2. Salliminen, voit määrittää käänteinen DNS-tietue Azure-palveluun määritetty IP-osoite.

Tukemaan entisen, Azure DNS avulla voidaan ylläpitää ARPA vyöhykkeet ja hallita kunkin käänteinen DNS-haku PTR-tietueet.  Prosessin ARPA vyöhykkeen luominen ja määrittäminen delegoinnin määrittäminen PTR-tietueet on sama kuin tavallinen DNS-vyöhykkeet.  Vain erot ovat delegoinnin on määritettävä kautta Palveluntarjoajan DNS-rekisteröintipalvelu sijaan, että vain PTR tietuetyypin voidaan käyttää.

Jälkimmäinen tukemaan Azure avulla voit määrittää käänteinen haku kohdistetaan palvelun IP-osoitteet.  Azure määritetty tämä käänteisen vastaavan ARPA vyöhykkeen PTR tietueena.  Microsoft isännöidään ARPA vyöhykkeiden, vastaava kaikki käyttämä Azure-IP-alueita. **Jäljempänä tässä artikkelissa kuvataan Tämä skenaario yksityiskohtaiset tiedot.**
