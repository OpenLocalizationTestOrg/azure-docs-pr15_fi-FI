<properties
    pageTitle="DHCPv6 määrittäminen Linux VMs | Microsoft Azure"
    description="Voit määrittää DHCPv6 Linux VMs."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6: ta, azure kuormituksen, kahden pinon, julkiseen ip, alkuperäisen ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="configuring-dhcpv6-for-linux-vms"></a>Linux VMs DHCPv6 määrittäminen

Joissakin Linux virtuaalikoneen Azure Marketplacesta ei ole määritetty oletusarvoisesti DHCPv6. IPv6-tuki DHCPv6 on määritettävä sisällä, jota käytät Linux OS-jakauman. Eri Linux jaot on määrittämisestä DHCPv6, koska ne käyttävät erilaisia käyttötapoja.

>[AZURE.NOTE] Viimeisimmät Azure Marketplacesta SUSE Linux ja CoreOS kuvat ole esimääritettyjä DHCPv6 kanssa. Muita muutoksia ei tarvita, kun käytät kuvat.

Tässä asiakirjassa kerrotaan, miten käyttöön DHCPv6 niin, että Linux virtuaalikoneen hakee IPv6-osoite.

>[AZURE.WARNING] Verkkotiedostojen muokkaamiseen väärin voi aiheuttaa menetät verkkokäyttö oman AM. On suositeltavaa testata tuotannon järjestelmiin määritysten muutokset. Tässä artikkelissa annettuja ohjeita on testattu Linux kuvat Azure Marketplacesta uusinta versiota. Oppaasta tietyn version Linux tarkempia ohjeita.

## <a name="ubuntu"></a>Ubuntu

1. Muokkaa tiedostoa `/etc/dhcp/dhclient6.conf` ja lisää seuraava rivi:

        timeout 10;

2. Muokkaa verkon määritysten eth0-liittymän, jonka asetukset ovat seuraavat:

    * **Ubuntu 12.04 ja 14.04**Muokkaa tiedostoa`/etc/network/interfaces.d/eth0.cfg`
    * Muokkaa tiedostoa **Ubuntu 16.04**`/etc/network/interfaces.d/50-cloud-init.cfg`

    ```bash
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. Uusiminen IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Muokkaa tiedostoa `/etc/dhcp/dhclient6.conf` ja lisää seuraava rivi:

        timeout 10;

2. Muokkaa tiedostoa `/etc/network/interfaces` ja lisää seuraavat määritykset:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Uusiminen IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Muokkaa tiedostoa `/etc/sysconfig/network` ja lisää seuraava parametri:

        NETWORKING_IPV6=yes

2. Muokkaa tiedostoa `/etc/sysconfig/network-scripts/ifcfg-eth0` ja lisää seuraavat kaksi parametria:

        IPV6INIT=yes
        DHCPV6C=yes

3. Uusiminen IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Viimeisimmät Azure SLES ja openSUSE kuvat ovat olleet esimääritettyjä DHCPv6 kanssa. Muita muutoksia ei tarvita, kun käytät kuvat. Jos sinulla AM vanhat tai mukautettuja SUSE kuvan perusteella, käytä seuraavia ohjeita:

1. Asenna `dhcp-client` pakata, tarvittaessa:

    ```bash
    # sudo zypper install dhcp-client
    ```

2. Muokkaa tiedostoa `/etc/sysconfig/network/ifcfg-eth0` ja lisää seuraava parametri:

        DHCLIENT6_MODE='managed'

3. Uusiminen IPv6-osoite:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 ja openSUSE harppaus

Viimeisimmät Azure SLES ja openSUSE kuvat ovat olleet esimääritettyjä DHCPv6 kanssa. Muita muutoksia ei tarvita, kun käytät kuvat. Jos sinulla AM vanhat tai mukautettuja SUSE kuvan perusteella, käytä seuraavia ohjeita:

1. Muokkaa tiedostoa `/etc/sysconfig/network/ifcfg-eth0` ja korvaa tämä parametri

        #BOOTPROTO='dhcp4'

    Seuraava arvo:

        BOOTPROTO='dhcp'

2. Lisää seuraavat parametri `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Uusiminen IPv6-osoite:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Azure viimeisimmät CoreOS kuvat ovat olleet esimääritettyjä DHCPv6 kanssa. Muita muutoksia ei tarvita, kun käytät kuvat. Jos sinulla AM vanhat tai mukautettuja CoreOS kuvan perusteella, käytä seuraavia ohjeita:

1. Muokkaa tiedostoa`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Uusiminen IPv6-osoite:

    ```bash
    # sudo systemctl restart systemd-networkd
    ```
