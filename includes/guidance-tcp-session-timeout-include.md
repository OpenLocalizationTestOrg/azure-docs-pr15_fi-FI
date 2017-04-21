##<a name="tcp-settings-for-azure-vms"></a>Azure VMs TCP-asetukset

Azure VMs yhteydenpito julkinen Internet käyttämällä [NAT] [ nat] (NAT). NAT-laitteiden määrittäminen julkiseen IP-osoite ja portin Azure-AM, sallimisen, AM muodostaa socket viestintään muiden laitteiden kanssa. Jos paketit lopettaa juoksutus kautta, socket tietyn ajan kuluttua, NAT-laitteen keskeyttää yhdistäminen ja socket on ilmainen käytettävän muiden VMs.

Tämä on yleisiä NAT-ominaisuus, joka voi aiheuttaa tietoliikenne mukaan TCP-sovelluksia, jotka toiminta socket Ylläpidettävän aikakatkaisun päättymisen jälkeen. On kaksi aikakatkaisun asetusten huomioitavia istuntojen *yhteys* tilaan:

- **saapuvien** kautta [Azure kuormituksen][azure-lb-timeout]. Aikakatkaisu oletusarvo on 4 minuuttia ja voidaan säätää 30 minuuttia.
- **Lähtevän** käyttämällä [SNAT] [ snat] (lähteen NAT). Aikakatkaisu on määritetty 4 minuuttia ja voidaan muuttaa.

Jotta yhteydet eivät säily lisäksi aikakatkaisu-rajoitus, olisi varmistat sovelluksesi säilyttää istunnon toiminnassa tai voit määrittää pohjana käyttöjärjestelmän tekemään niin. Käytettävät asetukset määräytyvät Linux- ja Windows-järjestelmiä alla kuvatulla tavalla.

Saat [Linux][linux], sinun on muutettava ydin muuttujat alla.
NET.IPv4.tcp_keepalive_time = 120 net.ipv4.tcp_keepalive_intvl = 30 net.ipv4.tcp_keepalive_probes = 8
 
[Windowsin][windows], sinun on muutettava alla rekisteriarvoja.
KeepAliveInterval = 30 KeepAliveTime = 120 TcpMaxDataRetransmissions = 8


Yllä asetukset Varmista Säilytä toiminnassa paketin lähetetään joutoaika 2 minuuttia (120 sekuntia) jälkeen ja lähettää 30 sekunnin välein. Ja jos 8 näiden pakettien epäonnistuu, istunnon poistetaan.

<!-- links -->
[nat]: http://computer.howstuffworks.com/nat.htm
[snat]: ../load-balancer/load-balancer-overview.md/#source-nat
[linux]: http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
[windows]: http://blogs.technet.com/b/nettracer/archive/2010/06/03/things-that-you-may-want-to-know-about-tcp-keepalives.aspx
[azure-lb-timeout]: ../load-balancer/load-balancer-tcp-idle-timeout.md