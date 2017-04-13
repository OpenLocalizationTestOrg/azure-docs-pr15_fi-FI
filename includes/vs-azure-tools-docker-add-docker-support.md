1. Visual Studio **Ratkaisunhallinnassa**projektin hiiren kakkospainikkeella ja valitse **Lisää > Docker tuki** pikavalikosta.

    ![Lisää Docker tuki-pikavalikko](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. ASP.NET-Core Docker tuen lisääminen web project hakutulosten useita Docker liittyvät tiedostot lisätään projektiin, mukaan lukien Docker laatia tiedostoja, käyttöönoton Windows PowerShell-komentosarjojen ja Docker ominaisuuden tiedostoja lisäksi. 

    ![Docker-tiedostoja, jotka on lisätty projektiin](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE]Jos käytät [Windows Docker beetaversio](https://beta.docker.com), Avaa Properties\Docker.props, Poista oletusarvo ja Käynnistä Visual Studio-arvoksi tulee voimaan.
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=='' "></DockerMachineName>
> ```
