#<a name="how-to-create-a-custom-virtual-machine"></a>Voit luoda mukautetun Virtual Machine

*Mukautetun* virtual machine viittaa virtual koneen luot **-Valikoiman** menetelmällä, koska sen avulla voit määrittää Lisää asetuksia kuin **Nopea luominen** -menetelmää. Nämä asetukset ovat:

- Lisää vaihtoehtoja kuvan luomiseen virtuaalikoneen (AM)
- Yhteyden muodostaminen näennäisen verkkoon AM
- Lisääminen AM aiemmin pilvipalveluun
- Lisääminen AM käytettävyyden määrittäminen

> [AZURE.IMPORTANT] Voit halutessasi virtuaalikoneen käyttämistä virtual verkossa, jotta voit muodostaa siihen suoraan isäntänimi tai paikallisen-yhteyksien määrittäminen Varmista, että voit määrittää virtual verkkoon, kun luot virtuaalikoneen. Virtual machine voi määrittää liittymään virtual verkon vain, kun luot virtuaalikoneen. Saat lisätietoja virtual verkkojen [Azure Virtual yleistä](http://go.microsoft.com/fwlink/p/?LinkID=294063).

1. Kirjautuminen [Azure portal](http://manage.windowsazure.com).

2. Valitse **Uusi**komentopalkista.

3. Valitse **Laske**ja valitse **virtuaalikoneen** **-Valikoimassa**.

4. Valitse kuva, jota haluat käyttää, ja Jatka napsauttamalla nuolta.

5. Jos kuva on useita versioita ovat käytettävissä- **Version Julkaisupäivä**, valitse versio, jota haluat käyttää.

6. Kirjoita nimi, jota haluat käyttää virtuaalikoneen **Virtuaalikoneen nimi**.

7. Valitse haluamasi koon virtuaalikoneen **taso** ja **koko** avulla. Voit valita koko vaikuttaa virtuaalikoneen sekä hinnoittelua suurin määrityksiä. Määritys on artikkelissa [virtuaalikoneen ja Azure Cloud palvelun koot](http://go.microsoft.com/fwlink/p/?LinkID=389844).

8. Kirjoita **Uuden käyttäjänimi**, jonka haluat avulla voit hallita palvelimen järjestelmänvalvojan tilin nimi.

9. Kirjoita **Uusi salasana**, järjestelmänvalvojan tilin vahva salasana. Sama salasana uudelleen **Vahvista salasana**.

10. Napsauta nuolta, jos haluat jatkaa.

11. **Pilvipalvelussa**tee jompikumpi seuraavista:

    - Jos tämä on ensimmäinen tai vain virtuaalikoneen pilvipalvelussa, valitse **Luo uusi pilvipalvelussa**. Kirjoita **Cloud palvelun DNS nimi**nimi, joka käyttää 3 – 24 pieniä kirjaimia ja numeroita. Tämä nimi tulee osaksi URI, jota käytetään yhteystiedot virtuaalikoneen cloud-palvelun kautta.
    - Jos virtual tämän tietokoneen lisätään pilvipalveluun, valitse se luettelosta.

    > [AZURE.NOTE] Lisätietoja sijoittaminen näennäiskoneiden saman pilvipalvelussa Katso, [miten muodostaa näennäiskoneiden pilvipalvelussa](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/).

12. Valitse **Alue/affiniteetti ryhmän/Virtual verkko**-alue, affiniteetti ryhmän tai virtual verkon, jota haluat käyttää virtuaalikoneen. Lisätietoja affiniteetti ryhmät-kohdassa [tietoja affiniteetti Groups Virtual verkossa](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

13. Valitse käytössä olevan tallennustilan tilin Näennäiskiintolevytiedosto **Tallennustilan tili**, tai käyttää automaattisesti luotu tallennustilan tilin. Vain yksi tallennustilan tilin alueittain luodaan automaattisesti. Kaikkien muiden näennäiskoneiden, jonka luot tämä asetus on ohjeaiheessa tallennustilan tähän tiliin. Sinun on oikeutettu enintään 20 tallennustilan tilit.

14. Voit halutessasi virtuaalikoneen kuuluu käytettävyys-joukko, **Saatavuus**, valitse **Luo saatavuus** tai lisätä sen käytettävyys luotua tietojoukkoa.

    **Huomautus**: käytettävyys määrittäminen näennäiskoneiden on otettu käyttöön eri vika toimialueeseen. Useita näennäiskoneiden sijoittaminen saatavuus määrittäminen auttaa varmistamaan, että sovellus on käytettävissä verkon virheet, paikalliseen levyasemaan laitteiston virheet ja minkä tahansa suunnitellun käyttökatkot aikana.

15.  Valitse **päätepisteet**Tarkista, jotka sallivat virtuaalikoneen, kuten etätyöpöydän kautta tai suojattu runko (SSH)-asiakasohjelman luodaan uusi päätepisteet. Voit myös lisätä päätepisteet nyt tai luo ne myöhemmin. Ohjeita luominen myöhemmin Katso, [miten voit määrittää määrittäminen päätepisteet Virtual tietokoneeseen](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

16.  **AM Agent**-kohdassa päättää, asenna AM-agentti. Tämä agentti on ympäristö, voit asentaa tunnisteet, joiden avulla voit olla vuorovaikutuksessa virtuaalikoneen kanssa. Lisätietoja on artikkelissa [Hallitse tunnisteet](http://go.microsoft.com/FWLink/p/?LinkID=390493).

17. Voit luoda virtuaalikoneen nuolta.

    ![Mukautetun virtuaalikoneen luonti onnistui](./media/howto-custom-create-vm/VMSuccessWindows.png)

##<a name="next-steps"></a>Seuraavat vaiheet##
Kun virtuaalikoneen on luotu, se käynnistyy automaattisesti. Kun portaalin näkyvät tilan käytön, voit kirjautua virtuaalikoneen. Lisätietoja on seuraavissa artikkeleissa:

- [Miten Virtual koneeseen, jossa käytetään Linux Kirjaudu](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)
- [Miten Kirjaudu Virtual-Machine, joissa on käytössä Windows Server](../articles/virtual-machines/virtual-machines-windows-classic-connect-logon.md)

