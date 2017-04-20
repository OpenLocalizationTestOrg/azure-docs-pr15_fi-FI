<properties
   pageTitle="SAP-Windows näennäiskoneiden (VMs) – suunnittelu ja käyttöönotto-oppaassa NetWeaver | Microsoft Azure"
   description="SAP NetWeaver Windows virtual tietokoneissa (VMs) – suunnittelu ja käyttöönotto-opas"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# <a name="sap-netweaver-on-windows-virtual-machines-vms--planning-and-implementation-guide"></a>SAP NetWeaver Windows virtual tietokoneissa (VMs) – suunnittelu ja käyttöönotto-opas

[767598]:https://service.sap.com/sap/support/notes/767598
[773830]:https://service.sap.com/sap/support/notes/773830
[826037]:https://service.sap.com/sap/support/notes/826037
[965908]:https://service.sap.com/sap/support/notes/965908
[1031096]:https://service.sap.com/sap/support/notes/1031096
[1139904]:https://service.sap.com/sap/support/notes/1139904
[1173395]:https://service.sap.com/sap/support/notes/1173395
[1245200]:https://service.sap.com/sap/support/notes/1245200
[1409604]:https://service.sap.com/sap/support/notes/1409604
[1558958]:https://service.sap.com/sap/support/notes/1558958
[1585981]:https://service.sap.com/sap/support/notes/1585981
[1588316]:https://service.sap.com/sap/support/notes/1588316
[1590719]:https://service.sap.com/sap/support/notes/1590719
[1597355]:https://service.sap.com/sap/support/notes/1597355
[1605680]:https://service.sap.com/sap/support/notes/1605680
[1619720]:https://service.sap.com/sap/support/notes/1619720
[1619726]:https://service.sap.com/sap/support/notes/1619726
[1619967]:https://service.sap.com/sap/support/notes/1619967
[1750510]:https://service.sap.com/sap/support/notes/1750510
[1752266]:https://service.sap.com/sap/support/notes/1752266
[1757924]:https://service.sap.com/sap/support/notes/1757924
[1757928]:https://service.sap.com/sap/support/notes/1757928
[1758182]:https://service.sap.com/sap/support/notes/1758182
[1758496]:https://service.sap.com/sap/support/notes/1758496
[1772688]:https://service.sap.com/sap/support/notes/1772688
[1814258]:https://service.sap.com/sap/support/notes/1814258
[1882376]:https://service.sap.com/sap/support/notes/1882376
[1909114]:https://service.sap.com/sap/support/notes/1909114
[1922555]:https://service.sap.com/sap/support/notes/1922555
[1928533]:https://service.sap.com/sap/support/notes/1928533
[1941500]:https://service.sap.com/sap/support/notes/1941500
[1956005]:https://service.sap.com/sap/support/notes/1956005
[1973241]:https://service.sap.com/sap/support/notes/1973241
[1984787]:https://service.sap.com/sap/support/notes/1984787
[1999351]:https://service.sap.com/sap/support/notes/1999351
[2002167]:https://service.sap.com/sap/support/notes/2002167
[2015553]:https://service.sap.com/sap/support/notes/2015553
[2039619]:https://service.sap.com/sap/support/notes/2039619
[2121797]:https://service.sap.com/sap/support/notes/2121797
[2134316]:https://service.sap.com/sap/support/notes/2134316
[2178632]:https://service.sap.com/sap/support/notes/2178632
[2191498]:https://service.sap.com/sap/support/notes/2191498
[2233094]:https://service.sap.com/sap/support/notes/2233094
[2243692]:https://service.sap.com/sap/support/notes/2243692

[azure-cli]:../xplat-cli-install.md
[azure-portal]:https://portal.azure.com
[azure-ps]:../powershell-install-configure.md
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../azure-subscription-service-limits.md#subscription

[dbms-guide]:virtual-machines-windows-sap-dbms-guide.md (Windows näennäiskoneiden (VMs) – DBMS käyttöönotto-oppaassa SAP-NetWeaver) [dbms-guide-2.1]:virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (välimuisti VMs ja näennäiskiintolevyjen) [dbms-guide-2.2]:virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (ohjelmiston RAID) [dbms-guide-2.3]:virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azuren tallennustilaan) [dbms-guide-2]:virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (rakenteen RDBMS käyttöönoton) [dbms-guide-3]:virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (suuri käytettävyys ja palauttaminen ja Azure VMs) [dbms-guide-5.5.1]:virtual-machines-windows-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 ja sitä uudemmissa versioissa) [dbms-guide-5.5.2]:virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 ja aikaisempien versioiden) [dbms-guide-5.6]:virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (SQL Server-kuvien käyttämisestä ulos Microsoft Azure Marketplacesta) [dbms-guide-5.8]:virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (Yleinen SQL Server Azure yhteenveto-SAP: n) [dbms-guide-5]:virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (yksityiskohtia SQL Server RDBMS) [dbms-guide-8.4.1]:virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (tallennustilan määritys) [dbms-guide-8.4.2]:virtual-machines-windows-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (varmuuskopioiminen ja palauttaminen) [dbms-guide-8.4.3]:virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (suorituskyvyn Huomioitavaa varmuuskopiointi ja palauttaminen) [dbms-guide-8.4.4]:virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (muu) [dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-windows-sap-deployment-guide.md (Windows näennäiskoneiden (VMs) – käyttöönotto-oppaassa SAP-NetWeaver) [deployment-guide-2.2]:virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP resursseja) [deployment-guide-3.1.2]:virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (käyttöönotto mukautetun kuvan AM) [deployment-guide-3.2]:virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (tapaus 1: käyttöönotto ulos SAP Azure Marketplacesta AM) [deployment-guide-3.3]:virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (tapaus 2: käyttöönotto AM mukautetun kuvan SAP: n) [deployment-guide-3.4]:virtual-machines-windows-sap-deployment-guide.md# a9a60133-a763-4de8-8986-ac0fa33aa8c1 (tapaus 3: siirrettäessä AM paikallisen Näennäiskiintolevyn Azure-generalized käyttäminen SAP) [deployment-guide-3]:virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (käyttöönoton tilanteita, joissa on VMs Microsoft Azure-SAP: n) [deployment-guide-4.1]:virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (käyttöönotto Azure PowerShell cmdlet-komennot) [deployment-guide-4.2]:virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Lataa ja tuo SAP asiaa PowerShell cmdlet-komennot) [deployment-guide-4.3]:virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (liity AM paikallisen toimialueelle - vain Windows) [deployment-guide-4.4.2]:virtual-machines-windows-sap-deployment-guide.md# 6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [deployment-guide-4.4]:virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (lataaminen, asennus ja ota Azure AM agentti) [deployment-guide-4.5.1]:virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (PowerShellin Azure) [deployment-guide-4.5.2]:virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI) [deployment-guide-4.5]:virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Määritä Azure parannetun seuranta-laajennus SAP) [deployment-guide-5.1]:virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (valmiuden Tarkista Azure parannetun seurannasta SAP: n) [käyttöönotto-oppaan-5.2]: Virtual-Machines-Windows-SAP-Deployment-Guide.MD#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Azure seuranta infrastruktuurin määritysten kuntotarkistuksen) [deployment-guide-5.3]:virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (edelleen vianmääritys Azure seuranta-infrastruktuuria SAP: n)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:virtual-machines-windows-sap-high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:virtual-machines-windows-sap-planning-guide.md (Windows näennäiskoneiden (VMs) – suunnittelu ja käyttöönotto-oppaassa SAP-NetWeaver) [planning-guide-1.2]:virtual-machines-windows-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (resursseja) [planning-guide-11.4]:virtual-machines-windows-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (suuri käytettävyys (HA) ja tietojen palauttaminen (DR) käynnissä-Azuren näennäiskoneiden SAP NetWeaverin) [planning-guide-11.4.1]:virtual-machines-windows-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (suuren käytettävyyden SAP-sovelluksen palvelinten) [planning-guide-11.5]:virtual-machines-windows-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (SAP esiintymien avulla määrittää) [planning-guide-2.1]:virtual-machines-windows-sap-planning-guide.md# 1625df66-4cc6-4d60-9202-de8a0b77f803 (vain Pilvipalveluita - virtuaalikoneen ominaisuuksissa kyselyjä Azure ilman riippuvuuksien paikallisen asiakkaan verkossa) [planning-guide-2.2]:virtual-machines-windows-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (rajat tiloissa - käyttöönoton yhden tai useita SAP VMs kyselyjä Azure kanssa on täysin integroitu paikalliseen verkkoon vaatimus) [planning-guide-3.1]:virtual-machines-windows-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure alueet) [planning-guide-3.2.1]:virtual-machines-windows-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (vika toimialueita) [planning-guide-3.2.2]:virtual-machines-windows-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Päivitä toimialueita) [planning-guide-3.2.3]:virtual-machines-windows-sap-planning-guide.md#18810088- f9be - 4c 97-958a - 27996255c 665 (Azure käytettävyys sääntöjoukot) [planning-guide-3.2]:virtual-machines-windows-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Microsoft Azure virtuaalikoneen käsite) [planning-guide-3.3.2]:virtual-machines-windows-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium tallennus) [planning-guide-5.1.1]:virtual-machines-windows-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (siirtyvät AM paikallisen Azure-generalized levyllä) [planning-guide-5.1.2]:virtual-machines-windows-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (käyttöönotto asiakkaan tietyn kuvan AM) [planning-guide-5.2.1]:virtual-machines-windows-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (valmistelu AM siirtyä Azure kanssa paikallisen generalized levy) [planning-guide-5.2.2]:virtual-machines-windows-sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (valmistelu käyttöönoton AM asiakkaan jotakin tiettyä kuvaa ja SAP: n) [planning-guide-5.2]:virtual-machines-windows-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (SAP Azure kanssa valmistellaan VMs) [planning-guide-5.3.1]:virtual-machines-windows-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (ero välillä Azure levy ja Azure kuva) [planning-guide-5.3.2]:virtual-machines-windows-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (lataamisesta, Azure-paikallisen Näennäiskiintolevyn) [planning-guide-5.4.2]:virtual-machines-windows-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (kopioiminen levyjen Azure-tallennustilan tilien välillä) [suunnittelu-opas-5.5.1]: Virtual-Machines-Windows-SAP-Planning-Guide.MD#4efec401-91e0-40c0-8e64-f2dceadff646 (AM/Näennäiskiintolevyn rakenteen SAP-versioiden) [planning-guide-5.5.3]:virtual-machines-windows-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (asetus levyjen automount) [planning-guide-7.1]:virtual-machines-windows-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (yksi AM SAP NetWeaver esittely/koulutus skenaarion kanssa) [planning-guide-7]:virtual-machines-windows-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Cloud-Only käsitteitä käyttöönotosta SAP esiintymien) [planning-guide-9.1]:virtual-machines-windows-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure seuranta ratkaisu SAP) [planning-guide-azure-premium-storage]:virtual-machines-windows-sap-planning-guide.md# ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium tallennus)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:virtual-machines-windows-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-windows-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../resource-group-overview.md
[resource-groups-networking]:../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../storage/storage-premium-storage.md
[storage-redundancy]:../storage/storage-redundancy.md
[storage-scalability-targets]:../storage/storage-scalability-targets.md
[storage-use-azcopy]:../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:virtual-machines-linux-attach-disk-portal.md
[virtual-machines-windows-attach-disk-portal]:virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:virtual-machines-windows-create-vm-generalized.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]:virtual-machines-windows-create-vm-generalized.md
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:virtual-machines-windows-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

Microsoft Azure avulla yritykset voivat hankkia suorittaminen ja tallennustilaa resursseja ilman pitkiä hankintoja jaksot mahdollisimman vähän aikaa. Azuren näennäiskoneiden Salli yritykset voivat ottaa käyttöön klassinen-sovellukset, kuten SAP NetWeaver perusteella Azure sovellukset ja laajentaa niiden luotettavuutta ja käytettävyyttä tarvitsematta resurssien käytettävissä paikallisen. Azure virtuaalikoneen Services tukee myös paikallisen-yhteys, jonka avulla yritysten Azuren näennäiskoneiden integroida aktiivisesti niiden paikallisen toimialueet-yksityinen niiden Paveikslėlis ja niiden SAP-järjestelmää vaaka.
Tiedotteessa kuvataan perustiedot, Microsoft Azure Virtual Machine ja tarjoaa hallintapaketteihin, suunnittelu ja käyttöönotto SAP NetWeaver asennusten Azure-tietokannassa Huomioitavaa ja sellaisenaan pitäisi lukea ennen aloittamista SAP NetWeaver todellinen-versioiden Azure-asiakirja.
Paperin täydentää SAP asennusohjeet ja SAP muistiinpanoja, joka edustaa asennusten ja SAP-ohjelmisto-versioiden ensisijainen resurssit ympäristöissä annettu.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="summary"></a>Yhteenveto
Cloud tietojenkäsittely on yleisesti käytetty termi, joka on kasvun enemmän tärkeys sisällä IT, alaan pienten yritysten suuria ja kansainvälisissä yritysten ylöspäin.

Microsoft Azure on Microsoftin, joka tarjoaa useita eri uusi pikavalikon Cloud Services-ympäristön. Kukin asiakas on nyt nopeasti säännöstä ja jään säännöstä sovellusten palveluna pilvipalvelussa, voit, joten niitä ei rajoitettu teknisiä tai budjetointi rajoitukset. Sen sijaan, että investointien ajasta ja kyselyjä laitteiston infrastruktuuri, yritykset voit keskittyä sovellus, liiketoimintaprosessien ja sen edut asiakkaat ja käyttäjille.

Microsoft tarjoaa Microsoft Azure virtuaalikoneen Services-täydellinen infrastruktuurin kuin Service (IaaS)-ympäristössä. SAP NetWeaver mukaan sovelluksia tuetaan-Azuren näennäiskoneiden (IaaS). Tämä julkaisu kerrotaan, miten voit suunnitella ja toteuttaa Microsoft Azure SAP NetWeaver mukaan-sovelluksissa kuin valinta ympäristö.

Paperi itseensä keskitytään kaksi tärkeimmät ominaisuuksia:

* Ensimmäinen osa kerrotaan kaksi tuettu käyttöönotto kuviot SAP NetWeaver mukaan sovellusten Azure. Se myös kuvataan yleiset Azure käsittely ja SAP: N käyttöönottoa huomioon.
* Toinen osa on tiedot käyttöönoton ensimmäinen osa on kuvattu kaksi eri skenaarioissa.

Katso lisäresursseja luvun resurssien [suunnittelu-opas-1.2] Tässä asiakirjassa.

### <a name="definitions-upfront"></a>Määritelmien toimiston ulkopuolella
Koko asiakirjan Käytämme seuraavat ehdot:

* IaaS: Infrastruktuuri palveluna.
* PaaS: Ympäristö palveluna.
* SaaS: Ohjelmiston palveluna.
* ARM: Azure Resurssienhallinta
* SAP-osan: yksittäisen SAP sovelluksen esimerkiksi ECC, Mustavalkoinen, ratkaisun hallinta tai EP.  SAP-osat voivat perustua perinteinen ABAP tai Java-tekniikoiden tai ei-NetWeaver mukaan-sovellus, esimerkiksi Business objekteja.
* SAP-ympäristön: vähintään yksi SAP-osien loogisesti ryhmittää liiketoimintatoimintojen suorittamisen esimerkiksi kehitystä, QAS, koulutus, DR tai tuotannon.
* SAP-vaaka: Tämä koskee koko SAP-kohteita asiakkaan IT vaaka. SAP-vaaka sisältää kaikki tuotannon ja tuotannon ympäristössä.
* SAP-järjestelmää: DBMS kerroksen ja sovelluskerroksen, kuten SAP ERP kehitysympäristö ja SAP Mustavalkoinen testi järjestelmän, SAP CRM tuotannon järjestelmän jne yhdistelmä... Azure käyttöönotoissa se ei tue jakaa nämä kaksi tasoa paikallisen ja Azure välillä. Tämä tarkoittaa, että SAP-järjestelmä on joko käyttöön paikallisen tai se on otettu käyttöön Azure. Voit kuitenkin ottaa SAP-vaaka Azure tai paikallisen eri järjestelmiä. Voit esimerkiksi SAP CRM: N kehittämisketjun käyttöönotto ja esikatsella järjestelmien Azure, mutta SAP CRM tuotannon järjestelmän paikalliseen.
* Vain pilvipalveluita käyttöönoton: käyttöönoton, jossa Azure tilaus ei ole yhdistetty sivusto sivusto tai paikallisen verkkoinfrastruktuuria ExpressRoute-yhteyden kautta. Yhteiset Azure dokumentaatio tällaiset ominaisuuksissa on myös kuvailla vain Cloud-käyttöönotoissa. Käyttämällä tätä menetelmää käyttöön näennäiskoneiden käytetään internet- ja julkiseen ip-osoite ja/tai julkinen DNS-nimi myönnetyt VMs Azure-tietokannassa kautta. Microsoft Windowsin paikallisen Active Directory (AD) ja DNS ei ole laajennettu Azure tämäntyyppisten ominaisuuksissa. Näin ollen VMs eivät ole paikallisen Active Directory-osa. Sama koskee Linux käyttöotot käyttämällä esimerkiksi OpenLDAP + Kerberos.

> [AZURE.NOTE] Vain pilvipalveluita ominaisuuksissa tässä asiakirjassa on määritetty valmis SAP maisemien suoritetaan yksinomaan Azure Active Directory-laajennus ilman / OpenLDAP tai nimenselvitys-paikalliseen julkisen cloud kyselyjä. Vain pilvipalveluita määrityksiä ei tue tuotannon SAP-järjestelmiin tai määritykset, jossa SAP STM tai paikallisen muita resursseja on Azure ja resurssit, jotka asuvat paikallisen isännöimät SAP-järjestelmiin keskenään.

* Usean paikallisen: Kuvataan tilanne, jossa VMs otetaan käyttöön Azure tilaukseen, joka sisältää sivusto sivusto, usean sivuston tai ExpressRoute paikallisen datacenter(s) ja Azure väliset yhteydet. Yhteiset Azure asiakirjat, tällaiset ominaisuuksissa on myös kuvailla paikallisen-skenaarioita. Syy yhteys on Laajenna paikallisen toimialueen paikallisen Active Directory / OpenLDAP ja paikallisen DNS Azure kyselyjä. Paikallisen vaaka on laajennettu tilaus Azure varoja. Ottaa tämän tunnisteen, VMs voi olla osa paikallisen toimialueen. Toimialueen paikallisen toimialueen käyttäjät voivat käyttää palvelimet ja services voit suorittaa nämä VMs (kuten DBMS palvelut). Viestintä- ja nimi tarkkuus paikalliseen käyttöön VMs ja Azure käyttöön VMs välillä on mahdollista. Tämä on useimmissa SAP varat otettavan Odotamme skenaario.  Katso [Tämä] [ vpn-gateway-cross-premises-options] artikkelissa ja [tämän] [ vpn-gateway-site-to-site-create] lisätietoja.

> [AZURE.NOTE] SAP-järjestelmiin, jossa paikallisen toimialueen Azuren näennäiskoneiden SAP-järjestelmiin kuuluvat paikallisen-versioiden tuetaan tuotannon SAP-järjestelmiin. Paikallisen-tuetaan käyttöönotto osia tai suorittaa SAP maisemien Azure kyselyjä. Valmis SAP-vaaka käytössä myös Azure edellyttää ottaa ne VMs osaksi paikallisen toimialueen ja Active Directory / OpenLDAP. Entisen versioiden asiakirjojen puhuimme Hybrid IT käyttötavoista, jossa termi 'Hybrid' sijaitsee siitä, että on paikallisen-yhteys, paikallisen ja Azure välillä. Sekä siitä, että Azure-tietokannassa VMs ovat osa paikallisen Active Directory / OpenLDAP.

Jotkin Microsoft ohjeissa kerrotaan paikallisen-skenaariot hieman eri tavalla, erityisesti niiden DBMS HA-määrityksiä. Kyseessä SAP liittyvät tiedostot-vain paikallisen-skenaario boils siten, että sivusto sivusto tai yksityisten (ExpressRoute)-yhteys- ja SAP-vaaka on jaettu paikallisen ja Azure kertoma.  

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Resurssit
Seuraavat apuviivoja ovat käytettävissä SAP-käyttöönotoissa, valitse Azure aihe:

* [Windows näennäiskoneiden (VMs) – suunnittelu ja käyttöönotto-opas (tämä asiakirja) SAP-NetWeaver] [-suunnitteluoppaan]
* [Windows näennäiskoneiden (VMs) – käyttöönotto-oppaassa SAP-NetWeaver] [käyttöönotto-oppaan]
* [Windows näennäiskoneiden (VMs) – DBMS käyttöönotto-oppaassa SAP-NetWeaver] [dbms-opas]
* [Valitse Windows näennäiskoneiden (VMs) – suuren käytettävyyden käyttöönotto-oppaassa SAP NetWeaver][ha-guide]

> [AZURE.IMPORTANT] Missä käytetään mahdollista linkin viittaavan SAP: N asennus-opas (viittaus InstGuide-01, katso <http://service.sap.com/instguides>). Jos edellytykset ja asentunut, SAP NetWeaver asennuksen apuviivat aina luetaan huolellisesti, tässä asiakirjassa kerrotaan vain tiettyihin tehtäviin SAP NetWeaver Systemsin asennettuna: Microsoft Azure Virtual kone.

SAP-Azure Aiheeseen liittyvät seuraavat SAP-Huomautukset:

| Numero | Otsikko |
|--------------|-------|
| [1928533] | SAP-sovellusten Azure: tuetut tuotteet ja koon muuttaminen |
| [2015553] | Valitse Microsoft Azure SAP: tukevat edellytykset |
| [1999351] | Parannetun Azure seurannan SAP: n vianmääritys |
| [2178632] | Seurannan arvot SAP Microsoft Azure-näppäin |
| [1409604] | Virtualisointi Windows: parannetun seuranta |
| [2191498] | Linux Azure ja SAP: parannetun seuranta
| [2243692] | Valitse Microsoft Azure (IaaS) AM Linux: SAP käyttöoikeuden vianmääritys
| [1984787] | SUSE LINUX Enterprise Server 12: Asennusohjeet
| [2002167] | Punainen on Enterprise Linux 7.x: asennuksen ja päivitys

Lue myös [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) , joka sisältää kaikki SAP muistiinpanot Linux.

Yleiset rajoituksia sekä suurin rajoitukset Azure tilauksista löytyy [sisältö][azure-subscription-service-limits-subscription] 

## <a name="possible-scenarios"></a>Mahdollisia skenaarioita
SAP: N usein nähnyt yhtenä eniten kriittisten yrityksille-sovelluksissa. Arkkitehtuuri ja toimintojen nämä sovellukset on lähinnä monimutkainen ja on tärkeää varmistaa, että saatavuus ja suorituskyvyn vaatimukset täyttyvät.

Näin yrityksille on mieti tarkkaan siitä, mitkä sovellukset voidaan suorittaa julkisen Cloud-ympäristössä, riippumaton valitsemasi Cloud-palvelun.

Järjestelmän tyypit käyttöönotto SAP NetWeaver perustuu ympäristöissä on lueteltu alla julkisen cloud-sovelluksissa:

1. Normaali samankokoista tuotannon järjestelmät
1. Kehittäminen järjestelmät
1. Järjestelmien testaaminen
1. Prototyypin järjestelmät
1. Oppimiskeskuksen / järjestelmien esittely

Onnistunut käyttöönotto SAP-järjestelmiin Azure IaaS tai IaaS yleisesti, on tärkeää ymmärtää perinteinen outsourcers tai palveltavat järjestelmänvalvojat voivat tarjouksia ja IaaS palveluja merkittäviä erot. Olisi perinteinen isäntä- tai outsourcer mukauttaa infrastruktuurin (verkkoon, tallennuskiintiön ja tyyppi) asiakas haluaa isännöidä työmäärää, on sen sijaan voit valita oikean työmäärää IaaS käyttöönotoissa asiakkaan vastuu.

Ensimmäisessä vaiheessa asiakkaiden on Tarkista seuraavat seikat:

* SAP tueta Azure AM tyypit
* SAP tukemien tuotteiden/versioiden Azure
* Tuetut OS ja DBMS vapauttaa tietyn SAP-julkaisuissa Azure-tietokannassa
* SAP-siirtonopeuden myöntämä eri Azure-tuotteissa

Vastaamalla näihin kysymyksiin voidaan lukea SAP Huomautus [1928533]. 

Toinen vaihe Azure resurssi ja kaistanleveyden rajoitukset tarvitsee, jota verrataan todellinen resurssi kulutus paikallisen järjestelmien. Tämän vuoksi asiakkaiden on oltava tuttuja Azure tyypit SAP-alueella tueta eri ominaisuuksia:

* Suorittimen ja muistin resurssien AM erityyppisiä ja
* IOPS kaistanleveyden AM erityyppisiä ja 
* Verkon AM erilaisia ominaisuuksia.

Useimmat tiedot löytyvät [tähän][virtual-machines-sizes]

Muista, että rajoituksia luetellut yllä olevaa linkkiä ovat ylemmässä rajoitukset. Se ei tarkoita, että jokainen resurssit, kuten IOPS rajoitukset annettu kaikissa tilanteissa. Poikkeuksena vaikka valittu AM tyyppi suorittimen ja muistin resursseja. SAP: N tukemat AM kohdalla suorittimen ja muistin resurssit ovat varattu ja sellaisenaan käytettävissä milloin tahansa ajan kuluessa AM kulutus.

Microsoft Azure-ympäristö, kuten IaaS muiden ympäristöjen on usean vuokraajan ympäristö. Tämä tarkoittaa, että tallentaminen, verkon ja muita resursseja jaetaan omistajien välillä. Älykäs rajoitin- ja kiintiötiedot logiikan käytetään estää yhdestä vuokraajasta toiseen vuokraajan (häiriöistä yhteisön) suorituskykyyn radikaaleja tavalla. Vaikka Azure funktioiden yrittää Säilytä kaistanleveyden listan pieni, erittäin jaetun ympäristöjen varianssit yleensä oma suurempi varianssit resource/kaistanleveyden käytettävyyttä kuin asiakkaille useita käytetään niiden paikallisen käyttöönotoissa. Tuloksena voi kärsiä eri tasoilla terveisin verkko- tai i/o (-asema sekä viive) kaistanleveyden minuutti minuuttia. Todennäköisyys, että SAP-järjestelmän kanssa Azure saattaa toimia suurempi kuin varianssit paikalliseen järjestelmään on otettava huomioon.

Viimeinen vaihe on arvioiminen käytettävyyttä. Se voi tapahtua pohjana Azure infrastruktuurin Hae päivitettävä ja edellyttää käynnissä VMs käynnistettävä isännät. Tällaisissa tapauksissa näiden isännät-virtuaalilaitteiksi Sammuta ja käynnistetään uudelleen myös. Huolto ajoituksen on valmis-core työaikana tietyn alueen mutta muutaman tunnin aikana ilmenee uudelleen mahdolliset ikkuna on suhteellisen leveä. Useilla eri tekniikoita Azure ympäristössä, joka on määritetty pienentämään tai osa niistä tällaisia päivityksiä vaikutus. Tulevien parannukset Azure-ympäristön DBMS ja SAP-sovellus on suunniteltu Pienennä vaikutus tällaisia käynnistyy. 

Onnistuneesti käyttöön SAP-järjestelmää Azure päälle, jotta paikallista SAP alkuperäisiä pakojärjestelmiä käyttöjärjestelmän mukaan tietokannan ja SAP Azure tuki-matriisissa on näkyvät SAP-sovellusten Sovita resursseista Azure infrastruktuurin tarjoavat ja jossa voit käsitellä käytettävyys palvelutasosopimuksia Microsoft Azure-tarjouksia. Yksilöityä järjestelmät sinun on määritettävä jossain seuraavista tilanteista kaksi käyttöönotto.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Vain pilvipalveluita - virtuaalikoneen ominaisuuksissa kyselyjä Azure ilman paikallisen asiakasverkoston riippuvuudet
 
![Yksittäisen AM SAP esittely tai otettu käyttöön Azure koulutus-skenaario][planning-guide-figure-100]

Tämä skenaario on tyypillinen koulutukset tai esittely systems, johon kaikki SAP- ja -SAP ohjelmiston osat on asennettu sisällä yhden AM. Tämä käyttöönottotapa tuotannon SAP-järjestelmiin ei tueta. Yleensä tällöin täyttää seuraavat vaatimukset:

* Itsensä VMs voi käyttää julkisessa verkossa. Suora verkkoyhteys paikalliseen verkkoon joko omistaa esittelyt tai koulutukset-sisältöä tai asiakkaan yrityksen sisällä VMs sovellusten ei tarvitse. 
* Useita VMs koulutukset tai esittely skenaarion, kun verkon viestintä- ja tarkkuus on työskentely VMs välillä. Mutta VMs joukko välisten yhteyksien on oltava erillään niin, että useita ehtojoukkoja ja VMs voi asentaa rinnakkain ilman häiriöitä.  
* Internet-yhteys tarvitaan remote kirjautuessasi ylläpidettävä Azure VMs käyttäjää. Vierailijan mukaan OS, terminaalissa Services/RDS tai VNC ssh käytetään täyttävät koulutus tehtävät tai suorittaa esittelyt AM käyttämiseen. Jos SAP-portit kuten 3200, 3300 ja 3600 voi myös tarjoamia SAP-sovelluksen esiintymää niitä voi käyttää mitä tahansa Internet yhdistetyn työpöydältä.
* SAP: in alkuperäisiä pakojärjestelmiä (ja VM(s)) edustavat erillinen tilanne Azure-tietokannassa, joka edellyttää julkinen internet-yhteys peruskäyttäjän käytön ja ei edellytä Azure muiden VMs yhteyden vain.
* SAPGUI ja selain on asennettu ja suoritetaan AM. 
* Nopea palauttaminen alkuperäiseen tilaan AM, ja uusi asentamista, että alkuperäiseen tilaan uudelleen tarvitaan. 
* Esittely ja koulutus tilanteita, jotka toteutuvat useita VMs, Active Directory- / OpenLDAP ja/tai DNS-palvelu, jota tarvitaan kukin ehtojoukko VMs.


![Ryhmän AM 's edustava yhden esittely tai koulutukseen skenaarion Azure-pilvipalvelussa][planning-guide-figure-200]

On tärkeää muista, että kussakin siirtämistä VM(s) on otettava käyttöön rinnakkain, joissa kussakin joukon AM-nimet ovat samat.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>Usean tiloissa - käyttöönoton yhden tai useita SAP VMs kyselyjä Azure kanssa vaatimus on täysin integroitu paikalliseen verkkoon
 
![VPN-yhteyttä sivuston sivuston Connectivity (rajat tiloissa)][planning-guide-figure-300]

Tässä skenaariossa on monta mahdollista käyttöönoton kuvioilla paikallisen-skenaario. Se voidaan kuvata vain käynnissä joitakin osia SAP vaakasuuntaiseen paikallisen ja muita osia SAP vaaka-Azure. ‑Sivun siitä, että osa SAP-osat ovat käytössä Azure on oltava läpinäkyvä käyttäjille. Näin ollen SAP Transport korjaus järjestelmän (STM), RFC viestintä, tulostaminen, (kuten SSO) suojaus, jne toimii saumattomasti SAP-järjestelmiin Azure-käyttöjärjestelmässä. Mutta paikallisen-skenaario kuvataan myös tilanne, jossa valmis SAP-vaaka suoritetaan Azure asiakkaan toimialueella ja Azure laajentamista DNS. 

> [AZURE.NOTE] Tämä on käyttöönottotapa, jota tuetaan tehokkaasti SAP-järjestelmiin suorittamista varten.

[Tässä] artikkelissa[ vpn-gateway-create-site-to-site-rm-powershell] Lisätietoja paikallisen verkon muodostaminen Microsoft Azure

> [AZURE.IMPORTANT] Kun olemme keskustelet tietoja paikallisen-skenaariot välillä Azuren ja paikallisten asiakas-käyttöönotoissa, emme ovat katsoo koko SAP-järjestelmiin rakeisuuden. Skenaariot, joita _ei tueta_ paikallisen-skenaariot ovat seuraavat:
> 
> * Käynnissä kerroksia SAP-sovellusten käyttöönoton eri tavoilla. Esimerkiksi käynnissä DBMS kerroksen paikallisen, mutta SAP-sovelluskerroksen VMs käyttöön Azure VMs tai päinvastoin.
> * SAP-kerroksen Azure ja paikallisen joitakin osia. Esimerkiksi jakaminen SAP sovelluskerroksen paikallisen ja Azure VMs esiintymät. 
> * Jakautumisen virtuaalilaitteiksi SAP esiintymät järjestelmää Azure alueille päälle ei tueta.
> 
> Näiden rajoitusten syynä on erittäin pienen viiveen erinomainen suorituskyky verkon SAP-järjestelmässä, Palvelusovellusten esiintymät ja SAP-järjestelmää DBMS kerroksen välillä vaatimus.



### <a name="supported-os-and-database-releases"></a>Tuetut OS ja tietokannan versioista

* Microsoft server-ohjelmiston tueta tässä artikkelissa luetellut Azure virtuaalikoneen Services: <http://support.microsoft.com/kb/2721672>. 
* Tuetut käyttöjärjestelmän versioiden tietokannan järjestelmän versioiden tuettua Azure virtuaalikoneen Services SAP-ohjelmiston yhteydessä on kuvattu SAP Huomautus [1928533]. 
* SAP-sovellusten ja versioiden tuettua Azure virtuaalikoneen Services on kuvattu SAP Huomautus [1928533].
* Suorittaa Vieras VMs Azure-tietokannassa SAP skenaarioissa tuetaan vain 64-bittinen kuvia. Tämä tarkoittaa sitä, että vain 64-bittinen SAP-sovellusten ja tietokantojen ovat tuettuja.

## <a name="microsoft-azure-virtual-machine-services"></a>Microsoft Azure virtuaalikoneen palvelut
Microsoft Azure-ympäristö on internet-akseli cloud services ympäristössä ja niitä voi käyttää Microsoft tietojen keskikohdan mukaan. Ympäristön on Microsoft Azure virtuaalikoneen Services (infrastruktuuri palvelun tai IaaS) ja joukko monipuolisia ympäristö kuin palvelun (PaaS)-ominaisuuksia.

Azure-ympäristö vähentää huolellisesti tekniikka edellyttää ja infrastruktuurin ostaa. Se helpottaa toimivia sovelluksia antamalla tarvittaessa suorittaminen ja tallennustilaa isännöidä skaalata ja hallita web-sovelluksen ja sovelluksia ja ylläpito. Infrastruktuurin hallinta on automaattisiin alustaksi, joka on tarkoitettu suuren käytettävyyden kanssa ja dynaaminen skaalaus vastaamaan käyttö tarpeiden tukiyhteyksiä hinnoittelu malli-vaihtoehto.


 
![Microsoft Azure virtuaalikoneen palvelujen sijoittaminen][planning-guide-figure-400]

Azure virtuaalikoneen Services Microsoft ansiosta voit ottaa käyttöön mukautetun palvelimen kuvia Azure kuin IaaS esiintymät (katso kuva 4). Azure-tietokannassa näennäiskoneiden perustuvat Hyper-V virtual kiintolevyillä (Näennäiskiintolevyn) ja voivat suorittaa eri käyttöjärjestelmien Vieras OS.

Toiminnallisia näkökulmasta virtuaalikoneen Azure-palvelu on samanlainen kokemukset näennäiskoneiden on otettu käyttöön paikallisen nimellä. On kuitenkin merkittäviä etu, että sinun ei tarvitse hankitaan, hallintaan ja hallita infrastruktuuri. Kehittäjät ja järjestelmänvalvojilla on täydet oikeudet nämä näennäiskoneiden käyttöjärjestelmän kuvan. Järjestelmänvalvojat voit kirjautua etäyhteyden kyselyjä näiden näennäiskoneiden ylläpidon ja vianmäärityksen tehtävät sekä ohjelmiston käyttöönoton tehtävien suorittamiseen. Käyttöönoton osalta vain rajoitukset ovat kokojen ja Azure VMs ominaisuuksia. Nämä eivät ehkä niin hieno hajautetun kokoonpanossa, kun tämän voi tehdä paikallinen. Ei AM tyypit, jotka edustavat yhdistelmän vaihtoehtoa:

* VCPUs, määrä
* Muistin
* Näennäiskiintolevyjen, joka voidaan liittää, määrä
* Verkko- ja tallennustilaa kaistanleveyksien.

Koko ja eri kokoja tarjota näkevät [tämän] artikkelin taulukon eri näennäiskoneiden rajoitukset[virtual-machines-sizes]

Kun huomaat on eri perheille tai näennäiskoneiden sarjaa. Vuodesta joulu 2015 voit erottaa VMs seuraavat perheille:

* A0 A7 AM tyypit: kaikki, jotka ovat sertifioituja SAP: n. Ensimmäisen AM sarjan, joka Azure IaaS käytössä otettu käyttöön.
* A8 A11 AM tyypit: tehokas tietojenkäsittely esiintymät. Käynnissä eri paremmin suorittamisesta Laske kuin muiden A sarjan VMs isännät.
* D-sarjan AM tyypit: suorittamiseen parempi kuin A0 A7. Kaikki AM tiedostotyypit ovat sertifioituja SAP: N kanssa.
* Hakemistopalvelun sarjan AM tyypit: käyttää samaa isännät D-sarjan, mutta on pystyttävä muodostamaan yhteys Azure Premium Storage (katso tämän artikkelin luvun [Azure Premium tallennustilan] [suunnittelu-opas-3.3.2]). Uudelleen kaikki AM tiedostotyypit ovat sertifioituja SAP: N kanssa.
* G-sarjan AM tyypit: hyvin muistin AM tyyppejä. 
* GS sarjan AM tyypit: G-sarjan, kuten mutta mukaan lukien asetus, kun Azure Premium Storage (katso tämän artikkelin luvun [Azure Premium tallennustilan] [suunnittelu-opas-3.3.2]). Käytettäessä GS sarjan VMs tietokantapalvelimen on pakollinen Premium tallennustilan käytettävät DB-tietojen ja tapahtuman lokitiedostot


Voit etsiä saman suorittimen ja muistin määritykset AM sarjoille. Kuitenkin, kun haet näiden ulos toisen sarjan VMs siirtonopeuden suorituskyvyn ne voi eroavat merkittävästi. Huolimatta suorittimen ja muistin määrityksiä. Syynä on pohjana Host (isäntä)-palvelimen laitteiston osoitteessa AM erityyppisiä johdanto oli eri siirtonopeuden ominaisuudet.  Yleensä ero siirtonopeuden suorituskyvyn näkyvät myös näkyy eri VMs hinta.

Huomaa, että kaikki eri AM sarjan saattaa tarjota yksitellen Azure alueiden (Azure alueiden Katso seuraava luku). Huomaa myös, että kaikki VMs tai AM sarjan on hyväksytty SAP: n.

> [AZURE.IMPORTANT] SAP NetWeaver mukaan-sovellusten käyttö tuetaan vain alijoukko AM tyypit ja SAP Huomautus [1928533] luetellut määrityksiä.

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure alueet
Microsoft sallii ottamaan näennäiskoneiden on kutsuttu Azure alueisiin. Azure-alueen voi olla yksi tai useita tietojen keskukset, jotka sijaitsevat lähellä toisiaan. Useimpien geopoliittisten alueiden maailmanlaajuisesti Microsoft on vähintään kaksi Azure-alueita. Esimerkiksi Euroopan on Azure-alue, "Pohjois Eurooppa" ja "Länsi Eurooppa" jokin. Näiden kahden Azure alueiden geopoliittisten alueella on erotettu toisistaan riittävän merkittäviä etäisyys niin, että luonnollinen tai teknisiä lieventämiseksi eivät vaikuta samalla geopoliittisten alueella Azure molemmat alueet. Koska Microsoft sitä perustaminen, kaksi uutta Azure eri geopoliittisten alueiden yleisesti, näiden alueiden määrä kasvaa jatkuvasti ja saavuttaman joulu 2015 saavuttanut 20 Azure-alueet, joiden ilmoitettiin jo muita alueiden määrän. Asiakkaaksi, voit ottaa käyttöön SAP-järjestelmiin kaikki nämä alueisiin, mukaan lukien Azure kummallakin alueella Kiinassa. Nykyisen mennessä Azure alueiden tietoja on tämän sivuston: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>Microsoft Azure virtuaalikoneen käsite
Microsoft Azure on infrastruktuuri Service (IaaS) ratkaisuksi isäntään näennäiskoneiden samankaltaisia toimintoja ja paikallisen-virtualisointi ratkaisuksi. Olet voivat luoda näennäiskoneiden from Azure-portaalissa, PowerShell tai CLI, jotka tarjoavat myös käyttöönotto- ja hallintatoiminnot.

Azure Resurssienhallinta voit valmistella sovellustesi määritettäviä mallin avulla. Yksittäisen mallia voit ottaa palveluihin ja niiden välisten riippuvuuksien. Saman mallin avulla voit ottaa toistuvasti sovelluksesi jokaisen vaiheen aikana sovelluksen elinkaaren.

Löytyy lisätietoja ARM-mallien avulla:

* [Ottaa käyttöön ja hallita näennäiskoneiden Azure Resurssienhallinta mallit ja Azure-CLI][virtual-machines-linux-cli-deploy-templates]
* [Hallitse näennäiskoneiden Azure Resurssienhallinta ja PowerShellin avulla][virtual-machines-deploy-rmtemplates-powershell]
* <https://Azure.microsoft.com/Documentation/Templates/>

Toisen kiinnostava ominaisuus on voi luoda kuvia näennäiskoneiden, jonka avulla voit laatia tiettyjä säilöjen tietoihin, josta olet voivat ottaa nopeasti virtuaalikoneen esiintymät, jotka täyttävät tarpeen.

Lisätietoja kuvien luominen näennäiskoneiden löytyy [tämän] artikkelin (Windows)[ virtual-machines-windows-capture-image] tai [tämän]artikkelin (Linux)[virtual-machines-linux-capture-image].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Vian toimialueet
Vian toimialueiden edustavat fyysinen yksikkö virheestä, aivan läheisesti liittyvät sisältyy tietojen keskikohdan mukaan ja samalla, kun fyysinen sivu tai Teline voi pitää vika toimialueen fyysinen infrastruktuuri, ei ole suoraa yksi-yhteen yhdistämistä välillä. 

Kun otat käyttöön useita näennäiskoneiden osana Microsoft Azure virtuaalikoneen Servicesin SAP-järjestelmää, voivat vaikuttaa Azure kangasta ohjauskoneen eri vika toimialueiden kokouksen siten Microsoft Azure SLA vaatimukset jakaa sovelluksen käyttöönotto. Jakauman vika toimialueiden Azure asteikko (Laske solmujen tai tallennustilan solmujen ja verkko satoja sivustokokoelma) kautta tai VMs tietyn vika toimialueen määritys on kuitenkin jotain tulosjoukko, jolle ei ole suoraa ohjausobjektin. Jotta voit ohjata Azure kangasta ohjain käyttöön VMs joukko eri vika toimialueiden kautta, sinun täytyy määrittää Azure käytettävyys määrittäminen, VMs käyttöönoton aikana. Katso lisätietoja Azure käytettävyys joukoissa, luvun [Azure käytettävyys joukot] [suunnittelu-opas-3.2.3] Tässä asiakirjassa.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Toimialueiden päivittäminen
Päivityksen toimialueiden edustavat loogisena yksikkönä, joiden avulla voit määrittää, miten SAP-järjestelmässä, joka koostuu käynnissä useita VMs SAP-esiintymät, AM päivitetään. Päivityksen yhteydessä, Microsoft Azure käy läpi päivitys nämä päivityksen toimialueiden yksi kerrallaan. Levittämällä VMs käyttöönoton milloin päälle eri päivittäminen toimialueiden suojaamalla SAP-järjestelmää osittain mahdolliset käyttökatkot. Pakottaa Azure eri päivittäminen toimialueiden levitä SAP-järjestelmää VMs käyttöön, jotta haluat määrittää tietyn määritteen kunkin AM käyttöönoton yhteydessä. Kaltaisilta vika toimialueiden Azure-asteikko on jaettu päivittää useita toimialueita. Jotta voit ohjata Azure kangasta ohjain käyttöön VMs joukko eri päivittäminen toimialueiden kautta, sinun täytyy määrittää Azure käytettävyys määrittäminen, VMs käyttöönoton aikana. Lisätietoja Azure käytettävyys joukot noudata seuraavia ohjeita luvun [Azure käytettävyys joukot] [suunnittelu-opas-3.2.3].

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Azure käytettävyys joukot
Azure-Virtuaalikoneissa sisällä yhden Azure käytettävyys määrittäminen jaetaan Azure kangasta valvoja eri vika ja päivittää toimialueet. Jakauman eri vika ja päivittää toimialueiden tarkoituksena on estää kaikki VMs SAP-järjestelmän suljetaan infrastruktuurin ylläpidon tai epäonnistui virheen-toimialueella. Oletusarvon mukaan VMs eivät ole osa käytettävyys määrittäminen. AM osallistuminen käytettävyys määrittäminen on määritetty käyttöönoton milloin tai uudempi kokoonpanon uudelleenmääritys ja uudelleen AM käyttöönotto.

Selvittääksesi Azure käytettävyys joukot sekä siitä, miten käytettävyys joukot liittyvät vika ja päivittää toimialueiden käsite, lue [artikkeli][virtual-machines-manage-availability]

Määritä käytettävyys KÄDESSÄ joukkoja kautta json mallin artikkelissa [rest api-Etkö](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) ja Etsi "käytettävyyttä".

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Tallennustilan: Microsoft Azure-tallennustilan ja tietojen levyjen
Microsoft Azuren näennäiskoneiden käyttämiseen eri sijainteihin. Kun SAP Azure virtuaalikoneen Services on tärkeä ottaa nämä kaksi päätyyppiä tallennustilaa erot:

* Muut pysyvä, muuttuvat tallennuspaikka.
* Pysyvä tallennuspaikka.

Jatkuva tallennustilan liitetään suoraan käynnissä näennäiskoneiden ja Laske solmut itse – paikalliseen storage (väliaikaisten) sijaitsee. Koko määräytyy valinnut käyttöönoton käynnistyessään virtuaalikoneen kokoa. Tämä tallennustyyppi on muuttuvat ja näin ollen levyn alustaa virtuaalikoneen esiintymän uudelleenkäynnistyksen yhteydessä. Yleensä sivutustiedoston käyttöjärjestelmän sijaitsee tilapäinen levyä.

___

> ![Windows][Logo_Windows] Windows
>
> Valitse Windows VMs temp asema on otettu käyttöön asema käyttöön AM D:\ kuin.
>
> ![Linux][Logo_Linux] Linux
> 
> Linux VMs se on liitetty /mnt/resource tai /mnt. Katso lisätietoja tähän:
> 
> * [Tietoja DVD-levyllä liittämisestä Linux-Virtual Machine][virtual-machines-linux-how-to-attach-disk]
> * <http://blogs.msdn.com/b/mast/Archive/2013/12/07/Understanding-the-Temporary-Drive-on-Windows-Azure-Virtual-Machines.aspx>

___

Todellinen asema ei muuttuvat, koska se on käytön tallennettu itse Host (isäntä)-palvelimeen. Jos AM siirto lukea (kuten vuoksi ylläpidon isännän tai Sammuta ja käynnistä se uudelleen) aseman sisällön menetetään. Tämän vuoksi ei voi tallentaa tärkeitä tietoja asemassa. Tämäntyyppisen tallennustilan medioiden tyypin eroaa eri AM sarjaan suorituskyvyn hyvin eri ominaisuuksia, jotka saavuttaman kesäkuussa 2015 näyttää tältä:

* A5 – A7: Hyvin vähän suorituskykyä. Ei suositella mitään sivun tiedoston lisäksi
* A8-A11: 10 000 joitakin IOPS erittäin hyvä suorituskyky ominaispiirteet ja > 1 gt/sec siirtonopeuden.
* D-sarja: Jotkin sitten tuhat IOPS erittäin hyvä suorituskyky ominaispiirteet ja > 1 gt/sec siirtonopeuden.
* DS-sarja: 10 000 joitakin IOPS erittäin hyvä suorituskyky ominaispiirteet ja > 1 gt/sec siirtonopeuden.
* G-sarja: 10 000 joitakin IOPS erittäin hyvä suorituskyky ominaispiirteet ja > 1 gt/sec siirtonopeuden.
* GS-sarja: 10 000 joitakin IOPS erittäin hyvä suorituskyky ominaispiirteet ja > 1 gt/sec siirtonopeuden.

Yllä lauseet ovat soveltaminen AM tyypit, jotka ovat sertifioituja SAP: N kanssa. AM-sarja, jossa on erinomainen IOPS ja siirtonopeuden oikeutettuja suorituskykykertoimen DBMS joitakin ominaisuuksia. Katso [DBMS käyttöönotto-oppaan] [dbms-opas] Lisätietoja.

Microsoft Azuren tallennustilaan on pysyviä tallennustilan ja suojaus ja nähdä SAN säilössä redundancy tyypillinen tasoja. Azuren tallennustilaan perusteella levyjen ovat virtual kiintolevyn (näennäiskiintolevyjen) Azure-tallennustilan Services sijaitsee. Paikallinen OS-levy (Windows C:\, Linux / (/ keskihajonta/sda1)) on tallennettu Azure-tallennustilan ja muita asemat/levyjä AM lisätallennustilaa Hae tallennettuja, liian.

Se on mahdollista Lähetä aiemmin Näennäiskiintolevyn, valitse paikallisen tai Luo tyhjä tiedostoja Azure sisällä ja liitä niiden käyttöön VMs. Näiden näennäiskiintolevyjen viitataan Azure levyjen nimellä. 

Luoda tai ladata Näennäiskiintolevyn Azure varastoon, kun on mahdollista, voit ottaa käyttöön ja liittää niiden aiemmin virtuaalikoneen ja kopioida aiemmin (ottamattomien) Näennäiskiintolevyn.

Näiden näennäiskiintolevyjen säily, kun tiedot ja muutosten ne ovat turvallisten, kun uudelleenkäynnistyksen ja luotaessa virtuaalikoneen esiintymä. Vaikka erillisen poistetaan, nämä näennäiskiintolevyjen turvallisuus ja voit otettava tai jos-OS levyjä voidaan asentaa muiden VMs avulla.

Azuren tallennustilaan verkoston eri redundancy tasoja voi määrittää:

* Valittavissa olevat taso on 'paikallisen kopion", mikä on sama kuin kolmen Replika1 saman tietokeskuksen Azure-alueen tiedot (katso luvun [Azure Regions][planning-guide-3.1]). 
* Vyöhykkeen tarpeettomat tallentamista, joka kolme kuvat levitä eri tietojen keskittää samalla Azure-alueella.
* Redundancy oletustaso on maantieteelliset redundanssi, joka kopioi asynkronisesti toiseen 3 kuviksi tietojen tuominen toisesta Azure aluetta, joka nykyisessä saman geopoliittisten alueen sisältö.

Taulukon päälle terveisin eri redundancy asetukset tämän artikkelin Katso myös: <https://azure.microsoft.com/pricing/details/storage/> 

Löytyy lisätietoja terveisin Azuren tallennustilaan: 

* <https://Azure.microsoft.com/Documentation/Services/Storage/>
* <https://Azure.microsoft.com/Services/Site-Recovery>
* <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>
* <https://blogs.msdn.com/b/azuresecurity/Archive/2015/11/17/Azure-Disk-Encryption-for-Linux-and-Windows-Virtual-Machines-Public-Preview.aspx>


#### <a name="azure-standard-storage"></a>Azure vakio tallennustila
Azure vakio-BLOB-säiliö on tallennustilaa tyypin, kun Azure IaaS julkaistiin. Käytettävissä on IOPS kiintiön pakotettu yhden Näennäiskiintolevyn kohden. Listan viive ei ole sama luokan kuin SAN/NAS laitteiden yleensä käyttöön tehokkaimpia SAP-järjestelmiin isännöidään paikallisen. Kuitenkin vakio Azure-tallennustilan osoitetaan sovellu monta satoja SAP-järjestelmiin tänä käyttöön Azure-tietokannassa.

Azure vakio tallennustilan veloitetaan mukaan tallennettuja tietoja, äänenvoimakkuuden tallennustilan tapahtumat, lähtevä tiedonsiirron ja valittu redundancy-vaihtoehto. Monta näennäiskiintolevyjen voidaan luoda kooltaan enintään 1 TT, mutta kun ne ovat tyhjiä on maksutonta. Jos täytät sitten yksi Näennäiskiintolevyn 100 Gigatavua kanssa, voit veloitetaan projektiin 100 Gigatavua ja ei Näennäiskiintolevyn käytössä luotu koon.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Azure Premium-tallennustilan
Huhtikuussa 2015 Microsoft käyttöön Azure Premium-tallennustilan. Tavoite antamaan Premium tallennustilan käytössä otettu käyttöön:

* Parempi i/o viive.
* Parempi siirtonopeuden.
* Vähemmän hajontaa i/o viive.

Tähän tarkoitukseen paljon muutoksia on tuotu, jotka tärkein kahden ovat:

* Azuren tallennustilaan solmut Suoritettaessa levyjen käyttö
* Uusi luku, joka varmuuskopioidaan Azure Laske-solmu paikallisen Suoritettaessa välimuistin

Vakio tallennustilaa, jossa ominaisuuksia ei muuttanut vastakkaiseen kokoa levy (tai Näennäiskiintolevyn) määräytyy Premium tallennustilan tällä hetkellä on 3 toiselle levylle luokat, jotka näkyvät usein kysytyt kysymykset-osassa ennen tämän artikkelin lopussa: <https://azure.microsoft.com/pricing/details/storage/>

Huomaat, että IOPS/Näennäiskiintolevyn ja levytilan siirtonopeuden/Näennäiskiintolevyn on riippuvainen levyjä kokoa-luokka

Kustannusten peruste Premium tallennustilan kyseessä ei ole tallennettu tällaisten näennäiskiintolevyjen, mutta tällaisen SITEN, tiedot, jotka on tallennettu Näennäiskiintolevyn määrä riippumatta kokoa luokan todellisten tietojen äänenvoimakkuuden.

Voit myös luoda näennäiskiintolevyjen Premium-tallennustilan, jota ei ole suoraan yhdistäminen näkyvän koon luokkiin. Tämä voi olla tapaukselle, erityisesti silloin, kun näennäiskiintolevyjen kopioiminen vakio tallennustilan Premium varastoon. Tässä tapauksessa yhdistämismäärityksen seuraava Premium tallennustilan suurin levylle-komento on suoritettu. 

Ota huomioon, että vain tietyt AM sarjan voit hyötyä Azure Premium-tallennustilan. Nämä ovat DS - ja GS-sarjan saavuttaman joulu 2015. DS-sarjan arvo on lähinnä sama kuin D sarja DS-sarjan on asennustapa Premium tallennustilan mahdollisuus poikkeusten perusteella VMs lisäksi voit näennäiskiintolevyjen, joita isännöidään vakio Azure-tallennustilan. Samojen kelpaa G-sarjan GS sarjan verrattuna.

Jos kuitataan ulos DS-sarjan VMs [tämän] artikkelin osa[ virtual-machines-sizes] myös huomaat, että liittyy tietoja aseman rajoituksia, Premium tallennustilan näennäiskiintolevyjen rakeisuuden AM tason. Eri DS-sarjan tai GS sarjan VMs valmisteltu eri rajoitukset terveisin näennäiskiintolevyjen, joka voidaan asentaa määrän. Nämä raja-arvot on kuvattu artikkelissa sekä edellä mainituista. Mutta pohjimmiltaan se tarkoittaa, että otat käyttöön 32 x P30 levyjen/näennäiskiintolevyjen yksittäisen DS14 AM, kuten ei saat 32 suurin nopeus P30 levyn x. Sen sijaan suurin nopeus AM tasolla on artikkelissa suorittimessa rajoittaa tiedonsiirron. 

Saat lisätietoja tallennustilan Premium löytyy tähän: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="azure-storage-accounts"></a>Azure-tallennustilan tilit
Services-palveluiden vai VMs Azure otettaessa näennäiskiintolevyjen ja AM kuvia on järjestettävä mittayksikkö kutsutaan Azure-tallennustilan tilit. Azure käyttöönoton suunnittelun, sinun täytyy Mieti Azure rajoitukset. Yksi-puolella on rajoitettu määrä tallennustilan tilit Azure tilauskohtaisten. Vaikka kunkin Azure-tallennustilan tilin mahtuu näennäiskiintolevytiedostoja suuri määrä,-tiliä ja tallennustilaa yhteensä IOPS on kiinteä rajoitus. Otettaessa SAP VMs satoja, jossa DBMS luominen merkittäviä IO puhelut kannattaa jakaa hyvin IOPS DBMS VMs useiden Azure-tallennustilan tilien välillä. Korkeintaan Azure-tallennustilan tilit nykyisen raja tilauskohtaisten on otettava tarkkaan. Koska tallennustila on olennainen osa tietokannan käyttö SAP-järjestelmän, tämän käsitteen kerrotaan tarkemmin jo viitatun [DBMS käyttöönotto-oppaan] [dbms-opas].

Lisätietoja Azure-tallennustilan tilit löytyy [tämän]artikkelin[storage-scalability-targets]. Luettaessa tässä artikkelissa huomaat, että eroja rajoitukset Azure vakio tallennustilan ja Premium tallennustilan tilien välillä. Sivustot eroavat toisistaan ovat äänenvoimakkuuden tiedot, joita voi olla tallennettuna on tallennustilan-tili. Vakio tallennustilan ääni on sitä, joka on suurempi kuin Premium-tallennustilan. Valitse toinen puoli normaalin tallennustilan tilin on vakavasti rajallinen IOPS (Katso sarakkeen "Yhteensä pyytää korko"), Azure Premium tallennustilan tilin on tällaista rajoitusta. Käsittelemme tiedot ja tulokset nämä erot kun SAP-järjestelmiin, erityisesti DBMS-palvelimet-versioiden keskustella.

Tallennustilan-tilin sinun on mahdollisuus luoda eri säilöjen järjestäminen ja luokitella eri näennäiskiintolevyjen. Näiden säilöjen käytetään yleensä esimerkiksi erillisessä näennäiskiintolevyjen eri VMs. Ei ole suorituskyvyn vaikutukset sisään yhden säilö tai useita säilöjen yksittäisen Azure-tallennustilan tilin alapuolella.

Azure sisällä Näennäiskiintolevyn nimi tapahtuu on yksilöllinen nimi sisällä Azure näennäiskiintolevyn nimeämiskäytäntö yhteyden:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Mainittu edellä merkkijono on yksilöivät Näennäiskiintolevyn, joka on tallennettu Azure-tallennustilan.

### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Microsoft Azure verkko
Microsoft Azure antaa verkkoinfrastruktuuria, jonka avulla kaikki tilanteita, jotka haluamme huomaat SAP-ohjelmiston määritys. Ominaisuuksia ovat seuraavat:

* Pääsy ulkopuolelle, suoraan Windows päätepalvelut tai ssh/VNC VMs
* Access services ja tietyn porteista VMs-sovelluksissa
* Sisäinen viestintää ja nimenselvitys käyttöön kuin Azure VMs VMs välille
* Paikallisen-asiakkaan paikallisen verkko- ja Azure verkon väliset yhteydet
* Ristiin Azure alue tai center yhdistämispalvelun Azure sivustosta toiseen 

Lisätietoja löytyy tähän: <https://azure.microsoft.com/documentation/services/virtual-network/>

On useita eri vaihtoehtoja Määritä nimi ja IP-tarkkuus Azure-tietokannassa. Tässä asiakirjassa vain Pilvipalveluita skenaariot luottavat oletusarvo Azure DNS: n avulla (toisin kuin määrittäminen oman DNS-palvelusta). On myös uusi Azure DNS-palvelu, jota voidaan käyttää sen sijaan, että DNS-palvelimen määrittäminen. Lisätietoja [tämän] artikkelin löytyy[ virtual-networks-manage-dns-in-vnet] ja [tällä](https://azure.microsoft.com/services/dns/)sivulla.

Paikallisen-skenaarioissa on ovat käyttäisit tieto, joka paikallista AD/OpenLDAP/DNS on laajennettu VPN tai yksityinen yhteyden Azure. Tietyissä skenaarioissa seuraavassa suorittimessa voi olla tarpeen on asennettu Azure AD/OpenLDAP-replikan.

Koska verkko nimenselvitys on tärkeä osa tietokannan käyttö SAP-järjestelmän sekä tämän käsitteen kerrotaan tarkemmin [DBMS käyttöönotto-oppaan] [dbms-opas].


##### <a name="azure-virtual-networks"></a>Azure Virtual verkot

Muodostamalla Azure Virtual verkon voit määrittää yksityinen IP-osoitteiden kohdistettu Azure DHCP toiminnon mukaan osoitealueen. Paikallisen-tilanteissa määritetty IP-osoitealueita edelleen jaetaan käyttämällä DHCP Azure mukaan. Kuitenkin toimialueen nimenselvitys tapahtuu paikallisen (olettaen VMs ovat paikallisen toimialueen osa) ja näin ollen voit ratkaista lisäksi eri Azure pilvipalveluihin osoitteet.

[Kommentti]: <>  (MSSedusch vielä tarvitaan? TODO alun perin jonkin Azure Virtual verkossa on sidottu affiniteetti-ryhmään. Jonka Azure Virtual verkon käytössä rajoitettu, affiniteetti ryhmän käytössä myönnetyt Azure-asteikko. Lopuksi tämä tarkoitetaan Virtual Network on rajoitettu Azure-asteikko resurssit. Tämän jälkeen on muutettu ja nyt Azure Virtual verkkojen voit venyttämällä yli useita Azure-asteikko. Kuitenkin, joka edellyttää, että Azure Virtual verkot ovat ** ei ** liittyvät affiniteetti ryhmät enää luominen aikaan. Jo mainittiin, valitse vastakkaiseen suosituksia vuodessa muuttui kannattaa ** hyödyntää Azure affiniteetti ryhmille ei enää **. Lisätietoja on artikkelissa < https://azure.microsoft.com/blog/regional-virtual-networks/>)

Azure Virtual jokaiseen tietokoneeseen on yhdistetty Virtual verkkoon.

Lisätietoja [tämän] artikkelin löytyy[ resource-groups-networking] ja [tällä](https://azure.microsoft.com/documentation/services/virtual-network/)sivulla.

[Kommentti]: <>  (MShermannd TODO ei löydä artikkeliin, joka sisältää OpenLDAP aiheen + ARM;)
[Kommentti]: <>  (MSSedusch < https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [AZURE.NOTE] Oletusarvoisesti, kun AM on otettu käyttöön ei voi muuttaa VPN-määritys. TCP/IP-asetukset on vasemmalta Azure luokaksi. Oletustapa on dynaaminen IP-tehtävän.

Virtuaalinen verkkokortti MAC-osoitteen voivat muuttua esimerkiksi koon uudelleen ja Windows- tai Linux Vierailijan OS noutaa uusia verkkokortti ja käyttää automaattisesti DHCP määrittää DNS- ja IP-osoitteiden tällöin jälkeen.

##### <a name="static-ip-assignment"></a>Staattinen IP-määritys
On mahdollista määrittää kiinteän tai varattu IP-osoitteet VMs Virtual Azure-verkossa. Hyvien mahdollisuus hyödyntää tätä toimintoa, jos tarvitaan tai joissakin tilanteissa vaaditaan käynnissä VMs Azure Virtual verkossa avautuu. IP-varauksen pysyy voimassa koko AM, riippumatta siitä, onko AM käynnissä tai Sammuta olemassaolosta. Tuloksena on otettava kokonaismäärästä VMs (käynnissä ja pysäytetty VMS), huomioon määritettäessä alueen IP-osoitteiden Virtual verkossa. IP-osoite pysyy määritettynä, kunnes AM ja sen verkko-käyttöliittymän poistetaan tai kunnes IP-osoite saa varaustiedoista määritetty uudelleen. Lue lisätietoja [tämän]artikkelin[virtual-networks-static-private-ip-arm-pportal].

##### <a name="multiple-nics-per-vm"></a>Useita NIC AM kohden
Voit määrittää useita VPN-liittymän kortteja (vNIC), Azure Virtual Machine. Mahdollisuuden on useita vNICs, voit aloittaa verkkoliikennettä määrittäminen erottaminen kohtaa, johon, kuten asiakkaan liikenne reititetään läpi yksi vNIC ja Taustajärjestelmä liikenne reititetään toisen vNIC. Riippuvaiset AM tyyppi on ovat eri terveisin vNICs määrän rajoitukset. Tarkat tiedot, toiminnot ja rajoituksia löytyvät seuraavissa artikkeleissa:
 
* [Luo AM useita NIC][virtual-networks-multiple-nics]
* [Ottaa käyttöön usean NIC VMs mallin avulla][virtual-network-deploy-multinic-arm-template]
* [Ottaa käyttöön usean NIC VMs PowerShellin avulla][virtual-network-deploy-multinic-arm-ps]
* [Usean NIC VMs käyttämällä Azure-CLI käyttöönotto][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Sivusto yhteys
Paikallisen-on Azure VMs ja paikallisen linkitetty läpinäkyvä ja pysyvät VPN-yhteyttä. Odotetaan luomisella yleisimmät SAP-käyttöönoton kuvion Azure-tietokannassa. Olettaen on, että toiminnallisia ohjeita ja -prosessien havainnollistaminen SAP esiintymät Azure-tietokannassa pitäisi toimia läpinäkyvä. Tämä tarkoittaa pitäisi onnistua tulostaminen järjestelmät ulos sekä käyttö SAP liikenteen hallinta järjestelmän (TMS) liikennettä muuttuu kehittäminen järjestelmästä Azure-testi-käyttöjärjestelmä, joka on otettu käyttöön paikallisen. [Tässä artikkelissa] löytyvät Lisää dokumentaatio sivusto sivusto ympärille[vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>VPN-tunnelin laite
Jotta voit luoda sivuston sivuston yhteyden (paikallisen data Centerissä Azure data Centerissä), sinun on voit hankkia ja määritä VPN-laite tai käyttää RRAS Routing and Remote Access Service () käyttöön tullut ohjelmisto-osan kanssa Windows Server 2012. 

* [Luo virtuaalisia verkko sivusto sivusto VPN-yhteyden PowerShellin avulla][vpn-gateway-create-site-to-site-rm-powershell]
* [Tietoja sivuston sivuston VPN-yhdyskäytävän yhteyksien VPN-laitteet][vpn-gateway-about-vpn-devices]
* [VPN-yhdyskäytävän usein kysytyt kysymykset][vpn-gateway-vpn-faq]

![Sivuston sivuston paikallisen ja Azure välinen yhteys][planning-guide-figure-600]

Edellä olevassa kuvassa näkyy, kaksi Azure tilaukset on IP-osoite subranges varattu käytön Azure Virtual verkot. Azure paikallisen verkosta yhteys on muodostettu VPN-verkon kautta.

#### <a name="point-to-site-vpn"></a>Pisteen sivuston VPN
Pisteen sivuston VPN edellyttää, että jokaisen asiakaskoneeseen yhdistämään omassa VPN Azure kyselyjä. Olemme katsoo SAP-tilanteessa pisteen sivuston connectivity ei kannata. Tämän vuoksi ei ole vielä viittaukset kiinnitetään pisteen sivuston VPN-yhteys.

[Kommentti]: <>  (Lisätietoja löytyy tässä, MSSedusch--)
[Kommentti]: <>  (MShermannd TODO linkki ei ole enää voimassa. Mutta ARM silti ei tueta - seuraava linkki alla)
[Kommentti]: <>  (MSSedusch--< http://msdn.microsoft.com/library/azure/dn133798.aspx>.)
[Kommentti]: <>  (Sivusto ei tue vielä KÄDESSÄ MShermannd TODO, osoita)
[Kommentti]: <>  (MSSedusch--< https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/>)

#### <a name="multi-site-vpn"></a>Usean sivuston VPN
Azure on myös Internetissä, voit luoda Azure tilaukselle usean sivuston VPN-yhteys. Aiemmin yksi tilaus on rajoitettu sivusto sivusto-VPN-yhteyttä. Tämä rajoitus tapahtui poissa yhden tilauksen usean sivuston VPN-yhteyttä. Näin hyödyntää useita Azure-alueen tietyt project_pro_noversion paikallisen-määrityksiä.

Lisätietoja on [artikkelissa][vpn-gateway-create-site-to-site-rm-powershell]
[Kommentti]: <> (MShermannd TODO löytyy KÄDESSÄ helppokäyttötoimintoa linkkiä)

#### <a name="vnet-to-vnet-connection"></a>VNet VNet yhteyteen
Usean sivuston VPN: N käyttäminen, sinun täytyy määrittää erillisen Azure Virtual verkon kaikkien alueiden. Sinun on kuitenkin usein vaatimus, että ohjelmisto-osia eri alueilla pitäisi olla yhteydessä toisiinsa. Ihannetapauksessa tämän viestintä ei reititetään alueelta Azure paikalliseen ja sieltä Azure-alueelle. Pikakuvakkeen Azure tarjoaa mahdollisuutta yhteyden Azure Virtual verkon määrittäminen jonkin alueen toiseen Azure Virtual verkkoon isännöidään toisen alueen. Tämä toiminto on nimeltään VNet VNet yhteyden. Lisätietoja toiminto löytyy tähän: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-to-azure--expressroute"></a>Yksityisen yhteyden Azure – ExpressRoute
Microsoft Azure ExpressRoute avulla Azure tietojen keskikohdan mukaan- ja joko asiakkaan paikallisen infrastruktuurin tai rinnakkain ympäristössä yksityinen yhteyksien luomista. ExpressRoute tarjoama eri MPLS (paketin vaihtaa) VPN tarjoajat tai verkon palveluntarjoajia. ExpressRoute yhteydet ei siirry julkisen Internetin välityksellä. ExpressRoute yhteydet tarjouksen tehokkaamman, Lisää luotettavuutta useita rinnakkaisia piirit, nopeampi nopeuksia ja pienempi viiveet suurempia kuin tavallinen yhteydet Internetin välityksellä. 

Etsi lisätietoja Azure ExpressRoute ja palveluja tähän:

* <https://Azure.microsoft.com/Documentation/Services/expressroute/>
* <https://Azure.microsoft.com/pricing/details/expressroute/>
* <https://Azure.microsoft.com/Documentation/articles/expressroute-FAQs/>

Express reitin mahdollistaa useita Azure tilauksia läpi yksi ExpressRoute piiri suorittimessa tähän 

* <https://Azure.microsoft.com/Documentation/articles/expressroute-howto-linkvnet-ARM/> 
* <https://Azure.microsoft.com/Documentation/articles/expressroute-howto-circuit-ARM/>


#### <a name="forced-tunneling-in-case-of-cross-premise"></a>Jos rajat tiloissa tunneling pakotettu
VMs liittyminen paikallisen toimialueen sivusto, piste sivuston tai ExpressRoute kautta sinun on varmistaa, että Internet-välityspalvelimen ovat käytön käyttöön kyseiset VMs kaikille käyttäjille. Oletusarvon mukaan näiden VMs tai käyttäjien selaimella Internetiä ohjelmiston ei mennyt yrityksen välityspalvelimen kautta, mutta muodostaa yhteyden suoraan Azure Internet kautta. Mutta myös välityspalvelimen asetukset ei ole 100 %: n ratkaisu ohjaamaan liikenne yrityksen välityspalvelimen kautta, koska se on vastuussa ohjelmistojen ja palveluiden välityspalvelimen tarkistavan. Jos käynnissä AM ohjelmiston ei käytetä, tai järjestelmänvalvoja käsittelevää asetuksia, Internet-liikenteen detoured uudelleen suoraan Azure Internetin välityksellä. 

Siten vältät tämän, voit määrittää pakotettu Tunneling sivusto sivusto paikallisen ja Azure väliset yhteydet. Yksityiskohtainen kuvaus pakotettu Tunneling-ominaisuus on julkaistu tähän <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/> 

Pakotetun Tunneling ExpressRoute kanssa on käytössä, asiakkaat mainonta oletusarvon reitin ExpressRoute erityisen peering istunnot kautta.

#### <a name="summary-of-azure-networking"></a>Azure Verkko Yhteenveto
Tämän luvun sisälsivät tietoja Azure verkko monta tärkeät seikat. Tässä on yhteenveto pääkohdat:


* Azure Virtual verkkojen sallii omien tarpeidesi mukaan-verkoston määrittäminen
* Azure Virtual verkkojen voidaan hyödyntää IP-osoitealueet varaaminen VMs tai VMs kiinteä IP-osoitteiden määrittäminen
* Sivusto sivusto tai pisteen sivuston yhteyden määrittämisessä, sinun täytyy ensin Azure Virtual verkoston luominen
* Kun virtual machine on otettu käyttöön ei enää voi muuttaa AM myönnetyt Virtual Network

### <a name="quotas-in-azure-virtual-machine-services"></a>Kiintiön Azure virtuaalikoneen Services-palveluissa
Säilytys- ja verkko-infrastruktuurin jaetaan välillä erilaisia palveluja Azure infrastruktuurin virtuaalilaitteiksi siitä selvästi annettava. Ja vain asiakas Omat tiedot-keskukset, kuten ylimitoitettava joistain infrastruktuurin resurssit asetetaan määrin. Microsoft Azure-ympäristön käyttää levyn, suorittimen, verkon ja muut kiintiön rajoittaa resurssin kulutus ja säilyttää yhdenmukaisia ja deterministic suorituskyvyn.  AM eri osiin (A5, A6 jne.) on eri kiintiöt levyjen suorittimen ja RAM-Muistia verkon määrä.

> [AZURE.NOTE] SAP: N tukemat AM tietuetyypeistä suorittimen ja muistin resurssit ovat valmiiksi kohdistettu host solmuissa. Tämä tarkoittaa, että AM on otettu käyttöön, kun isännän resurssit ovat käytettävissä määritysten AM-tyypin mukaan.

Jokaisen virtuaalikoneen koon kiintiöiden on otettava huomioon suunniteltaessa ja koon SAP Azure ratkaisuja.  AM-kiintiön on kuvattu [seuraavassa][virtual-machines-sizes].

Kiintiön, joka on kuvattu edustavat teoreettinen suurimmat arvot.  IOPS kohti Näennäiskiintolevyn raja voidaan toteuttaa pieni IOs (8 kt) kanssa, mutta mahdollisesti ole voidaan toteuttaa ja suuri IOs (1 Mt).  IOPS rajoitus on pakotetun yksittäisen näennäiskiintolevyjen rakeisuuden.

Alla Päätöspuun avulla voidaan valita, onko SAP-järjestelmää sopii Azure virtuaalikoneen Services ja sen ominaisuuksia tai onko olemassa olevan järjestelmän varten on määritettävä eri tavalla käyttöönotossa Azure järjestelmää karkea Päätöspuun, kuin:
 
![Päätöspuun päättää voi ottaa käyttöön SAP-Azure][planning-guide-figure-700]

**Vaihe 1**: tärkeimmät tiedot on aluksi määritetyn SAP-järjestelmää SAP vaatimus. SAP-vaatimuksista on erotettava DBMS-osa ja SAP-sovelluksen osan vaikka SAP-järjestelmää on jo otettu käyttöön paikallisesti 2 taso-määritys. Aiemmin Systemsin liittyvät käytössä laitteiston usein SAP voit määritetään tai aiemmin SAP viitearvoja perusteella. Täältä löytyvät tulokset: <http://global.sap.com/campaigns/benchmark/index.epx>. Äskettäin käyttöön SAP-järjestelmiin sinun kannattaa välillä on lähetetty – koon muuttaminen-Harjoitus, joka tulee määrittää järjestelmän SAP-vaatimukset
Katso myös tämä blogiin ja SAP koon muuttamista varten liitetyn tiedoston Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**Vaihe 2**: aiemmin Systemsin i/o määrä ja i/o-toimintoja sekunnissa DBMS palvelimessa olisi mitattuna. Äskettäin suunnitellun Systemsin koonmuuttokahvan Harjoitus uuden järjestelmän myös tulisi ilmoittaa karkea i/o-vaatimukset ideoita DBMS reunassa. Jos ole varma, sinun on myöhemmin Järjestä kuitti käsite.

**Vaihe 3**: verrata SAP vaatimus Azure AM erityyppiset tarjoavat SAP DBMS palvelimeen. SAP-Huomautus [1928533]on kuvattu Azure AM erityyppisiä SAP tiedot. Kohdistuksen pitäisi olla DBMS AM ensin tietokannan kerros kerroksen on SAP NetWeaver järjestelmä, joka ei skaalaudu ominaisuuksissa useimpia ulos. Sen sijaan SAP-sovelluskerroksen skaalata ulos. Jos mikään SAP tuettu Azure AM tyypit voit toimittaa tarvittavat SAP, suunnitellun SAP-järjestelmää työmäärää ei voi suorittaa Azure. Voit joko täytyy ottaa käyttöön järjestelmän paikalliseen tai sinun on muutettava järjestelmän kuormituksen äänenvoimakkuutta.

**Vaihe 4**: kuin kuvata [tähän][virtual-machines-sizes], Azure pakottaa IOPS-kiintiön Näennäiskiintolevyn riippumattoman kohti, käytätkö vakio tallennus- tai Premium-tallennustilan. Määräytyy AM tyyppi näennäiskiintolevyjen, joka voidaan asentaa määrä vaihtelee. Voit laskea tuloksena IOPS enimmäismäärä, jotka voidaan kunkin AM erityyppisiä kanssa. Määräytyy tietokannan tiedoston asettelun voit voit stripe näennäiskiintolevyjen luomisella Vieras OS yhden tilavuus. Jos nykyinen IOPS äänenvoimakkuuden käyttöön SAP-järjestelmää ylittää suurimman AM tyyppi Azure ja onko mahdollisuuden hyvittää kanssa muistia laskettu rajoitukset, SAP-järjestelmää työmäärää voi heikentyä vakavasti. Tässä tapauksessa valitset kohta, johon olisi Ota Azure järjestelmään.

**Vaihe 5**: erityisesti SAP-järjestelmiin, jotka ovat käyttöön paikallisesti 2 tason määritykset, hyvinkin, että järjestelmä on ehkä reititystiedot Azure 3 tason määrityksessä. Tässä vaiheessa sinun tarvitse tarkistaa, onko SAP-sovelluskerroksen, joka voi skaalata ja joka ei mahdu Azure AM erilaista tarjota suorittimen ja muistin resursseja on osa. Jos todella näkyvissä on esimerkiksi osa, SAP-järjestelmää ja sen työmäärää ei voi ottaa käyttöön Azure kyselyjä. Mutta jos olet voit skaalaus-kohtaa SAP-sovelluksen osat useita Azure VMs kyselyjä, järjestelmä käyttöön Azure kyselyjä. 

**Vaihe 6**: Jos DBMS ja SAP-sovelluksen kerros-osat voidaan suorittaa Azure VMs, kokoonpano on määritettävä suhteessa:

* Azure VMs määrä
* AM yksittäisten osien tyypit
* DBMS AM antamaan tarpeeksi IOPS näennäiskiintolevyjen määrä

## <a name="managing-azure-assets"></a>Azure resurssien hallinta

### <a name="azure-portal"></a>Azure Portal
Azure-portaalissa on kolme liittymät hallittavan Azure AM ominaisuuksissa. Basic hallintatehtäviä, kuten käyttöönotto VMs kuvia, voit tehdä palvelun Azure-portaalissa. Lisäksi luomisen tallennustilan asiakkaat, Virtual verkkojen ja muut Azure osat ovat myös Azure-portaalissa voit käsitellä hyvin tehtäviä. Kuitenkin toimintoja, kuten lataamisen näennäiskiintolevyjen-ja Azure paikallista tai kopioimalla sisällä Azure Näennäiskiintolevyn ovat tehtävät, joita tarvitaan muiden valmistajien työkaluja tai hallinta PowerShellin tai CLI avulla.
 
![Microsoft Azure Portal - virtuaalikoneen yleiskatsaus][planning-guide-figure-800]

[Kommentti]: <>  (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[Kommentti]: <>  (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Hallinta ja määritys virtuaalikoneen esiintymän tehtävät ovat mahdollista Azure-portaalissa. 

Käynnistämistä ja Virtual Machine suljetaan lisäksi voit myös liittää, irrottaminen ja luoda tietojen levyjen virtuaalikoneen-esiintymän esiintymän kuva valmistelu ja virtuaalikoneen esiintymän enimmäiskoon määrittäminen.

Azure-portaalissa on perustoiminnot käyttöönotto ja määrittäminen VMs ja monia muita Azure palveluita. Azure-portaalin piiriin kuitenkin ei kaikki käytettävissä olevat toiminnot. Azure-portaalissa ei ole mahdollista suorittaa tehtäviä, kuten:

* Azure näennäiskiintolevyjen lataaminen
* VMs kopioiminen

[Kommentti]: <>  (MShermannd TODO Entä automaatio-palvelua SAP VMs?)
[Kommentti]: <>  (Useita VMs os tänä mahdollista MSSedusch käyttöönotto)
[Kommentti]: <> Myös MSSedusch  (kaikenlaisia automaatio koskeva käyttöönoton ei ole mahdollista Azure-portaalissa. Tehtävät, kuten useita VMs komentosarjalla toteutettavan käyttöönoton ei ole mahdollista kautta Azure-portaalin.) 

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>Hallinnan kautta Microsoft Azure PowerShellin cmdlet-komennot
Windows PowerShellin on tehokas ja laajennettavissa kehys, jotka on hyväksytty laajasti asiakkaiden käyttöönotto Azure järjestelmien suurempi määrä. Työpöydän, kannettava tietokone tai Oma hallinta-asema PowerShellin cmdlet-komennot asennuksen jälkeen PowerShellin cmdlet-komennot voidaan suorittaa etäyhteyden välityksellä.

Paikallinen työpöydän tai kannettavan tietokoneen Azure PowerShellin cmdlet-komennot ja niiden määrittäminen käyttäjille ja Azure tilauksia käyttö on kuvattu [tämän artikkelin]käytön sallimiseksi prosessin[powershell-install-configure]. 

Tarkempia ohjeita siitä, miten voit asentaa, päivittäminen ja määrittäminen Azure PowerShellin cmdlet-komennot löytyvät myös [tämän luvun käyttöönotto-oppaan] [käyttöönotto-oppaan-4.1].

Asiakkaan kokemukset ratkaisut on PowerShell (PS) on varmasti tehokkaampia työkalun ottamaan VMs ja luo mukautettuja VMs käyttöönoton. Kaikki käynnissä SAP esiintymät Azure asiakkaat täydentää hallintatehtäviä ne tehdään Azure-portaalin tai jopa käytät PS cmdlet-komennot ainoastaan, jos haluat hallita niiden ominaisuuksissa Azure-PS cmdlet-komentojen avulla. Azure tietyn cmdlet-komennot jakaa saman nimeämiskäytäntöä kuin yli 2 000 Windows liittyvät cmdlet-komennot, se on helppoa Windows-järjestelmänvalvojat voivat hyödyntää näiden cmdlet-komennot.

Katso tässä esimerkissä: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[Kommentti]: <>  (MShermannd TODO kuvaavat uusi CLI-komentoa, kun)
Azure seuranta tunnisteen SAP: n käyttöönoton (katso luvun [Azure seuranta ratkaisu SAP] [suunnittelu-opas-9.1] Tässä asiakirjassa) on mahdollista vain PowerShell tai CLI kautta. Tämän vuoksi on pakollinen asetukset ja määritä PowerShell tai CLI, kun otat tai hallinta SAP NetWeaver-järjestelmän Azure-tietokannassa.  

Azure tarjoaa enemmän toimintoja, kuten uuden PS cmdlet-komennot ovat siirtymällä lisätään, joka edellyttää päivityksen Cmdlet-komentoja. Tämän vuoksi kannattaa tarkistaa Azure-lataussivustolle vähintään kerran kuukauden <https://azure.microsoft.com/downloads/> uuden version Cmdlet-komentoja. Uusi versio asennetaan vain vanhemman version päälle.

Varten Azure Yleinen luettelo liittyvät PowerShell-komennoilla Etsi täältä: <https://msdn.microsoft.com/library/azure/dn708514.aspx>. 

### <a name="management-via-microsoft-azure-cli-commands"></a>Microsoft Azure CLI komennot kautta hallinta

Asiakkaat, jotka käyttävät Linux ja haluat hallita Azure resurssien Powershell eivät välttämättä ole haluamasi vaihtoehto. Microsoft tarjoaa Azure CLI vaihtoehtona.
Azure-CLI on joukko Avaa lähde Office kaikissa ympäristöissä komennot Azure-ympäristön käyttämisen. Azure-CLI tarjoaa paljon löydy Azure-portaalissa samalla tavalla.

Lisätietoja asentaminen, määrittäminen ja käyttäminen CLI komentoja, joilla voit suorittaa Azure tehtäviä on

* [Asenna Azure CLI][xplat-cli]
* [Ottaa käyttöön ja hallita näennäiskoneiden Azure Resurssienhallinta mallit ja Azure-CLI][virtual-machines-linux-cli-deploy-templates]
* [Käytä Azure CLI Mac, Linux ja Windows kanssa Azure resurssien hallinta][xplat-cli-azure-resource-manager]

Lue myös luvun [Azure Linux VMs CLI] [käyttöönotto-oppaan-4.5.2]-[käyttöönotto-oppaan] [-suunnitteluoppaan] käyttämisestä Azure CLI Azure seuranta tunnisteen SAP: n käyttöön.

## <a name="different-ways-to-deploy-vms-for-sap-in-azure"></a>Ottaa käyttöön SAP Azure VMs käyttötapoja
Tämän luvun kerrotaan eri tavoista, joilla ottamaan AM Azure-tietokannassa. Valmistelun menettelyt sekä näennäiskiintolevyjen ja VMs Azure käsittely on kuvattu tämän luvun.

### <a name="deployment-of-vms-for-sap"></a>SAP: n VMs käyttöönotto
Microsoft Azure tarjoaa useita tapoja VMs ja liittyvän levyjen käyttöönotto. Näin on tärkeää ymmärtää erot, koska VMs valmistelut saattavat vaihdella sen käyttöönoton menetelmän mukaan. Yleensä otamme katsaus seuraavissa tilanteissa:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Siirtäminen AM paikallisen Azure-generalized avulla
Aiot tietyn SAP-järjestelmää Siirry Azure-ympäristöön. Tämä voidaan tehdä lataamalla Näennäiskiintolevyn, joka sisältää OS, SAP binaaritiedostot ja DBMS binaaritiedostoja plus näennäiskiintolevyt DBMS Azure tiedot ja kirjaudu-tiedostoja. Kontrastia [skenaarion #2 alla], [suunnittelu-opas-5.1.2] säilytetään isäntänimi, SAP SUOJAUSTUNNUS ja SAP-käyttäjätilit-Azure AM, kun ne on määritetty paikallisen ympäristön. Tämän vuoksi joilla kuvaa ei tarvitse. Katso luvuissa [valmistelu siirtymisen AM paikallisen Azure-generalized levyjen] [suunnittelu-opas-5.2.1] paikallisen valmistavia vaiheita ja lataa-generalized VMs tai näennäiskiintolevyjen Azure tämän asiakirjan. Lue luvun [tapaus 3: siirrettäessä AM paikallisen Näennäiskiintolevyn Azure-generalized käyttäminen SAP] [käyttöönotto-oppaan-3.4]-[käyttöönotto-oppaan] [käyttöönotto-oppaan] yksityiskohtaisia ohjeita käyttöönottamisesta kuva, Azure-tietokannassa.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Asiakkaan tietyn kuvan AM käyttöönotto
Vuoksi tietyn korjaustiedoston vaatimukset OS tai DBMS-versio annettu kuvat Azure Marketplacesta ehkä ei vastaa tarpeitasi. Sen vuoksi voit joutua muuttamaan luominen AM, käyttämällä "Yksityinen" Omat OS/DBMS AM kuvaa, joka voidaan ottaa käyttöön useita kertoja jälkeenpäin. Yksityinen' kuvan valmisteleminen monistaminen, seuraavat kohteet on otettava huomioon:

___

> ![Windows][Logo_Windows] Windows
>
> Windows-asetukset (kuten Windows SUOJAUSTUNNUS ja hostname) on oltava otetaan/generalized-paikallisen-AM sysprep-komennon kautta.
[Kommentti]: <>  (MSSedusch > Lisätietoja tässä artikkelissa:)
[Kommentti]: <>  (MShermannd TODO ensimmäisen linkin on perinteinen mallin tietoja. Ei löydä Azure helppokäyttötoimintoa artikkelin)
[Kommentti]: <>  (MSSedusch >< https://azure.microsoft.com/documentation/articles/virtual-machines-create-upload-vhd-windows-server/>)
[Kommentti]: <>  (MSSedusch >< http://blogs.technet.com/b/blainbar/archive/2014/09/12/modernizing-your-infrastructure-with-hybrid-cloud-using-custom-vm-images-and-resource-groups-in-microsoft-azure-part-21-blain-barton.aspx>)
>
> ![Linux][Logo_Linux] Linux
>
> Ohjeiden näissä artikkeleissa on kuvattu, [SUSE] [ virtual-machines-linux-create-upload-vhd-suse] tai [Punainen on] [ virtual-machines-linux-redhat-create-upload-vhd] valmisteleminen ladattava Azure Näennäiskiintolevyn.

___

Jos olet jo asentanut SAP sisällön paikallisen-AM (erityisesti niiden 2 tason järjestelmät), voit mukauttaa SAP-järjestelmäasetuksista jälkeen kautta esiintymän Azure-AM käyttöönoton nimeä toimintosarjan tukemat SAP ohjelmiston valmistelu Manager (SAP Huomautus [1619720]). Katso luvuissa [käyttöönotto AM asiakkaan jotakin tiettyä kuvaa ja SAP: n valmistelu] [suunnittelu-opas-5.2.2] ja [lataamisesta, Azure-paikallisen Näennäiskiintolevyn] [suunnittelu-opas-5.3.2] paikallisen valmistavia vaiheita ja generalized AM Azure, Lataa tämän asiakirjan. Lue luvun [tapaus 2: käyttöönotto AM mukautetun kuvan SAP: n] [käyttöönotto-oppaan-3.3]-[käyttöönotto-oppaan] [käyttöönotto-oppaan] yksityiskohtaisia ohjeita käyttöönottamisesta kuva, Azure-tietokannassa.

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Käyttöönotto AM ulos Azure Marketplacesta
Haluat käyttää Microsoft tai 3 osapuolen annettu AM kuva ottamaan yhteyttä AM Azure Marketplacesta. Azure oman AM käyttöönoton seuraamasi saman ohjeita ja asenna SAP-ohjelmiston ja/tai DBMS sisällä oman AM tapaan paikallisen ympäristön työkalut. Tarkemmat käyttöönoton kuvaus, katso luvun [tapaus 1: käyttöönotto AM ulos SAP Azure Marketplacesta] [käyttöönotto-oppaan-3,2]-[käyttöönotto-oppaan] [käyttöönotto-oppaan].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>SAP: N kanssa VMs valmisteleminen Azure
Ennen lataamista VMs kyselyjä Azure, varmista, että tarvitset VMs ja näennäiskiintolevyjen täyttävät tietyt vaatimukset. Sen mukaan, käyttöönottomenetelmä, jota käytetään pieniä eroja. 

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Siirtymisen AM paikallisen Azure-generalized levyjen valmistelu
Yleisiä käyttöönottomenetelmä on siirrettävä aiemmin AM, joka suoritetaan SAP-järjestelmää paikallisen from Azure. Että AM ja AM SAP-järjestelmää vain olisi toimivat Azure saman isäntänimi ja hyvin todennäköisesti saman SAP-SUOJAUSTUNNUS. Tässä tapauksessa Vieras AM OS olisi generalized ei useita käyttöönotoissa. Jos paikalliseen verkkoon, onko käytössäsi laajentamista Azure (katso luvun [rajat tiloissa - käyttöönoton yhden tai useita kyselyjä Azure vaatimus on täysin integroitu paikalliseen verkkoon ja SAP VMs] [suunnittelu-opas-2.2] Tässä asiakirjassa), valitse myös saman toimialuetilit voidaan käyttää AM, kun ne on käyttänyt paikallisen. 

Oman Azure AM levyn valmisteltaessa vaatimukset ovat seuraavat:

* Alun perin sisältävä käyttöjärjestelmän Näennäiskiintolevyn voi olla koko on enintään 127 gt vain. Tämä rajoitus käytössä on poistettu maaliskuussa 2015 lopussa. Nyt sisältävä käyttöjärjestelmän Näennäiskiintolevyn voi olla enintään 1 TT kokoinen, kuten Azure-tallennustilan isännöidään Näennäiskiintolevyn sekä.
[Kommentti]: <>  (Voit tarkistaa, onko CLI muuntaa staattiseksi on MShermannd TODO)
* Se on oltava kiinteä Näennäiskiintolevyn-muodossa. Dynaaminen näennäiskiintolevyjen tai näennäiskiintolevyjen VHDx muodossa ei vielä tueta Azure. Dynaaminen näennäiskiintolevyjen muunnetaan staattiseksi näennäiskiintolevyjen kun lataat Näennäiskiintolevyä PowerShell komentosovelmat kanssa tai CLI
* Näennäiskiintolevyjen, jotka on otettu käyttöön AM- ja on asennettava uudelleen Azure on kiinteä Näennäiskiintolevyn muodossa sekä AM tarvetta. Sama kokorajoitus OS levyn koskee myös tietojen levyjä. Näennäiskiintolevyjen voi olla enimmäiskoko on 1 Teratavua. Dynaaminen näennäiskiintolevyjen muunnetaan staattiseksi näennäiskiintolevyjen kun lataat Näennäiskiintolevyä PowerShell komentosovelmat kanssa tai CLI
* Lisää toinen paikallisen tilin ja järjestelmänvalvojan oikeudet, joita voidaan käyttää Microsoft-tuen tai joka voidaan varata kontekstissa palvelut ja sovellukset toimimaan, kunnes AM on otettu käyttöön ja tarkoituksenmukaisempia käyttäjät voidaan käyttää.
* Siltä varalta, käyttää vain Pilvipalveluita käyttöönottotapa (katso luvun [vain Pilvipalveluita - virtuaalikoneen ominaisuuksissa kyselyjä Azure ilman riippuvuudet paikallisen asiakkaan verkossa] [suunnittelu-opas-2.1] tämän asiakirjan) yhdessä käyttöönoton tätä menetelmää toimialuetilit ei ehkä toimi kun Azure-levy on otettu käyttöön Azure. Tämä koskee erityisesti tilit, joita käytetään palvelujen, kuten DBMS tai SAP-sovellusten suorittamiseen. Tämän vuoksi tarvitset tällaisten toimialuetilit korvaaminen AM paikalliset tilit ja poistaa AM paikallisen toimialuetilit. Paikallisen Toimialuekäyttäjät pitäminen AM kuvassa ei ole ongelma kun AM otetaan käyttöön paikallisen-skenaario kuvatulla tavalla luvun [rajat tiloissa - käyttöönoton yhden tai useita kyselyjä Azure vaatimus on täysin integroitu paikalliseen verkkoon ja SAP VMs] [suunnittelu-opas-2.2] Tässä asiakirjassa.
* Jos toimialuetilit käytettiin DBMS kirjautumiset tai käyttäjät, kun järjestelmän paikalliseen ja näiden VMs on määritetty käyttöön vain Pilvipalveluita tilanteissa, Toimialuekäyttäjät on poistetaan. Haluat varmistaa, että paikallisen järjestelmänvalvojan sekä toinen AM paikallinen käyttäjä lisätään login/käyttäjänä DBMS järjestelmänvalvojia.
* Lisää muita paikallisten käyttäjätilien ne voidaan tarvita tietyn käyttöönottotapa.

___

> ![Windows][Logo_Windows] Windows
>
> Tässä skenaariossa ei Yleistys (sysprep) AM tarvitaan ladata ja asentaa Azure-AM.
> Varmista, että, että asema D:\ ei voi käyttää Aseta levy automount levyjen kuvatulla tavalla luvun [asetus levyjen automount] [suunnittelu-opas-liitteen 5.5.3] Tässä asiakirjassa.
> 
> ![Linux][Logo_Linux] Linux
>
> Tässä skenaariossa ei Yleistys (waagent-deprovision) AM, voit ladata ja asentaa Azure-AM tarvitaan.
> Varmista, että/mnt/resurssin ei käytetä ja kaikkien levyjen ovat käytettävissä uuid kautta. Varmista OS levy, Käynnistyslataimen tapahtuma kuvastaa myös uuid perustuva käyttöönoton.

___

#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>SAP: n käyttöönoton AM asiakkaan tietyn kuvan valmistelu
Näennäiskiintolevytiedostoja, jotka sisältävät generalized OS tallennetaan myös säilöt Azure-tallennustilan tilit. Voit ottaa käyttöön uuden AM, esimerkiksi kuvan Näennäiskiintolevyn viittaamalla Näennäiskiintolevyn lähteenä käyttöönoton mallitiedostot Näennäiskiintolevyn kuvatulla tavalla luvun [tapaus 2: käyttöönotto AM mukautetun kuvan SAP: n] [käyttöönotto-oppaan-3.3] [käyttöönotto-oppaan] [käyttöönotto-oppaan]. 

Vaatimukset, kun valmistelet oman Azure AM kuva ovat seuraavat:

* Alun perin sisältävä käyttöjärjestelmän Näennäiskiintolevyn voi olla koko on enintään 127 gt vain. Tämä rajoitus käytössä on poistettu maaliskuussa 2015 lopussa. Nyt sisältävä käyttöjärjestelmän Näennäiskiintolevyn voi olla enintään 1 TT kokoinen, kuten Azure-tallennustilan isännöidään Näennäiskiintolevyn sekä.
[Kommentti]: <>  (Voit tarkistaa, onko CLI muuntaa staattiseksi on MShermannd TODO)
* Se on oltava kiinteä Näennäiskiintolevyn-muodossa. Dynaaminen näennäiskiintolevyjen tai näennäiskiintolevyjen VHDx muodossa ei vielä tueta Azure. Dynaaminen näennäiskiintolevyjen muunnetaan staattiseksi näennäiskiintolevyjen kun lataat Näennäiskiintolevyä PowerShell komentosovelmat kanssa tai CLI
* Näennäiskiintolevyjen, jotka on otettu käyttöön AM- ja on asennettava uudelleen Azure on kiinteä Näennäiskiintolevyn muodossa sekä AM tarvetta. Sama kokorajoitus OS levyn koskee myös tietojen levyjä. Näennäiskiintolevyjen voi olla enimmäiskoko on 1 Teratavua. Dynaaminen näennäiskiintolevyjen muunnetaan staattiseksi näennäiskiintolevyjen kun lataat Näennäiskiintolevyä PowerShell komentosovelmat kanssa tai CLI
* Koska Toimialuekäyttäjät rekisteröity AM käyttäjien ei ole vain Pilvipalveluita tilanne (katso luvun [vain Cloud - virtuaalikoneen ominaisuuksissa kyselyjä Azure ilman riippuvuudet paikallisen asiakkaan verkossa] [suunnittelu-opas-2.1] tämän asiakirjan), -palveluita käyttämällä toimialuetta tilit eivät ehkä toimi, kun kuva on otettu käyttöön Azure. Tämä koskee erityisesti tilit, joita käytetään suorittaa palveluja, kuten DBMS tai SAP-sovelluksia. Tämän vuoksi tarvitset tällaisten toimialuetilit korvaaminen AM paikalliset tilit ja poistaa AM paikallisen toimialuetilit. Säilyttämällä AM kuvan paikallisen Toimialuekäyttäjät eivät ehkä ole ongelma kun AM otetaan käyttöön rajat tiloissa skenaariossa kuvatulla tavalla luvun [rajat tiloissa - käyttöönoton yhden tai useita kyselyjä Azure vaatimus on täysin integroitu paikalliseen verkkoon ja SAP VMs] [suunnittelu-opas-2.2] Tässä asiakirjassa.
* Lisää toinen paikallisen tilin ja järjestelmänvalvojan oikeudet, joita voidaan käyttää Microsoft-tuen ongelma tutkimuksia tai joka voidaan varata kontekstissa palvelut ja sovellukset toimimaan, kunnes AM on otettu käyttöön ja lisää haluamasi käyttäjät voidaan käyttää.
* Vain Pilvipalveluita ominaisuuksissa ja jossa, jonka toimialuetilit käytettiin DBMS kirjautumiset tai käyttäjät, kun käytössä järjestelmän paikalliseen Poistetaanko Toimialuekäyttäjät. Haluat varmistaa, että paikallisen järjestelmänvalvojan sekä toinen AM paikallinen käyttäjä lisätään järjestelmänvalvojia DBMS login/käyttäjäksi.
* Lisää muita paikallisten käyttäjätilien ne voidaan tarvita tietyn käyttöönottotapa.
* Jos kuva on SAP NetWeaver asennuksen ja isännän nimen Azure käyttöönoton sekä alkuperäinen nimi on todennäköisesti kannattaa kopioida mallin SAP ohjelmiston valmistelu hallinta DVD-levyn uusinta versiota. Tämän avulla voit mukauttaa muutetut isäntänimi ja/tai muuttaa sisällä käyttöön AM kuva SAP-järjestelmää SUOJAUSTUNNUS heti, kun uuden aloitetaan helposti SAP annettu nimeä uudelleen-toimintojen avulla.

___

> ![Windows][Logo_Windows] Windows
>
> Varmista, että, että asema D:\ ei voi käyttää Aseta levy automount levyjen kuvatulla tavalla luvun [asetus levyjen automount] [suunnittelu-opas-liitteen 5.5.3] Tässä asiakirjassa.
> 
> ![Linux][Logo_Linux] Linux
>
> Varmista, että/mnt/resurssin ei käytetä ja kaikkien levyjen ovat käytettävissä uuid kautta. Käynnistyslataimen tapahtuma kuvastaa myös OS levyn Varmista, että uuid perustuva käyttöönoton.

___

* SAP-Graafisen (for järjestelmänvalvojan ja tarkoituksiin määritys) on valmiiksi asennettuna tällaisten mallissa.
* Muiden ohjelmien VMs suoritettava onnistuneesti paikallisen-skenaariot voidaan asentaa, kunhan tämän ohjelmiston käsitellä AM nimeä.

Jos AM on valmisteltu riittävän Yleinen ja tekstin riippumaton tilit/käyttäjät kohdennettujen Azure käyttöönottotapa ei ole käytettävissä, joilla tällaista kuvaa viimeinen valmistelu vaihe suoritetaan.

##### <a name="generalizing-a-vm"></a>Joilla AM 

___

[Kommentti]: <>  (MShermannd TODO tarvitse parempia artikkeleita / helppokäyttötoimintoa tietoja VMs ARM, joilla)
> ![Windows][Logo_Windows] Windows
>
> Viimeinen vaihe on kirjautua AM järjestelmänvalvojatilillä. Avaa Windows-komentoikkunassa "järjestelmänvalvojana". Siirry ...\windows\system32\sysprep ja Suorita sysprep.exe.
> Pieni ikkuna tulee näkyviin. On tärkeää Sammuta-asetuksen muuttaminen oletusarvoiset "Uudelleenkäynnistys"-'Sulkea' ja 'Yleistä' vaihtoehto (oletusarvo on ei valittu). Tässä toimintosarjassa oletetaan, että Sysprepin prosessi on suoritettu paikallisen AM Vieras-käyttöjärjestelmässä.
> Jos haluat toiminnon suorittaminen AM, joka on jo käynnissä Azure kanssa, noudata [Tässä artikkelissa]kuvattujen vaiheiden[virtual-machines-windows-capture-image].
> 
> ![Linux][Logo_Linux] Linux
>
> [Voit siepata Linux virtual kone käyttämään Resurssienhallinta-mallina][virtual-machines-linux-capture-image]

___

### <a name="transferring-vms-and-vhds-between-on-premises-to-azure"></a>VMs ja näennäiskiintolevyjen siirtäminen paikalliseen Azure, välillä
Koska AM kuvia ja levyjen lataaminen Azure ei ole mahdollista Azure-portaalin kautta, sinun on käytettävä Azure PowerShellin cmdlet-komennot tai CLI. Toinen mahdollisuus on "AzCopy"-työkalun. Työkalu voi kopioida näennäiskiintolevyjen paikallisen ja Azure välillä (molempiin suuntiin). Se myös kopioida näennäiskiintolevyjen Azure alueiden välillä. Lue [Tämä] käyttöohjeista[ storage-use-azcopy] ladattavissa ja AzCopy käyttö.

Kolmas vaihtoehto on käyttää erilaisia kolmannen osapuolen Graafisen aloittaminen työkaluja. Varmista, että nämä työkalut tukevat sivun Azure-BLOB. Tämän vuoksi annettava käyttää sivun Azure-Blob-säilö (erot on kuvattu seuraavassa: <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>). Azure työkalujen on myös erittäin tehokkaasti pakkaaminen VMs ja näennäiskiintolevyjen, joka on ladattava. Tämä on tärkeää, koska tämä pakkaamisen tehokkuuden vähentää Lataa aikaa (joka ei ole enää sama silti sen mukaan, lataa linkin Internet paikallisen tilojen ja kohdistettu Azure käyttöönotto-alue). On tiedenäyttelyn olettaen, että lataaminen AM tai Näennäiskiintolevyn Euroopan sijainnista Yhdysvaltojen perusteella Azure tietojen keskikohdan mukaan kestää kauemmin kuin saman VMs/näennäiskiintolevyjen lataaminen Euroopan Azure tietojen keskikohdan mukaan. 

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Voit Azure-paikallisen Näennäiskiintolevyn lataaminen
Ladata olemassa olevan AM tai Näennäiskiintolevyn paikallisen verkosta tällaista AM tai Näennäiskiintolevyn on täytettävä vaatimukset luetellut luvun [siirtäminen AM paikallisen Azure-generalized levyjen valmistautumista] [suunnittelu-opas-5.2.1] tämän asiakirjan.

Näiden AM ei tarvitse olla generalized ja voi ladata tilan ja muodon reunan paikallisen tietokoneen sammuttamisen jälkeen on. Sama koskee muita näennäiskiintolevyjen, jotka eivät sisällä sellaisille käyttöjärjestelmille, varten. 

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Näennäiskiintolevyn lataamisesta ja tehdä siihen Azure-levyn
Tässä tapauksessa haluamme SITEN, kanssa tai ilman niitä OS uudelleen ja ota käyttöön AM kuin tietojen DVD-levyllä tai käyttää sitä OS levy. Tämä on monivaiheinen prosessi 

__PowerShellin__

* Kirjaudu sisään-tilaukseen _Kirjautuminen AzureRmAccount_ kanssa
* Määritä kontekstia _Määrittäminen AzureRmContext_ ja parametri SubscriptionId tai SubscriptionName tilaus - kohdassa <https://msdn.microsoft.com/library/mt619263.aspx>
* Lataa ja _Lisää AzureRmVhd_ Näennäiskiintolevyn Azure-tallennustilan tilin - kohdassa <https://msdn.microsoft.com/library/mt603554.aspx>
* Määritä uusi AM config OS-levyn Näennäiskiintolevyn _Määrittäminen AzureRmVMOSDisk_ kanssa - kohdassa <https://msdn.microsoft.com/library/mt603746.aspx>
* Voit luoda uuden AM AM config _Uusi AzureRmVM_ kanssa - kohdassa <https://msdn.microsoft.com/library/mt603754.aspx>
* Tietoja DVD-levyllä lisääminen uuteen AM _Lisää AzureRmVMDataDisk_ kanssa - kohdassa <https://msdn.microsoft.com/library/mt603673.aspx>

__Azure CLI__

* Siirry Azure Resurssienhallinta tilaa _azure config tilassa kädessä_
* Kirjautuminen ‑tilauksen _azure kirjautuminen_
* Valitse tilauksen kanssa _azuren määritetty `<subscription name or id` _
* Lataa ja _tallennustilaa azure-Blob-objektien lataaminen_ Näennäiskiintolevyn - ohjeaiheessa [Azure CLI kanssa Azuren tallennustilaan][storage-azure-cli]
* Luo uusi AM, lisäämällä ladatut Näennäiskiintolevyn OS levyn _azure AM luominen_ ja parametri -d
* Uusi AM kanssa _AM levyn liittää uudet_ tiedot DVD-levyllä lisääminen

__Malli__

* Lataa Näennäiskiintolevyn Powershell tai Azure CLI kanssa
* Ota käyttöön AM JSON-malli viittaat Näennäiskiintolevyn [Tämän esimerkin JSON mallin](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-from-specialized-vhd/azuredeploy.json)mukaisesti.

#### <a name="deployment-of-a-vm-image"></a>Käyttöönoton AM-kuva
Ladata olemassa olevan AM tai Näennäiskiintolevyn paikallisen verkosta voi käyttää Azure AM kuvana tällaista AM tai Näennäiskiintolevyn on ehtojen mukaan luvussa [käyttöönotto AM asiakkaan jotakin tiettyä kuvaa ja SAP: n valmistelu] [suunnittelu-opas-5.2.2] tämän asiakirjan.

* _Sysprepin_ käyttäminen Windows tai _waagent-deprovision_ Linux generalize oman AM – Katso, [miten voit siepata Windows-virtual machine Resurssienhallinta käyttöönoton mallin] [ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] tai [sieppaaminen Linux virtual kone käyttämään Resurssienhallinta-mallina][virtual-machines-linux-capture-image-capture]
* Kirjaudu sisään-tilaukseen _Kirjautuminen AzureRmAccount_ kanssa
* Määritä kontekstia _Määrittäminen AzureRmContext_ ja parametri SubscriptionId tai SubscriptionName tilaus - kohdassa <https://msdn.microsoft.com/library/mt619263.aspx>
* Lataa ja _Lisää AzureRmVhd_ Näennäiskiintolevyn Azure-tallennustilan tilin - kohdassa <https://msdn.microsoft.com/library/mt603554.aspx>
* Määritä uusi AM config OS-levyn Näennäiskiintolevyn kanssa _määrittäminen AzureRmVMOSDisk - SourceImageUri - CreateOption fromImage_ -kohdassa <https://msdn.microsoft.com/library/mt603746.aspx>
* Voit luoda uuden AM AM config _Uusi AzureRmVM_ kanssa - kohdassa <https://msdn.microsoft.com/library/mt603754.aspx>

__Azure CLI__

* _Sysprepin_ käyttäminen Windows tai _waagent-deprovision_ Linux generalize oman AM – Katso, [miten voit siepata Windows-virtual machine Resurssienhallinta käyttöönoton mallin] [ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] tai [Linux-virtual machine käyttämään Resurssienhallinta-mallina sieppaaminen] [ virtual-machines-linux-capture-image-capture] Linux varten
* Siirry Azure Resurssienhallinta tilaa _azure config tilassa kädessä_
* Kirjautuminen ‑tilauksen _azure kirjautuminen_
* Valitse tilauksen kanssa _azuren määritetty `<subscription name or id` _
* Lataa ja _tallennustilaa azure-Blob-objektien lataaminen_ Näennäiskiintolevyn - ohjeaiheessa [Azure CLI kanssa Azuren tallennustilaan][storage-azure-cli]
* Luo uusi AM, lisäämällä ladattujen Näennäiskiintolevyn OS levyn _azure AM luominen_ ja parametri -Q

__Malli__

* _Sysprepin_ käyttäminen Windows tai _waagent-deprovision_ Linux generalize oman AM – Katso, [miten voit siepata Windows-virtual machine Resurssienhallinta käyttöönoton mallin] [ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] tai [Linux-virtual machine käyttämään Resurssienhallinta-mallina sieppaaminen] [ virtual-machines-linux-capture-image-capture] Linux varten
* Lataa Näennäiskiintolevyn Powershell tai Azure CLI kanssa
* Ota käyttöön AM kuvaan Näennäiskiintolevyn, kuten [Tässä esimerkissä JSON](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json)-mallissa JSON-malli.

#### <a name="downloading-vhds-to-on-premises"></a>Näennäiskiintolevyjen lataaminen paikalliseen
Azure infrastruktuurin palvelu ei ole yksisuuntainen katu, että vain voit ladata näennäiskiintolevyt ja SAP systems. Voit siirtää SAP järjestelmien azuren takaisin paikalliseen maailman sekä.

Lataamisen aikana näennäiskiintolevyt ei voi olla käytössä. Vaikka lataamalla VMs näennäiskiintolevyjen, jotka on otettu käyttöön, AM on oltava Sammuta. Jos haluat ladata tietokannan sisältö mitä sitten käytetään määrittämään uuden järjestelmän paikalliseen ja jos se on hyväksyttävä, että lataamisen aikana ja uuden järjestelmän, että järjestelmä Azure-tietokannassa voi olla yhä toiminnallisia asennuksen aikana saattaa vältät pitkä käyttökatkot suorittamalla pakattu tietokannan varmuuskopiointi Näennäiskiintolevyn kyselyjä ja juuri lataa kyseisen Näennäiskiintolevyn sen sijaan, että myös lataamalla OS perus AM.

#### <a name="powershell"></a>PowerShellin

Kun SAP-järjestelmää pysäytetään ja AM suljetaan, voit käyttää PowerShell-cmdlet-komennon Tallenna AzureRmVhd paikallisen aikataulussa lataamaan Näennäiskiintolevyn levyjen takaisin paikalliseen maailmaa. Pspingiä, sinun on Näennäiskiintolevyn, jotka löytyvät URL-osoite Azure-portaalin (Siirry tallennustilan tilin ja tallennustilan säilö, jossa Näennäiskiintolevyn luotiin tarvetta) tallennustilan osaan ja sinun on tiedettävä, minne Näennäiskiintolevyn kopioidaan.

Valitse komento voidaan hyödyntää määrittämällä vain parametrin SourceUri URL-osoitteena Näennäiskiintolevyn lataaminen ja LocalFilePath fyysinen sijainniksi näennäiskiintolevyn (mukaan lukien sen nimeä). Komento voi näyttää samalta kuin:

```powerhell
Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
```

Lisätietoja Tallenna AzureRmVhd cmdlet-komento Tarkista tässä <https://msdn.microsoft.com/library/mt622705.aspx>. 

#### <a name="cli"></a>CLI

Kun SAP-järjestelmää pysäytetään ja AM suljetaan, voit käyttää Azure CLI komento azure tallennustilan blob Lataa paikallisen aikataulussa lataamaan Näennäiskiintolevyn levyjen takaisin paikalliseen maailmaa. Jotta voit tehdä sen nimi on ja Näennäiskiintolevyn, joka löytyy tallennustila kohdasta Azure-portaalin (Siirry tallennustilan tilin ja tallennustilan säilö, jossa Näennäiskiintolevyn luotiin tarvetta) säilön ja tarvitsee tietää, minne Näennäiskiintolevyn kopioidaan.

Valitse komento voidaan hyödyntää määrittämällä yksinkertaisesti parametrit Blob-objektien ja säilö näennäiskiintolevyn lataaminen ja kohde nimellä Fyysinen kohdesijainnin näennäiskiintolevyn (mukaan lukien sen nimeä). Komento voi näyttää samalta kuin:

```
azure storage blob download --blob <name of the VHD to download> --container <container of the VHD to download> --account-name <storage account name of the VHD to download> --account-key <storage account key> --destination <destination of the VHD to download> 
```

### <a name="transferring-vms-and-vhds-within-azure"></a>VMs ja näennäiskiintolevyjen sisällä Azure siirtäminen

#### <a name="copying-sap-systems-within-azure"></a>SAP-järjestelmiin sisällä Azure kopioiminen

SAP-järjestelmää tai jopa erillinen DBMS palvelimen tukevat SAP sovelluskerroksen muodostuvat todennäköisesti useita näennäiskiintolevyjen, jotka sisältävät joko OS binaaritiedostoja kanssa tai SAP-tietokannan tietojen ja log-tiedostot. Ole Azure toimintoja kopioiminen näennäiskiintolevyjen eikä Azure toimintoja näennäiskiintolevyjen tallentaminen levylle on synkronointi-järjestelmä joka tilannevedoksen useita näennäiskiintolevyjen synkronoidusti. Tämän vuoksi kopioidut tai tallennettu näennäiskiintolevyt tilan myös silloin, kun ne ovat käytettävissä vastaan samaan AM olisi eri. Tämä tarkoittaa, että ottaa eri tietoja eri näennäiskiintolevyjen sisältämien logfile(s) betonin tapauksessa loppuun tietokannan olisi ristiriidassa. 

**Tekemistä: Kopioi tai Tallenna näennäiskiintolevyjen, jotka ovat osa SAP-Järjestelmäkokoonpano haluat lopettaa SAP-järjestelmää ja lisäksi on otettu käyttöön AM Sammuta. Vasta sen jälkeen voit kopioida tai ladata Näennäiskiintolevyjen luominen joko Azure tai paikallisen kopion SAP-järjestelmää joukko.**

Tietoja levyjen näennäiskiintolevytiedostoja Azure-tallennustilan tilin tallennetaan, ja se voi olla suoraan virtual machine liittäminen tai voidaan käyttää kuvana. Tässä tapauksessa Näennäiskiintolevyn kopioidaan ennen beeing virtuaalikoneen yhdistyy toiseen sijaintiin. Azure-tietokannassa Näennäiskiintolevytiedosto koko nimen on oltava yksilöivä Azure. Kuten edellä jo aiemmin, nimi on millaisen kolmiosaisen nimi, joka näyttää tältä: 

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

##### <a name="powershell"></a>PowerShellin
Azure PowerShell cmdlet-komentojen avulla voit kopioida Näennäiskiintolevyn [Tässä]artikkelissa kuvatulla[storage-powershell-guide-full-copy-vhd].

##### <a name="cli"></a>CLI
Voit kopioida Näennäiskiintolevyn [Tässä] artikkelissa kuvatulla käyttämällä Azure CLI[storage-azure-cli-copy-blobs]

##### <a name="azure-storage-tools"></a>Azure tallennustilan Työkalut

* <http://azurestorageexplorer.Codeplex.com/Releases/View/125870>

Liittyy myös professional versiot Azure-tallennustilan hallinnasta joka täällä:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer> 


Tallennustilan tilin itse Näennäiskiintolevyn kopio on prosessi, joka kestää vain muutaman sekunnin (muistuttaa SAN laitteiston luominen tilannevedoksia kuitata kopio ja kopioi kirjoittaminen). Kun käytössäsi on Näennäiskiintolevytiedosto, voit liittää virtual machine tai käyttää sitä kuvana Näennäiskiintolevyn kopioita liittäminen näennäiskoneiden.

##### <a name="powershell"></a>PowerShellin

```powershell
# attach a vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path to vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of the vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <new path of vhd> -SourceImageUri <path to image vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption fromImage
$vm | Update-AzureRmVM
```
##### <a name="cli"></a>CLI
```
azure config mode arm 

# attach a vhd to a vm
azure vm disk attach <resource group name> <vm name> <path to vhd>

# attach a copy of the vhd to a vm
# this scenario is currently not possible with Azure CLI. A workaround is to manually copy the vhd to the destination.
```

#### <a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Azure-tallennustilan tilien välillä levyjen kopioiminen
Azure-portaalissa ei voi suorittaa tämän tehtävän. Voit ise: Azure PowerShell cmdlet-komennot, Azure CLI tai kolmannen osapuolen tallennustilan selaimessa. PowerShellin cmdlet-komennot tai CLI komentoja voi luoda ja hallita BLOB-objektit, jotka sisältävät voi kopioida asynkronisesti BLOB Storage tilien välillä ja Azure-tilauksen piiriin kuuluvien alueiden välillä.

##### <a name="powershell"></a>PowerShellin 

Kopiointi näennäiskiintolevyjen tilausten välillä on myös mahdollista. Katso [Lisätietoja artikkeli][storage-powershell-guide-full-copy-vhd]. 

PS cmdlet-logiikkaa basic työnkulku on seuraavanlainen:

* Luo lähde-tallennustilan tilin tallennustilan tilin kontekstissa, jossa _Uusi AzureStorageContext_ – Katso <https://msdn.microsoft.com/library/dn806380.aspx>
* Luo _Uusi AzureStorageContext_ tallennustilan tilin kontekstin kohde-tallennustilan tilin - kohdassa <https://msdn.microsoft.com/library/dn806380.aspx>
* Käynnistä kopion

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Kopioi kanssa silmukassa tilan tarkistaminen
 
```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Liittää uuden Näennäiskiintolevyn virtual machine yllä olevien ohjeiden mukaisesti.

Katso [tämän artikkelin] esimerkkejä[storage-powershell-guide-full-copy-vhd]

##### <a name="cli"></a>CLI
* Käynnistä kopion

```
  azure storage blob copy start --source-blob <source blob name> --source-container <source container name> --account-name <source storage account name> --account-key <source storage account key> --dest-container <target container name> --dest-blob <target blob name> --dest-account-name <target storage account name> --dest-account-key <target storage account name>
```

* Tarkista tila, jos kopioi silmukassa kanssa

```
azure storage blob copy show --blob <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```
  
* Liittää uuden Näennäiskiintolevyn virtual machine yllä olevien ohjeiden mukaisesti.

Katso [tämän artikkelin] esimerkkejä[storage-azure-cli-copy-blobs]

### <a name="disk-handling"></a>Levyn käsittely
#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>SAP-versioiden AM/Näennäiskiintolevyn rakenne
Rakenne AM ja liittyvän näennäiskiintolevyjen käsittelyn pitäisi olla erittäin yksinkertaisia. Paikallisen asennusten asiakkaiden kehittänyt monia tapoja rakenteen suunnitteluun server-asennuksen. 

* Yksi perus Näennäiskiintolevyn sisältää käyttöjärjestelmän ja DBMS ja/tai SAP binaaritiedostoja. Maaliskuussa 2015, koska tämä Näennäiskiintolevyn voi olla enintään 1 TT: n kokoisia aiemmissa rajoitukset, jotka rajoitettu 127 gigatavua sijaan. 
* Yhden tai usean näennäiskiintolevyjen, jossa on DBMS lokitiedosto SAP-tietokannan ja lokitiedoston DBMS temp tallennustilan alueen (Jos DBMS tukee tätä). Jos tietokannan lokin IOPS vaatimukset on suuri, joudut stripe useita näennäiskiintolevyjen päästäkseen tarvittava IOPS tilavuus. 
* Määrä näennäiskiintolevyjen, joka sisältää yksi tai kaksi tietokantatiedostoja SAP-tietokannan ja DBMS temp datatiedostot sekä (Jos DBMS tukee tätä).

![Viittaus Azure IaaS AM SAP: n määrittäminen][planning-guide-figure-1300]

[Kommentti]: <>  (MShermannd TODO kuvaavat Linux rakenne)

___

> ![Windows][Logo_Windows] Windows
>
> Useiden asiakkaiden kanssa on tuli määritykset, kuten SAP ja DBMS binaaritiedostot ei asennettu c:\-asema, johon Käyttöjärjestelmä on asennettu. Käytettävissä on useita syitä, mutta kun olemme tapahtui takaisin pääkansion, se yleensä on, että asemat on pieni ja OS päivityksiin tarvittavat ylimääräistä tilaa 10-15 vuosia sitten. Molemmat ehdot eivät vaikuta liian usein enää nämä päivää. Tänään c:\-asema voidaan määrittää erittäin levyjen tai VMs. Jos haluat säilyttää ominaisuuksissa yksinkertainen rakennetta, kannattaa noudattamalla seuraavia käyttöönoton kuviota, SAP NetWeaver Systemsin Azure-tietokannassa
>
> Windows-käyttöjärjestelmän sivutustiedoston pitäisi olla D:-asemassa (pysyvä levy) 
> 
> ![Linux][Logo_Linux] Linux
>
> Valitse /mnt/mnt/resurssin swapfile Linux-toimintoa sijoittaminen Linux [Tässä artikkelissa]kuvatulla tavalla[virtual-machines-linux-agent-user-guide]. Vaihda-tiedoston voi määrittää Linux agentti /etc/waagent.conf määritystiedostossa. Lisätä tai muuttaa seuraavia asetuksia:

```
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

Haluat ottaa muutokset käyttöön, sinun on käynnistettävä uudelleen ja Linux-agentti

```
sudo service waagent restart
```

Lue lisätietoja suositellut Vaihda tiedostokokoa SAP Huomautus [1597355]

___

Näennäiskiintolevyjen DBMS datatiedostot ja Azure-tallennustilan nämä näennäiskiintolevyjen isännöidään tyypin määrän määritettävä IOPS vaatimukset ja pakollinen viive. [Tässä artikkelissa] kuvattuja tarkka kiintiön[virtual-machines-sizes]

SAP-käyttöönotoissa viimeisen 2 vuosien kokemus opetettujen us joitakin opitut joka kuin yhteenvetoa:

* Eri datatiedostoihin IOPS liikenne ei aina sama koska aiemmat asiakkaan järjestelmät ehkä on eri tavalla esitystapa datatiedostot, joka edustaa niiden SAP-tietokannat. Tuloksena se poissa paremmin käyttävät RAID määritys päälle useita näennäiskiintolevyjen paikoilleen LUNs carved ne ulos datatiedostot. Käytettävissä on tilanteissa, erityisesti missä IOPS korko osumien yksittäisen Näennäiskiintolevyn vastaan DBMS tapahtumalokin kiintiön vakio Azure-tallennustilan. Näiden tilanteissa Premium tallennustilan käyttäminen on suositeltavaa tai yhdistäminen myös useita vakio tallennustilan-näennäiskiintolevyjen RAID-ohjelmistolla.

___

> ![Windows][Logo_Windows] Windows
>
> * [Suorituskyvyn SQL Server Azuren näennäiskoneiden parhaat käytännöt][virtual-machines-sql-server-performance-best-practices]
> 
> ![Linux][Logo_Linux] Linux
>
> * [Määritä ohjelmiston RAID Linux][virtual-machines-linux-configure-raid]
> * [LVM määrittämistä Linux-AM Azure-tietokannassa][virtual-machines-linux-configure-lvm]
> * [Azure tallennustilan tietoja ja Linux i/o optimointi](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)

___

* Premium tallennustila on näkyvissä merkittäviä toiminnallisuuden, erityisesti niiden tärkeän tapahtuman log kirjoituksia. SAP-tilanteessa, jotka on tarkoitus toimittaa tuotannon, kuten suorituskyvyn on erittäin suositeltavaa käyttää AM-sarjan, joka voidaan hyödyntää Azure Premium-tallennustilan.

Muista, että Näennäiskiintolevyn, joka sisältää Käyttöjärjestelmää, ja on suositeltavaa, SAP binaaritiedostoja ja tietokannan (perus AM) sekä, ei ole enää enintään 127 Gigatavua. Se nyt voi olla enintään 1 TT kokoinen. Tämä on oltava tarpeeksi tilaa, jos haluat säilyttää kaikki tarvittavaa tiedostoa, kuten SAP erän työn lokit.

Lisää ehdotuksia ja lisää tiedot, erityisesti DBMS VMs tilanteeseesi [DBMS käyttöönotto-oppaan] [dbms-opas]


#### <a name="disk-handling"></a>Levyn käsittely
Useimmissa tapauksissa sinun täytyy Luo muita levyjä, jotta voit asentaa SAP-tietokantaan AM. Puhuimme seikat enimmäismäärään näennäiskiintolevyjen luvun [AM/Näennäiskiintolevyn rakenteen SAP-versioiden] [suunnittelu-opas-5.5.1] tämän asiakirjan. Azure-portaalin sallii liittäminen ja irrottaminen levyjä, kun perus AM on otettu käyttöön. Levyjen voi olla liitetty/irrotettu AM ollessa ylös- ja käytössä sekä milloin se pysäytetään. Kun liität DVD-levyllä, Azure-portaalissa on liitettävä tyhjä levy tai aiemmin levy, joka tässä vaiheessa kohdassa aikaa ei ole liitetty toiseen AM. 

**Huomautus**: näennäiskiintolevyjen voidaan liittää vain yhden AM annetun milloin tahansa.
 
![Liitä tai irrota levyllä on vakio Azure-tallennustilan][planning-guide-figure-1400]

Sinun on määritettävä, haluatko luoda uuden ja tyhjien Näennäiskiintolevyn (jotka luoda saman tallennustilan tilin, AM ovat pohjana) tai onko, jonka haluat valita aiemmin Näennäiskiintolevyn, joka on ladattu aiemmin ja liitetään nyt AM. 

**Tärkeää**: joita **Et** haluat käyttää Host välimuistiin vakio Azure-tallennustilan. Jätä Host Cache tilaksi oletusarvo on ei mitään. Azure Premium tallennustilan ja ota käyttöön luku välimuistiin Jos i/o-ominaisuus on vain luku enimmäkseen kuten tyypillinen i/o-liikenne vastaan tietokannan datatiedostot. Jos tietokannan tapahtuman lokitiedoston ei välimuistiin suositellaan.

___

> ![Windows][Logo_Windows] Windows
>
> [Voit liittää tiedot DVD-levyllä Azure-portaalissa][virtual-machines-windows-attach-disk-portal]
>
> Jos levyjen on liitetty, tarvitset kirjautua Windows-levyn hallinnan avaaminen AM. Jos automount ei ole käytössä, kun suositellut luvun [asetus levyjen automount] [suunnittelu-opas-liitteen 5.5.3], juuri liitetyn äänenvoimakkuus on otettava online-tilassa ja valmisteltu.
>
> ![Linux][Logo_Linux] Linux
>
> Levyjen on liitetty, jos haluat kirjautua AM ja alustaa levyjä [Tässä artikkelissa] kuvatulla tavalla[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]

___

Uuden levyn on tyhjä levy, jos haluat muotoilla levyn sekä. Muotoilemiseen, erityisesti niiden DBMS tietojen ja lokitiedostojen DBMS paljas-versioiden kuin saman suositukset-koskevat.

Jo mainitut luvun [Microsoft Azure virtuaalikoneen käsite] [suunnittelu-opas-3,2] Azure-tallennustilan tilin ei tarjoa äärettömän resurssien muokata asemaa i/o IOPS ja tietojen äänenvoimakkuutta. Yleensä DBMS VMs vaikuttavat eniten tässä. Sinun kannattaa ehkä käytettävät kunkin AM erillinen tallennustilan-tili, jos sinulla on muutamia i/o määrää VMs otetaan käyttöön, jotta pysyt Azure-tallennustilan tilin äänenvoimakkuuden rajoissa. Muussa tapauksessa haluat nähdä, miten voit kokonaissaldo näitä VMs eri ilman pallolla jokaisen tilistä tallennustilan enimmäismäärä tallennustilan tilien välillä. Lisätietoja käsitellään [DBMS käyttöönotto-oppaan] [dbms-opas]. Näiden rajoitusten myös pidä mielessä täysin SAP-sovelluksen palvelimen VMs tai muita VMs, joka ilmestyy voi edellyttää muita näennäiskiintolevyjen.

Toisen aiheen, joka sopii tallennustilan tilit on, onko tallennustilan tilillä näennäiskiintolevyjen jää Geo replikoida. GEO replikoinnin käytössä vai ei-tallennustilan tilin taso tai ei AM tasolla. Jos geo replikointi on käytössä, tallennustilan tilin näennäiskiintolevyjen replikoida siirtäminen toiseen Azure tietokeskuksen samalla alueella. Ennen kuin valitset tämän, kannattaa ottaa huomioon seuraavat rajoitukset:

Azure Geo-replikoinnin toimii paikallisesti AM kunkin Näennäiskiintolevyn ja ei replikoida aikajärjestyksessä IOs-AM useita näennäiskiintolevyjen yli. Tämän vuoksi Näennäiskiintolevyn, joka edustaa perus AM sekä kaikki muut näennäiskiintolevyjen liitetyt AM, replikoida toisistaan riippumatta. Tämä tarkoittaa sitä ei voi synkronoida muutokset eri näennäiskiintolevyjen välillä. Kertoma IOs, replikoida ulkopuolisista siinä järjestyksessä, jossa ne on kirjoitettu tarkoittaa, että geo replikointia ei ole tietokannan palvelimet, joilla on useita näennäiskiintolevyjen suhteutettuna tietokantansa arvo. Lisäksi DBMS myös ehkä muissa sovelluksissa, jossa prosessit kirjoittaminen ja käsitellä tietoja eri näennäiskiintolevyjen ja jossa on tärkeää säilyttää muutokset järjestystä. Jos se on tarpeen, geo-replikointi Azure ei käytössä. Riippumatta siitä, tarvitsetko vai haluat geo replikoinnin VMs joukko, mutta ei toiset, jo luokittelemalla VMs ja niihin liittyvät näennäiskiintolevyjen tuominen eri tallennustilan-tilejä, joiden geo-replikoinnin käyttöön tai poistaa käytöstä.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Levyjen automount määrittäminen

___


> ![Windows][Logo_Windows] Windows
> 
> VMs, jotka on luotu oman kuvia tai levyjä se on tarpeen tarkistaa ja määrittää mahdollisesti automount-parametrin. Tämä parametrin määrittäminen Salli AM jälkeen uudelleenkäynnistyksen tai lukea Azure-tietokannassa liittämään liitetty otettu käyttöön asemat uudelleen automaattisesti. 
> Parametrin arvoksi asetetaan kuvien myöntämä Microsoft Azure Marketplacesta.
>
> Jotta voit määrittää automount, tarkista asiakirjat komento suoritettavan diskpart.exe tähän: 
> 
> * [DiskPart komentorivivalitsimet](https://technet.microsoft.com/library/cc766465.aspx)
> * [Automount](http://technet.microsoft.com/library/cc753703.aspx)
> 
> Windowsin komentorivi-ikkuna on avattava järjestelmänvalvojana.
> 
> Jos levyjen on liitetty, tarvitset kirjautua Windows-levyn hallinnan avaaminen AM. Jos automount ei ole käytössä, kun suositellut luvun [asetus levyjen automount] [suunnittelu-opas-liitteen 5.5.3], juuri liitetyn äänenvoimakkuuden > otettava online-tilassa ja valmisteltu.
>
> ![Linux][Logo_Linux] Linux
>
> Sinun täytyy alustaa juuri liitetyn tyhjä levy [Tässä artikkelissa]kuvatulla tavalla[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> Haluat myös lisätä uusia levyjä /etc/fstab.

___


### <a name="final-deployment"></a>Lopullinen käyttöönotto
Lopullinen käyttöönotto ja tarkat vaiheet, erityisesti tapauksen käyttöönottoa SAP laajennettu seuranta Tutustu [käyttöönotto-oppaan] [käyttöönotto-oppaan].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>SAP-järjestelmiin sisällä Azure VMs käyttäminen
Vain Pilvipalveluita tilanteessa haluat ehkä muodostaa ne SAP-järjestelmiin julkisen Internetin SAP Graafisen. Tällaisissa tapauksissa seuraavasti on voi käyttää.

Edempänä asiakirjassa käsittelemme toinen pää skenaario, yhteyden muodostaminen SAP-järjestelmiin paikallisen-käyttöönotoissa, joilla on sivuston sivuston yhteyden (VPN tunneli) tai Azure ExpressRoute paikallisen ja Azure järjestelmien välinen yhteys.


### <a name="remote-access-to-sap-systems"></a>SAP-järjestelmiin etäkäyttö

Azure resurssien hallinnan päätepisteitä ei ole oletusarvoisesti enää kuten entisen perinteinen mallin. Azure-HAARAN AM kaikki portit ovat avoinna, kunhan:

1. Ei ole verkko-käyttöoikeusryhmän määritetään aliverkon tai Verkkokäyttöliittymän. Azure VMs verkkoliikenteen voidaan suojata niin kutsutut "verkon käyttöoikeusryhmät" kautta. Katso lisätietoja [verkon suojauksen ryhmän (NSG) ominaisuudet?][virtual-networks-nsg]
1. Azure ei ole kuormituksen on määritetty verkkokäyttöliittymä   
 
Perinteinen mallin ja ARM arkkitehtuuri ero on [Tässä artikkelissa]kuvatulla tavalla[virtual-machines-azure-resource-manager-architecture].
 
#### <a name="configuration-of-the-sap-system-and-sap-gui-connectivity-for-cloud-only-scenario"></a>SAP-järjestelmää ja SAP Graafisen yhteydet vain Pilvipalveluita skenaarion määrittäminen
Tässä artikkelissa, jossa tiedot, joista tässä ohjeaiheessa kuvataan: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx> 

#### <a name="changing-firewall-settings-within-vm"></a>AM sisällä palomuurin asetusten muuttaminen
Voi olla tarpeen palomuurin määrittämiseksi oman näennäiskoneiden sallimaan saapuvan liikenteen SAP-järjestelmään. 

___

> ![Windows][Logo_Windows] Windows
>
> Azure käyttöön AM sisällä Windowsin palomuuri on käytössä oletusarvoisesti. Sinun on nyt avata SAP-portin sallittava, muuten SAP-Graafisen voi muodostaa.
> Toiminto:
>
>  * Avaa ohjausobjektin Panel\System ja Security\Windows palomuurin 'lisäasetusten".
>  * Napsauta nyt saapuvan liikenteen säännöt ja valitse sitten "Uusi sääntö".
>  * Valitse seuraavat '' uuden Porttisäännön luominen ohjatun toiminnon-Valitse.
>  * Ohjatun toiminnon seuraavassa vaiheessa jätä TCP ja kirjoita asetusta porttinumero, jonka haluat avata. Koska Microsoftin SAP-tunniste on 00, on kulunut 3200. Jos omassa on eri esiintymän numero, on määritetty portti aiemmin perusteella esiintymän voi avata.
>  * Ohjatun toiminnon seuraavaan osaan sinun on jättävät kohteen 'Salli yhteyden"valittuna.
>  * Seuraavassa vaiheessa ohjatun toiminnon, sinun täytyy määrittää sääntö koskee toimialue, yksityinen ja julkinen verkossa. Muuta sitä tarvittaessa tarpeitasi. Kuitenkin muodostaa yhteyden SAP Graafisen kanssa ulkopuolelle julkisen verkon kautta, tarvitset sääntöä käytetään yleiseen verkkoon.
>  * Ohjatun toiminnon viimeisessä vaiheessa haluat Anna säännölle nimi ja valitse Tallenna sääntö painamalla "Loppu"
>
>  Sääntö tulee voimaan heti.
>
> ![Portti säännön määritys][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> Azure Marketplacesta Linux kuvat Älä ota iptables palomuurin oletusarvoisesti ja SAP-järjestelmää yhteys pitäisi toimia. Jos iptables tai jokin muu palomuuri on käytössä, katso ohjeet iptables tai käyttää palomuurin sallimaan saapuvan tcp-liikenne paikalliseen portin 32xx (xx on SAP-järjestelmää järjestelmän määrä). 

___

#### <a name="security-recommendations"></a>Suojausta koskevia suosituksia

SAP-Graafisen ei yhteyttä heti kaikkiin SAP esiintymien (portin 32xx), jossa on käytössä, mutta muodostaa ensin avata viestin SAP-palvelimen prosessi (portin 36xx)-portin kautta. Aiemman hyvin samaa porttia on käytetty: n sisäisen tietoliikenteen sovellusten esiintymiä viestin palvelimeen. Jos haluat estää paikalliset sovelluksen palvelinten vahingossa tietoliikennettä Azure sisäisen tietoliikenteen portit voi muuttaa viestin palvelimen kanssa. On erittäin suositeltavaa muuttaa viestin SAP-palvelimen ja sen sovelluksen esiintymät sisäisen tietoliikenteen eri porttinumero järjestelmiin, joissa on on kopioitu paikallisen järjestelmistä, kuten Kloonaa kehitystä varten projektin testausta jne. Tämän voi tehdä oletusarvon profiili-parametrin kanssa:

>   rdisp/msserv_internal

Valitse suorittimessa: <https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm> 

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>Vain Pilvipalveluita käyttöönoton SAP esiintymien käsitteitä

### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Yksittäisen AM SAP NetWeaver esittely/koulutus skenaarion kanssa
 
![On yksittäinen AM SAP esittely järjestelmien AM samannimistä, eristää Azure pilvipalveluihin][planning-guide-figure-1700]

Tässä skenaariossa (luku [vain Cloud] [suunnittelu-opas-2.1] tämän artikkelin Katso) Microsoft toteuttaa tyypillinen koulutus/esittely järjestelmän tilanne missä valmis koulutus/esittely-skenaario sisältämät yksittäisen AM. Oletetaan käyttöönotto on valmis – AM avulla. Myös oletetaan, että nämä esittely/koulutukset kerrannaiseen VMs tarvitse voidaan ottaa käyttöön VMs, joilla on sama nimi.

Olettaen on luotu AM kuva kuvatulla tavalla joitakin osia luvun [kanssa SAP Azure valmistellaan VMs] [suunnittelu-opas-5.2] Tässä asiakirjassa.

Toteuttamisesta skenaarion tapahtumien järjestys on seuraavanlainen:

[Kommentti]: <>  (MShermannd TODO on annettava KÄDESSÄ otoksen kuvaus käyttämällä json mallin + selventää koskeva AM käyttäjänimen KÄDESSÄ virtual verkossa oleville)   
##### <a name="powershell"></a>PowerShellin

* Luo uusi resoure ryhmä, joka koulutus/esittely vaaka

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```

* Luo uusi tallennustilan-tili

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Luo uusi virtual verkko, joka koulutus/esittely vaaka käyttöön sama isäntänimi ja IP-osoitteiden käyttö. Virtuaalinen verkko on suojattu verkon käyttöoikeusryhmän, jonka avulla vain liikenteen portin 3389 etätyöpöydän käytön käyttöön ottaminen ja portin 22 SSH. 

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Luo uusi julkinen IP-osoite, jonka avulla voidaan käyttää virtuaalikoneen Internetistä

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Luo uuden Verkkokäyttöliittymän virtuaalikoneen varten

```powershell 
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip 
```

* Luo virtual machine. Vain Pilvipalveluita skenaarion jokaisen AM on sama nimi. SAP-SUOJAUSTUNNUS näiden VMs SAP NetWeaver-esiintymien on sama paikan päällä. Azure-resurssiryhmä AM nimen on oltava yksilöllinen, mutta eri Azure resurssin ryhmiin VMs voit suorittaa saman niminen. Windows-tai pääkansion, jos saat Linux "Järjestelmänvalvojan" oletustilin eivät ole kelvollisia. Tämän vuoksi uusi järjestelmänvalvojan käyttäjänimi on määritetty ja salasanan. Koon AM on myös määritettävä.

```powershell
#####
# Create a new virtual machine with an official image from the Azure Marketplace
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES" -Skus "12" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="os"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"
$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Windows
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Linux
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* Voit myös lisätä muita levyjä ja tarvittavat sisällön palauttaminen. Huomaa, että kaikki Blob-objektien nimet (URL BLOB-objektit) on oltava yksilöllinen Azure.

```powershell
# Optional: Attach additional data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM
```

##### <a name="cli"></a>CLI

Seuraava esimerkkikoodi voidaan Linux. (Windows) Poista Käytä PowerShell yllä olevien ohjeiden mukaisesti tai mukauttaa rgName % Käytä sen sijaan, että $rgName ja määritä käyttämällä _määrittäminen_Windows-komento-ympäristömuuttuja esimerkin.

* Luo uusi resoure ryhmä, joka koulutus/esittely vaaka

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
azure group create $rgName "North Europe"
```

* Luo uusi tallennustilan-tili

```
azure storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku-name LRS $rgNameLower
```

* Luo uusi virtual verkko, joka koulutus/esittely vaaka käyttöön sama isäntänimi ja IP-osoitteiden käyttö. Virtuaalinen verkko on suojattu verkon käyttöoikeusryhmän, jonka avulla vain liikenteen portin 3389 etätyöpöydän käytön käyttöön ottaminen ja portin 22 SSH. 

```
azure network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

azure network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
azure network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group-name SAPERPDemoNSG
```

* Luo uusi julkinen IP-osoite, jonka avulla voidaan käyttää virtuaalikoneen Internetistä

```
azure network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --domain-name-label $rgNameLower --allocation-method Dynamic
```

* Luo uuden Verkkokäyttöliittymän virtuaalikoneen varten

```
azure network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-name SAPERPDemoPIP --subnet-name Subnet1 --subnet-vnet-name SAPERPDemoVNet 
```

* Luo virtual machine. Vain Pilvipalveluita skenaarion jokaisen AM on sama nimi. SAP-SUOJAUSTUNNUS näiden VMs SAP NetWeaver-esiintymien on sama paikan päällä. Azure-resurssiryhmä AM nimen on oltava yksilöllinen, mutta eri Azure resurssin ryhmiin VMs voit suorittaa saman niminen. Windows-tai pääkansion, jos saat Linux "Järjestelmänvalvojan" oletustilin eivät ole kelvollisia. Tämän vuoksi uusi järjestelmänvalvojan käyttäjänimi on määritetty ja salasanan. Koon AM on myös määritettävä.

```
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn SUSE:SLES:12:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn RedHat:RHEL:7.2:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
```

```
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path to image vhd> --disable-boot-diagnostics
#azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path to image vhd> --disable-boot-diagnostics
```

* Voit myös lisätä muita levyjä ja tarvittavat sisällön palauttaminen. Huomaa, että kaikki Blob-objektien nimet (URL BLOB-objektit) on oltava yksilöllinen Azure.

```
# Optional: Attach additional data disks
azure vm disk attach-new --resource-group $rgName --vm-name SAPERPDemo --size-in-gb 1023 --vhd-name datadisk
```

##### <a name="template"></a>Malli
Voit käyttää github azure-pikaopas-mallit-tietovarasto esimerkkimallit.

* [Yksinkertainen Linux AM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Yksinkertainen Windows AM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [AM kuvasta](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-which-need-to-communicate-within-azure"></a>Toteuta VMs, jossa viestintään sisällä Azure joukko
Vain Pilvipalveluita artikkelista on tavallinen skenaario koulutukseen ja esittely tarkoituksiin where edustava esittely/koulutus ohjelmiston skenaario on levitä useita VMs. Eri osat, jotka on asennettu eri VMs on keskenään. Tässä skenaariossa ei ole paikallista verkon tietoliikenteen uudelleen tai paikallisen-skenaario tarvita.

Tässä skenaariossa on kuvattu luvun [yksittäisen AM ja SAP NetWeaver esittely/koulutus skenaarion] asennuksen laajennus [suunnittelu-opas-7.1] tämän asiakirjan. Tässä tapauksessa lisää näennäiskoneiden lisätään resurssi-ryhmään. Seuraavassa esimerkissä koulutus vaaka koostuu SAP ASCS/SCS AM, AM, tietokannan Hallintajärjestelmään ja SAP: N sovelluspalvelin erillisen AM.

Ennen kuin luot tässä skenaariossa haluat ottaa huomioon perusasetukset, kuten jo käyttäneet tilanne ennen.

#### <a name="resource-group-and-virtual-machine-naming"></a>Resurssiryhmä ja virtuaalikoneen nimeäminen
Kaikki resurssin ryhmien nimet on oltava yksilöllinen. Kehittää oman nimeämiskäytäntö värimallin resurssit, kuten `<rg-name`>-liitettä. 

Virtuaalikoneen nimen on oltava yksilöivä resurssiryhmän. 

#### <a name="setup-network-for-communication-between-the-different-vms"></a>Eri VMs väliseen viestintään verkon asennus
 
![VMs Azure Virtual verkon määrittäminen][planning-guide-figure-1900]

Jos haluat estää nimeät törmäykset saman koulutus/esittely maisemien kloonit, sinun on Azure Virtual verkoston, joka vaaka luominen. DNS-nimenselvitys toimittamien Azure tai voit määrittää omia DNS-palvelin Azure (ei voi edelleen käsitellään tähän) ulkopuolella. Tässä tapauksessa emme ei määritetä omissa DNS. Kaikki näennäiskoneiden yhden Azure Virtual verkoston sisällä isäntänimet viestintä otetaan käyttöön. 

Erota koulutukseen tai esittely maisemien virtual verkkojen ja ei vain resurssiryhmät syynä voi olla:

* SAP-vaaka määritetty on oma AD/OpenLDAP ja toimialueen palvelin on oltava osa maisemien.  
* SAP-vaaka määritetty on osat, jotka on kiinteä IP-osoitteet-käyttöä varten.

Lisätietoja Azure Virtual verkkojen ja miten voit määrittää ne löytyvät [tämän artikkelin][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>SAP-VMs käyttöönotto ja yrityksen verkkoyhteyden (paikallisen rajat)

Suorita SAP-vaaka ja haluat jakaa käyttöönoton paljas tehokkaimpia DBMS palvelinten välillä, paikallisen virtualisoidussa ympäristössä sovelluksen kerrokset ja pienempi 2 tason määritetty SAP-järjestelmiin ja Azure IaaS. Perus olettaen on, että SAP-järjestelmiin sisällä yhden SAP vaaka viestiä keskenään ja monia muita käyttöön yrityksessä riippumaton käyttöönoton lomakkeen ohjelmisto-osia. Myös pitäisi olla eroja otetaan käyttöön yhteyden SAP Graafisen tai muita liittymät peruskäyttäjän käyttöönoton lomakkeen mukaan. Näiden ehtojen on täytyttävä vain, kun paikallisen Active Directory-/ OpenLDAP ja sivusto-,-sivusto tai Avainhankkeiden monivaiheisen-site connectivity tai yksityisiä yhteyksiä, kuten Azure ExpressRoute Azure järjestelmiin laajennettu DNS-palvelut on.

Jotta saat lisää taustatietoja SAP Azure-käyttöönotto-tietoja, on suositeltavaa lukea luvun [Cloud-Only käsitteitä käyttöönoton SAP esiintymien] [suunnittelu-opas-7] tämän asiakirjan, jossa kerrotaan joistakin perustiedot-rakenteita Azure ja kuinka ne käytetään Azure SAP-sovellusten kanssa.

### <a name="scenario-of-a-sap-landscape"></a>SAP-vaaka skenaario

Paikallisen-skenaario voidaan karkeasti kuvata kuten alla olevassa kuvassa:
 
![Sivuston sivuston paikallisen ja Azure varat väliset yhteydet][planning-guide-figure-2100]

Yllä skenaarion kuvataan tilanne kohtaa, johon paikallista AD/OpenLDAP ja Azure jatketaan DNS. Paikallisen reunassa tiettyjä IP-osoitealueita on varattu Azure tilauskohtaisten. IP-osoitealueita määritetään Azure reunassa Azure Virtual-verkkoon.

#### <a name="security-considerations"></a>Suojausasiat

Vähimmäisvaatimukset on suojattu tietoliikenne-protokollia, kuten SSL/TLS käyttäminen selaimessa access- tai VPN-yhteyksien järjestelmä käyttää Azure palveluja. Olettaen on, että yritykset käsitellä välillä niiden yritysverkon ja Azure VPN-yhteyden hyvin eri tavalla. Jotkin yritykset blankly voi avata kaikki portit. Muiden yritysten hyvä olla erittäin tarkan portit kiinni tarvittaessa avata jne. 

Tyypillinen SAP alla olevassa taulukossa on lueteltu tietoliikenteen portit. Lähinnä on riittävä Avaa SAP yhdyskäytävän portti.

| Palvelun | Porttinimi | Esimerkki `<nn`> = 01 | Oletusaikaväli (min-max) | Kommentti |
|---------|-----------|-------------------|-------------------------|---------|
| Lähettäjä | sapdp`<nn>` Katso * | 3201 | 3200 – 3299 | SAP-lähettäjä, käyttämä SAP Graafisen for Windowsista tai Java |
| Viestin server | sapms`<sid`> Katso ** | 3600 | vapaa sapms`<anySID`> | suojaustunnus = SAP-järjestelmää-tunnus |
| Yhdyskäytävän | sapgw`<nn`> Katso * | 3301 | vapaa | SAP-yhdyskäytävää, käytetään CPIC ja RFC viestintään |
| SAP-reitittimen | sapdp99 | 3299 | vapaa | Vain CI (keskitetyn esiintymän)-Palvelunimet voidaan delegoida /etc/services haluamaansa arvo asennuksen jälkeen. |

*) nn = SAP esiintymän numero

*) suojaustunnus = SAP-järjestelmää-tunnus

Lisätietoja eri SAP-tuotteiden vaaditaan portit tai palvelujen SAP-tuotteiden Täältä löytyvät <http://scn.sap.com/docs/DOC-17124>. Sinun pitäisi Avaa Oma portit tarvittavat tietyn SAP-tuotteiden ja-skenaariot VPN-laite tästä asiakirjasta.

Muut toimenpiteitä, kun käyttöönotto VMs tällainen tilanne voi olla [Verkko-käyttöoikeusryhmän] luominen[ virtual-networks-nsg] käyttösäännöt määrittämiseen.

### <a name="dealing-with-different-virtual-machine-series"></a>Virtuaalikoneen sarjoille käsittely

Viimeisen 12 kuukauden kuluessa Microsoft on lisännyt useita AM erilaisia, vaihtelevat joko vCPUs, muistin määrän tai tärkeämpää laitteistoon se suoritetaan. Kaikki nämä VMs tuetaan ja SAP: N (Katso tuetut AM tyypit-SAP Huomautus [1928533]). Joitain näistä VMs suorittaa eri host laitteiston sukupolvea. Nämä host laitteiston sukupolvien ovat käytön käyttöön rakeisuuden, Azure-asteikko. Tarkoittaa tapauksissa voi syntyä, jossa valitsit AM erikokoisia ei voi suorittaa saman asteikko. Käytettävyys määrittää on rajoitettu mahdollisuus kattavat aikayksikön-pohjainen eri laitteista.  Esimerkiksi Jos haluat suorittaa DBMS A5 A11 VMs ja SAP-sovelluskerroksen-G-sarjan VMs, voit pakotettu ottamaan yksittäisen SAP-järjestelmää tai eri SAP-järjestelmiin käytettävyys eri tiedostojoukot kuluessa.


#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>SAP-esiintymästä Azure paikalliseen verkkoon kirjauduttaessa tulostinta
##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>Tulostuksen tukipalvelun paikallisen-skenaario


Paikallisen TCP/IP-pohjaisen verkkotulostimet-Azure-AM määrittäminen on yleinen samoja kuin yrityksen verkossa, mikäli sinulla ole VPN sivusto sivusto tunnelin tai ExpressRoute yhteys. 

___

> ![Windows][Logo_Windows] Windows
>
> Toiminto:
> - Jotkin verkkotulostimet mukana määritystoiminto, joka helpottaa määrittänyt tulostinta Azure-AM. Jos tulostimessa on jaettu ohjattu toiminto ei ole ohjelmiston määrittämään tulostin "komennolla" tapa on luoda uusi TCP/IP-tulostimen portti.
> - Avaa Ohjauspaneeli -> laitteet ja tulostimet -> Lisää tulostin 
> - Valitse Lisää tulostin, TCP/IP-osoitetta tai isäntänimi
> - Kirjoita tulostimen IP-osoite
> - Portin vakio 9100
> - Tarvittaessa Asenna sopiva tulostinohjain manuaalisesti. 
> 
> ![Linux][Logo_Linux] Linux
>
> - Pidä Windows vain seurantaa varten Vakiomenettely Asenna verkkotulostin
> - Noudata julkisen Linux apuviivat [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) tai [Punainen on](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) tulostimen lisäämisestä.

___

 
![Verkon tulostaminen][planning-guide-figure-2200]



##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Host (isäntä)-pohjainen tulostimen SMB (jaettu tulostin) kautta paikallisen-tilanne

Host (isäntä)-pohjainen tulostimet eivät ole verkko-yhteensopiva suunniteltu ominaisuus. Mutta host-tulostin voidaan jakaa tietokoneiden verkossa, kunhan tulostin on yhdistetty muistiin-tietokoneeseen. Yhdistä yrityksen verkko-sivusto sivusto tai ExpressRoute ja jakaa paikallisen tulostimen. SMB-protokollaa käyttää sen sijaan, että DNS NetBIOS nimi palveluna. NetBIOS isäntänimi voi olla eri kuin DNS-isäntänimi. Normaali kirjainkoko on NetBIOS isäntänimi ja DNS-isäntänimi ovat samat. DNS-toimialueen ei tulkita NetBIOS nimitila. Näin ollen täydellinen DNS-isäntänimen DNS-isännän nimen ja DNS-toimialueen ei saa käyttää NetBIOS nimi-tilaan.

Jaetun tulostimen tunnistetaan verkon yksilöllinen nimi:

* Isäntänimi SMB-isännän (aina tarvitaan). 
* Nimi (aina tarvitaan) Jaa. 
* Toimialueen nimi, jos tulostimen jakaminen ei ole samaa toimialuetta SAP-järjestelmää. 
* Lisäksi käyttäjänimen ja salasanan saattaa edellyttää käyttää jaetun tulostimen.

Miten tehdään:

___

> ![Windows][Logo_Windows] Windows
>
> Voit jakaa paikallisen tulostimen.
> Avaa Resurssienhallinta ja kirjoita resurssin nimi tulostimen Azure AM.
> Ohjattu tulostimen asennus opastaa sinua asentunut.
>
> ![Linux][Logo_Linux] Linux
>
> Seuraavassa on joitakin esimerkkejä ohjeista määrittäminen verkkotulostimet Linux tai luvun mukaan lukien liittyy tulostaminen Linux. Se toimii samalla tavalla kuin Azure-Linux AM, kunhan VPN AM kuuluu:
>
> * SLES <https://en.opensuse.org/SDB:Printing_via_SMB_ (Samba) _Share_or_Windows_Share>
> * RHEL <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-printing-smb-printer.html>

___


##### <a name="usb-printer-printer-forwarding"></a>USB-tulostin (tulostimen edelleenlähetys) 

Azure Etätyöpöytäpalvelut mahdollisuus tarjota käyttäjille paikallisen tulostimen laitteensa käyttöoikeus etäistunnon ei ole käytettävissä.

___

> ![Windows][Logo_Windows] Windows
>
> Lisätietoja tulostamisesta Windowsin löytyy tähän: <http://technet.microsoft.com/library/jj590748.aspx>.

___

 
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>SAP: N integrointi Azure järjestelmien korjaus ja paikallisen-järjestelmän Transport (TMS)

SAP-muuttaminen ja Transport järjestelmän (TMS) varten on määritettävä voi viedä ja tuoda transport pyynnön järjestelmiin vaaka. Oletetaan, että SAP-järjestelmää (keskihajonta) kehittäminen esiintymät sijaitsevat Azure laadun varmistaminen (Kysymysosio) ja tehokkaasti järjestelmien (PRD) ovat paikallisen. Lisäksi on oletetaan olevan keskitetyn transport hakemisto.

##### <a name="configuring-the-transport-domain"></a>Transport-toimialueen määrittäminen
Transport toimialueen määrittämistä Transport toimialueen ohjauskoneen kuvatulla tavalla [Transport toimialueen ohjauskoneen määrittämisestä](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm)määritetty järjestelmään. Järjestelmäkäyttäjän TMSADM luodaan ja tarvittavat RFC kohde luodaan. Voit tarkistaa RFC asiakasyhteydet käyttämällä tapahtuman SM59. Hostname tarkkuus on otettava käyttöön kaikissa toimialueen transport. 

Miten tehdään:

* Tässä skenaariossa on päättänyt paikallisen QAS järjestelmä on CTS toimialueen ohjauskoneen. Soita tapahtuman STM. TMS-valintaikkuna. Määritä Transport toimialue-valintaikkuna tulee näkyviin. (Tämä valintaikkuna näkyy vain, jos et ole vielä määrittänyt transport toimialueen.)
* Varmista, että automaattisesti luotu käyttäjän TMSADM sallitaan (SM59 -> ABAP yhteys -> TMSADM@E61.DOMAIN_E61 -> tiedot -> Utilities(M) luvan testi ->). Tapahtuman aloitusnäyttö STM pitäisi näkyä, että SAP-järjestelmää nyt toimi kuin transport toimialueen ohjauskoneen, tässä esitetyllä tavalla:
 
![Toimialueen ohjauskoneen STM tapahtuman Alkunäyttö][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-the-transport-domain"></a>SAP-järjestelmiin sisällyttäminen Transport toimialueen

Järjestyksen mukaan lukien SAP-järjestelmää transport toimialueen näyttää seuraavasti:

* Valitse Azure keskihajonta-järjestelmän transport järjestelmän (asiakas 000)-kutsu tapahtuman STM. Valitse muut määritys-valintaikkunassa ja jatka sisältää järjestelmän toimialueeseen. Määritä toimialueen ohjauskoneen target host ([Mukaan lukien SAP-järjestelmiin Transport toimialueen](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). Järjestelmä odottaa nyt sisällytetään transport toimialueen.
* Tietoturvasyistä sitten on palaa toimialueesta Vahvista. Valitse järjestelmän yleiskatsaus ja hyväksy odottaa-järjestelmän. Vahvista kehotteen ja määritykset jaetaan.

SAP-järjestelmää sisältää nyt kaikki muut SAP järjestelmät transport toimialueen tarvittavat tiedot. Yhtä aikaa uuden SAP-järjestelmän osoitetiedot lähetetään kaikkiin muihin SAP-järjestelmiin, ja SAP-järjestelmää syötetään transport profiilin liikenteen hallinta-ohjelman. Tarkista, onko RFC ja transport hakemiston toimialueen toimivat.

Jatka transport järjestelmän tavalliseen tapaan kuvatulla tavalla asiakirjat [tai muuta niitä ja Transport järjestelmän](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)kokoonpano.

Miten tehdään:

* Varmista, että paikallinen STM on määritetty oikein.
* Varmista, että Transport toimialueen ohjauskoneen hostname ovat virtuaalikoneen Azure ja päinvastoin viisumin käyttöön.
* Kutsu tapahtuman STM -> muut määritys -> sisältää järjestelmän toimialueeseen.
* Vahvista yhteyden järjestelmässä käytössä paikallisen TMS.
* Määritä transport tiet, ryhmiä ja kerrokset tavalliseen tapaan.

Sivuston sivuston yhdistetyn paikallisen-tilanteissa paikallisen ja Azure välinen viive edelleen voi olla merkittäviin. Jos on noudattamalla järjestyksen siirrosta objektien kautta kehittämisen ja testaa tuotannon järjestelmät huomioon otettavia asioita käyttämällä tiedonsiirron tai tukevat pakettien eri järjestelmiin, huomaat, että riippuvainen keskitetyn transport-kansion sijainti joitakin järjestelmien ilmenee odotusaika lukeminen tai kirjoittaminen keskitetyn transport hakemistossa. Tilanne on samanlainen kuin SAP vaaka-määrityksiä kohtaa, johon eri järjestelmiä ovat leviävät eri tietojen keskikohdan mukaan tiedot-keskukset merkittäviin etäisyys kanssa.

Kiertää tällaisten viive ja toimii nopeasti lukeminen tai kirjoittaminen, tai transport hakemistosta, voit määrittää järjestelmien on kaksi STM transport toimialueiden (yksi paikallisen ja yksi Azure-tietokannassa järjestelmien kanssa ja linkittää transport toimialueet. Tarkista, onko näitä ohjeita, jossa kerrotaan takana SAP-TMS tämän käsitteen periaatteiden: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/38dd924eb711d182bf0000e829fbfe/frameset.htm>. 

Miten tehdään:

* Kunkin sijainti (paikallisen ja Azure) transport toimialueen määrittäminen käyttämällä tapahtuman STM <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Linkki toimialueet toimialueen linkin ja varmista, että kaksi toimialuetta välisen linkin. 
  <http://Help.SAP.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/Content.htm>
* Jaa linkitetyn järjestelmän kokoonpano.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>RFC liikenne SAP esiintymät sijaitsevat Azuren ja paikallisten (paikallisen rajat) välillä

RFC liikenne järjestelmät, jotka ovat paikallisen ja Azure-tietokannassa on käsiteltävä. Asennuksen yhteyden puhelu tapahtumaa SM59 tarvittaessa määrittää RFC-yhteyden olisi kohteen käyttöjärjestelmän järjestelmän. Kokoonpanon muistuttaa vakio RFC-yhteyden asetukset.

Olemme oletetaan, että paikallisen-tilanteessa VMs, jotka suoritetaan SAP-järjestelmiin vaikuttavat keskenään samaa toimialuetta. Tämän vuoksi SAP-järjestelmiin RFC-välisessä asetukset eroavat ei määrityksen vaiheet ja syötteiden paikallisen tilanteissa.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Käytettäessä paikallisen' fileshares SAP esiintymistä Azure tai päinvastoin

SAP-esiintymät Azure sijaitsevat pystyttävä käyttämään tiedostoresurssit, jotka kuuluvat yrityksen paikallisen. Lisäksi paikallista SAP esiintymät pystyttävä käyttämään tiedostoresurssit, jotka sijaitsevat Azure. Jos haluat ottaa tiedostoresurssit, sinun on määritettävä käyttöoikeudet ja muut jakamisasetukset paikalliseen järjestelmään. Varmista, että Avaa porttien VPN- tai ExpressRoute Azure ja että palvelinkeskuksen välinen yhteys.

## <a name="supportability"></a>Supportability
### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Azure seurantaa SAP ratkaisu
Jos haluat ottaa käyttöön seurantaa toiminta kriittinen SAP-järjestelmiin Azure-SAP SAPOSCOL tai SAP Host (isäntä)-agentti Azure virtuaalikoneen palvelun isäntä kautta tiedostotunnistetta Azure seuranta käytöstä tietojen noutaminen SAP: n Valvontatyökalut. Koska tarpeiden mukaan SAP olivat tarkkojen SAP sovellukset, Microsoft päättänyt olla yhdistyvät toisten tietokantajärjestelmien Toteuta kyselyjä Azure tarvittavat toiminnot, mutta jätä se tarvittavat seurantaa osat ja määritykset käyttöön niiden Azuren näennäiskoneiden asiakkaille. Kuitenkin käyttöönotto ja elinkaaren hallinta seurantaa osat on enimmäkseen automaattinen Azure mukaan.

#### <a name="solution-design"></a>Ratkaisu rakenne

Kehittänyt käyttöön SAP seuranta ratkaisu perustuu Azure AM agentti ja tunniste framework arkkitehtuuri. Azure AM agentti ja tunniste Frameworkin on asennusta ohjelmisto-sovelluksista käytettävissä sisällä AM Azure AM tunniste-valikoimassa. Tarpeen tämän käsitteen on sallimaan (tapauksissa, kuten Azure seuranta tunnisteen SAP: n), kyselyjä AM erikoistoimintoja käyttöönottoa ja ohjelmiston määrittäminen käyttöönoton aikana. 

Helmikuussa 2014, koska "Azure AM agentti", joka mahdollistaa tietyn Azure AM tunnisteet sisällä AM käsittelyn syötetään Windows VMs oletusarvoisesti AM luominen Azure-portaalissa. SUSE tai punainen on Linux AM agentti on jo osa Azure Marketplace-kuva. Siltä varalta, että jokin ladata Linux AM paikallisen from Azure AM-agentti on asennettava manuaalisesti.


SAP-näyttää Azure seuranta-ratkaisun basic rakenneosia tältä:
 
![Microsoft Azure-laajennus osat][planning-guide-figure-2400]

Kuten edellä lohkokaavio, yksi osa SAP: n seuranta-ratkaisua nykyisessä Azure AM kuva ja Azure tunniste valikoiman on yleisesti replikoitua säilöön, jota hallitaan Azure toiminnot. Näin yhteinen SAP/MS-ryhmän toimimasta Azure toteuttamisesta SAP Azure toimintojen julkaista uusia versioita Azure seuranta-laajennus SAP-käyttöä varten. Tämä Azure seuranta-laajennus SAP käyttää Microsoft Azure diagnostiikka (WAD) tunniste tai Linux Azure diagnostiikka (LAD) tarvittavat tiedot. 

Kun otat käyttöön uuden Windows-AM, 'Azure AM "-agentti lisätään automaattisesti AM. Tämä agentti-funktio on järjestämiseen ladataan ja määritykset Azure-laajennukset SAP NetWeaver järjestelmien seurantaa varten. Linux VMs Azure AM agentti on jo Azure Marketplace OS kuvan osa.

On kuitenkin vaiheeseen, joka on vielä suoritettava asiakkaan. Tämä on mahdollistamisen ja suorituskyky-sivustokokoelman määrittäminen. 'Määrityksiin' liittyviä prosessi on automaattinen PowerShell-komentosarjaa tai CLI-komentoa. PowerShell-komentosarjaa voi ladata Microsoft Azure Script Centerissä kuvatulla tavalla [käyttöönotto-oppaan] [käyttöönotto-oppaan].

SAP: n Azure seuranta-ratkaisun arkkitehtuurista näyttää seuraavanlaiselta:
 
![Azure seurantaa SAP NetWeaver ratkaisu][planning-guide-figure-2500]

**Tarkat ohjeet ja yksityiskohtaisia ohjeita näiden PowerShellin cmdlet-komennot tai CLI komennon käytön aikana noudata annettuja [käyttöönotto-oppaan] [käyttöönotto-oppaan].**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Azure integrointi sijaitsevat yhdeksi SAProuter SAP-esiintymä

SAP-esiintymät Azure käytössä on oltava käytettävissä olevat SAProuter sekä.
 
![SAP-reitittimen verkkoyhteys][planning-guide-figure-2600]

SAProuter mahdollistaa TCP/IP-välisessä osallistuvien järjestelmät, jos ei ole suoraa IP-yhteyttä. Tämä on etuna viestintä välillä ei ole lopusta loppuun-yhteyden tarvitaan verkon tasolla. SAProuter kuuntele porttia 3299 oletusarvoisesti.
Voit muodostaa yhteyden SAP esiintymät SAProuter kautta, sinun täytyy SAProuter merkkijonon ja isännän nimi ja yrität muodostaa.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver AS Java

Tähän mennessä kohdistus asiakirjan on ollut SAP NetWeaver yleinen tai SAP NetWeaver ABAP kääntää. Pieni sisältö on lueteltu SAP Java-pino tietyn Huomioitavaa. Yksi tärkeimmistä SAP NetWeaver Java yksinomaan mukaan-sovellukset on SAP-yritysportaalin. Muiden SAP NetWeaver sovelluksiin, kuten SAP pii ja SAP-ratkaisun hallinta käyttää sekä SAP NetWeaver ABAP Java pinoa. Tämän vuoksi varmasti on tarpeen ottaa huomioon, että tiettyjä ominaisuuksia liittyvät sen SAP NetWeaver Java-osaa.

### <a name="sap-enterprise-portal"></a>SAP-yritysportaalin

SAP-portaalissa Azure Virtual Machine asetukset eroavat ei käytössä paikallisen asennuksen Jos otat paikallisen-tilanteissa. Koska paikallisen tehdään DNS-portin asetuksia yksittäisen esiintymien voidaan toteuttaa määritetyn paikallisen. Suosituksia ja rajoituksia tässä asiakirjassa kuvatut mennessä hakea jokin sovellus, esimerkiksi SAP-yritysportaalin tai SAP NetWeaver Java-pino yleinen. 

![Näkyviä SAP-portaalissa][planning-guide-figure-2700]

Erityistä käyttöönottotapa joitakin asiakkaiden on SAP-yritysportaalin suora näyttäminen Internet virtuaalikoneen host ollessa yhdistettynä sivusto sivusto VPN-tunnelin tai ExpressRoute yrityksen verkkoon. Näiden skenaariossa on Varmista, että tietyt portit ovat avoinna ja ei estetty palomuurin tai verkon käyttöoikeusryhmän mukaan. Käytetään, kun haluat muodostaa yhteyden SAP Java esiintymä paikalliseen vain Pilvipalveluita tilanne on sama suunnittelu.

Alkuperäinen URI-portaali on http (s):`<Portalserver`>: 5XX00/irj, jossa portti on muodostettu 50000 plus (Systemnumber × 100). Oletus-portaalin URI SAP järjestelmä 00 on `<dns name`>. `<azure region`>.Cloudapp.azure.com:PublicPort/irj. Lisätietoja on katsoa <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>. 
 
![Päätepistemääritys][planning-guide-figure-2800]

Jos haluat mukauttaa URL-osoite ja/tai SAP-yritysportaalin portit, tarkista näitä ohjeita:

* [Portaalin URL-Osoitteen muuttaminen](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL) 
* [Muuta oletusarvoista porttinumerot-portaalin porttinumerot](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers) 


## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Suuri käytettävyys (HA) ja tietojen palauttaminen (DR) käynnissä-Azuren näennäiskoneiden SAP NetWeaverin
### <a name="definition-of-terminologies"></a>Terminologies määritys

Termin **suuren käytettävyyden (HA)** liittyy yleensä joukko, jonka minimoi antamalla Liiketoiminnan jatkuvuus IT-palveluiden IT-keskeytyksiä tekniikoita tarpeettomat, vikasietoinen tai automaattisesti suojatut osat **samaan** tietokeskuksen sisällä. Tässä tapauksessa Azure-alueella.

**Palauttaminen (DR)** myös kohdistamisen pienentäminen IT-palveluiden häiriöitä ja niiden palauttamista, mutta **eri** tietojen yli keskukset, jotka ovat yleensä sijaitsevat satoja kilometreiksi poissa. Tässä tapauksessa yleensä eri Azure alueiden samalla geopoliittisten alueella tai itse vahvistettu asiakkaaksi välillä.

### <a name="overview-of-high-availability"></a>Suuren käytettävyyden yleiskatsaus
Emme voi erota SAP suuren käytettävyyden Azure-tietokannassa kahteen osaan keskustelu:

* **Azure infrastruktuurin suuren käytettävyyden**, kuten HA Laske (VMs), verkon tallennustilan jne ja sen edut lisääntyvien SAP-sovellusten käytettävyyden.
* **SAP-sovelluksen suuren käytettävyyden**, kuten HA ja SAP-ohjelmiston osat:
    * SAP-sovelluksen palvelimet
    * SAP ASCS/SCS esiintymä 
    * DB-palvelin

ja kuinka se voidaan yhdistää Azure infrastruktuuriin HA.

SAP: N suuren käytettävyyden Azure-tietokannassa on joitakin eroja verrattuna SAP suuren käytettävyyden paikallisen fyysinen tai näennäinen ympäristössä. SAP-seuraavat paperin kuvataan vakio SAP suuren käytettävyyden määritykset virtualisoidussa ympäristöissä Windows: <http://scn.sap.com/docs/DOC-44415>. Ei ole sapinst integroitu SAP-HA määritykset Linux esimerkiksi Windows olemassa. Koskeva SAP HA paikalliseen Linux, Lisätietoa on ohjeaiheessa tähän: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Azure infrastruktuurin suuri käytettävyys
Ei ole yhden AM SLA-Azuren näennäiskoneiden tällä hetkellä. Saat käsityksen siitä, miten yhden AM käytettävyyttä voi näyttää esimerkiksi riittää, että voit luoda eri käytettävissä Azure palvelutasosopimuksia tulon: <https://azure.microsoft.com/support/legal/sla/>.

Laskentaperuste on 30 päivää kuukaudessa tai 43200 minuuttia. Tämän vuoksi 0,05 % käyttökatkot vastaa 21,6 minuuttia. Eri palveluissa käytettävyys tavalliseen tapaan kertoa seuraavalla tavalla:

(Käytettävyyspalvelu #1/100) *(Käytettävyyspalvelu #2/100)* (Käytettävyyspalvelu #3/100) *...

Esimerkiksi:

(99.95/100) *(99,9/100)* (99,9/100) = 0.9975 tai yleinen saatavuuden 99.75 %.

#### <a name="virtual-machine-vm-high-availability"></a>Virtual Machine (AM) suuri käytettävyys

Azure ympäristö tapahtumista, jotka voivat vaikuttaa oman näennäiskoneiden käytettävyyttä kahdenlaisia: suunniteltu ylläpito ja suunnittelematon ylläpito.

* Suunniteltu ylläpito tapahtumat ovat säännöllisiä tehdyt päivitykset Microsoft Azure pohjana Platformin yleinen luotettavuutta, suorituskykyä ja tietoturva infrastruktuuria oman näennäiskoneiden käynnissä olevat.
* Suunnittelematon ylläpito tapahtumat toteutuvat laitteiston tai fyysinen infrastruktuurin pohjana virtuaalikoneen on ilmennyt virhe jollakin tavalla. Tähän voi kuulua paikallisen verkon virheet, paikalliseen levyasemaan virheet tai muiden Teline tason virheet. Kun esimerkiksi virhe havaitaan, Azure ympäristö Siirry automaattisesti virtuaalikoneen isäntäpalvelimeen perusasemassa fyysinen virtuaalikoneen kunnossa fyysinen palvelimeen. Tällaisten tapahtumien ovat harvinaisissa, mutta voivat aiheuttaa virtuaalikoneen käynnistää uudelleen.

Lisätietoja löytyy dokumentaatio: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Azure-tallennustilan Redundancy

Microsoft Azure-tallennustilan tilin tiedot aina replikoida kestävyyttä ja suuren käytettävyyden, kokouksen Azure-tallennustilan SLA jopa menettämisen tapahtuessa lyhytkestoisia laitteisto varmistamiseksi

Koska Azuren tallennustilaan pitää 3 kuvia tietojen oletusarvoisesti, RAID5 tai RAID1 useille Azure levyille ei ole tarpeen.

Lisätietoja tämän artikkelin löytyy: <http://azure.microsoft.com/documentation/articles/storage-redundancy/> 

#### <a name="utilizing-azure-infrastructure-vm-restart-to-achieve-higher-availability-of-sap-applications"></a>Käyttävien Azure infrastruktuurin AM uudelleenkäynnistyksen saavuttamiseksi "Suurempi käytettävyys" SAP-sovellusten

Jos et halua käyttää toimintoja, kuten Windows Server-automaattisesti klusterointi (WSFC) tai Linux vastaavat (jälkimmäinen jokin ei tueta vielä Azure yhdessä SAP-ohjelmistolla), Azure AM Käynnistä on käytössä suojaaminen SAP-järjestelmää vastaan suunnitellut ja suunnittelemattomat käyttökatkot fyysinen server Azure-infrastruktuuria ja yleistä pohjana Azure ympäristö. 
 
> [AZURE.NOTE] On tärkeää kertoa, että ensisijaisesti Azure AM uudelleen suojaa VMs ja ei-sovellukset. AM uudelleen ei tarjoa suuren SAP-sovellusten käytettävyyden, mutta se tarjoaa tason infrastruktuuri käytettävyys ja näin ollen epäsuorasti "suurempi saatavuuden" SAP-järjestelmiin. Uudelleen AM jälkeen suunniteltu tai suunnittelematon host käyttökatkosta kestää sisäänkirjautuminen on myös ei SLA. Tämän vuoksi tätä menetelmää 'suuren käytettävyyden' ei ole sopivia kriittinen SAP-järjestelmään, kuten (A) SCS tai DBMS osat.

Toisen suuren tärkeitä infrastruktuurin elementti on tallennustilan. Esimerkiksi Azure tallennustilan SLA on 99,9 % käytettävyyttä. Jos kyseinen ottaa käyttöön ja sen levyjen kaikki VMs yksittäisen Azure tallennustilan tilille, siitä aiheuttaa siitä kaikki VMs, joka sijoitetaan Azure tallennustilan tilin ja myös kaikki SAP-osien sisäisistä näiden VMs mahdolliset Azure-tallennustilan.  

Sen sijaan, että kaikki VMs liikkeelle yhden yksittäisen Azure-tallennustilan tilin, voit myös käyttää varatun tallennustilan tilit kunkin AM ja näin suurentaa yleinen AM ja SAP-sovelluksen käytettävyyden usean riippumaton Azure-tallennustilan tilin käyttämällä. 

Esimerkki-arkkitehtuuri SAP NetWeaver järjestelmää, joka käyttää Azure infrastruktuurin HA näyttää tältä:
 
![Käyttävien Azure infrastruktuurin HA saavuttamiseksi SAP-sovelluksen "suurempi" käytettävyys][planning-guide-figure-2900]

Kriittinen SAP-osien on saavutettu seuraavat tähän mennessä:

* Suuren käytettävyyden SAP palvelinten (AS)

SAP-sovelluksen server-esiintymät ovat tarpeettomia osia. Kunkin SAP kuin esiintymää on otettu käyttöön omassa AM, joka toimii eri Azure vika ja Päivitä toimialueen (Katso luvuissa vika toimialueiden [suunnittelu-opas-3.2.1] ja [päivittäminen Domains][planning-guide-3.2.2]). Tämä on varmistettava käyttämällä Azure käytettävyys joukot (katso luvun [Azure käytettävyys Sets][planning-guide-3.2.3]). Mahdollinen suunniteltu tai suunnittelematon siitä Azure vika tai Päivitä toimialueen aiheuttaa on rajoitettu määrä VMs selattava ja niiden SAP-AS esiintymät. Kunkin SAP kuin esiintymän sijoitetaan Azuren tallennustilaan Oma tili – mahdolliset siitä yhden Azure-tallennustilan tilin aiheuttaa vain yksi AM selattava kanssa sen SAP-AS esiintymä. Muista kuitenkin, että yhden Azure-tilauksen piiriin kuuluvien tilien Azure-tallennustilan enimmäismäärä on. Jotta automaattisessa aloituksessa SCS (A) esiintymän AM uudelleenkäynnistyksen jälkeen, varmista, että määrittää määrittää parametrin (A) SCS esiintymän Käynnistä käyttäjäprofiilin kuvattu luvun [SAP esiintymien avulla määrittää] [suunnittelu-opas-11,5].
Lue myös luvun [suuren käytettävyyden SAP-sovelluksen palvelinten] [suunnittelu-opas-11.4.1] Lisätietoja.

* _Uudempi versio_ SAP: in A SCS esiintymän käytettävyys
 
Seuraavassa on käyttämiseen Azure AM uudelleen suojaa AM asennettu SAP (A) SCS esiintymää. Suunniteltu tai suunnittelematon käyttökatkot ja Azure kyseessä katkaisee, VMs käynnistetään toisessa saatavilla palvelimessa. Kuten edellä mainittiin, ensisijaisesti Azure AM uudelleen suojaa VMs ja ei-sovelluksia, tässä tapauksessa (A) SCS esiintymän. – AM uudelleen olemme saavuttaa epäsuorasti "suurempi käytettävyys" SAP (A) SCS esiintymän. Voit varmistaa automaattisessa aloituksessa SCS (A) esiintymän AM uudelleenkäynnistyksen jälkeen, varmista, että voit määrittää määrittää parametrin (A) SCS esiintymän Käynnistä käyttäjäprofiilin kuvattu luvun [SAP esiintymien avulla määrittää] [suunnittelu-opas-11,5]. Tämä tarkoittaa, että (A) SCS esiintymän kuin yhden pisteen, virheen (SPOF) yksittäisen AM käytössä on koko SAP-vaaka käytettävyyttä pääasiallisen korjaustermi. 

* _Uudempi versio_ DBMS-palvelimen käytettävyys

Tässä kaltaisilta SAP (A) SCS esiintymän käyttötapaus on käyttämiseen Azure AM uudelleen suojaa asennettu DBMS-ohjelmistolla AM ja olemme saavuttamiseksi "suurempi käytettävyys" DBMS ohjelmiston kautta AM uudelleen. DBMS yksittäisen AM käytössä on myös SPOF ja koko SAP-vaaka käytettävyyttä pääasiallisen korjaustermi on. 

### <a name="sap-application-high-availability-on-azure-iaas"></a>SAP: in Azure IaaS sovelluksen suuri käytettävyys
Tavoitteet koko SAP järjestelmän suuren käytettävyyden annettava suojaa kaikki tärkeät SAP järjestelmäosat, kuten tarpeettomat SAP sovelluksen-palvelimet- ja yksilöllisiä osia (kuten yhden pisteen, virhe), kuten SAP (A) SCS esiintymän ja DBMS. 

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>SAP-sovelluksen palvelinten suuri käytettävyys
SAP-sovelluksen palvelinten/valintaikkunan esiintymien ei ole tarpeen huomioon otettavia asioita tietyn suuri käytettävyys-ratkaisun. Suuren käytettävyyden saavutetaan yksinkertaisesti redundancy ja siten on riittävän paljon niitä eri näennäiskoneiden. Ne kaikki sijoitetaan saman Azure käytettävyys Set välttämiseksi VMs voi päivittää yhtä aikaa suunnitellun ylläpidon käyttökatkot aikana. Perustoimintoja, joka perustuu eri päivitys- ja Azure-asteikko vika toimialueiden jo otettiin käyttöön luvun [päivittäminen toimialueiden] [suunnittelu-opas-3.2.2]. Azure käytettävyys joukot on esitetty luku [Azure käytettävyys joukot] [suunnittelu-opas-3.2.3] tämän asiakirjan. 

Ei ole ääretön määrä vika ja päivittää toimialueet, joita voidaan käyttää Azure käytettävyys määrittäminen Azure-asteikko. Tämä tarkoittaa, että yhden käytettävyyden asettaminen ennemmin tai myöhemmin siitä, että useita AM päättyy vika tai Päivitä toimialueen VMs useita liikkeelle

Muutama SAP sovelluksen server esiintymät niiden erillinen VMs-käyttöönotto ja olettaen 5 päivittäminen toimialueet on käytössä, seuraavassa kuvassa käy ilmi lopussa. Käytettävyys määrittäminen vika ja Päivitä toimialueiden todellinen enimmäismäärän voi muuttua myöhemmin:
 
![HA SAP palvelinten Azure-tietokannassa][planning-guide-figure-3000]

Lisätietoja löytyy dokumentaatio: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>


#### <a name="high-availability-for-the-sap-ascs-instance-on-windows"></a>SAP: in A SCS esiintymän Windowsin suuri käytettävyys

Windows Server automaattisesti klusterin (WSFC) on usein käytettyjen ratkaisu suojaaminen SAP (A) SCS esiintymä. Se on myös integroitu sapinst "HA-asennuksen"-lomakkeessa. Tässä vaiheessa kohdassa kerralla Azure infrastruktuurin ei pysty määrittämään tarvittavat Windows Server-vikasietoklusteriin samalla tavalla kuin se on tehnyt paikallisen-toimintoja.

Tammikuussa 2016 vuodesta Azure cloud-ympäristössä, Windows-käyttöjärjestelmää ei tarjoa mahdollisuutta käyttää jaettu klusterin aseman jaeta kaksi Azure VMs levylle.

Kelvollinen ratkaisu on kuitenkin 3rd osapuolen ohjelmiston, joka sisältää jaettuun asemaan synkronoidusti ja läpinäkyvä levyn replikointi, joka on integroitu WSFC käyttö. Tämän menetelmän tarkoittaa, että aktiivinen klusteri-solmu on käyttää toinen levyn vaiheessa ajassa. Tammikuussa 2016 tämän HA saavuttaman määritysten tuetaan suojaaminen Windows Vieras OS Azure VMs-ohjelmistolla 3rd osapuolen SIOS DataKeeper yhdessä SAP (A) SCS esiintymään.

SIOS DataKeeper ratkaisu on jaetun levyn klusteriresurssin Windows-vikasietoklusterit siten, että:

* Lisätietoja Azure-Näennäiskiintolevyn liitetty jokaiseen näennäiskoneiden (VMs), jotka ovat Windows-klusterin määritys
* SIOS DataKeeper klusterin Edition, jossa on käytössä sekä AM solmuissa
* On määritetty siten, että se vastaa synkronoidusti muita Näennäiskiintolevyn sisällön SIOS DataKeeper klusterin Edition liitetty äänenvoimakkuutta lähteestä VMs muita Näennäiskiintolevyn liitetty kohde AM äänenvoimakkuuden.
* SIOS DataKeeper abstracting lähde- ja asemat ja esitys Windows automaattisesti klusterin kuin yhdelle jaetun levylle.
 
Löydät kaikki tiedot siitä, miten voit asentaa Windows automaattisesti klusterin SIOS Datakeeper ja SAP [klusterointi SAP ASCS esiintymän käyttämällä Windows Server automaattisesti klusterin kanssa SIOS DataKeeper Azure-] [ ha-guide-classic] tekninen raportti. 

#### <a name="high-availability-for-the-sap-ascs-instance-on-linux"></a>SAP: in A SCS esiintymän Linux suuri käytettävyys
 
Vuodesta joulu 2015 on myös jaetun levyn Linux VMs Azure-WSFC ei vastaa. Käyttämällä 3rd osapuolen-ohjelmistoa, kuten SIOS Windows tarkistetaan ei ole vielä suorittamisen SAP Linux Azure-vaihtoehtoisia ratkaisuja.



#### <a name="high-availability-for-the-sap-database-instance"></a>SAP-tietokannan esiintymän suuri käytettävyys
Tyypillinen SAP DBMS HA määritykset perustuu kaksi DBMS VMs, jossa DBMS suuren käytettävyyden toimintoja käytetään replikoida tietojen aktiivinen DBMS esiintymän toisen AM kyselyjä passiivinen DBMS esiintymä.

Suuri käytettävyys ja tietojen palauttaminen toiminnot DBMS yleisesti sekä tietyt DBMS on kuvattu [DBMS käyttöönotto-oppaan] [dbms-opas].


#### <a name="end-to-end-high-availability-for-the-complete-sap-system"></a>Lopusta loppuun valmis SAP-järjestelmän suuri käytettävyys

Seuraavassa on kaksi esimerkkiä valmis SAP NetWeaver HA arkkitehtuurista Azure - yksi Windowsia ja yksi Linux.
Käsitteitä jäljempänä on ehkä kärsiä hieman, kun monta SAP-järjestelmiin käyttöönotto ja VMs käyttöön määrän ylittävät enimmäismäärä tallennustilan tilit, tilausta kohti. Tässä tapauksessa VMs näennäiskiintolevyjen on voidaan yhdistää yhden tallennustilan tilin. Yleensä tee niin yhdistämällä SAP näennäiskiintolevyjen sovelluskerroksen eri SAP-järjestelmiin VMs.  On myös yhdistää eri DBMS VMs eri SAP-järjestelmiin yhden Azure-tallennustilan tilin eri näennäiskiintolevyjen. Pitäminen siten IOPS rajojen Azure-tallennustilan tilien mielessä ( <https://azure.microsoft.com/documentation/articles/storage-scalability-targets> )

##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] HA Windows

![SAP NetWeaver sovelluksen HA arkkitehtuuri SQL Server Azure IaaS][planning-guide-figure-3200]

Seuraavat Azure rakenteita käytetään SAP NetWeaver järjestelmä-ja pienentää vaikutus infrastruktuurin ongelmien korjaaminen:

* Koko järjestelmän on otettu käyttöön Azure (pakollinen - DBMS kerroksen, (A) SCS esiintymän ja valmis sovelluskerroksen on suoritettava samassa sijainnissa).
* Koko järjestelmän suoritetaan yksi Azure tilaus (pakollinen).
* Valmis järjestelmä suorittaa yhden Azure Virtual verkossa, (pakollinen).
* SAP-järjestelmää VMs erottelua kolme käytettävyys joukkoihin on mahdollista, vaikka kaikki samaan Virtual verkkoon kuuluvat VMs kanssa.
* Kaikki virtuaalilaitteiksi SAP-järjestelmää DBMS esiintymät ovat yhtä käytettävyys määrittäminen. Oletetaan, että on useampi kuin yksi AM käynnissä DBMS esiintymää järjestelmän alkuperäisen DBMS suuren käytettävyyden, ominaisuudet ovat käytössä, kuten SQL Server AlwaysOn tai Oracle tietojen suojaa jälkeen.
* Kaikki virtuaalilaitteiksi DBMS esiintymät käyttää tallennustilan oman tilin. DBMS tietojen ja lokitiedostojen replikoida tallennustilan-tililtä toiselle tallennustilan tilille DBMS suuri käytettävyys-funktioilla, synkronoida tiedot. Tallennustilan tilien selattava aiheuttavat selattava yhden SQL Windows-klusterisolmu, mutta ei koko SQL Server-palvelu. 
* Kaikki virtuaalilaitteiksi (A) SCS esiintymän SAP-järjestelmää ovat yhtä käytettävyys määrittäminen. Näiden VMs sisällä on määrittäminen Windows Server automaattisesti klusterin (WSFC) suojaaminen SCS (A) esiintymä.
* Kaikki virtuaalilaitteiksi SCS (A) esiintymät käyttää tallennustilan oman tilin. (A) SCS esiintymän sekä SAP yleisen kansion, replikoida tallennustilan-tililtä toisen tallennustilan tilin käyttämällä SIOS DataKeeper replikoinnin. Tallennustilan tilien selattava aiheuttaa siitä yhden (A) SCS Windows-klusterisolmu, mutta ei kokonaisuudessaan (A) SCS-palvelun. 
* KAIKKI edustava SAP server sovelluskerroksen VMs kolmannessa käytettävyys määritetään.
* KAIKKI SAP-sovelluksen palvelimet VMs käyttää tallennustilan oman tilin. Tallennustilan tilien selattava aiheuttavat yksi SAP-sovelluspalvelin, jossa SAP muiden kuin edelleen siitä.

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] HA Linux

Arkkitehtuuri SAP HA Linux Azure-arvo on lähinnä sama kuin Windowsin yllä olevien ohjeiden mukaisesti. Vuodesta Jan 2016 liittyy kaksi rajoituksista kuitenkin:

* vain SAP Ietokannan 16 on tällä hetkellä tueta Linux-Azure ilman Ietokannan replikoinnin ominaisuuksia. 
* ei ole SAP (A) SCS HA ratkaisua Linux Azure-vielä tueta

Vuoksi saavuttaman tammikuussa 2016 SAP-Linux-Azure-järjestelmän et voi saavuttaminen saman käytettävyys SAP-Windows-Azure-järjestelmää puuttuu HA (A) SCS esiintymän ja yhden esiintymän SAP Ietokannan tietokannan.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Käyttämällä automaattinen käynnistys SAP esiintymien

SAP: in tarjoaa toiminnot Aloita SAP esiintymät heti sisällä AM OS alkamisen jälkeen. Tarkat ohjeet on kuvattu SAP Knowledge Base-artikkelissa [1909114] - automaattisesti käyttämällä parametri määrittää käynnistäminen SAP-esiintymät. Kuitenkin SAP ei suositella asetusta muistikirjaa, koska ei ole ohjausobjektin esiintymän käynnistyy järjestyksessä, olettaen useita AM käytössä vaikuttaa tai useita kertoja suoritettiin AM kohden. Jos tavallinen Azure skenaario yhden SAP sovelluksen server-esiintymän AM ja yksittäisen AM, tekstin hakeminen uudelleen kirjainkokoa, automaattinen käynnistys ei ole todella tärkeitä ja voidaan ottaa käyttöön lisäämällä parametriä:

    Autostart = 1

SAP-ABAP ja/tai Java-esiintymän alku-profiiliin.

> [AZURE.NOTE] 
> Käynnistä-parametri voi olla joitakin downfalls. Tarkemmin sanottuna parametri tuottaa SAP ABAP tai Java esiintymän alkuun, kun esiintymän liittyvät Windows-tai Linux-palvelu on käynnistetty. Näin on varmasti kun käyttöjärjestelmät käynnistyy ylöspäin. Kuitenkin käynnistyy SAP-palvelut ovat myös yleisiä asian SAP ohjelmiston elinkaaren hallinta-toiminnon, kuten summa tai muu päivittää tai päivittää. Näitä toimintoja ei odotat erillisen käynnistettävä automaattisesti lainkaan. Tämän vuoksi automaattinen käynnistys-parametri poistetaan käytöstä ennen näiden tehtävien suorittamista. Määrittää-parametrin myös asiakkaan kanssa ei voi käyttää SAP-esiintymät, jotka on liitetty, kuten ASCS/SCS/CI.

SAP: n määrittää koskevat lisätiedot esiintymät tässä artikkelissa

* [Aloita/Lopeta SAP sekä oman Unix palvelimen Aloita/Lopeta](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Aloitus- ja SAP NetWeaver hallinta tekijöiden pysäyttäminen](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Automaattinen Aloita sekä HANA tietokannan ottaminen käyttöön](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)


### <a name="larger-3-tier-sap-systems"></a>Suurempi 3 tason SAP-järjestelmiin
3 tason SAP käyttömahdollisuudet suuren käytettävyyden ominaisuuksia käytössä kuvatut aiemmat osat jo. Mutta Entä järjestelmien, jossa DBMS palvelinvaatimukset ovat liian suuria se sijaitsee Azure, mutta SAP-sovelluskerroksen voitu käyttöön kyselyjä Azure?

#### <a name="location-of-3-tier-sap-configurations"></a>3 tason SAP-määrityksiä sijainti

Voit jakaa sovelluksen tason ei tueta itse tai sovelluksen ja DBMS taso paikallisen ja Azure välillä. SAP-järjestelmä on joko täysin käyttöön paikallisessa tai Azure-tietokannassa. Se myös ei tueta on suorittaa paikallisen ja jotkin Azure sovelluksen-palvelimiin. Tämä on sen keskustelun aloituskohdan. On myös ei tukevat on DBMS osat SAP-järjestelmää ja SAP server sovelluskerroksen käyttöön kaksi eri Azure-alueilla. Esimerkiksi Valitse Yhdysvaltojen Keski-Länsi Yhdysvalloissa ja SAP sovelluskerroksen DBMS. Syy tukemiseen tällaisia määrityksiä ei on SAP NetWeaver-arkkitehtuuri viive suojaustasoa.

Päälle, Edellinen vuosi tietojen center kehittänyt kumppanit tiedostojen sijainnit Azure alueiden kurssin. Nämä tiedostojen sijainnit on hyvin lähellä toisiaan Azure tiedot usein keskikohdan mukaan Azure-alueella. Lyhyen etäisyyden ja yhteyden kyselyjä Azure kautta ExpressRoute rinnakkain kohteita voi aiheuttaa viive, joka on pienempi kuin 2ms. Tässä tapauksessa paikantaa (mukaan lukien tallennustilan SAN/NAS) DBMS kerroksen tiedostojen sijainti ja SAP sovelluskerroksen Azure-tietokannassa on mahdollista. Vuodesta joulu 2015 olemme ei ole mitään ominaisuuksissa kuin. Mutta eri asiakkaiden kanssa-SAP sovellusten käyttävät esimerkiksi tavoista jo. 

### <a name="offline-backup-of-sap-systems"></a>Offline-tilassa varmuuskopiointi ja SAP-järjestelmiin

Määräytyy SAP-kokoonpanon valinnut (tason 2 tai 3 tason) voitu tarvitse varmuuskopiointi. Tietokannan varmuuskopion itse sekä AM sisältö. DBMS liittyvät varmuuskopioiden odotetaan tehtävä tietokannan menetelmillä. Yksityiskohtainen kuvaus eri tietokantojen löytyy DBMS oppaasta [dbms-opas]. Toisaalta SAP-tiedot voidaan varmuuskopioida (mukaan lukien tietokannan-sisältö) offline-tilassa tavalla kuin tässä osiossa esitettyjä online- tai seuraavan osion ohjeiden mukaisesti.

Offline-tilassa varmuuskopioinnin lähinnä edellyttäisi AM palvelun Azure-portaalissa, sulkeminen ja peruste AM levyn ja kaikki kopio liitetään näennäiskiintolevyjen AM. Tämä säilyttää AM ja sen liittyvät levylle aika-kuva-kohtaa. On suositeltavaa kopioida 'varmuuskopioiden' eri Azure-tallennustilan tilin. Näin ollen menettelyä kuvattu luvun [kopioiminen levyjen Azure-tallennustilan tilien välillä] [suunnittelu-opas-5.4.2] tämän asiakirjan sovelletaan.
Lisäksi Sammuta Azure-portaalissa jokin myös seuraavalla tavalla Powershell tai CLI seuraavassa kuvatulla tavalla: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Palauttaa valtion koostuvat poistaminen perus AM sekä peruste AM alkuperäisen levyille ja näennäiskiintolevyjen kopioimalla alkuperäisen tallennustilan tilin takaisin tallennetun näennäiskiintolevyjen ja mallirakenteeseen järjestelmän otettu käyttöön.
Tässä artikkelissa kerrotaan esimerkiksi komentosarjan Powershell toimintaohjeet: <http://www.westerndevs.com/azure-snapshots/>

Varmista, että Asenna uusi SAP-käyttöoikeus, koska restoing AM varmuuskopion yllä olevien ohjeiden mukaisesti Luo uusi laitteisto-avain.

### <a name="online-backup-of-an-sap-system"></a>SAP-järjestelmää online varmuuskopiointi

Varmuuskopion DBMS suoritetaan DBMS tietyn menetelmiä kuvatulla tavalla [DBMS oppaan] [dbms-opas]. 

Muut VMs SAP-järjestelmässä voi varmuuskopioida Azure virtuaalikoneen varmuuskopiointi-toiminnon avulla. Azure virtuaalikoneen varmuuskopion käytössä een etuajassa 2015 ja on tänä normaaliin tapaan, voit varmuuskopioida valmis AM Azure-tietokannassa. Azure varmuuskopiointi tallentaa varmuuskopioista Azure ja sallii AM palautuksen uudelleen. 

> [AZURE.NOTE] 
> Vuodesta joulu 2015 AM varmuuskopioinnista pidä yksilöllinen tunnus AM koon muuttamiseen tarkoitettu SAP: n käyttöoikeudet. Tämä tarkoittaa, että palauttaminen varmuuskopiosta AM vaatii asennuksen uuden SAP käyttöoikeuden avaimen palautettu AM pidetään uusi AM ja ei korvaa entisen siinä, joka on tallennettu. Jan 2016 vuodesta Azure AM varmuuskopiointi ei tue VMs, jotka on otettu Azure Resourc hallinnan vielä.

> ![Windows][Logo_Windows] Windows
>
> Teoriassa VMs, suorita tietokannat voidaan varmuuskopioida yhdenmukaisesti sekä jos DBMS järjestelmien tukee Windows-VSS (tilannevedospalvelu <https://msdn.microsoft.com/library/windows/desktop/bb968832(v=vs.85).aspx> ) muodossa, kuten SQL Server suorittaa.
> Muista kuitenkin, jotka perustuvat Azure AM varmuuskopioiden ajankohta: palauttaa tietokantojen eivät ole mahdollista. Tämän vuoksi Suosituksena on tietokantojen varmuuskopioiden DBMS toiminnon sen sijaan, että käyttäisit Azure AM varmuuskopiointi
>
> Tutustu Azure virtuaalikoneen varmuuskopiointi Ota Aloita tästä: <https://azure.microsoft.com/documentation/articles/backup-azure-vms/>.
>
> Muita vaihtoehtoja on käytettävä yhdistelmä, Microsoft Data Protection Manager asentaa Azure AM ja Azure varmuuskopion varmuuskopiointi ja palauttaminen tietokantoihin. Lisätietoja löytyy tähän: <https://azure.microsoft.com/documentation/articles/backup-azure-dpm-introduction/>.  


> ![Linux][Logo_Linux] Linux
> 
> Ei-Linux VSS Windows ei vastaa. Tämän vuoksi vain tiedoston yhdenmukaisia varmuuskopiot on mahdollista, mutta ei sovelluksen-yhdenmukaisia varmuuskopiot. SAP-DBMS varmuuskopioinnin olisi valmis käyttämällä DBMS-toimintoja. Tiedostojärjestelmän mukaan lukien SAP liittyviä tietoja voi tallentaa esimerkiksi käyttämällä tar kuvatulla tähän: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>


### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Azure kuin tuotannon SAP maisemien DR-sivustossa

Mid 2014 jälkeen eri osien ympärille Hyper-V, System Center ja Azure laajennuksia Ota käyttöön Azure käyttö, kun paikallinen virtuaalilaitteiksi DR sivuston perusteella Hyper-V. 

Blogi, joka käsittelee ottamisesta käyttöön tämä ratkaisu on kuvattu seuraavassa: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>

## <a name="summary"></a>Yhteenveto
SAP-järjestelmiin Azure suuren käytettävyyden avainkohtia ovat seuraavat:

* Tässä vaiheessa kohdassa kerralla virheen SAP yksittäisen-kohdan ei voi suojata täsmälleen samalla tavalla kuin se voidaan toteuttaa paikallisten versioiden. Syynä on jaettu levyn klustereiden ei ole vielä voi muodostaa Azure-tietokannassa ilman 3 osapuolen ohjelmistoja.
* DBMS kerroksen sinun on käytettävä DBMS toimintoja, jotka eivät ole riippuvaisia jaetun levyn klusterin tekniikka. Tiedot on kuvattu [DBMS oppaan] [dbms-opas].
* Pienennä Azure infrastruktuurin tai host ylläpito sisällä vika toimialueiden vianmääritys vaikutus, kannattaa käyttää Azure käytettävyys määrittää:
    * On suositeltavaa on yksi käytettävyys määrittää SAP-sovelluskerroksen.
    * On suositeltavaa olla erillinen käytettävyys SAP DBMS kerroksen.
    * On ei ole suositeltavaa käyttää samaa käytettävyys eri SAP-järjestelmiin VMs määrittäminen.
* SAP-DBMS kerroksen tarkoituksiin varmuuskopiointi Tarkista [DBMS oppaan] [dbms-opas].
* SAP-valintaikkunan esiintymät varmuuskopioiminen on pieni järkevää, koska se on usein nopeampaa Ota uudelleen yksinkertainen valintaikkunan esiintymät.
* Varmuuskopiointi AM, joka sisältää yleisen kansion SAP-järjestelmää ja sitä eri esiintymien profiilit tulkita ja suoritetaan Windows varmuuskopioimalla tai Linux esimerkiksi Kohdekeh. Koska Windows Server 2008: n (R2) ja Windows Server 2012 (R2) väliset erot, joka helpottaa uudempaan Windows Serverissä voit varmuuskopioida julkaisee, on suositeltavaa käyttää Windows Server 2012 (R2) Vieras Windows-käyttöjärjestelmä. 
