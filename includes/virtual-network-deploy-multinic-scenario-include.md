## <a name="scenario"></a>Skenaario

Tämä asiakirja käy läpi, joka käyttää useita NIC VMs tietyn tilanne käyttöönotto. Tässä skenaariossa on kaksitasoista IaaS työmäärä ylläpidettävä Azure. Kunkin tason on otettu käyttöön omassa aliverkon virtual verkon (VNet). Edusta-taso sisältää useita web-palvelimet-ryhmitelty kuormituksen, suuren käytettävyyden määrittäminen. Uudelleen taso koostuu useita tietokantapalvelimiin. Tietokannan seuraavissa palvelimissa otetaan käyttöön kaksi NIC kunkin tietokannan käyttämiseen, muut hallinnan kanssa. Skenaario sisältää myös verkon käyttöoikeusryhmät (NSGs) voit määrittää, mitä liikenne on sallittua kunkin aliverkon ja NIC ympäristön. Alla olevassa kuvassa tässä skenaariossa basic arkkitehtuuri.  

![MultiNIC-skenaario](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

