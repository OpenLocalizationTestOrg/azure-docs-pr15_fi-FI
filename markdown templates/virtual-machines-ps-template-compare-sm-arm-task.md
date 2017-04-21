<!--save a copy of this file to your local repo. It's important that you follow the naming conventions by starting with the service name, and use lowercase only for the file name. See "file-names-and-locations.md" under the "contributor-guide" folder in your repo.

Info to help you use the template are enclosed in the Markdown comments using the caret, hyphen, dash syntax. Delete these from your file.

Text not wrapped in comment syntax is intended to be used as is, or with updates enclosed in [  ]. Add the info and delete the bracket. 

Pay attention to spacing and indents. They affect formatting. 

--> 

<!--replace this with Properties and Tags sections. These are required sections. See "article-metadata.md" in under the "contributor-guide" folder in your repo. Attributes in each section can be placed on separate lines to make them easier to read and check-->

# <a name="use-azure-powershell-to-task"></a>Azure-PowerShellin avulla [tehtävä]

Tässä artikkelissa näytetään, miten [tehtävän] Azure moduulin ja Azure Resurssienhallinta-moduuli-komennoilla. Tämä on tarkoitettu, joiden avulla opit uusien komentojen sekä siirtää aiemmin komentosarjat uusien komentojen.

## <a name="prerequisite-install-a-recent-version-of-azure-powershell"></a>Edellyttää: Viimeisimmät Azure PowerShell-Version asentaminen

Jos et ole jo tehnyt niin, asenna vähintään PowerShellin Azure [versionumero]-versio paikallisessa tietokoneessa. Jos käytät aiemmalla versiolla, sillä ei ole kuvattu tämän artikkelin Azure resurssien hallinnan cmdlet-komennot. Lisätietoja on artikkelissa:
 
- [Asentaminen ja määrittäminen PowerShellin Azure](install-configure-powershell.md) PowerShellin Azure määrittämisestä.
- [Windows PowerShellin resurssien hallinnan](powershell-azure-resource-manager.md) resurssien hallinnan käytön perusteet.

> [AZURE.NOTE] Useimmat tehtävistä edellyttää, että käytät järjestelmänvalvojan PowerShellin Azure komentorivi-ikkuna.

## <a name="command-comparison"></a>Komennon vertailu

Tämä [taulukon | osan] näkyy komennon syntaksi.

<!--[optional image - to use an image in this article, add a folder with the same name as the article file name without extension, inside the Media folder of the repo. Use only this folder to store the images. Don't attempt to use a common folder to share images you want to use in more than 1 file.]
Then, use the following syntax to add a reference to the image in your article:
![](./media/name-of-file-without-extension/image-name-no-spaces.png)
-->

<!--if a command string uses variables, define the variables first, using the  following construction. In some cases the variable is straightforward and doesn't need much explanation. But parameters such as location and size can benefit from brief explanation or listing all accepted values:--> 

Komennon näissä esimerkeissä käytetään seuraavia muuttujia:

$FriendlyName"<Describe value>"

<!-- if it makes more sense to present this in a table, use this. Otherwise, delete. The table won't render until it's in Github or published to Sandbox.-->

Hallinta | Resurssien hallinta
---|----
`syntax` | `syntax`


<!--if it makes more sense to present this one command block after the other instead of a table, use this. Otherwise, delete-->
  
[Lyhyt esittely lauseen tietoja-komennosta. Ohita, jos ei ole varmasti sanoa. Mutta jos se käyttää tällaisia tavoista putkijohto, selitetään,]:

    [command string]

## <a name="script-examples"></a>Komentosarjaesimerkkejä

Tässä on esimerkki, jossa käytetään [nimet cmdlet-komento)] [tehtävään]. Se sisältää komennot, jotka:

- [lyhyt verbin, käyttää, on, on jne]
- [seuraava lyhyt verbin] 

<!--include this statement if it uses variables that weren't introduced earlier-->Se sisältää seuraavat muuttujat:

- [1 muuttuja]
- [muuttuja 2]

<!--This shows you how a recent example was presented as well as how it was formatted. Preceding each line with one tab or four spaces to format in a code block-->

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
    
    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"
    
    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4
    
    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching
    
    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="additional-resources"></a>Lisäresursseja
<!--At a minimum, include a link back to the migration task list article. Use the formats shown below. See create-links-markdown.md for more info -->
<!--use this format for links to other articles, such as the migration task list. -->
[Käytettävyyden hallinta](virtual-machines-windows-manage-availability.md)

<!--To link to an ACOM page outside the /documentation/ subdomain (such as a pricing page, SLA page or anything else that is not a documentation article), use an absolute URL, but omit the locale:

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)-->

<!--use this for URLs outside of ACOM. Be sure to locale, and if you're linking to the Azure library on MSDN, include the '/azure/' part of the URL-->
[Näennäiskoneiden dokumentaatio](https://msdn.microsoft.com/library/azure/jj156003.aspx)

