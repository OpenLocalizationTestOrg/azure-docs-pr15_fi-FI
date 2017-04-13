## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Perinteinen VNet, käyttämällä Azure CLI luominen

Azure-CLI avulla voit hallita Azure resursseja komentoriviltä, miltä tahansa tietokoneelta, joka on käynnissä Windows, Linux tai OS x. Voit luoda VNet Azure-CLI, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../articles/xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.
2. Voit luoda VNet ja aliverkon, **azure verkon vnet luominen** -komennon suorittaminen alla kuvatulla tavalla. Luettelon, kun tulos kerrotaan käytetyt parametrit.

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    Odotettu tulos:

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **--vnet**. Luoda VNet nimi. Tässä skenaariossa *TestVNet*
    - **-e (tai--osoitetilaa varten)**. VNet osoitetilaa varten. Tässä skenaariossa *192.168.0.0*
    - **-i (tai - cidr)**. Verkkopeite CIDR-muodossa. Tässä skenaariossa *16*.
    - **- n (tai--aliverkon nimi**). Ensimmäinen aliverkon nimi. Tässä tapauksessa *FrontEnd*.
    - **-p (tai--aliverkon Käynnistä ip-)**. Aloitetaan aliverkon tai aliverkon osoitetilaa IP-osoite. Tässä skenaariossa *192.168.1.0*.
    - **-r (tai--aliverkon cidr)**. Verkkopeite aliverkon CIDR-muodossa. Tässä skenaariossa *24*.
    - **-l (tai--sijainti)**. Azure alue, jossa VNet luodaan. Tutustu skenaarion *Keskitetyn US*.

3. Suorita **azure verkon vnet aliverkon luominen** -komento luo aliverkon alla kuvatulla tavalla. Luettelon, kun tulos kerrotaan käytetyt parametrit.

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    Näin Odotettu tulos yllä-komennon:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (tai--vnet nimi**. Jos aliverkon luodaan VNet nimi. Tässä tapauksessa *TestVNet*.
    - **-n (tai--nimi)**. Uusi aliverkon nimi. Tässä skenaariossa *Taustajärjestelmä*.
    - **-(tai--osoitteen etuliite)**. Aliverkon CIDR estä. Neljä Microsoftin tapaus, *192.168.2.0/24*.

4. Uusi vnet ominaisuuksien tarkasteleminen **azure verkon vnet Näytä** -komennon suorittaminen alla kuvatulla tavalla.

            azure network vnet show

    Näin Odotettu tulos yllä-komennon:

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK
