<properties
   pageTitle="Valitse Linux näennäiskoneiden (VMs) – DBMS käyttöönotto-oppaassa SAP NetWeaver | Microsoft Azure"
   description="SAP NetWeaver-Linux näennäiskoneiden (VMs) – DBMS käyttöönotto-opas"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# <a name="sap-netweaver-on-azure-virtual-machines-vms--dbms-deployment-guide"></a>SAP NetWeaver-Azure-virtuaalikoneissa (VMs) – DBMS käyttöönotto-opas

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

[dbms-guide]:virtual-machines-linux-sap-dbms-guide.md (Linux näennäiskoneiden (VMs) – DBMS käyttöönotto-oppaassa SAP-NetWeaver) [dbms-guide-2.1]:virtual-machines-linux-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (välimuisti VMs ja näennäiskiintolevyjen) [dbms-guide-2.2]:virtual-machines-linux-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (ohjelmiston RAID) [dbms-guide-2.3]:virtual-machines-linux-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azuren tallennustilaan) [dbms-guide-2]:virtual-machines-linux-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (rakenteen RDBMS käyttöönoton) [dbms-guide-3]:virtual-machines-linux-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (suuri käytettävyys ja palauttaminen ja Azure VMs) [dbms-guide-5.5.1]:virtual-machines-linux-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 ja sitä uudemmissa versioissa) [dbms-guide-5.5.2]:virtual-machines-linux-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 ja aikaisempien versioiden) [dbms-guide-5.6]:virtual-machines-linux-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (SQL Server-kuvien käyttämisestä ulos Microsoft Azure Marketplacesta) [dbms-guide-5.8]:virtual-machines-linux-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (Yleinen SQL Server Azure yhteenveto-SAP: n) [dbms-guide-5]:virtual-machines-linux-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (yksityiskohtia SQL Server RDBMS) [dbms-guide-8.4.1]:virtual-machines-linux-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (tallennustilan määritys) [dbms-guide-8.4.2]:virtual-machines-linux-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (varmuuskopioiminen ja palauttaminen) [dbms-guide-8.4.3]:virtual-machines-linux-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (suorituskyvyn Huomioitavaa varmuuskopiointi ja palauttaminen) [dbms-guide-8.4.4]:virtual-machines-linux-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (muu) [dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-linux-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-linux-sap-deployment-guide.md (Linux näennäiskoneiden (VMs) – käyttöönotto-oppaassa SAP-NetWeaver) [deployment-guide-2.2]:virtual-machines-linux-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP resursseja) [deployment-guide-3.1.2]:virtual-machines-linux-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (käyttöönotto mukautetun kuvan AM) [deployment-guide-3.2]:virtual-machines-linux-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (tapaus 1: käyttöönotto ulos SAP Azure Marketplacesta AM) [deployment-guide-3.3]:virtual-machines-linux-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (tapaus 2: käyttöönotto AM mukautetun kuvan SAP: n) [deployment-guide-3.4]:virtual-machines-linux-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 ( Tapaus 3: Siirtämisestä AM paikallisen Näennäiskiintolevyn Azure-generalized käyttäminen SAP) [deployment-guide-3]:virtual-machines-linux-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (käyttöönoton tilanteita, joissa on VMs Microsoft Azure-SAP: n) [deployment-guide-4.1]:virtual-machines-linux-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (käyttöönotto Azure PowerShell cmdlet-komennot) [deployment-guide-4.2]:virtual-machines-linux-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Lataa ja tuo SAP asiaa PowerShell cmdlet-komennot) [deployment-guide-4.3]:virtual-machines-linux-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (paikallisen toimialueelle - vain Windows liittyä AM) [deployment-guide-4.4.2]:virtual-machines-linux-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [käyttöönotto-oppaan-4.4]: Virtual-Machines-Linux-SAP-Deployment-Guide.MD#c7cbb0dc-52a4-49DB-8e03-83e7edc2927d (lataaminen, asennus ja ota Azure AM agentti) [deployment-guide-4.5.1]:virtual-machines-linux-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (PowerShellin Azure) [deployment-guide-4.5.2]:virtual-machines-linux-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI) [deployment-guide-4.5]:virtual-machines-linux-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Määritä Azure parannetun seuranta-laajennus SAP) [deployment-guide-5.1]:virtual-machines-linux-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (valmiuden Tarkista Azure parannetun seurannasta SAP: n) [deployment-guide-5.2]:virtual-machines-linux-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (kuntotietojen tarkistuksessa Azure varten Seuranta infrastruktuurin määritys) [deployment-guide-5.3]:virtual-machines-linux-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (edelleen vianmääritys Azure seuranta-infrastruktuuria SAP: n)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-linux-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-linux-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-linux-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-linux-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-linux-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-linux-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-linux-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-linux-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-linux-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-linux-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-linux-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-linux-sap-get-started.md
[getting-started-dbms]:virtual-machines-linux-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-linux-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-linux-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:virtual-machines-linux-sap-planning-guide.md (Linux näennäiskoneiden (VMs) – suunnittelu ja käyttöönotto-oppaassa SAP-NetWeaver) [planning-guide-1.2]:virtual-machines-linux-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (resursseja) [planning-guide-11]:virtual-machines-linux-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (suuri käytettävyys (HA) ja tietojen palauttaminen (DR) käynnissä-Azuren näennäiskoneiden SAP NetWeaverin) [planning-guide-11.4.1]:virtual-machines-linux-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (suuren käytettävyyden SAP-sovelluksen palvelinten) [planning-guide-11.5]:virtual-machines-linux-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (SAP esiintymien avulla määrittää) [planning-guide-2.1]:virtual-machines-linux-sap-planning-guide.md# 1625df66-4cc6-4d60-9202-de8a0b77f803 (vain Pilvipalveluita - virtuaalikoneen ominaisuuksissa kyselyjä Azure ilman riippuvuudet paikallisen asiakkaan verkossa) [planning-guide-2.2]:virtual-machines-linux-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (rajat tiloissa - käyttöönoton yhden tai useita SAP VMs kyselyjä Azure kanssa on täysin integroitu paikalliseen verkkoon vaatimus) [planning-guide-3.1]:virtual-machines-linux-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure alueet) [planning-guide-3.2.1]:virtual-machines-linux-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (vika toimialueita) [planning-guide-3.2.2]:virtual-machines-linux-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Päivitä toimialueita) [planning-guide-3.2.3]:virtual-machines-linux-sap-planning-guide.md#18810088- f9be - 4c 97-958a - 27996255c 665 (Azure käytettävyys sääntöjoukot) [planning-guide-3.2]:virtual-machines-linux-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Microsoft Azure virtuaalikoneen käsite) [planning-guide-3.3.2]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium tallennus) [planning-guide-5.1.1]:virtual-machines-linux-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (siirtyvät AM paikallisen Azure-generalized levyllä) [planning-guide-5.1.2]:virtual-machines-linux-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (käyttöönotto asiakkaan tietyn kuvan AM) [planning-guide-5.2.1]:virtual-machines-linux-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (valmistelu siirtymisen AM paikallisen Azure-generalized levyllä) [ Planning-Guide-5.2.2]:Virtual-Machines-Linux-SAP-Planning-Guide.MD#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (valmistelu käyttöönoton AM asiakkaan jotakin tiettyä kuvaa ja SAP: n) [planning-guide-5.2]:virtual-machines-linux-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (SAP Azure kanssa valmistellaan VMs) [planning-guide-5.3.1]:virtual-machines-linux-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (ero välillä Azure levy ja Azure kuva) [planning-guide-5.3.2]:virtual-machines-linux-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (lataamisesta, Azure-paikallisen Näennäiskiintolevyn) [planning-guide-5.4.2]:virtual-machines-linux-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (kopioiminen levyjen Azure-tallennustilan tilien välillä) [planning-guide-5.5.1]:virtual-machines-linux-sap-planning-guide.md# 4efec401-91e0-40c0-8e64-f2dceadff646 (AM/Näennäiskiintolevyn rakenteen SAP-versioiden) [planning-guide-5.5.3]:virtual-machines-linux-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (asetus levyjen automount) [planning-guide-7.1]:virtual-machines-linux-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (yksi AM SAP NetWeaver esittely/koulutus skenaarion kanssa) [planning-guide-7]:virtual-machines-linux-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Cloud-Only käsitteitä käyttöönotosta SAP esiintymien) [planning-guide-9.1]:virtual-machines-linux-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure seuranta ratkaisu SAP) [planning-guide-azure-premium-storage]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium tallennus)

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
[planning-guide-microsoft-azure-networking]:virtual-machines-linux-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-linux-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

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
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-linux-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes]:virtual-machines-linux-sizes.md
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
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönottomalli.

Tässä oppaassa on osa ohjeiden soveltamisesta ja Microsoft Azure SAP-Ohjelmiston käyttöönotto. Ennen luetaan oppaan, lue [suunnittelu ja käyttöönotto-oppaassa] [-suunnitteluoppaan]. Tässä asiakirjassa kerrotaan eri relaatio tietokannan Management Systems (RDBMS) ja siihen liittyvien tuotteiden SAP-Microsoft Azuren näennäiskoneiden (VMs) yhdessä käyttäen Azure-infrastruktuurin Service (IaaS)-ominaisuuksien käyttöönotto.

Paperin täydentää SAP asennusohjeet ja SAP muistiinpanoja, joka edustaa asennusten ja SAP-ohjelmisto-versioiden ensisijainen resurssit ympäristöissä annettu

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="general-considerations"></a>Yleistä
Tämän luvun odotettujen käynnissä SAP liittyvät DBMS järjestelmien Azure VMs otetaan käyttöön. On muutamia viittausta tiettyihin DBMS järjestelmiin tämän luvun. Sen sijaan tiettyihin DBMS järjestelmiin käsitellään sisällä tässä asiakirjassa tämän luvun jälkeen.

### <a name="definitions-upfront"></a>Määritelmien toimiston ulkopuolella
Koko asiakirjan Käytämme seuraavat ehdot:

* IaaS: Infrastruktuuri palveluna.
* PaaS: Ympäristö palveluna.
* SaaS: Ohjelmiston palveluna.
* SAP-osan: yksittäisen SAP sovelluksen esimerkiksi ECC, Mustavalkoinen, ratkaisun hallinta tai EP.  SAP-osat voivat perustua perinteinen ABAP tai Java-tekniikoiden tai ei-NetWeaver mukaan-sovellus, esimerkiksi Business objekteja.
* SAP-ympäristön: vähintään yksi SAP-osien loogisesti ryhmittää liiketoimintatoimintojen suorittamisen esimerkiksi kehitystä, QAS, koulutus, DR tai tuotannon.
* SAP-vaaka: Tämä koskee koko SAP-kohteita asiakkaan IT vaaka. SAP-vaaka sisältää kaikki tuotannon ja tuotannon ympäristössä.
* SAP-järjestelmää: Yhdistelmä DBMS kerros ja sovelluskerroksen, kuten SAP ERP kehitysympäristö ja SAP Mustavalkoinen testi järjestelmän, SAP CRM tuotannon järjestelmän jne. Azure käyttöönotoissa se ei tue jakaa nämä kaksi tasoa paikallisen ja Azure välillä. Tämä tarkoittaa, että SAP-järjestelmä on joko käyttöön paikallisen tai se on otettu käyttöön Azure. Voit kuitenkin ottaa SAP-vaaka Azure tai paikallisen eri järjestelmiä. Voit esimerkiksi SAP CRM: N kehittämisketjun käyttöönotto ja esikatsella järjestelmien Azure, mutta SAP CRM tuotannon järjestelmän paikalliseen.
* Vain pilvipalveluita käyttöönoton: käyttöönoton, jossa Azure tilaus ei ole yhdistetty sivusto sivusto tai paikallisen verkkoinfrastruktuuria ExpressRoute-yhteyden kautta. Yhteiset Azure dokumentaatio tällaiset ominaisuuksissa on myös kuvailla vain Cloud-käyttöönotoissa. Käyttämällä tätä menetelmää käyttöön näennäiskoneiden käytetään kautta Internet- ja julkinen Internet-päätepisteet myönnetyt VMs Azure-tietokannassa. Paikalliseen Active Directory (AD) ja DNS ei ole laajennettu Azure tämäntyyppisten ominaisuuksissa. Näin ollen VMs eivät ole paikallisen Active Directory-osa. Huomautus: Vain Pilvipalveluita ominaisuuksissa tässä asiakirjassa määritellään valmis SAP maisemien, joka suoritetaan yksinomaan Azure Active Directory-tai nimenselvitys ilman-paikallisen julkisen cloud kyselyjä. Vain pilvipalveluita määrityksiä ei tue tuotannon SAP-järjestelmiin tai määritykset, jossa SAP STM tai paikallisen muita resursseja on Azure ja resurssit, jotka asuvat paikallisen isännöimät SAP-järjestelmiin keskenään.
* Usean paikallisen: Kuvataan tilanne, jossa VMs otetaan käyttöön Azure tilaukseen, joka sisältää sivusto sivusto, usean sivuston tai ExpressRoute paikallisen datacenter(s) ja Azure väliset yhteydet. Yhteiset Azure asiakirjat, tällaiset ominaisuuksissa on myös kuvailla paikallisen-skenaarioita. Yhteys syy on Laajenna paikallisen toimialueen, paikallisen Active Directory ja paikallisten DNS Azure kyselyjä. Paikallisen vaaka on laajennettu tilaus Azure varoja. Ottaa tämän tunnisteen, VMs voi olla osa paikallisen toimialueen. Toimialueen paikallisen toimialueen käyttäjät voivat käyttää palvelimet ja services voit suorittaa nämä VMs (kuten DBMS palvelut). Viestintä- ja nimi tarkkuus välillä VMs käyttöön paikallisen ja otettu käyttöön Azure VMs on mahdollista. Odotamme tämä käyttöönoton SAP kalusto-Azure useimmin käytetty vaihtoehto. Katso [Tämä] [ vpn-gateway-cross-premises-options] artikkelissa ja [tämän] [ vpn-gateway-site-to-site-create] lisätietoja.

> [AZURE.NOTE] SAP-järjestelmiin, jossa paikallisen toimialueen Azuren näennäiskoneiden SAP-järjestelmiin kuuluvat paikallisen-versioiden tuetaan tuotannon SAP-järjestelmiin. Paikallisen-tuetaan käyttöönotto osia tai suorittaa SAP maisemien Azure kyselyjä. Valmis SAP-vaaka käytössä myös Azure edellyttää ottaa ne VMs osaksi paikallisen toimialueen ja Active Directory. Entisen versioiden asiakirjojen puhuimme Hybrid IT käyttötavoista, jossa termi 'Hybrid' sijaitsee siitä, että on paikallisen-yhteys, paikallisen ja Azure välillä. Tässä tapauksessa 'Hybrid' sähköpostitiliäsi VMs Azure-tietokannassa on osa paikallisen Active Directory.

Jotkin Microsoft ohjeissa kerrotaan paikallisen-skenaariot hieman eri tavalla, erityisesti niiden DBMS HA-määrityksiä. Kyseessä SAP liittyvät asiakirjat paikallisen-skenaario vain kiehuu alaspäin ottaa sivusto sivusto tai yksityisten (ExpressRoute)-yhteyden ja SAP-vaaka on jaettu paikallisen ja Azure kertoma.

### <a name="resources"></a>Resurssit
Seuraavat apuviivat ovat käytettävissä SAP-käyttöönotoissa, valitse Azure aihe:

* [Azuren näennäiskoneiden (VMs) – suunnittelu ja käyttöönotto-oppaassa SAP-NetWeaver] [-suunnitteluoppaan]
* [Azuren näennäiskoneiden (VMs) – käyttöönotto-oppaassa SAP-NetWeaver] [käyttöönotto-oppaan]
* [Azuren näennäiskoneiden (VMs) – DBMS käyttöönotto-opas (tämä asiakirja) SAP-NetWeaver] [dbms-opas]

SAP-Azure Aiheeseen liittyvät seuraavat SAP-Huomautukset:

| Numero   | Otsikko
|------------|--------
| [1928533] | SAP-sovellusten Azure: tuetut tuotteet ja Azure AM tyypit
| [2015553] | Valitse Microsoft Azure SAP: tukevat edellytykset
| [1999351] | Parannetun Azure seurannan SAP: n vianmääritys
| [2178632] | Seurannan arvot SAP Microsoft Azure-näppäin
| [1409604] | Virtualisointi Windows: parannetun seuranta
| [2191498] | Linux Azure ja SAP: parannetun seuranta
| [2039619] | SAP-sovellusten Microsoft Azure käyttämällä Oracle-tietokantaan: tuetut tuotteet ja versiot
| [2233094] | DB6: IBM DB2 käyttäminen Linux, UNIX ja Windows - lisätietoja Azure SAP-sovellukset
| [2243692] | Valitse Microsoft Azure (IaaS) AM Linux: SAP käyttöoikeuden vianmääritys
| [1984787] | SUSE LINUX Enterprise Server 12: Asennusohjeet
| [2002167] | Punainen on Enterprise Linux 7.x: asennuksen ja päivitys

Lue myös [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) , joka sisältää kaikki SAP muistiinpanot Linux.

Sinulla on oltava kokemusta Microsoft Azure-arkkitehtuuri ja miten Microsoft Azuren näennäiskoneiden on otettu käyttöön ja ylläpitämä. Voit etsiä lisätietoja tähän <https://azure.microsoft.com/documentation/>
 
> [AZURE.NOTE] Olemme **keskustella Microsoft Azure-ympäristön (PaaS) palveluja Microsoft Azure-ympäristön kuin** . Tässä asiakirjassa on käynnissä tietokannan hallintajärjestelmään (DBMS)-Microsoft Azuren näennäiskoneiden (IaaS) vain, kun suorittaa DBMS paikallisen ympäristön. Tietokannan ominaisuudet ja toiminnot nämä kaksi tarjouksia välillä on täysin erilainen ja olisi ei saa sekoittaa keskenään. Katso myös: <https://azure.microsoft.com/services/sql-database/>

Vaikka emme ovat keskustella IaaS, Windows, Linux ja DBMS asennuksesta ja määrityksestä ovat yleensä olennaisesti sama kuin mikä tahansa virtual tai ajoneuvon metalli tietokoneeseen, sinun on asennettava paikallisen. On kuitenkin joitakin arkkitehtuuri ja järjestelmän hallinta käyttöönoton päätökset, jotka ovat erilaisia, kun käyttävien IaaS. Tämän asiakirjan tarkoituksena on selitetään koskevien arkkitehtuuri ja järjestelmän hallinnan erot, jotka sinun on valmisteltava, kun käytät IaaS.

Yleensä erotuksella, että tässä asiakirjassa keskustella yleinen alueet ovat:

* SAP-järjestelmiin, jotta sinulla on oikeat tiedot ERISNIMI AM/Näennäiskiintolevyn asettelun suunnitteleminen tiedoston asettelu ja toteuttaa tarpeeksi IOPS havainnollistamiseen varten.
* Verkko-huomioitavista IaaS käytettäessä.
* Tietokannan avaamiseksi ominaisuuksia, jotta optimointi tietokannan asettelun voit.
* Varmuuskopiointi ja palauttaminen huomioon seuraavat seikat IaaS.
* Käyttävien erityyppisiä kuvia käyttöönottoa varten.
* Suuri käytettävyys Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Rakenteen RDBMS käyttöönotto
Järjestyksessä noudattamalla tämän luvun, se on tarpeen selvittääksesi, mikä on esitetty [käyttöönotto-oppaan] [käyttöönotto-oppaan] [käyttöönotto-oppaan-3] [tässä] luvussa. Tietoa eri AM-arvosarjat ja niiden erot ja Azure vakio- ja Premium tallennustilan erotusten pitäisi ymmärtää ja tunnetut ennen tämän luvun lukeminen.

Maaliskuussa 2015, kunnes Azure näennäiskiintolevyjen, jotka sisältävät käyttöjärjestelmä on enintään 127 gt kokoisia. Tämä rajoitus käytössä poistetaan maaliskuussa 2015 (Saat enemmän tietoja valintaruudun <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/> ). Näennäiskiintolevyjen siitä sisältävä käyttöjärjestelmä, jota voi olla yhtä suuri kuin muiden Näennäiskiintolevyn. On kuitenkin yhä mieluummin rakenne, jossa käyttöjärjestelmä, DBMS ja potentiaalisen SAP-binaaritiedostoja ovat erillään tietokantatiedostoja käyttöönoton. Tämän vuoksi Odotamme Azuren näennäiskoneiden SAP-järjestelmiin on kantaluku AM eli Näennäiskiintolevyn käyttöjärjestelmä asennetaan, tietokanta hallinta järjestelmän suoritettavat ja SAP suoritettavat. DBMS tietojen ja lokitiedostojen tallennettu eri näennäiskiintolevytiedostoja Azuren tallennustilaan (Standard tai Premium tallennustilan) ja liitteenä loogisten levyjen Azure käyttöjärjestelmän alkuperäisen kuvan AM. 

Määräytyy hyödyntäminen Azure vakio- tai Premium Storage (esimerkiksi DS-sarjan tai VMs GS sarja käyttämällä) siellä on muita Azure kiintiön, joka on kuvattu [seuraavassa][virtual-machines-sizes]. Kun suunnittelet Azure näennäiskiintolevyjen, sinun on etsiä parhaan saldo kiintiöiden seuraavat asiat:

* Tietoja tiedostojen määrä.
* Näennäiskiintolevyjen, jotka sisältävät tiedostojen määrä.
* Yksittäisen Näennäiskiintolevyn IOPS kiintiöitä.
* Tiedonsiirto Näennäiskiintolevyn kohden.
* Lisää näennäiskiintolevyjen mahdollista AM koon määrä.
* Yleistä tallennustilan siirtonopeuden AM voidaan lisätä.
 
Azure pakottaa käyttöön IOPS-kiintiön Näennäiskiintolevyn asema kohden. Näiden kiintiöiden määräytyvät näennäiskiintolevyjen isännöimät Azure vakio tallennustilan ja Premium-tallennustilan. I/o viiveitä on täysin erilainen toiseen tallennustilan Premium tallennustilan välittää tekijät paremmin i/o viiveitä kanssa. Kunkin AM erityyppisiä on rajoitettu määrä, jotka ovat liittää näennäiskiintolevyjen. Toinen rajoitus on vain AM tietyntyyppisten voidaan hyödyntää Azure Premium-tallennustilan. Tämä tarkoittaa AM tietynlaista päätös saattaa ei vain määräydy suorittimen ja muistin vaatimuksia, mutta myös IOPS, joita viive ja levytilan siirtonopeuden vaatimukset, jotka on yleensä skaalattu näennäiskiintolevyjen määrä tai Premium tallennustilan levyjen tyyppi. Erityisesti Premium tallennustilan kanssa Näennäiskiintolevyn kokoa myös ehkä vaikuta IOPS ja siirtonopeuden saavuttaa kunkin Näennäiskiintolevyn korjattava määrällä.

Yleistä IOPS nopeutta, näennäiskiintolevyjen määrän otettu käyttöön ja AM kokoa on kaikki liitetty yhdessä voi aiheuttaa Azure määritysten SAP-järjestelmän eri tavalla kuin sen paikalliseen ympäristöön. LUN IOPS rajoituksia ovat yleensä määritettävissä paikallisen käyttöönotoissa. Azure-tallennustilan rajoitukset ovat kiinteä tai kuin Premium tallennustilan määräytyy levy. Niin paikallisten versioiden kanssa näemme asiakkaan käyttömahdollisuudet tietokanta-palvelimien jotka käyttävät useita eri tietomääristä määräten suoritettavat, kuten SAP ja DBMS tai erityinen tietomääristä väliaikaiset tietokannat tai taulukon välilyöntejä. Kun paikalliseen-järjestelmän siirretään Azure, se saattaa johtaa mahdollisten IOPS kaistanleveyden mukaan tuhlausta suoritettavat Näennäiskiintolevyn tai tietokantoihin, jonka Älä tee mitään tai ei IOPS paljon. Tämän vuoksi Azure VMs-on suositeltavaa, että DBMS ja SAP-suoritettavat asentaa OS levyn Jos mahdollista.

Sijaintia tietokantatiedostoja ja lokitiedostojen ja Azure tallennustilaa, tyyppi on määritettävä IOPS, viiveen ja siirtonopeuden vaatimuksia. On tarpeeksi IOPS tapahtumalokin, jotta voit ehkä pakko hyödyntää useita näennäiskiintolevyjen tapahtuman lokitiedoston tai suurempi Premium tallennustilan levy. Tässä tapauksessa jokin yksinkertaisesti luominen ohjelmiston RAID (kuten Windows tallennustilan resurssivarantoon Windows tai MDADM ja LVM (loogisen levyn hallinta), Linux) näennäiskiintolevyjen, joka sisältää tapahtumalokiin kanssa.

___

> ![Windows][Logo_Windows] Windows
>
> Asema Azure-AM D:\ on ei ole samanlainen asema joka varmuuskopioidaan joidenkin paikallisen levyjen Azure Laske solmun. Koska kyseessä ei ole samanlainen, tämä tarkoittaa D:\-asema sisällön tehdyt muutokset menetetään, kun AM käynnistetään. "Muutokset" tärkeää tehdä tallennetut tiedostot, kansiot, jotka on luotu, asennetut sovellukset.
>
> ![Linux][Logo_Linux] Linux
>
> Linux Azure VMs liittää automaattisesti /mnt/resource, joka ei ole samanlainen asema palautettua mukaan paikallisen levyjen Azure Laske solmun aseman. Koska kyseessä ei ole samanlainen, tämä tarkoittaa /mnt/resource sisällön tehdyt muutokset menetetään, kun AM käynnistetään. Muutokset tärkeää tehdä tallennetut tiedostot, kansiot, jotka on luotu, asennetut sovellukset.

___

Määräytyy Azure AM-sarjan Laske-solmu paikallisen levyjä Näytä eri suorituskyvyn, jotka kuuluvat esimerkiksi:

* A0 – A7: Hyvin vähän suorituskykyä. Ei voi käyttää kaikkeen lisäksi windows sivun tiedoston
* A8-A11: 10 000 joitakin IOPS erittäin hyvä suorituskyky ominaispiirteet ja > 1 gt/sec siirtonopeuden
* D-sarja: 10 000 joitakin IOPS erittäin hyvä suorituskyky ominaispiirteet ja > 1 gt/sec siirtonopeuden
* DS-sarja: 10 000 joitakin IOPS erittäin hyvä suorituskyky ominaispiirteet ja > 1 gt/sec siirtonopeuden
* G-sarja: 10 000 joitakin IOPS erittäin hyvä suorituskyky ominaispiirteet ja > 1 gt/sec siirtonopeuden
* GS-sarja: 10 000 joitakin IOPS erittäin hyvä suorituskyky ominaispiirteet ja > 1 gt/sec siirtonopeuden

Yllä lauseet ovat soveltaminen AM tyypit, jotka ovat sertifioituja SAP: N kanssa. AM-sarja, jossa on erinomainen IOPS ja siirtonopeuden oikeutettuja suorituskykykertoimen DBMS joitakin toimintoja, kuten tempdb tai väliaikaisen taulukossa tilan mukaan.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>VMs ja näennäiskiintolevyjen välimuistiasetukset
Kun luodaan näiden levyjen/näennäiskiintolevyjen portaalin kautta tai olemme ladatut näennäiskiintolevyjen VMs voit ottaa käyttöön, emme Valitse i/o-liikenne välillä AM ja ne sijaitsevat Azure tallennustilan näennäiskiintolevyjen välimuistissa. Azure vakio- ja Premium tallennustilan käyttää kaksi eri tekniikoiden tällaista välimuistin. Sekä tapauksissa välimuistin itse levyn tilannevedosten käyttämä tilapäinen levy ja AM (D:\ Windows) tai Linux /mnt/resource saman asemista.
 
Azure vakio tallennustilan mahdollista välimuistin tiedostotyypit ovat:

* Ei välimuistiin tallentaminen
* Lue välimuistiin tallentaminen
* Lukeminen ja kirjoittaminen välimuistiin tallentaminen

Jotta saat yhdenmukaisia ja deterministic suorituskyvyn, sinun kannattaa asettaa välimuistiin Azure vakio säilössä, kaikki näennäiskiintolevyjen sisältävä **DBMS liittyvät datatiedostot, lokitiedostot ja taulukkotilaa "ei mitään"**. Välimuistiin AM voivat jäädä oletusarvoisella.

Azure Premium tallennustilan olemassa seuraavista vaihtoehdoista:

* Ei välimuistiin tallentaminen
* Lue välimuistiin tallentaminen

Azure Premium tallennustilan suositus on hyödyntää SAP-tietokannan **lukea datatiedostot välimuistiasetukset** ja valitsit **ei välimuistiasetukset lokitiedostojen VHD(s)**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Ohjelmiston RAID
Kuten jo mainittu edellä tarvitset saldo tarvittavat tietokantatiedostoja yli näennäiskiintolevyjen, voit määrittää määrän IOPS määrän ja Azure-AM antaa kohti Näennäiskiintolevyn tai Premium tallennustilan enimmäismäärä IOPS Kirjoita. Helpoin tapa käsitellä IOPS kuormituksen näennäiskiintolevyjen päälle on luoda ohjelmistoa RAID eri näennäiskiintolevyjen päälle. Aseta sitten ulos ohjelmiston RAID carved LUNS SAP-DBMS tiedostojen määrä. Määräytyy vaatimukset, sinun kannattaa harkita Premium tallennustilan käyttö sekä jälkeen kaksi kolme eri Premium tallennustilan levyä on suurempi kuin Vakio tallennustilan perusteella näennäiskiintolevyjen IOPS kiintiön. Lisäksi merkittäviä paremmin i/o viive Azure Premium tallennustilan myöntämä. 

Luo eri DBMS järjestelmien tapahtumaloki. Paljon ne lisätään vain Tlog tiedostoja ei avulla, koska DBMS järjestelmien kirjoittaa yhdeksi tiedostoja vain kerrallaan. Tarvittaessa suurempi IOPS korvaukset kuin yhden standardi tallennustilan perusteella Näennäiskiintolevyn voit toimittaa, voit stripe useita vakio tallennustilan näennäiskiintolevyjen päälle tai voit käyttää suurempi Premium tallennustilan levyn tyyppi, joka suurempi IOPS korvaukset lisäksi myös toimittaa tekijät alemman viive kirjoittaminen i tapahtumalokin kyselyjä.
 
Azure käyttöönotoissa, johon Web-sivuston yhteensopivaksi ohjelmiston avulla listan tilanteissa RAID ovat seuraavat:

* Tapahtumaloki lokin/tehdä uudelleen edellyttää enemmän IOPS kuin Azure tarjoaa yhden Näennäiskiintolevyn. Kuten edellä mainittiin tämä voidaan ratkaista rakentamalla LUN ohjelmistoa RAID käyttämällä useita näennäiskiintolevyjen päälle.
* Erimittainen i/o työmäärää jakelua eri datatiedostojen SAP-tietokannasta. Tässä tapauksessa jokin kohdata yhden datatiedoston pallolla kiintiön mieluummin usein. Tämän vuoksi muita tiedostoja ei toimi jopa lähellä yksittäisen Näennäiskiintolevyn IOPS kiintiö. Tässä tapauksessa helpoin ratkaisu on muodostaa yhden LUN ohjelmistoa RAID käyttämällä useita näennäiskiintolevyjen päälle. 
* Et tiedä, mitä tarkka i/o työmäärää, tietojen tiedostoa kohden on, ja vain karkeasti tiedä, mikä yleinen IOPS työmäärää, vastaan DBMS on. Helpoin tehtävä on muodostaa yhden LUN ohjelmiston RAID avulla. Useita näennäiskiintolevyjen takana tämän LUN kiintiön summa sitten olisi täytettävä tunnetut IOPS korko.

___

> ![Windows][Logo_Windows] Windows
>
> Windows Server 2012: ssa tai uudempi versio tallennustilan välilyöntejä käyttö on suositeltavampaa, koska se on tehokkaampaa kuin Windowsin raitasarjoittamista Windowsin aiempien versioiden. Ota huomioon, että voit joutua muuttamaan luominen tallennustilan välilyöntejä ja tallennustilaa jakavat Windows PowerShell-komennoilla käytettäessä Windows Server 2012 käyttöjärjestelmä. PowerShell-komennoilla Täältä löytyvät <https://technet.microsoft.com/library/jj851254.aspx>

> 
> ![Linux][Logo_Linux] Linux
>
> Luoda ohjelmistoa RAID Linux tuetaan vain MDADM ja LVM (loogisen levyn hallinta). Lisätietoja on seuraavissa artikkeleissa:
>
> * [Määritä ohjelmiston RAID Linux] [ virtual-machines-linux-configure-raid] (for MDADM)
> * [LVM määrittämistä Linux-AM Azure-tietokannassa][virtual-machines-linux-configure-lvm]


___

Huomioitavaa hyödyntäminen AM-sarjan, jotka voivat käsitellä Azure Premium tallennustilan yleensä ovat seuraavat:

* Vaatimukset, i/o viiveitä, joka on lähellä SAN/NAS laitteiden aikana.
* Tekijöiden paremmin i/o viive kuin Vakio Azure-tallennustilan voit toimittaa tarve.
* Suurempi IOPS kohti AM kuin mitä voidaan parantaa useita vakio tallennustilan-näennäiskiintolevyjen vastaan AM tietynlaista kanssa.

Koska pohjana Azure-tallennustilan kopioi kunkin Näennäiskiintolevyn vähintään kolme tallennustilan solmujen yksinkertainen RAID 0 raitasarjoittamista voidaan käyttää. Ei ole tarpeen RAID5 tai RAID1.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azuren tallennustilaan
Microsoft Azuren tallennustilaan tallentaa perus AM (kanssa OS) ja näennäiskiintolevyjen tai BLOB ainakin 3 erillisessä tallennustilan solmujen. Kun tallennustilan tilin luominen ei valinta suojan tässä esitetyllä tavalla:

![GEO replikoinnin Azure-tallennustilan tilin käytössä][dbms-guide-figure-100]

Azure tallennustilan paikallista replikointia (paikallisesti tarpeettomat) on suojausominaisuuksia tietojen katoamiselta vuoksi infrastruktuuri-virhe, joka muutamia asiakkaat voi antaa käyttöön. Kun edellä kanssa viidennelle parhaillaan yhden ensimmäiset kolme muunnelma on 4 eri vaihtoehtoja. Etkö löytänyt tarkempi niitä on erottaa:

* **Premium paikallisesti tarpeettomat Storage (LRS)**: Azure Premium tallennustilan toimittaa tehokas, pieni viive levyn tuki näennäiskoneiden voin/O-paljon toiminnoista. On 3 replikoita saman Azure palvelinkeskuksen Azure alueen tiedot. Kopiot on eri vika tai päivittää toimialueita (käsitteitä Katso [Tämä] [suunnittelu-opas-3,2] luvun [suunnittelu Guide][planning-guide]). Replikan tietojen siirtymällä poissa tallennustilan solmun virheen tai levy-virheen vuoksi, kun uusi replika luodaan automaattisesti.
* **Paikallisesti (LRS tarpeettomat tallennustilan)**: Tässä tapauksessa on 3 replikoita saman Azure palvelinkeskuksen Azure alueen tiedot. Kopiot on eri vika tai päivittää toimialueita (käsitteitä Katso [Tämä] [suunnittelu-opas-3,2] luvun [suunnittelu Guide][planning-guide]). Replikan tietojen siirtymällä poissa tallennustilan solmun virheen tai levy-virheen vuoksi, kun uusi replika luodaan automaattisesti. 
* **GEO tarpeettomat Storage (GRS)**: Tässä tapauksessa on asynkroninen replikoinnin, joka syötteen muita 3 replikoiden tiedot toiseen Azure alueessa, joka on useimmissa tapauksissa samaa maantieteellisen alueen (kuten Pohjois Eurooppa ja Länsi Europe). Tämä aiheuttaa 3 muita replikassa, niin, että summa on 6 replikoita. Muunnelma, tämä on lisäys käyttökohteen geo replikoitua Azure alueen tiedot luku tarkoituksiin (lukuoikeudet Geo-tarpeettomat).
* **Vyöhykkeen tarpeettomat Storage (ZRS)**: Tässä tapauksessa 3 replikat tiedot pysyvät samassa Azure-alueella. Kuvatulla tavalla [tämän] [suunnittelu-opas-3.1] [suunnitteluoppaassa] [-suunnitteluoppaan] Azure alueen luvun voi olla useita palvelinkeskusten lähellä toisiaan. LRS replikat jaettava eri palvelinkeskusten, jotka helpottavat yhden Azure alueen yli.

Lisätietoja löytyy [tähän][storage-redundancy].
 
> [AZURE.NOTE] DBMS käyttöönotoissa Geo tarpeettomat tallennustilan käyttö ei ole suositeltavaa
>
> Azure tallennustilan Geo replikoinnin on asynkroninen. Yksittäisten näennäiskiintolevyjen lisätallennustilaa yksittäisen AM replikointi eivät synkronoidu Lukitse vaiheessa. Tämän vuoksi ei ole sopivaa replikoida DBMS-tiedostoja, jotka ovat eri näennäiskiintolevyjen suhteutettuna tai ottaa vastaan ohjelmiston RAID useita näennäiskiintolevyjen perusteella. DBMS ohjelmisto edellyttää, että pysyvä levytilasta synkronoidaan tarkasti eri LUNs ja pohjana levyjen/näennäiskiintolevyjen/Kiintolevylaitteiden. DBMS ohjelmisto käyttää erilaisia menetelmiä sarjan IO kirjoittaminen toimintoihin ja tietokannan Hallintajärjestelmään ilmoittaa, että replikointi kohteena levytilasta on vioittunut, jos nämä vaihtelevat edes muutaman millisekuntia. Jos jokin todella haluaa tietokannan kokoonpano venytetään yli useita näennäiskiintolevyjen geo replikoida tietokannan kanssa, esimerkiksi replikoinnin on näin ollen suoritettavat tietokannan keskiarvot ja toiminnot. Yksi ei kannata-Azure tallennustilan Geo-replikoinnin työn tekemiseen. 
>
> Ongelma kannattaa kerrotaan esimerkiksi järjestelmään. Oletetaan, että sinulla on ladattu Azure on 8 näennäiskiintolevyjen sisältävä DBMS plus yksi Näennäiskiintolevyn tapahtuman lokitiedoston sisältävään datatiedostojen tuominen SAP-järjestelmää. Yksitellen nämä 9 näennäiskiintolevyjen on yhtenäinen menetelmän mukaan DBMS, niihin kirjoitetut tiedot, onko tiedot kirjoitetaan tietoja tai tapahtuman kirjautuvat lokitiedostoihin.
>
> Oikein geo toisinto tiedot ja ylläpitää yhdenmukaisen tietokannan kuva-, kaikki yhdeksän näennäiskiintolevyjen sisältö on oltava geo replikoida tarkalleen siinä järjestyksessä, i/o-toiminnot on kohdistaa yhdeksän eri näennäiskiintolevyjen. Kuitenkin Azuren tallennustilaan geo-replikointi eivät salli määritellä näennäiskiintolevyjen väliset riippuvuudet. Tämä tarkoittaa, että nämä yhdeksän eri näennäiskiintolevyjen sisällön liittyvät toisiinsa ja tietojen muutokset ovat yhdenmukaisia vain silloin, kun replikoiminen järjestyksessä i/o-toiminnot on tapahtunut yli 9 näennäiskiintolevyjen ei tunnista Microsoft Azuren tallennustilaan geo-replikoinnin.
>
> Lisäksi on suuri, että geo replikoida kuvat skenaariota ei ole yhdenmukaisia tietokannan kuvan, myös viivästyy suorituskyvyn, jossa näkyy geo tarpeettomat tallennustilan, jota voi vakavasti ja mahdollisuuksia vaikuttaa suorituskykyyn. Yhteenveto: Älä käytä tällaista tallennustila-arvojen DBMS tyyppi toiminnoista.
 
#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Näennäiskiintolevyjen yhdistäminen yhdeksi Azure virtuaalikoneen tallennustilan palvelutilejä
Azure-tallennustilan tilin on järjestelmänvalvojan rakennetta lisäksi myös rajoitus aihe. Tämän vuoksi rajoitukset vaihtelevat sen mukaan, onko käsittelemme Azure vakio tallennustilan-tili tai Azure Premium tallennustilan tili. Tarkka ominaisuudet ja rajoitukset on lueteltu [Tässä][storage-scalability-targets]
 
Jotta vakio Azure-tallennustilan on tärkeää muistaa IOPS tallennustilan tiliä ei rajoitukset (rivin sisältävä 'Yhteensä pyytää korko' [on]artikkelissa[storage-scalability-targets]). Lisäksi on alkuperäinen raja 100 tallennustilan tilien Azure tilauskohtaisten (vuodesta heinäkuussa 2015). Tämän vuoksi suositellaan saldo IOPS VMs useiden tallennustilan tilien käytettäessä Azure vakio tallennustilan välillä. Tämän vuoksi yhden AM käyttää tallennustilan tilien Ihannetapauksessa Jos mahdollista. Niin Jos käsittelemme DBMS-käyttöönotoissa, joissa kunkin Näennäiskiintolevyn, joka sijaitsee vakio Azure-tallennustilan voi saavuttaa sen kiintiö, kannattaa vain ottaa käyttöön 30 – 40 näennäiskiintolevyjen kohti Azure-tallennustilan tilin, joka käyttää vakio Azure-tallennustilan. Toisaalta, jos hyödyntää Azure Premium tallennustilan ja haluat tallentaa suuren tietokannan asemat, voi hieno IOPS kannalta. Mutta tilin tallennustilan Azure Premium on data-asema, miten hyvin rajoittava kuin Azure vakio tallennustilan tili. Tuloksena vain Valittavanasi näennäiskiintolevyjen Azure Premium tallennustilan-tilin rajoitettu määrä ennen tietojen raja pallolla. "Virtual SAN" Azure tallennustilan tilin end-– Ajattele osoitteessa, joka on rajoitettu ominaisuuksien IOPS ja/tai kapasiteetti. Tämän vuoksi tehtävä säilyy, kuten paikallisten versioiden, voit määrittää eri SAP-järjestelmiin näennäiskiintolevyjen asettelun eri 'imaginaariosan SAN laitteet"tai Azure-tallennustilan tilit.
 
Azure vakio tallennustilan kannattaa ei esittää tallennustilan eri tallennustilan tileistä yksittäisen AM Jos mahdollista.

Olisi DS tai GS sarjaa Azure VMs on mahdollista asennustapa näennäiskiintolevyjen ulos Azure vakio tallennustilan ja Premium tallennustilan tilit. Käytä tapauksessa kirjoittaminen varmuuskopioiden vakio varastoon palautettua näennäiskiintolevyjen olisi DBMS tiedot ja lokitiedostojen Premium säilössä ovat voi ladata kohtaa, johon voi hyödyntää esimerkiksi erilaisten tallennustilan. 

Asiakkaan käyttöönotto ja testaus noin 30 40 näennäiskiintolevyjen tietokannan datatiedostot ja lokitiedostojen sisältävät voit valmisteltu yksittäisen Azure vakio tallennustilan tilin hyväksyttävät suorituskyvyn perusteella. Kuten edellä mainittiin, Azure Premium tallennustilan tilin rajoitus on todennäköisesti voivat sisältää tietoja-valmiuksien ja ei IOPS.

Kuin SAN laitteiden paikallisen, jakaminen edellyttää joitakin seuranta, jotta voit tunnistaa myöhemmin pullonkaulojen Azure-tallennustilan tilin. Azure seuranta tunnisteen SAP ja Azure-portaalissa on työkaluja, joilla voidaan tunnistaa varattu Azure-tallennustilan tilit, jotka voi välittää lainkaan IO suorituskykyä.  Jos tilanne havaitaan on suositeltavaa varattu VMs Siirry Azure-tallennustilan toiseen tiliin. Tutustu [käyttöönotto-oppaan] [käyttöönotto-oppaan] Lisätietoja siitä, miten voit aktivoida SAP-isännän seuranta ominaisuuksia.

Toinen artikkeli yhteenvetojen parhaita käytäntöjä Azure vakio tallennustilan ja Azure vakio tallennustilan tilit ympärille Täältä löytyvät <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>
 
#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>Siirtäminen käyttöön DBMS VMs Azure vakio säilöstä Azure Premium tallennustilaan
Emme kohdata aivan joissakin tilanteissa, jonka asiakkaaksi haluat käyttöön AM siirtäminen Azure Premium tallennustilan vakio Azure-tallennustilan. Tämä ei ole mahdollista ilman siirtämiseen tiedot. Tavoitteen saavuttamiseen seuraavilla tavoilla:

* Voi ainoastaan kopioi kaikki näennäiskiintolevyjen, perus Näennäiskiintolevyn sekä tietojen näennäiskiintolevyjen uudelle Azure Premium tallennustilan tilille. Usein valitsit näennäiskiintolevyjen määrän Azure vakio tallennustilaan ei tarvita data-asema kertoma vuoksi. Mutta monta näennäiskiintolevyjen tarvitaan IOPS vuoksi. Nyt, kun siirrät Azure Premium tallennustilan voit voi Siirry tapaa, jolla pienempi näennäiskiintolevyjen joitakin IOPS siirtonopeuden saavuttamiseksi. Ottaen huomioon, että Azure vakio tallennustilaan maksat käytetyt tiedot ja ei korko.vuosi levyn kokoa näennäiskiintolevyjen määrän asentaminen ei todella merkitystä kannalta kustannukset. Kuitenkin Azure Premium tallennuspaikkaa, jossa maksat korko.vuosi levyn kokoa. Tämän vuoksi useimmat asiakkaan yrittää säilyttää Azure näennäiskiintolevyjen määrän Premium tallennustilaan voidaan toteuttaa IOPS siirtonopeuden numerosta tarvittavat. Useimmat asiakkaat päättää vastaan yksinkertainen 1:1 tapa kopio.
* Jos ei ole vielä otettu käyttöön, ota käyttöön yhden Näennäiskiintolevyn, joka sisältää tietokannan varmuuskopiointi SAP-tietokannan. Varmuuskopiointi-jälkeen käytöstä kaikki näennäiskiintolevyjen, mukaan lukien varmuuskopion sisältävä Näennäiskiintolevyn ja kopioi perus Näennäiskiintolevyn ja Näennäiskiintolevyn varmuuskopiosta Azure Premium-tallennustilan tilin. Sitten Ota perus ylläpito mukaan AM ja käyttöön Näennäiskiintolevyn varmuuskopiosta. Nyt voit luoda Lisää tyhjä Premium tallennustilan levyjä, joiden avulla voit palauttaa tietokannan kahdeksi AM. Oletetaan, että DBMS avulla voit muuttaa polut tietojen ja lokitiedostojen palauttaminen yhteydessä.
* Entisen prosessi, jossa vain varmuuskopion Näennäiskiintolevyn Azure Premium varastoon ja liitä se vastaan AM äskettäin käyttöön ja asentamaasi variaatiota on myös mahdollista.
* Kun olet puutteessa datatiedostojen tietokannan numeron muuttaminen Valitse neljäs mahdollisuus. Tällöin voit suorittaa SAP yhdenmukaisemmat järjestelmän kopio-toiminnolla tuonti ja vienti. Voit tehdä ne Vie tiedostot Näennäiskiintolevyn, kopioituu Azure Premium tallennustilan tilin ja liitä se, jonka avulla Suorita tuonti-prosessien AM. Asiakkaiden käyttää tätä mahdollisuutta pääasiassa, kun he haluavat Vähennä datatiedostot.

### <a name="deployment-of-vms-for-sap-in-azure"></a>SAP-Azure VMs käyttöönotto
Microsoft Azure tarjoaa useita tapoja VMs ja liittyvän levyjen käyttöönotto. Mikä on erittäin tärkeää ymmärtää erot, koska VMs valmistelut saattavat vaihdella määräytyy deployment tavan. Yleensä Odotamme tilanteita, joissa on kuvattu seuraavissa luvuissa kyselyjä.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Käyttöönotto AM Azure Marketplacesta
Haluat ottaa käyttöön oman AM Microsoft tai 3 osapuolen annettu kuva Azure Marketplacesta. Azure oman AM käyttöönoton seuraamasi saman ohjeita ja työkaluja, voit asentaa oman AM sisällä SAP-ohjelmiston tapaan paikalliseen ympäristöön. Azure-AM sisällä SAP-ohjelmiston asentaminen SAP- ja Microsoft suosittelee ladataan ja tallentaa SAP-asennustietovälineen Azure näennäiskiintolevyjen tai luoda Azure-AM "tiedoston palvelimeksi", joka sisältää kaikki tarvittavat SAP asennustietoväline toimi.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Asiakkaan tietyn generalized kuvan AM käyttöönotto
Vuoksi tietyn korjaustiedoston vaatimukset terveisin OS tai DBMS-versioon annettu kuvat Azure Marketplacesta ehkä ei vastaa tarpeitasi. Sen vuoksi voit joutua muuttamaan luominen AM, käyttämällä "Yksityinen" Omat OS/DBMS AM kuvaa, joka voidaan ottaa käyttöön useita kertoja jälkeenpäin. Yksityinen' kuvan valmisteleminen monistaminen, käyttöjärjestelmän on generalized paikallisen AM. Tutustu [käyttöönotto-oppaan] [käyttöönotto-oppaan] Lisätietoja generalize AM.

Jos olet jo asentanut SAP sisällön paikallisen-AM (erityisesti niiden 2 tason järjestelmät), voit mukauttaa SAP-järjestelmäasetuksista jälkeen kautta esiintymän Azure-AM käyttöönoton nimeä toimintosarjan tukemat SAP ohjelmiston valmistelu Manager (SAP Huomautus [1619720]). Muussa tapauksessa voit asentaa SAP-ohjelmiston myöhemmin Azure AM käyttöönoton jälkeen.
 
Tietokannan sisältö SAP-sovelluksen käytössä voit joko luoda sisällön juuri SAP-asennuksen tai voit tuoda sisältöä Azure käyttämällä Näennäiskiintolevyn DBMS tietokannan varmuuskopiolla tai hyödyntäminen Microsoft Azure varastoon suoraan Varmuuskopiointi DBMS ominaisuuksia. Voit tässä tapauksessa valmistella myös näennäiskiintolevyjen, jossa DBMS tiedot ja kirjaudu tiedostoja paikallisen ja tuomalla ne levyjen Azure. Mutta DBMS tietoja, jotka on käytön ladattu paikallisen Azure voit siirtää toimivat Näennäiskiintolevyn levyjä, jotka valmisteltava paikallisen päälle.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>Siirtäminen AM paikallisen Azure-generalized avulla
Aiot tietyn SAP-järjestelmää Siirry Azure (hissin ja vaihto)-ympäristöön. Tämä voidaan tehdä lataamalla Näennäiskiintolevyn, joka sisältää OS SAP-binaaritiedostot ja potentiaalisen DBMS binaaritiedostot plus näennäiskiintolevyt DBMS Azure tiedot ja kirjaudu-tiedostoja. Skenaario #2 yllä vastakkaiseen säilytetään isäntänimi, SAP SUOJAUSTUNNUS ja SAP-käyttäjätilien Azure AM-, kun ne on määritetty paikallisen ympäristön. Tämän vuoksi joilla kuvaa ei tarvitse. Tässä tapauksessa koskevat lähinnä paikallisen-skenaarioissa, jossa osa SAP-vaaka suoritetaan paikallisen ja osat Azure.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Suuri käytettävyys ja Azure VMs ja palauttaminen
Azure tarjoaa seuraavat suuri käytettävyys (HA) ja tietojen palauttaminen (DR) toiminnot, jotka koskevat Käytämme SAP ja DBMS ympäristöissä eri osia

### <a name="vms-deployed-on-azure-nodes"></a>Ottaa käyttöön Azure solmujen VMs
Azure-ympäristö ei tarjoa käyttöön VMs ominaisuuksia, kuten Live siirron. Tämä tarkoittaa sitä, jos palvelinklusterin, jossa AM on otettu käyttöön on tarpeen ylläpito, AM on Hae pysäytetään ja käynnistetään uudelleen. Azure ylläpito suoritetaan käyttämällä ns päivittäminen toimialueiden klustereiden palvelinten kuluessa. Vain yksi Päivitä toimialueen kerrallaan säilytetään. Näiden uudelleenkäynnistyksen aikana ilmenee palvelun keskeytymisen aikana AM suljetaan ja ylläpito suoritetaan AM käynnistetään uudelleen. Useimmat DBMS valmistajat tarjoavat kuitenkin suuren käytettävyyden ja palauttaminen toimintoja, jotka nopeasti uudelleen toisen solmun DBMS-palveluja, jos ensisijainen solmu ei ole käytettävissä. Azure-ympäristö on jaettava VMs ja tallennustilaa Azure muiden päivittäminen toimialueiden varmistaa, että suunniteltu ylläpito tai infrastruktuuri virheet vain vaikuttaa pienen VMs tai palvelujen toimintoja.  Varovainen suunnittelu on mahdollista saada käytettävyys tasot vastaa paikallisen infrastruktuurin.

Microsoft Azure käytettävyys joukot ovat VMs looginen ryhmitys tai palveluja, joka takaa, että VMs ja muut palvelut jaetaan eri vika ja päivittää toimialueiden klusterin siten, että olisi vain yksi solmu Sammuta milloin tahansa yhdessä samanaikaisesti (Lue [Tämä] [ virtual-machines-manage-availability] lisätietoja artikkelissa).

Sitä varten on määritettävä käyttötarkoituksen mukaan, kun toteuttaa VMs tähän tarkastelu:

![Määrittelyn käytettävyys joukon DBMS HA käyttömahdollisuudet][dbms-guide-figure-200]

Jos haluamme luominen DBMS ominaisuuksissa käytettävissä käyttömahdollisuudet (riippumaton yksittäisiä DBMS HA käytettävät toiminnot)-DBMS VMs on:

* Lisää VMs samaan Azure Virtual verkkoon (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* HA-määritys VMs pitäisi olla myös saman aliverkon. Nimenselvitys eri aliverkosta välillä ei ole mahdollista vain Pilvipalveluita käyttöönotoissa, IP-ratkaisu toimii. Käytä sivusto sivusto tai ExpressRoute connectivity paikallisen-käyttöönotoissa, verkossa, jossa on vähintään yksi aliverkon jo muodostetaan. Nimenselvitys tapahtuu mukaan paikallista AD käytännöt ja verkko-infrastruktuuria. 
[Kommentti]: <>  (MSSedusch TODO Testaa Jos-HAARAN edelleen tosi)

#### <a name="ip-addresses"></a>IP-osoitteet
On erittäin suositeltavaa asennuksen HA-määrityksiä varten VMs joustavat tavalla. Käyttäisit IP-osoitteiden osoite HA-partner(s) sisällä HA-määritys ei ole luotettavaa Azure-tietokannassa, ellei staattinen IP-osoitteita. Azure-kielessä on kaksi "Sammuta-käsitteestä:

* Azure-portaalin tai Azure PowerShell cmdlet Stop AzureRmVM kautta Sammuta: Tässä tapauksessa virtuaalikoneen sammuttamisen noutaa ja poistaa varattu. Tämä AM enää peri Azure-tili, vain maksut, selvittää, joiden käytetään tallentamista varten. Jos verkkoliittymän yksityinen IP-osoite ei ole staattinen, IP-osoite on julkaistu ja sitä ei välttämättä verkon-liittymän saa määritetty uudelleen AM uudelleenkäynnistämisen jälkeen vanha IP-osoite. Suorittamiseen Sammuta alaspäin palvelun Azure-portaalissa tai Pysäytä AzureRmVM soittamalla aiheuttaa jään kohdistuksen automaattisesti. Jos et halua deallocat tietokone käyttää Stop AzureRmVM - StayProvisioned 
* Jos suljet OS tason AM AM saa Sammuta ja jakamatta varaustiedoista. Kuitenkin tässä tapauksessa Azure-tili edelleen veloitetaanko AM siitä huolimatta, että se suljetaan. Tässä tapauksessa määritys pysäytetty AM IP-osoite säilyy muuttumattomana. Suljetaan AM Pakota jään kohdistuksen automaattisesti.

Myös paikallisen-skenaariot, oletusarvoisesti sulkeminen ja jään kohdistus tarkoittaa jään varauksen IP-osoitteiden-AM, vaikka paikallisen käytännöt DHCP asetukset ovat eri. 

* Poikkeus on, jos jokin määrittää staattinen IP-osoite verkon-liittymän kuin kuvattu [seuraavassa][virtual-networks-reserved-private-ip].
* Siinä tapauksessa IP-osoite pysyy kiinteä, kunhan verkon-liittymän ei poisteta.

> [AZURE.IMPORTANT] Jos haluat säilyttää koko käyttöönotto perus- ja helpommin hallittaviin, tyhjennä suositus on organisaatio DBMS HA tai DR määritysten Azure sisällä siten, että yhdistävää eri VMs välillä on toimivassa nimenselvitys yhteistyötä toimittaakseen joka VMs: n määrittäminen.
 
## <a name="deployment-of-host-monitoring"></a>Käyttöönoton isännän seuranta
SAP-sovellusten Azuren näennäiskoneiden tehokkaasti käyttö SAP edellyttää mahdollisuus saada host on käynnissä Azuren näennäiskoneiden fyysinen isäntien-tietojen valvominen. Tietyn SAP HostAgent korjaustiedoston tason on suoritettava, jonka avulla SAPOSCOL ja SAP HostAgent tätä ominaisuutta. Tarkka korjaustiedoston taso on kuvattu SAP Huomautus [1409604].

Lisätietoja osista, jotka toimittavat host tiedot SAPOSCOL ja SAPHostAgent käyttöönotto ja elinkaaren hallinta kyseisten komponenttien Lue [käyttöönotto-oppaan] [käyttöönotto-oppaan]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Microsoft SQL Server-tiedot

### <a name="sql-server-iaas"></a>SQL Server IaaS
Microsoft Azure alkaen voit helposti siirtää sisältyy Windows Server-ympäristössä, Azuren näennäiskoneiden SQL Server sovellukset. SQL Server Virtual Machine avulla voit pienentää omistajuuden käyttöönoton, hallinta ja ylläpito yrityksen kehitysyhteisön sovellusten kokonaiskustannukset valinnanvapaus näiden sovellusten Microsoft Azure helposti. SQL Server Azure Virtual Machine-järjestelmänvalvojille ja kehittäjille edelleen käyttää samaa kehittäminen ja hallintatyökalut, jotka ovat käytettävissä paikallisen. 

> [AZURE.IMPORTANT] Huomaa, että keskustella Microsoft Azure SQL-tietokanta on ympäristö nimellä Microsoft Azure-ympäristön palvelun tarjoaminen. Tässä asiakirjassa keskustelu on käynnissä, kun se tunnetaan paikallisten versioiden-Azuren näennäiskoneiden, hyödyntäminen infrastruktuuri kuin Service-ominaisuutta, Azure SQL Server-tuotetta. Tietokannan ominaisuudet ja toiminnot nämä kaksi tarjouksia välillä erot ja olisi ei saa sekoittaa keskenään. Katso myös: <https://azure.microsoft.com/services/sql-database/>
 
On suositeltavaa voit tarkistaa [tämän] [ virtual-machines-sql-server-infrastructure-services] asiakirjat, ennen kuin jatkat.

Seuraavien osien laitteita asiakirjoista yllä olevaa linkkiä ja valitse koostetaan ja mainituista. Yksityiskohtia SAP ympärillä on mainittu sekä ja joidenkin käsitteiden kuvataan tarkemmin. Kuitenkin on erittäin suositeltavaa läpi ohjeista edellä ensimmäisen ennen lukeminen on SQL Serverin ohjeissa.

IaaS tiettyihin tietoihin, sinun tulee tietää ennen jatkamista on joitakin SQL Server:

* **Virtuaalikoneen SLA**: on SLA näennäiskoneiden käynnissä Azure-tietokannassa, johon täällä: <https://azure.microsoft.com/support/legal/sla/>  
* **SQL-versio tukevat**: SAP-asiakkaiden sovellus tukee SQL Server 2008 R2 tai uudempi versio, Microsoft Azure Virtual Machine. Aiemmat versiot eivät ole tuettuja. Lue lisätietoja Tämä yleinen [Tuki-lause](https://support.microsoft.com/kb/956893) . Huomaa, että yleensä SQL Server 2008 tukee Microsoft paikan päällä. Kuitenkin vuoksi merkittäviä toimintoja SAP joka otettiin käyttöön SQL Server 2008 R2, SQL Server 2008 R2 on pienin SAP-versioon. Ota huomioon, SQL Server 2012 ja 2014 käytössä laajennettuna tarkempaa integroida IaaS skenaarion (kuten varmuuskopioiminen suoraan Azuren tallennustilaan vastaan). Tämän vuoksi olemme rajoittaa tässä asiakirjassa SQL Server 2012 ja sen uusimman korjaustiedoston viereiset 2014 Azure.
* **SQL tukemista**: Useimmat SQL Server-ominaisuuksia voi käyttää, Microsoft Azuren näennäiskoneiden poikkeuksin. **SQL Server Vikasietoklusterointia käyttämällä jaettu levyjä ei tueta**.  Jaettu tekniikoita, kuten tietokantapeilausta, AlwaysOn käytettävyys ryhmät, replikoinnin, loki toimitus ja Service Brokeria tuetaan yksittäisen Azure-alueella. SQL Server AlwaysOn myös tuetaan Azure eri alueiden välillä tähän suorittimessa: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Lue lisätietoja [Tuki-lause](https://support.microsoft.com/kb/956893) . Esimerkki siitä, miten voit ottaa AlwaysOn-määritys on mukana [tämän] [ virtual-machines-workload-template-sql-alwayson] artikkelissa. Lisäksi Kuittaa ulos parhaita käytäntöjä kuvata [tähän][virtual-machines-sql-server-infrastructure-services] 
* **SQL-suorituskyky**: olemme varma, että Microsoft Azuren näennäiskoneiden isännöityä suorittaa hyvin verrattuna muihin julkisen cloud virtualization tarjouksia, mutta yksittäisille tuloksille voivat vaihdella. Katso [Tämä] [ virtual-machines-sql-server-performance-best-practices] artikkelissa.
* **Kuvien käyttäminen Azure Marketplacesta**: nopein tapa ottaa käyttöön uuden Microsoft Azure-AM on käytettävä kuva Azure Marketplacesta. Ei Azure Marketplacesta kuvia, jotka sisältävät SQL Server. Kuvat, jossa SQL Server on asennettuna ei voi käyttää SAP NetWeaver sovellusten heti. Syynä on SQL Server Oletuslajittelu on asennettu kuvat ja ei SAP NetWeaver järjestelmien vaatii lajittelua. Jotta voit käyttää esimerkiksi kuvia, Tarkista ohjeiden mukaan luvun [SQL Server-kuvien käyttämisestä ulos Microsoft Azure Marketplacesta] [dbms-opas-5.6]. 
* Tutustu lisätietoja [Hinnat tiedot](https://azure.microsoft.com/pricing/) . [SQL Server 2012 käyttöoikeudet opas](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) ja [SQL Server 2014 käyttöoikeudet opas](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) on myös tärkeä resurssi.
 
### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>SQL Serverin määritysten ohjeet SAP liittyvät Azure VMs SQL Serverissä

#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Suosituksia AM/Näennäiskiintolevyn rakenteen SAP liittyvät SQL Server-ympäristöissä
Yleinen kuvaus mukaisesti SQL Serverin suoritettavat olisi sijaitsevat tai asennettuna AM perus Näennäiskiintolevyn järjestelmän asemaan (levyasemassa c\).  Yleensä useimmat SQL Server-järjestelmän tietokannat ovat ei käyttämä ylätasolla SAP NetWeaver työmäärää. Näin ollen järjestelmän tietokantojen SQL Server (perustyyli, msdb: ja mallin) voivat jäädä C:\-asemaan. Poikkeuksen voi olla tempdb, jonka joitakin SAP-ERP ja kaikki Mustavalkoinen työmäärät voi edellyttää suurempi tietojen äänenvoimakkuutta tai i/o-toimintojen asema, joka ei mahdu alkuperäisen AM. Tällaisia järjestelmiä on suoritettava seuraavat toimet:

* Siirtyy samaan looginen asemaan, jossa ensisijaisen tietojen tiedostot SAP-tietokannan ensisijainen tempdb tietojen tiedostot.
* Lisää muita tempdb tiedostot kaikkien muiden tiedostojen SAP käyttäjän tietokannan tietoja sisältävä loogiset asemat.
* Lisää tempdb lokitiedoston looginen asema, joka sisältää käyttäjätietokannan lokitiedoston.
* Laske solmu tempdb tiedot ja kirjaudu tiedostoja **yksinomaan AM tyypeissä, jotka käyttävät paikallisen SSD** saatetaan D:\ asemaan. Kuitenkin voi olla suositeltavaa käyttää useita tempdb datatiedostot. Ota huomioon D:\ asemat eroavat AM lajin perusteella.
 
Määritysten käyttöön tempdb tarjoaman enemmän tilaa kuin järjestelmäasema ei voi antaa. Jotta ERISNIMI tempdb koon määrittäminen jokin tarkistaa tempdb koot aiemmin järjestelmiin, joka suorittaa-ympäristöön. Lisäksi kokoonpanossa ottaminen käyttöön IOPS luvut tempdb, joka ei anneta, järjestelmä-asema. Uudelleen järjestelmien käynnissä olevat paikallisen voidaan valvoa i/o työmäärää tempdb vastaan niin, että näet oman tempdb odotat IOPS luvut voi saada.

Näyttää AM määritys, joka toimii SQL Server SAP-tietokannan ja tempdb tiedot ja tempdb lokitiedoston sijoitetut D:\ asemassa seuraavasti:
 
![Viittaus Azure IaaS AM SAP: n määrittäminen][dbms-guide-figure-300]

Huomaa, että D:\-asema on riippuvainen AM tyyppi erikokoisia. Määräytyy tempdb koon vaatimus voit ehkä pakko pari tempdb tietojen ja lokitiedostojen ja SAP-tietokantatiedostoja tiedot ja kirjaudu tapauksia, joissa D:\ asema on liian pientä.

#### <a name="formatting-the-vhds"></a>Muotoilun näennäiskiintolevyt
SQL Serverin NTFS estää koon näennäiskiintolevyjen, joka sisältää SQL Serverin tietoihin ja lokitiedostojen on oltava 64 kilotavua. Ei ole tarpeen, jos haluat muotoilla D:\-asema. Asema tulee valmiiksi muotoilluille.

Jotta voit varmistaa, että palauttaminen tai tietokantojen luominen ei valmistellaan datatiedostot mukaan nollaan tiedostojen sisältöä, yksi Varmista, että käyttäjäkontekstissa, SQL Server-palvelu on käynnissä on tietyt käyttöoikeudet. Windowsin järjestelmänvalvojaryhmän käyttäjien on yleensä seuraavat oikeudet. Jos SQL Server-palvelu on muu kuin Windowsin järjestelmänvalvoja käyttäjän kontekstissa, sinun täytyy määrittää, käyttäjälle oikeuden suorittaa aseman ylläpito-tehtävät.  Katso lisätietoja tämän Microsoft Knowledge Base-artikkeli: <https://support.microsoft.com/kb/2574695>
 
#### <a name="impact-of-database-compression"></a>Tietokannan pakkaamisen vaikutus
Määritykset, jossa i/o-kaistanleveyden muuttuvat rajoittava tekijä joka mittayksikön, mikä vähentää IOPS voi olla apua venyttämällä jokin suorittaa IaaS-skenaario, kuten Azure työmäärää. Sen vuoksi, jos ei ole vielä valmis, käyttämällä SQL Server-sivun pakkaaminen on erittäin suositeltavaa SAP-ja Microsoft ennen lataamista aiemmin SAP Azure-tietokannat.
 
Suorittaa tietokannan pakkaamisen ennen lataamista Azure suositus saadaan pois kahdesta syystä:

* Ladattava tietojen määrä on pienempi.
* Pakkaamisen suorittamisen kesto on lyhyempi oletetaan, että jokin käyttää vahvempaa laitteiston Lisää suorittimessa tai suuremman i/o-kaistanleveyden tai vähemmän i/o viive paikalliseen.
* Tietokannan koko on pienempi saattaa johtaa vähemmän levyn kustannukset

Tietokannan pakkaaminen toimii sekä-Azuren näennäiskoneiden, kuin se tekee paikallisen. Lisätietoja siitä, miten voit pakata aiemmin SAP-SQL Serverin tietokantaan Tarkista tässä: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>
  
### <a name="sql-server-2014--storing-database-files-directly-on-azure-blog-storage"></a>SQL Server 2014 – tallentaa tiedostoja suoraan käytössä Azure blogin tallentaminen
SQL Server 2014 avautuu, voit tallentaa tietokannan tiedostoja suoraan Azure Blob-säilö ilman niiden ympärillä Näennäiskiintolevyn "paketti". Erityisesti käyttämällä vakio Azure-tallennustilan tai pienempi AM tyypit näin skenaariot, missä voit korjata IOPS, jotka suoritettaviksi rajoitettu määrä, joka on liitetty joissakin pienemmissä AM tyyppeihin näennäiskiintolevyjen rajoitukset. Tämä koskee käyttäjän tietokantoja ei kuitenkaan järjestelmän tietokantojen SQL Server. Se toimii myös tietojen ja lokitiedostojen SQL Server. Jos haluat ottaa käyttöön SAP SQL Server-tietokantaan näin sijaan 'rivitys' sen näennäiskiintolevyjen, säilytä seuraavat asiat:

* Tallennustilan tilin käytettyjen on sama Azure-alue, kuten jokin, jota käytetään AM SQL Serverin käyttöönotto on käynnissä.
* Huomioon otettavia seikkoja mainittuja terveisin näennäiskiintolevyjen jakaa eri Azure-tallennustilan tilien käyttää tätä tapaa ominaisuuksissa sekä. Merkitsee i/o-toimintojen määrä rajoituksiin Azure-tallennustilan tilin.
[Kommentti]: <>  (MSSedusch TODO mutta tämä käyttää verkon huippua ja ei tallennustilan huippua ei sen?)

Lisätietoja tällaista käyttöönoton on lueteltu täällä: <https://msdn.microsoft.com/library/dn385720.aspx>
 
Jotta voit tallentaa SQL Server data tiedostoja suoraan Azure Premium tallennustilaa, on oltava vähintään SQL Server 2014 korjaustiedoston levittäminen joka on kuvattu seuraavassa: <https://support.microsoft.com/kb/3063054>. SQL Server Azure vakio tallennustilan datatiedostot tallentaminen käsitellä SQL Server 2014 julkaisuversio. Kuitenkin hyvin samat korjaukset sisältää ratkaisuja, jotka tekevät Azure-Blob-säiliö suora käyttö SQL Server-datatiedostot ja varmuuskopioiden luotettava toinen sarja. Tästä syystä suosittelemme käyttämään nämä korjaukset yleinen.

### <a name="sql-server-2014-buffer-pool-extension"></a>SQL Server 2014 puskurin resurssivarantoon tunniste
SQL Server 2014 käyttöön uusi ominaisuus, jota kutsutaan puskurin resurssivarantoon tunniste. Tämä toiminto laajentaa SQL Server, joka on käytettävissä, jota paikallisen SSD palvelimen tai AM varmuuskopioidaan toisen tason välimuistin muistiin puskurin resurssivarantoon. Näin säilyttää suurempi Työsarja tiedot "muistissa". Verrattuna käyttäminen Azure vakio tallennustilan access kyselyjä puskurin altaan ne on tallennettu Azure-AM paikallisen SSD tunniste on monet eri tekijät nopeampaa.  Tämän vuoksi hyödyntäminen D:\ paikalliseen asemaan AM tyypit, jotka on erinomainen IOPS ja siirtonopeuden voi olla hyvin selvää tapaa vähentää IOPS kuormituksen Azuren tallennustilaan vastaan ja kehittää vastauksen kertaa kyselyjen huomattavasti. Tämä koskee erityisesti silloin, kun Käytä Premium-tallennustilan. Premium tallennustilan ja Laske-solmu Premium Azure luku-välimuistin käyttö kuin suositus datatiedostot, suuri eroja ei odoteta. Syy on, sekä tallentaa (SQL Server puskurin resurssivarantoon tunniste ja Premium luku välimuisti) käytät Laske solmut paikallisen levyjä.
Lisätietoja tämän toiminnon, tarkista dokumentaatio: <https://msdn.microsoft.com/library/dn133176.aspx> 

### <a name="backuprecovery-considerations-for-sql-server"></a>Varmuuskopiointi ja palauttaminen Huomioitavaa SQL Server
Kun otat SQL Server Azure varmuuskopioinnin kehitysmenetelmistä on tarkistettava kyselyjä. Vaikka järjestelmä ei ole tehokkaasti järjestelmän, SQL Server ylläpitämä SAP-tietokannan on varmuuskopioida säännöllisesti. Azuren tallennustilaan säilyttää kolme kuvia, ne on nyt vähemmän tärkeitä suhteessa jalostetuille tallennustilan kaatumisen. Prioriteetti syy ylläpito ERISNIMI varmuuskopiointi ja palauttaminen suunnitelma on useita, voit hyvittää looginen/manuaalinen virheet antamalla vaiheessa aika palautus ominaisuuksia. Jotta tavoitteena on joko Käytä varmuuskopioiden Palauta tietokanta takaisin tietyssä vaiheessa ajassa tai määritä toisen järjestelmän kopioimalla olemassa olevan tietokannan Azure varmuuskopioista avulla. Esimerkiksi voit voi siirtää 2 tason SAP-kokoonpanon saman järjestelmän 3 järjestelmän määritykseen palauttamalla varmuuskopion.

Liittyy varmuuskopion SQL Server Azure säilöön kolmella eri tavalla:
 
1. SQL Server 2012 CU4 ja can grafiikkatiedostomuotoja varmuuskopion uudemmissa URL-osoitteeseen. Tämä on yksityiskohtainen blogi [uudet toiminnot SQL Server 2014 – osa 5 – varmuuskopiointi ja palauttaminen parannukset](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Katso luvun [SQL Server 2012 SP1 CU4 ja sitä uudemmissa versioissa] [dbms-opas-5.5.1].
1. SQL Server-versioiden ennen SQL 2012 CU4 käyttää uudelleenohjaus toimintoja Näennäiskiintolevyn varmuuskopiointi ja Siirrä lähinnä kohti Azure tietojen tallennussijainti, joka on määritetty kirjoitus-muodossa. Katso luvun [SQL Server 2012 SP1 CU3 ja aikaisempien versioiden] [dbms-opas-5.5.2].
1. Lopullinen tapa on tavalliseen SQL Server-varmuuskopiointi levylle-komennon Näennäiskiintolevyn levy-laitteeseen.  Tämä on sama kuin paikallisen käyttöönoton kuvio ja käsitellään ei ole tässä asiakirjassa yksityiskohtaiset tiedot.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 ja sitä uudemmissa versioissa
Tämän toiminnon avulla voit suoraan Azure-BLOB-säiliö varmuuskopio. Tämä menetelmä ilman Varmuuskopioi, jotka kuluttavat Näennäiskiintolevyn ja IOPS kapasiteettia Azure muiden näennäiskiintolevyjen. Idea on lähinnä näin:
 
 ![SQL Server 2012: n varmuuskopion muodostaminen Microsoft Azuren tallennustilaan BLOB-OBJEKTIEN käyttäminen][dbms-guide-figure-400]

Tässä tapauksessa etuna on yksi ei tarvitse käyttävät näennäiskiintolevyjen tallentamiseksi SQL Server-varmuuskopiot. Sinulla on vähemmän näennäiskiintolevyjen varattu ja Näennäiskiintolevyn IOPS koko kaistanleveyden voi käyttää tietojen ja lokitiedostojen. Huomaa, että varmuuskopion enimmäiskokoa ei voi sisältää enintään 1 TT tämän artikkelin kohtaan "Rajoitukset" suorittimessa: <https://msdn.microsoft.com/library/dn435916.aspx#limitations>. Jos varmuuskopion koko huolimatta SQL Server varmuuskopion pakkaaminen ylittävät 1 TT: n kokoa, luvun [SQL Server 2012 SP1 CU3 ja aikaisempien versioiden] kuvatut toiminnot [dbms-opas-5.5.2] Tässä asiakirjassa on käytettävä.

[Aiheeseen liittyvät ohjeet](https://msdn.microsoft.com/library/dn449492.aspx) kuvaava tietokantojen vastaan Azure-Blob-säilö varmuuskopioista palauttaminen suosittelee ei, jos haluat palauttaa Azure-BLOB Storesta, jos varmuuskopioista > 25 Gigatavua. Tässä artikkelissa suositus perustuu yksinkertaisesti suorituskykyyn liittyviä tietoja ja ei toimi rajoitusten vuoksi. Tämän vuoksi eri ehdot koske tapaus mukaan välein.

[Tässä](https://msdn.microsoft.com/library/dn466438.aspx) opetusohjelmassa löytyvät siitä, miten tällaista varmuuskopioinnin määrittäminen ja hyödyntää ohjeet
 
Esimerkki vaiheiden järjestyksen voi lukea [tähän](https://msdn.microsoft.com/library/dn435916.aspx).

Automaattinen varmuuskopioiden on suurin merkitys, varmista, että kunkin varmuuskopiointi BLOB-objektit nimetään eri tavalla. Muussa tapauksessa ne korvataan ja palauttaminen ketju on katkennut.
 
Järjestyksessä eivät sekoita määrityksiä varmuuskopioiden 3 erityyppiset välillä on suositeltavaa luoda eri säilöjen tallennustilan-tili, jota käytetään varmuuskopioiden hakeminen alapuolella. Säilöjen voi olla AM mukaan vain tai AM ja varmuuskopiointi tyypin mukaan. Rakenteen saattaa näyttää tältä:
 
 ![Käyttämällä Microsoft Azure-tallennustilan BLOB – SQL Server 2012 varmuuskopio eri kohteet erillisessä tallennustilan-tilissä][dbms-guide-figure-500]

Yllä olevassa esimerkissä varmuuskopioista ei voi suorittaa samaan tallennustilan-tiliin, jossa VMs on otettu käyttöön. Olisi erityisesti varmuuskopioista uuden tallennustilan tilin. Tallennustilan-tilien sisältämien on eri säilöjen luotu matriisin varmuuskopiointi- ja AM nimi tyyppi. Näiden Segmentointi helpompi hallita eri VMs varmuuskopiot.

BLOB-objektit jokin kirjoittaa suoraan varmuuskopiot, ei lisätä AM näennäiskiintolevyjen määrää. Näin ollen jokin voi suurentaa näennäiskiintolevyjen liitetty olevat tietojen AM-versiot, enintään ja tapahtuman lokitiedostoon ja suorita edelleen varmuuskopion vastaan tallennustilan säilö. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 ja aiemmissa versioissa
Ensimmäiseksi sinun on suoritettava saavuttamiseksi varmuuskopion suoraan Azuren tallennustilaan vastaan olisi lataamaan msi, jotka on linkitetty KBA [Tässä](https://www.microsoft.com/download/details.aspx?id=40740) artikkelissa.
 
Lataa x64 asennustiedosto ja ohjeisiin. Tiedosto asentaa ohjelma nimeltä: "Microsoft SQL Server Backup Microsoft Azure-työkalu". Lue ohjeet tuotteen huolellisesti.  Työkalu toimii lähinnä seuraavalla tavalla:

* SQL Server-puolen SQL Server-backup disk sijainti määritetään (Älä käytä D:\-asema tämän).
* Työkalun avulla voit määrittää sääntöjä, joiden avulla voidaan ohjata erilaista eri Azure-tallennustilan säiliöiden varmuuskopiot.
* Kun säännöt ovat paikallaan, työkalu ohjaa varmuuskopion kirjoitus-muodossa näennäiskiintolevyjen/levyjä, joka oli määritetty aiemmin Azure tallennuspaikkaan.
* Työkalu jättää pieni kanta tiedoston muutaman kt koko on Näennäiskiintolevyn tai levy, johon on määritetty SQL Server-varmuuskopion. **Tämän tiedoston olisi jätettävä tallennuspaikkaan, koska se on palauttaa uudelleen Azure-tallennustilan.**
    * Jos olet valinnut vaihtoehdon varmuuskopioida Microsoft Azure-tallennustilan tilin on katkennut kanta-tiedosto (kuten kautta tappio tallennustilan median, joka sisältää kanta-tiedosto), Microsoft Azuren tallennustilaan – kanta-tiedoston voi palauttaminen lataamalla se tallennustilan säilö, johon se on haluamassasi. Paikallisessa tietokoneessa kansioon, johon työkalu on määritetty tunnistaa ja lataa saman säilöön samaan salaaminen salasanalla, jos salaus on käytetty alkuperäisen säännöllä sitten Sijoita kanta-tiedosto. 

Tämä tarkoittaa rakenteen SQL Serverin uudempia versioita yllä olevien ohjeiden mukaisesti voidaan siirtää paikoillaan myös SQL Server-versioista, jotka eivät salli suora osoite Azure-tallennuspaikka.
 
Tämä menetelmä ei voi käyttää uudempaan SQL Serverin kuuluvan, jotka tukevat varmuuskopioiminen määrittäminen grafiikkatiedostomuotoja vastaan Azure-tallennustilan. Poikkeuksena missä rajoitukset alkuperäisen varmuuskopion kyselyjä Azure estävät alkuperäisen varmuuskopioinnin suorittamisen Azure kyselyjä.

#### <a name="other-possibilities-to-backup-sql-server-databases"></a>Muita vaihtoehtoja varmuuskopion SQL Server-tietokantaan
Varmuuskopion tietokantoihin muita vaihtoehtoja on muita näennäiskiintolevyjen liittäminen AM, jonka avulla tallentaa varmuuskopiot. Tällöin sinun on Varmista, että näennäiskiintolevyt käytössäsi ei ole täydellinen. Jos näin on, tarvitset liitetyn Näennäiskiintolevyn ja, joten speak 'arkistoida' sen ja korvaa se uuden tyhjän Näennäiskiintolevyn. Siirtyessäsi alaspäin polun haluat säilyttää nämä näennäiskiintolevyjen erillisessä Azure-tallennustilan tilin niistä, näennäiskiintolevyt tietokanta-tiedostoja.

Toinen vaihtoehto on suuri AM, joissa on liitetty useita näennäiskiintolevyjen. Esimerkiksi D14 32VHDs kanssa. Tallennustilan välilyöntejä avulla voit luoda joustavia ympäristössä, jossa voi luoda jaettuja resursseja, joita käytetään sitten varmuuskopion kohteeksi eri DBMS-palvelimiin.
 
Parhaista käytännöistä käytössä kuvattu [seuraavassa](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) paikan päällä. 

#### <a name="performance-considerations-for-backupsrestores"></a>Suorituskyvyn Huomioitavaa varmuuskopioida ja palauttaa
Paljas käyttöönotoissa, kuten varmuuskopiointi ja palauttaminen suorituskyky on riippuvainen määrät kuinka monta voi lukea rinnakkain ja siirtonopeuden nämä asemat ehkä. Lisäksi varmuuskopion pakkaamisen käyttämä suorittimen kulutus voi toistaa merkittäviä roolin VMs vain enintään 8 suorittimen viestiketjuissa siirtyminen kanssa. Tämän vuoksi jokin oletetaan, voit:

* Mitä vähemmän, tietojen tallentamiseen näennäiskiintolevyjen tiedostoja, mitä pienempää yleinen siirtonopeuden lukutilassa.
* Mitä pienempi suorittimen määrän viestiketjut AM, Lisää vakavia varmuuskopion pakkaamisen vaikutus.
* Vähemmän kohteet (BLOB tai näennäiskiintolevyjen) kirjoittaa varmuuskopion, pienempi siirtonopeuden.
* Mitä pienempää AM kokoa, sitä vähemmän siirtonopeuden tallennuskiintiön kirjoittaminen ja luettaessa Azure-tallennustilan. Onko varmuuskopioista suoraan tallennetut Azure-Blob-objektien tai onko ne on tallennettu, jotka on tallennettu uudelleen Azure-BLOB näennäiskiintolevyjen riippumaton.

Käytettäessä Microsoft Azure Storage BLOB uudempia versioita varmuuskopion kohteena on rajoitettu nimeävät vain yksi kullekin tietyn varmuuskopion URL-osoite kohde.
 
Mutta kun käytät 'Microsoft SQL Server varmuuskopioinnin Microsoft Azure-työkalu' vanhempia versioita, voit määrittää useamman kuin yhden tiedoston kohde. Useamman kuin yhden kohteen varmuuskopioinnin skaalata ja varmuuskopion on suurempi. Seuraa sitten useita tiedostot sekä Azure-tallennustilan tilin. Tutustu testauksessa käyttämällä useita tiedosto-kohteet jokin ehdottomasti toteuttaa siirtonopeuden, joissa yksi voitu saavutetaan varmuuskopion tunniste on otettu käyttöön SQL Server 2012 SP1 CU4 kohteesta. Voit myös ei estä 1 TT rajoitus kuin alkuperäisen varmuuskopioinnin Azure kyselyjä.

Kuitenkin pitää mielessä, siirtonopeuden myös on riippuvainen Azure tallennustilan-tiliä, jota käytät varmuuskopion sijainti. Vertailla voi olla Etsi tallennustilan tili toisella alueella kuin VMs on käynnissä. Esimerkiksi käyttää AM määritykset Länsi Euroopan, mutta valitseminen tallennustilan tili, jonka avulla takaisin ylös vastaan Pohjois-Euroopan. Vaikuttaa varmuuskopion siirtonopeuden varmasti ja ei todennäköisesti Luo siirtonopeuden 150 Mt/s kuin on mahdollista tapauksissa, jossa kohde säilytys- ja VMs käyttävät samaa alueellisen palvelinkeskuksen tuntuu.

#### <a name="managing-backup-blobs"></a>Varmuuskopion BLOB hallinta
Tällä pyyntö Omat varmuuskopioiden hallinta. Oletuksena on monta BLOB-objektit luodaan suorittamalla usein käytetyt tapahtuman Lokin varmuuskopioiden, kyseiset BLOB hallinta voit helposti ylikuormittaa Azure-portaalissa. Tämän vuoksi on recommendable hyödyntää Azure-tallennustilan Explorer. On useita hyvä niistä käytettävissä joka auttaa Azure-tallennustilan tilin hallinta

* Microsoft Visual Studiossa Azure SDK asennettu (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure-tallennustilan Explorer (<https://azure.microsoft.com/downloads/>)
* 3 valmistajien työkaluja

[Kommentti]: <>  (Ei vielä tueta KÄDESSÄ)
[Kommentti]: <>  (### Azure AM varmuuskopiointi)
[Kommentti]: <> SAP-järjestelmässä VMs  (voi varmuuskopioida Azure virtuaalikoneen varmuuskopiointi-toiminnon avulla. Azure virtuaalikoneen varmuuskopion käytössä een etuajassa vuoden 2015 ja on tänä normaaliin tapaan, voit varmuuskopioida valmis AM Azure-tietokannassa. Tallentaa varmuuskopioista Azure ja sallii AM palautuksen uudelleen Azure varmuuskopiointi.) 
[Kommentti]: <> (VMs, suorita tietokantojen voit varmuuskopioida yhdenmukaisesti sekä DBMS järjestelmien tukee Windows VSS (tilannevedospalvelu - <https://msdn.microsoft.com/library/windows/desktop/bb968832.aspx>), kuten esimerkiksi SQL-palvelin ei. Jotta Azure AM varmuuskopioinnista voi olla voi siirtyä SAP-tietokannan restorable varmuuskopio. Muista kuitenkin, Azure AM varmuuskopioiden ajankohta: palauttaa tietokantojen perustuvat ei ole mahdollista. Tämän vuoksi suositus on tietokantojen varmuuskopioiden DBMS toiminnon sen sijaan, että käyttäisit Azure AM varmuuskopiointi.) [Kommentti]: <> (esitellään Azure virtuaalikoneen varmuuskopiointi aloittamalla tähän <https://azure.microsoft.com/documentation/services/backup/>)

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>SQL Server-kuvien käyttämisestä ulos Microsoft Azure Marketplacesta
Microsoft tarjoaa Azure Marketplacesta VMs, joka sisältää jo SQL Server-versiota. SAP-asiakkaille, jotka tarvitsevat käyttöoikeuksien SQL Server-ja Windows-syynä voi olla mahdollisuus kattavat lähinnä pyörivä VMs SQL Server on jo asennettu määrittäminen edellyttää käyttöoikeutta. Voi käyttää esimerkiksi kuvia SAP: n, seuraavat asiat on tehtävä:

* SQL Server-laskenta versiot hankkia kustannuksia kuin vain vain Windows-AM käyttöön Azure Marketplacesta. Lisätietoja näistä artikkeleista Vertaile hintoja: <https://azure.microsoft.com/pricing/details/virtual-machines/> ja <https://azure.microsoft.com/pricing/details/virtual-machines/#Sql>. 
* Voit käyttää vain SQL Server-versioiden, joita tue SAP, kuten SQL Server 2012.
* SQL Server ‑esiintymä, johon on asennettu tarjota Azure Marketplacesta VMs lajittelua ei ole SAP NetWeaver vaatii SQL Server-esiintymän suorittaa lajittelua. Voit muuttaa lajittelua mutta kanssa seuraavan osan ohjeita.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Microsoft Windowsin tai SQL Serverin AM SQL Server-lajittelun muuttaminen
SQL Server Azure Marketplace-kuvat on ei ole määritetty käyttämään lajittelu, joka vaaditaan SAP NetWeaver sovellukset, koska se on vaihdettava välittömästi käyttöönoton jälkeen. SQL Server 2012 tämän voi tehdä seuraavat toimet heti AM on otettu käyttöön ja järjestelmänvalvoja on pysty kirjautumaan käyttöön AM:

* Avaa Windows-komentoikkunassa "järjestelmänvalvojana".
* Vaihda kansio C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* Suorita komento: Setup.exe /QUIET/toiminto = REBUILDDATABASE /INSTANCENAME = MSSQLSERVER /SQLSYSADMINACCOUNTS =`<local_admin_account_name`> /SQLCOLLATION = SQL_Latin1_General_Cp850_BIN2   
    * `<local_admin_account_name`> on tili, jotka on määritelty järjestelmänvalvojatilin käyttöönotto AM valikoiman kautta ensimmäisen kerran.

Olisi vain kestää muutaman minuutin kuluttua. Jotta voit varmistaa, onko vaiheen päättyi oikean tuloksen, toimi seuraavasti:

* Avaa SQL Server Management Studiossa.
* Avaa kyselyikkuna.
* Suorittaa komennon sp_helpsort perusmuodon SQL Server-tietokantaan.

Halutun tuloksen pitäisi näyttää samalta kuin:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Jos tämä ei ole tulos, Pysäytä käyttöönotto SAP ja selvitä, miksi asetukset-komento ei ollut oikein. SQL Server-esiintymän eri kuin sen SQL Server valintanäytöt SAP NetWeaver sovelluksista käyttöönoton mainittu edellä ei **ole** tuettu.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server suuren käytettävyyden SAP Azure varten
Mainittu aiemmassa tässä asiakirjassa ei ole mahdollista, voit luoda jaettua tallennustilan, joita tarvitaan vanhimmasta SQL Serverin suuri käytettävyys-toiminnon käyttö. Tätä toimintoa asentamalla kahden tai useamman SQL Server-esiintymien-Windows Server automaattisesti klusterin (WSFC) käyttäen jaetun levyn käyttäjän tietokannoissa (ja tekstin tempdb). Tämä on kauan vakio suuren käytettävyyden menetelmä, jota myös tukee SAP. Azure tue jaetun, SQL Server suuren käytettävyyden määritykset jaetun levyn klusterin kokoonpanon ei toteutunut. Kuitenkin monia muita suuren käytettävyyden menetelmiä ovat silti ja on kuvattu seuraavissa osissa.

[Kommentti]: <>  (Artikkelissa on edelleen refering ASM avulla)
[Kommentti]: <>  (Ennen lukeminen eri tietyn suuren käytettävyyden tekniikoita käyttökelpoisia SQL Server Azure-tietokannassa, on erittäin hyvä asiakirjan, joka tarjoaa enemmän tietoja ja osoittimet [here][virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions])

#### <a name="sql-server-log-shipping"></a>SQL Server Kirjaudu toimitus
Suuren käytettävyyden (HA) tavoilla on SQL Server Log toimitus. Jos HA-määritysten osallistuminen VMs toimi nimenselvitys, ei ole ongelmia ja Azure-asetukset vaihtelevat ei asetuksista, joka on tehty paikallisen. Ei suositella riippuvaisia vain IP-tarkkuus. Terveisin määrittäminen Log lähetys- ja periaatteiden ympärille Log toimitus Tarkista näitä ohjeita:

<https://technet.microsoft.com/library/ms187103.aspx>
 
Saavuttamiseksi todella minkä tahansa suuren käytettävyyden jokin on ottaa lokiin toimitus kokoonpanossa rajoissa saman Azure-käytettävyys-määrittäminen kuuluvia VMs.

#### <a name="database-mirroring"></a>Tietokantapeilausta
Tietokannan peilaus kuin ylläpitämä SAP (Katso SAP Huomautus [965908]) riippuvainen määrittäminen SAP-yhteysmerkkijonon kumppanin automaattisesti. Paikallisen-tapausten on oletetaan, että kaksi VMs ovat samaa toimialuetta ja että käyttäjää kaksi SQL Server-esiintymää on käynnissä kohdassa toimialueen käyttäjiä sekä ja on riittävät oikeudet yhdistävää kaksi SQL Server-esiintymää. Tämän vuoksi asetusten tietokantapeilauksen Azure poiketa välillä tyypillinen paikallisen asennus ja määritys.

Vain Pilvipalveluita käyttöönotoissa saavuttaman helpoin tapa on olla toisen toimialueen määritystä Azure näiden DBMS VMs (ja ihannetapauksessa erillinen SAP VMs) tai yksi toimialueella.

Toimialue ei ole mahdollista, jos jokin voit myös käyttää varmenteet tietokantapeilaus päätepisteet seuraavassa kuvatulla tavalla: <https://technet.microsoft.com/library/ms191477.aspx>

Opetusohjelma, jotta määritetään Azure tietokantapeilausta löytyy tähän: <https://technet.microsoft.com/library/ms189852.aspx> 

#### <a name="alwayson"></a>AlwaysOn
AlwaysOn tuetaan, SAP paikallisen (Katso SAP Huomautus [1772688]), sitä tuetaan, jota käytetään yhdessä SAP Azure-tietokannassa. Että et ole voivat luoda jaettujen levyjen Azure ei tarkoita, että jokin ei voi luoda AlwaysOn Windows Server automaattisesti klusterin (WSFC)-määritysten eri VMs välillä. Se vain tarkoittaa, että sinulla ei ole mahdollisuutta käyttää jaettua DVD-levyllä asetettu vaatimus klusterin määrityksessä. Näin ollen voit luoda AlwaysOn WSFC-määritysten Azure-tietokannassa ja yksinkertaisesti Valitse koontityyppiä, joka hyödyntää jaetun levyn. Azure-ympäristön näiden VMs on otettu käyttöön tulee Ratkaise VMs nimelläsi ja VMs on oltava samaa toimialuetta. Tämä pätee vain Azure ja paikallisen-asennuksia. On seikat ympärille käyttöönotto SQL Server käytettävyys ryhmän Listener (ei sekoittaa Azure käytettävyys määrittäminen) jälkeen Azure tässä vaiheessa kohdassa kerralla ei salli AD/DNS-objektin luominen yksinkertaisesti, sillä se on mahdollista paikallisen. Jotkin eri asennusohjeita on tarpeen ratkaista Azure tietyn toimintaa.

Seikkoja, käyttämällä käytettävyys-ryhmän Listener ovat seuraavat:

* Käytettävyys-ryhmän Listener käyttämällä on mahdollista vain Windows Server 2012: ssa tai Windows Server 2012 R2 AM vieraana OS. Sinun on Windows Server 2012: Varmista, että tämä korjaus on määritetty: <https://support.microsoft.com/kb/2854082> 
* Windows Server 2008 R2, tämä korjaus ei ole ja AlwaysOn on käytettävä samalla tavalla kuin tietokantapeilausta määrittämällä automaattisesti kumppanin yhteydet merkkijonon (valmis SAP default.pfl parametrin hidastustekniikkaa/mss /-palvelimen kautta – Katso SAP Huomautus [965908]).
* Käytettävyys-ryhmän Listener käytettäessä tietokannan VMs on yhdistettävä erillinen kuormituksen. Vain Pilvipalveluita käyttöönotoissa nimenselvitys edellyttäisi joko kaikki VMs SAP-järjestelmän (sovelluksen-palvelimet-DBMS palvelimen ja () SCS) ovat samassa virtual verkossa tai edellyttäisi from SAP-sovelluskerroksen etc\host tiedoston säilyttäminen jotta saat SQL Server-VMs ratkennut AM nimet. Siten vältät Azure on määrittäminen uuden IP-osoitteiden tapauksissa, jossa molemmat VMs myös on suljettu, yksi Määritä staattinen IP-osoitteet, verkkoliittymät VMs AlwaysOn määrityksessä (määrittäminen staattinen IP-osoite on kuvattu [tämän] [ virtual-networks-reserved-private-ip] artikkelissa) [Kommentti]: <> (vanha blogit) [Kommentti]: <> (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx> <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* On erityisen lisävaiheet, kun rakentaminen WSFC klusterimääritys-kohtaa, johon klusterin on erityisen IP-osoite on määritetty, koska Azure sen nykyiset toiminnot ja määrittää klusterinimeä sama IP-osoite solmu klusterin luodaan. Tämä tarkoittaa manuaalinen vaihe on suoritettava liittäminen klusterin eri IP-osoite.
* Käytettävyys ryhmän kuuntelua suorittaminen voidaan luoda TCP/IP päätepisteet, jotka on määritelty VMs käynnissä käytettävyys ryhmän ensisijainen ja toissijainen replikat Azure-tietokannassa.
* Ongelma saattaa olla tarpeen suojaamiseen nämä käyttöoikeusluettelot päätepisteet.

[Kommentti]: <>  (TODO vanha blogi)
[Kommentti]: <>  (Yksityiskohtaisia ohjeita ja AlwaysOn-määritysten Azure-asentamisessa niin vaatiessa parhaiten listan, kun ominaisuussäilöjen – sovelluksen käytettävissä [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[Kommentti]: <>  (Valmiiksi AlwaysOn asennuksen kautta Azure valikoiman < https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[Kommentti]: <>  (Käytettävyys-ryhmän Listener luomisesta on kuvattu parhaiten [this][virtual-machines-windows-classic-ps-sql-int-listener] opetusohjelma)
[Kommentti]: <>  (Käyttöoikeusluettelot Securing verkon päätepisteet selvennetään parhaiten tähän:)
[Kommentti]: <>  (* < https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[Kommentti]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>)
[Kommentti]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[Kommentti]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Se on mahdollista käyttöön SQL Server AlwaysOn käytettävyys ryhmän eri sekä alueiden Azure kautta. Tämä toiminto hyödyntää Azure VNet Vnet yhteydet ([Lisätietoja][virtual-networks-configure-vnet-to-vnet-connection]).
[Kommentti]: <> (TODO vanha blogiin) [Kommentti]: <> (esimerkiksi tilanne SQL Server AlwaysOn käytettävyys-ryhmien asetukset on kuvattu seuraavassa: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>SQL Server Azure suuri käytettävyys yhteenveto
Koska Azuren tallennustilaan suojaa sisältöä, on vähintään yksi syy ennakkomaksu kuuman valmius-kuva. Tämä tarkoittaa suuri käytettävyys-skenaario on suojautua vain seuraavissa tapauksissa:

* Indeksoitava AM kokonaisuutena palvelinklusterin Azure tai muista syistä ylläpidon vuoksi
* SQL Server-esiintymän ohjelmiston ongelmat
* Manuaalinen virhe, jossa tietoja saa poistaa ja ajankohta palautus tarvita suojaaminen

Katsoo vastaavia technologies jokin voit argue tietokantapeilausta tai AlwaysOn, voidaan kattaa kaksi ensimmäistä tapauksissa olisi kolmannen kirjainkokoa voidaan kattaa vain Log toimitus.

Sinun on AlwaysOn, verrattuna AlwaysOn edut tietokantapeilausta, monimutkaisempaa määritystä saldo. Kuten voivat näkyä näiden etuja:

*   Toissijainen replikoiden luettavissa.
*   Toissijainen replikoiden-varmuuskopiot.
*   Parempi skaalattavuus.
*   Enemmän kuin yhden toissijaisen replikoita.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>SAP-Azure yhteenveto Yleiset SQL Server
On monia suosituksia tässä oppaassa ja lue se useammin kuin kerran ennen Azure käyttöönoton suunnitteleminen. Yleensä kuitenkin muista noudattaa kymmenen Yleiset DBMS Azure tiettyjä kohtia:

[Kommentti]: <>  (2.3 suurempi kuin mitä siirtonopeuden? Kuin yksi Näennäiskiintolevyn?)
1. Käytä uusin DBMS-versio, kuten SQL Server 2014, joka on useimpien etuja Azure-tietokannassa. SQL Server on SQL Server 2012 SP1 CU4, jossa näkyy tällöin varmuuskopioiminen työpöytäversioihin Azuren tallennustilaan ominaisuus. Kuitenkin SAP yhdessä Suosittelemme vähintään SQL Server 2014 SP1 CU1 tai SQL Server 2012 SP2 ja uusimmat Leikkaa.
1. Suunnitella huolellisesti Azure saldo tietojen tiedoston asettelu ja Azure rajoitukset SAP järjestelmän vaaka:
    * Ei ole liian monta näennäiskiintolevyjen, mutta on riittävästi, jotta pääset tarvittavat IOPS.
    * Muista, että IOPS on myös rajattu Azure-tallennustilan tilin kohden ja tallennustilan tilit on rajoitettu kunkin Azure-tilauksen piiriin kuuluvien ([Lisätietoja][azure-subscription-service-limits]). 
    * Vain raita näennäiskiintolevyjen, jos haluat saavuttaa korkeampi siirtonopeuden kautta.
1. Älä koskaan Asenna ohjelmisto tai valitseminen tiedostoja, jotka edellyttävät pysyvyys D:\ asemassa ei pysyvä on ja mitä tahansa asemassa menetetään Windows uudelleenkäynnistyksen yhteydessä.
1. Älä käytä Azure Näennäiskiintolevyn välimuistiasetukset vakio Azure-tallennustilan.
1. Älä käytä Azure geo replikoida tallennustilan tilit.  Käytä paikallisesti tarpeettomat DBMS toiminnoista.
1. DBMS toimittajan HA/DR ratkaisun avulla voit toistaa tietokannan tiedot.
1. Aina käyttää nimenselvitys, Älä luota siihen IP-osoitteet.
1. Käytä mahdollista ylin tietokannan pakkausta. SQL Server tällä sivulla pakkaus.
1. Varmista, ettet SQL Server-kuvien käyttämisestä Azure Marketplacesta. Jos käytät jotakin SQL Server, sinun on muutettava ennen kuin asennat sen SAP NetWeaver järjestelmän esiintymä-lajittelua.
1. Asenna ja määritä SAP Host seurantaa varten Azure kuvatulla tavalla käyttöönoton opas [käyttöönotto-oppaan].

## <a name="specifics-to-sap-ase-on-windows"></a>Yksityiskohtia SAP Ietokannan Windows
Microsoft Azure alkaen voit helposti siirtää SAP Ietokannan sovellukset, Azuren näennäiskoneiden. SAP Ietokannan Virtual Machine-avulla voit pienentää valinnanvapaus helposti Microsoft Azure näiden sovellusten käyttöönoton, hallinta ja ylläpito yrityksen kehitysyhteisön sovellusten omistajuuden kokonaiskustannukset. Kanssa Ietokannan SAP Azure Virtual Machine järjestelmänvalvojille ja kehittäjille edelleen käyttää samaa kehittäminen ja hallintatyökalut, jotka ovat käytettävissä paikallisen.

Ei SLA varten Azuren näennäiskoneiden joka täällä: <https://azure.microsoft.com/support/legal/sla>

Olemme varma, että Microsoft Azuren näennäiskoneiden isännöityä suorittaa hyvin verrattuna muihin julkisen cloud virtualization tarjouksia, mutta yksittäisille tuloksille voivat vaihdella. SAP: in koon SAP numerot eri SAP sertifioitu AM tuotteissa erillisen huomautuksen SAP- [1928533]toimitetaan.

Lausekkeet ja suositukset osalta käyttö Azure-tallennustilan, SAP VMs käyttöönoton tai SAP seuranta koskevat SAP-sovellusten yhteydessä Ietokannan SAP-versioiden ilmoitetulla neljä ensimmäistä luvut tämän asiakirjan koko.

### <a name="sap-ase-version-support"></a>SAP: in Ietokannan versio tuki 
SAP tällä hetkellä SAP Ietokannan SAP Business Suite tuotteiden kanssa käytettäväksi 16,0 versio tukee. Kaikki päivitykset SAP Ietokannan palvelimen tai JDBC ja ODBC-ohjaimet, jota käytetään SAP Business Suite tuotteet ovat käytettävissä ainoastaan SAP palvelun Marketplace: <https://support.sap.com/swdc>.

Asennusten paikallisen, kuten Älä lataa päivitykset SAP Ietokannan palvelimen tai JDBC ja ODBC-ohjaimet Sybase sivustoista. Lisätietoja korjaukset, jotka ovat tuettuja SAP Business Suite tuotteissa paikallisesti ja Azuren näennäiskoneiden on seuraavassa SAP-muistiinpanoja:

* [1590719]
* [1973241]
 
Yleistietoja SAP Business Suite käytössä SAP Ietokannan löytyvät [SCN](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>SAP Ietokannan määritysten ohjeet SAP liittyvät SAP Ietokannan asennusten Azure VMs:

#### <a name="structure-of-the-sap-ase-deployment"></a>SAP-Ietokannan käyttöönoton rakenne
Yleinen kuvaus mukaisesti SAP Ietokannan suoritettavat olisi sijaitsevat tai asennettuna AM perus Näennäiskiintolevyn järjestelmän asemaan (levyasemassa c\). Yleensä useimmat SAP Ietokannan järjestelmän ja työkalut tietokannat ovat ei todella hyödyntää kiintolevyn SAP NetWeaver työmäärää. Näin ollen järjestelmän ja työkalut-tietokannat (perustyyli, malli, saptools, sybmgmtdb, sybsystemdb) voivat jäädä C:\drive paikan päällä. 

Poikkeuksen voi olla väliaikainen tietokanta, joka sisältää kaikki työ taulukot ja SAP Ietokannan, jossa joitakin SAP-ERP-ja kaikki Mustavalkoinen työmäärät voi edellyttää suurempi tietojen äänenvoimakkuutta tai i/o-toimintojen asema, joka ei mahdu alkuperäisen AM perus Näennäiskiintolevyn luoma väliaikaisia taulukoita (levyasemassa c\).
 
Tietokanta saattaa sisältää SAPInst/SWPM asennuksessa järjestelmän version mukaan:

* Yksittäisen SAP Ietokannan tempdb, joka on luotu SAP Ietokannan asennettaessa
* SAP Ietokannan ja muita saptempdb, joka on luotu SAP-asennustoimintoa luoma Ietokannan SAP-tempdb
* SAP-Ietokannan-tempdb, luoma SAP Ietokannan ja muita tempdb, joka on luotu manuaalisesti (esimerkiksi seuraavat SAP Huomautus [1752266]) ERP/Mustavalkoinen tempdb tiettyjen ehtojen mukaan

Tietyn ERP tai kaikki Mustavalkoinen työmäärät se on järkevää, osalta suorituskyvyn lisäksi luotu tempdb (SWPM tai manuaalisesti) tempdb-laitteiden pitämiseksi C: asemaan\. jos se ei ole muita tempdb on suositeltavaa, luo sellainen (SAP Huomautus [1752266]).

Tällaisia järjestelmiä lisäksi luotu tempdb olisi voidaan suorittaa seuraavasti:

* Siirtää ensimmäisen tempdb laitteen SAP-tietokannan ensimmäinen laite
* Lisää jokaiseen sisältävä laitteiden SAP-tietokannan näennäiskiintolevyjen tempdb laitteita

Tässä määrityksessä mahdollistaa tempdb tarjoaman sellaisina kuin järjestelmäasema ei voi antaa enemmän tilaa. Viitteenä jokin tarkistaa tempdb laitteen koot aiemmin järjestelmiin, joka suorittaa-ympäristöön. Tai kokoonpanossa avulla voisi IOPS luvut tempdb, joka ei anneta, järjestelmä-asema. Uudelleen järjestelmien käynnissä olevat paikallisen voidaan valvoa i/o työmäärää tempdb vastaan.

Sijoita koskaan SAP Ietokannan laitteita AM D:\-asemassa. Tämä koskee myös tempdb, vaikka tempdb, mitä objekteja vain tilapäinen.

#### <a name="impact-of-database-compression"></a>Tietokannan pakkaamisen vaikutus
Määritykset, jossa i/o-kaistanleveyden muuttuvat rajoittava tekijä joka mittayksikön, mikä vähentää IOPS voi olla apua venyttämällä jokin suorittaa IaaS-skenaario, kuten Azure työmäärää. Tämän vuoksi on suositeltavaa, varmista, että SAP Ietokannan pakkaamisen käytetään ennen lataamista SAP-tietokannan Azure.

Suositus suorittamiseen pakkaamisen ennen lataamista Azure, jos se ei ole jo käytössä saadaan pois useita syitä:

* Ladattava Azure tietojen määrä on pienempi
* Pakkaamisen suorittamisen kesto on lyhyempi oletetaan, että jokin käyttää vahvempaa laitteiston Lisää suorittimessa tai suuremman i/o-kaistanleveyden tai vähemmän i/o viive paikalliseen
* Tietokannan koko on pienempi saattaa johtaa vähemmän levyn kustannukset

Tietoja ja LOB pakkaus toimi AM, isännöidään Azuren näennäiskoneiden kuin se tekee paikallisen. Lisätietoja siitä, miten voit tarkistaa, jos pakkaus on jo käytössä SAP Ietokannan-tietokannassa Tarkista SAP Huomautus [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>DBACockpit avulla voit valvoa tietokannan esiintymiä
SAP-järjestelmää, joita käytät SAP Ietokannan kuin tietokannan käyttöympäristön DBACockpit on käytettävissä tapahtuman DBACockpit upotetun selainikkunat tai Webdynpro. Kuitenkin täysi toiminnallisuus seuranta ja hallinta tietokanta on käytettävissä vain DBACockpit Webdynpro käyttöönoton.

Kuin paikallisen, jossa useita vaiheita tarvitaan kaikki SAP NetWeaver-toiminto DBACockpit Webdynpro käyttöönoton käyttämä otetaan käyttöön. Noudata SAP Huomautus [1245200] käyttöön webdynpros käyttö ja luo tarvittavat niistä. Kun noudattamalla edellä muistiinpanojen määrität myös Internet Communication Manageria (icm) sekä http- ja https-yhteyksien käytettävät portit. Http oletusarvo on seuraavanlainen:

> ICM/server_port_0 = PROT = HTTP-PORTIN = 8000, PROCTIMEOUT = 600, AIKAKATKAISU = 600
>
> ICM/server_port_1 = PROT = HTTPS-PORTTIIN 443$ $PROCTIMEOUT = = 600, AIKAKATKAISU = 600

ja luoda tapahtuman linkit DBACockpit näyttää tällainen:

> https://`<fullyqualifiedhostname`>: 44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/sap/bc/webdynpro/sap/dba_cockpit

Riippuen Jos ja miten Azure virtuaalikoneen SAP-järjestelmää isännöivän on yhdistetty kautta sivusto sivusto, usean sivuston tai ExpressRoute (rajat paikallisen ympäristön) haluat varmistaa, että ICM käyttää täydellinen isäntänimi, jotka ovat tietokoneeseen Jos yrität avata DBACockpit. Katso selvittääksesi, miten ICM määrittää sen mukaan, profiilin parametrien täydellinen isäntänimi SAP Huomautus [773830] ja määritä parametrin icm/host_name_full erikseen, tarvittaessa.

AM vain Pilvipalveluita tilanne ilman paikallisen ja Azure väliset yhteydet paikallisen-käyttöön, jos haluat määrittää julkiseen IP-osoite ja domainlabel. Julkinen DNS-nimi AM muoto sitten näyttää tältä:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Lisätietoja DNS-nimi löytyy [tähän][virtual-machines-azurerm-versus-azuresm].

SAP profiilin parametri-icm/host_name_full asettaminen Azure AM linkin DNS-nimeä voi näyttää seuraavanlaiselta:

> https://mydomainlabel.westeurope.cloudapp.NET:44300/sap/bc/webdynpro/sap/dba_cockpit

> http://mydomainlabel.westeurope.cloudapp.NET:8000/sap/bc/webdynpro/sap/dba_cockpit

Tässä tapauksessa on Varmista, että:

* Saapuvan liikenteen säännöt lisääminen verkko-käyttöoikeusryhmän TCP/IP-portit, joita käytetään pitää yhteyttä ICM Azure-portaalissa
* Lisää saapuvan liikenteen säännöt työnkulkuun Windowsin palomuuri ICM yhteydessä käytettävä TCP/IP-portit

Automaattinen tuoda kaikki käytettävissä olevat korjausten on suositeltavaa sovelletaan säännöllisesti korjaus sivustokokoelman SAP Huomautus koskevat SAP-versio:

* [1558958]
* [1619967]
* [1882376]

Lisätietoja DBA Cockpit for SAP Ietokannan löytyy huomautukset-SAP:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Varmuuskopiointi ja palauttaminen Huomioitavaa SAP Irjainkoko
Kun otat SAP Ietokannan Azure varmuuskopioinnin kehitysmenetelmistä on tarkistettava kyselyjä. Vaikka järjestelmä ei ole tehokkaasti järjestelmän, ylläpitää SAP Ietokannan SAP-tietokanta on varmuuskopioida säännöllisesti. Azuren tallennustilaan säilyttää kolme kuvia, ne on nyt vähemmän tärkeitä suhteessa jalostetuille tallennustilan kaatumisen. Ensisijainen syy ylläpito ERISNIMI varmuuskopiointi ja palauttaminen-palvelupaketti on enemmän, voit hyvittää looginen/manuaalinen virheet antamalla vaiheessa aika palautus ominaisuuksia. Jotta tavoitteena on joko Käytä varmuuskopioiden Palauta tietokanta takaisin tietyssä vaiheessa ajassa tai määritä toisen järjestelmän kopioimalla olemassa olevan tietokannan Azure varmuuskopioista avulla. Esimerkiksi voit voi siirtää 2 tason SAP-kokoonpanon saman järjestelmän 3 järjestelmän määritykseen palauttamalla varmuuskopion.

Varmuuskopiointia ja palauttamista Azure-tietokantaan toimii samalla tavalla kuin se tekee paikallisen. SAP-muistiinpanot ovat artikkeleissa:

* [1588316]
* [1585981]

Lisätietoja luominen dump määrityksiä ja ajoituksen varmuuskopiot. Strategia ja voit määrittää tarpeiden mukaan tietokannan ja kirjaudu kirjoittaa levyn aiemmin näennäiskiintolevyjen sivulle tai Lisää muita Näennäiskiintolevyn, varmuuskopion.  Voit pienentää tietojen menettämisen keneen aiheuttaa kannattaa käyttää Näennäiskiintolevyn, jossa ei ole tietokantatiedosto sijaitsee.

Pakkaamisen SAP Ietokannan on tietoja ja LOB varmuuskopion pakkaus. Säästämiseksi ja tietokanta ja kirjaa Vedostaa suositellaan varmuuskopion pakata. Katso lisätietoja SAP Huomautus [1588316] . Pakkaaminen varmuuskopioinnin on myös tärkeitä tietoja voi siirtää, jos aiot ladata varmuuskopioiden tai näennäiskiintolevyjen sisältävä varmuuskopion Vedostaa Azure virtuaalikoneen paikalliseen vähentää.

Älä käytä asema D:\ tietokannan tai loki dump kohde.

#### <a name="performance-considerations-for-backupsrestores"></a>Suorituskyvyn Huomioitavaa varmuuskopioida ja palauttaa
Paljas käyttöönotoissa, kuten varmuuskopiointi ja palauttaminen suorituskyky on riippuvainen määrät kuinka monta voi lukea rinnakkain ja siirtonopeuden nämä asemat ehkä. Lisäksi varmuuskopion pakkaamisen käyttämä suorittimen kulutus voi toistaa merkittäviä roolin VMs vain enintään 8 suorittimen viestiketjuissa siirtyminen kanssa. Tämän vuoksi jokin oletetaan, voit:

* Vähemmän näennäiskiintolevyjen voidaan tallentaa tietokannan-laitteet, sitä vähemmän yleinen siirtonopeuden lukutilassa
* Mitä pienempi suorittimen määrän viestiketjut AM, Lisää vakavia varmuuskopion pakkaamisen vaikutus
* Kirjoittaa varmuuskopion, pienempi siirtonopeuden vähemmän kohteet (raita kansioita, näennäiskiintolevyjen)

Voit laajentaa kohdemäärä kirjoittaminen on kaksi asetukset joita voidaan käyttää/yhdistetyn tarpeidesi:

* Kohteen varmuuskopioinnin äänenvoimakkuuden striping päälle useita liitetyn näennäiskiintolevyjen IOPS siirtonopeuden mustat asemassa parantamiseksi
* Dump kokoonpanon luomisen SAP Ietokannan tasolla, joka käyttää useita kohdekansio vedostaminen voit kirjoittaa

Striping aseman päälle useita liitetyn näennäiskiintolevyjen on käsitelty aiemmassa tässä oppaassa. Lisätietoja käyttämällä useita kansioita SAP Ietokannan dump määrityksessä tutustumaan tallennettu toimintosarja sp_config_dump, jolla voidaan luoda dump määritykset [Sybase Infocenter](http://infocenter.sybase.com/help/index.jsp)ohjeet.

### <a name="disaster-recovery-with-azure-vms"></a>Azure VMs ja palauttaminen

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Tietojen replikoinnin SAP Sybase replikoinnin palvelimen kanssa
Mahdollistaa SAP Sybase replikoinnin Server (SRS) SAP-Ietokannan lämpimän valmius-ratkaisun siirtäminen tietokannan tapahtumia asynkronisesti kaukana sijaintiin. 

Asennus- ja SRS toiminto toimii myös toiminnallisesti AM, ylläpidettävä Azure virtuaalikoneen palveluita kuin se tekee paikallisen.

SAP-replikoinnin palvelimen kautta Ietokannan HADR on suunniteltu uuteen versioon kanssa. Se testattu ja julkaistu Microsoft Azure ympäristöjen heti, kun se on käytettävissä.

## <a name="specifics-to-sap-ase-on-linux"></a>Yksityiskohtia SAP Ietokannan Linux

Microsoft Azure alkaen voit helposti siirtää SAP Ietokannan sovellukset, Azuren näennäiskoneiden. SAP Ietokannan Virtual Machine-avulla voit pienentää valinnanvapaus helposti Microsoft Azure näiden sovellusten käyttöönoton, hallinta ja ylläpito yrityksen kehitysyhteisön sovellusten omistajuuden kokonaiskustannukset. Kanssa Ietokannan SAP Azure Virtual Machine järjestelmänvalvojille ja kehittäjille edelleen käyttää samaa kehittäminen ja hallintatyökalut, jotka ovat käytettävissä paikallisen.

Käyttöönoton Azure VMs on tärkeää tietää virallinen palvelutasosopimuksia joka täällä: <https://azure.microsoft.com/support/legal/sla>

SAP koonmuuttokahvan tiedot ja SAP sertifioitu AM tuotteissa luettelo toimitetaan SAP Huomautus [1928533]. Lisää SAP Azure Virtual koneet tiedostojen koon Täältä löytyvät <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> ja tähän <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Lausekkeet ja suositukset osalta käyttö Azure-tallennustilan, SAP VMs käyttöönoton tai SAP seuranta koskevat SAP-sovellusten yhteydessä Ietokannan SAP-versioiden ilmoitetulla neljä ensimmäistä luvut tämän asiakirjan koko.

Seuraavat kaksi SAP huomautukset sisältävät yleisiä tietoja Ietokannan Linux ja Ietokannan pilvipalvelussa:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>SAP: in Ietokannan versio tuki 
SAP tällä hetkellä SAP Ietokannan SAP Business Suite tuotteiden kanssa käytettäväksi 16,0 versio tukee. Kaikki päivitykset SAP Ietokannan palvelimen tai JDBC ja ODBC-ohjaimet, jota käytetään SAP Business Suite tuotteet ovat käytettävissä ainoastaan SAP palvelun Marketplace: <https://support.sap.com/swdc>.

Asennusten paikallisen, kuten Älä lataa päivitykset SAP Ietokannan palvelimen tai JDBC ja ODBC-ohjaimet Sybase sivustoista. Lisätietoja korjaukset, jotka ovat tuettuja SAP Business Suite tuotteissa paikallisesti ja Azuren näennäiskoneiden on seuraavassa SAP-muistiinpanoja:

* [1590719]
* [1973241]
 
Yleistietoja SAP Business Suite käytössä SAP Ietokannan löytyvät [SCN](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>SAP Ietokannan määritysten ohjeet SAP liittyvät SAP Ietokannan asennusten Azure VMs:

#### <a name="structure-of-the-sap-ase-deployment"></a>SAP-Ietokannan käyttöönoton rakenne
Yleinen kuvaus mukaisesti SAP Ietokannan suoritettavat olisi sijaitsevat tai asentaa AM (/sybase) pääkansion tiedoston järjestelmään. Yleensä useimmat SAP Ietokannan järjestelmän ja työkalut tietokannat ovat ei todella hyödyntää kiintolevyn SAP NetWeaver työmäärää. Näin ollen järjestelmän ja työkalut-tietokannat (perustyyli, malli, saptools, sybmgmtdb, sybsystemdb) voivat jäädä pääkansion järjestelmässä sekä. 

Poikkeuksen voi olla väliaikainen tietokanta, joka sisältää kaikki työ taulukot ja SAP Ietokannan, jossa joitakin SAP-ERP-ja kaikki Mustavalkoinen työmäärät voi edellyttää suurempi tietojen äänenvoimakkuutta tai i/o-toimintojen asema, joka ei mahdu alkuperäisen AM OS levyn luoma väliaikaisia taulukoita.
 
Tietokanta saattaa sisältää SAPInst/SWPM asennuksessa järjestelmän version mukaan:

* Yksittäisen SAP Ietokannan tempdb, joka on luotu SAP Ietokannan asennettaessa
* SAP Ietokannan ja muita saptempdb, joka on luotu SAP-asennustoimintoa luoma Ietokannan SAP-tempdb
* SAP-Ietokannan-tempdb, luoma SAP Ietokannan ja muita tempdb, joka on luotu manuaalisesti (esimerkiksi seuraavat SAP Huomautus [1752266]) ERP/Mustavalkoinen tempdb tiettyjen ehtojen mukaan

Tietyn ERP tai kaikki Mustavalkoinen työmäärät se on järkevää, suorituskyvyn lisäksi luotu tempdb (SWPM tai manuaalisesti) tempdb-laitteiden pitämiseksi erillisenä tiedostona järjestelmään, jossa voidaan esittää yhden Azure tietojen DVD-levyllä tai Linux-RAID, jonka kesto on useita Azure tietojen levyjä osalta. Jos se ei ole muita tempdb kannattaa luoda (SAP Huomautus [1752266]).

Tällaisia järjestelmiä lisäksi luotu tempdb olisi voidaan suorittaa seuraavasti:

* Siirtää ensimmäisen tempdb hakemiston ensimmäisen tiedostojärjestelmässä SAP-tietokannan
* Tempdb kansioiden lisääminen kunkin näennäiskiintolevyjen sisältävä SAP-tietokannan tiedostojärjestelmässä

Tässä määrityksessä mahdollistaa tempdb tarjoaman sellaisina kuin järjestelmäasema ei voi antaa enemmän tilaa. Viitteenä jokin tarkistaa aiemmin järjestelmiin, jotka toimivat paikallisen tempdb kansion koon. Tai kokoonpanossa avulla voisi IOPS luvut tempdb, joka ei anneta, järjestelmä-asema. Uudelleen järjestelmien käynnissä olevat paikallisen voidaan valvoa i/o työmäärää tempdb vastaan.

Sijoita koskaan SAP Ietokannan kansioiden /mnt tai AM /mnt/resource päälle. Tämä koskee myös tempdb, vaikka tempdb, mitä objekteja ovat vain väliaikaisia, koska /mnt tai /mnt/resource on oletusarvoinen Azure AM temp tilaa, joka ei ole jatkuva. [Tässä artikkelissa] löytyy lisätietoja Azure AM temp-tila[virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Tietokannan pakkaamisen vaikutus
Määritykset, jossa i/o-kaistanleveyden muuttuvat rajoittava tekijä joka mittayksikön, mikä vähentää IOPS voi olla apua venyttämällä jokin suorittaa IaaS-skenaario, kuten Azure työmäärää. Tämän vuoksi on suositeltavaa, varmista, että SAP Ietokannan pakkaamisen käytetään ennen lataamista SAP-tietokannan Azure.

Suositus suorittamiseen pakkaamisen ennen lataamista Azure, jos se ei ole jo käytössä saadaan pois useita syitä:

* Ladattava Azure tietojen määrä on pienempi
* Pakkaamisen suorittamisen kesto on lyhyempi oletetaan, että jokin käyttää vahvempaa laitteiston Lisää suorittimessa tai suuremman i/o-kaistanleveyden tai vähemmän i/o viive paikalliseen
* Tietokannan koko on pienempi saattaa johtaa vähemmän levyn kustannukset

Tietoja ja LOB pakkaus toimi AM, isännöidään Azuren näennäiskoneiden kuin se tekee paikallisen. Lisätietoja siitä, miten voit tarkistaa, jos pakkaus on jo käytössä SAP Ietokannan-tietokannassa Tarkista SAP Huomautus [1750510]. Katso lisätietoja tietokannan pakkaamisen SAP Huomautus [2121797] myös.

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>DBACockpit avulla voit valvoa tietokannan esiintymiä
SAP-järjestelmää, joita käytät SAP Ietokannan kuin tietokannan käyttöympäristön DBACockpit on käytettävissä tapahtuman DBACockpit upotetun selainikkunat tai Webdynpro. Kuitenkin täysi toiminnallisuus seuranta ja hallinta tietokanta on käytettävissä vain DBACockpit Webdynpro käyttöönoton.

Kuin paikallisen, jossa useita vaiheita tarvitaan kaikki SAP NetWeaver-toiminto DBACockpit Webdynpro käyttöönoton käyttämä otetaan käyttöön. Noudata SAP Huomautus [1245200] käyttöön webdynpros käyttö ja luo tarvittavat niistä. Kun noudattamalla edellä muistiinpanojen määrität myös Internet Communication Manageria (icm) sekä http- ja https-yhteyksien käytettävät portit. Http oletusarvo on seuraavanlainen:

> ICM/server_port_0 = PROT = HTTP-PORTIN = 8000, PROCTIMEOUT = 600, AIKAKATKAISU = 600
>
> ICM/server_port_1 = PROT = HTTPS-PORTTIIN 443$ $PROCTIMEOUT = = 600, AIKAKATKAISU = 600

ja luoda tapahtuman linkit DBACockpit näyttää tällainen:

> https://`<fullyqualifiedhostname`>: 44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/sap/bc/webdynpro/sap/dba_cockpit

Riippuen Jos ja miten Azure virtuaalikoneen SAP-järjestelmää isännöivän on yhdistetty kautta sivusto sivusto, usean sivuston tai ExpressRoute (rajat paikallisen ympäristön) haluat varmistaa, että ICM käyttää täydellinen isäntänimi, jotka ovat tietokoneeseen Jos yrität avata DBACockpit. Katso selvittääksesi, miten ICM määrittää sen mukaan, profiilin parametrien täydellinen isäntänimi SAP Huomautus [773830] ja määritä parametrin icm/host_name_full erikseen, tarvittaessa.

AM vain Pilvipalveluita tilanne ilman paikallisen ja Azure väliset yhteydet paikallisen-käyttöön, jos haluat määrittää julkiseen IP-osoite ja domainlabel. Julkinen DNS-nimi AM muoto sitten näyttää tältä:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Lisätietoja DNS-nimi löytyy [tähän][virtual-machines-azurerm-versus-azuresm].

SAP profiilin parametri-icm/host_name_full asettaminen Azure AM linkin DNS-nimeä voi näyttää seuraavanlaiselta:

> https://mydomainlabel.westeurope.cloudapp.NET:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.NET:8000/sap/bc/webdynpro/sap/dba_cockpit

Tässä tapauksessa on Varmista, että:

* Saapuvan liikenteen säännöt lisääminen verkko-käyttöoikeusryhmän TCP/IP-portit, joita käytetään pitää yhteyttä ICM Azure-portaalissa
* Lisää saapuvan liikenteen säännöt työnkulkuun Windowsin palomuuri ICM yhteydessä käytettävä TCP/IP-portit

Automaattinen tuoda kaikki käytettävissä olevat korjausten on suositeltavaa sovelletaan säännöllisesti korjaus sivustokokoelman SAP Huomautus koskevat SAP-versio:

* [1558958]
* [1619967]
* [1882376]

Lisätietoja DBA Cockpit for SAP Ietokannan löytyy huomautukset-SAP:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Varmuuskopiointi ja palauttaminen Huomioitavaa SAP Irjainkoko
Kun otat SAP Ietokannan Azure varmuuskopioinnin kehitysmenetelmistä on tarkistettava kyselyjä. Vaikka järjestelmä ei ole tehokkaasti järjestelmän, ylläpitää SAP Ietokannan SAP-tietokanta on varmuuskopioida säännöllisesti. Azuren tallennustilaan säilyttää kolme kuvia, ne on nyt vähemmän tärkeitä suhteessa jalostetuille tallennustilan kaatumisen. Ensisijainen syy ylläpito ERISNIMI varmuuskopiointi ja palauttaminen-palvelupaketti on enemmän, voit hyvittää looginen/manuaalinen virheet antamalla vaiheessa aika palautus ominaisuuksia. Jotta tavoitteena on joko Käytä varmuuskopioiden Palauta tietokanta takaisin tietyssä vaiheessa ajassa tai määritä toisen järjestelmän kopioimalla olemassa olevan tietokannan Azure varmuuskopioista avulla. Esimerkiksi voit voi siirtää 2 tason SAP-kokoonpanon saman järjestelmän 3 järjestelmän määritykseen palauttamalla varmuuskopion.

Varmuuskopiointia ja palauttamista Azure-tietokantaan toimii samalla tavalla kuin se tekee paikallisen. SAP-muistiinpanot ovat artikkeleissa:

* [1588316]
* [1585981]

Lisätietoja luominen dump määrityksiä ja ajoituksen varmuuskopiot. Strategia ja voit määrittää tarpeiden mukaan tietokannan ja kirjaudu kirjoittaa levyn aiemmin näennäiskiintolevyjen sivulle tai Lisää muita Näennäiskiintolevyn, varmuuskopion.  Voit pienentää tietojen menettämisen keneen aiheuttaa kannattaa käyttää Näennäiskiintolevyn, jossa ei ole tietokannan-kansio ja tiedosto sijaitsee.

Pakkaamisen SAP Ietokannan on tietoja ja LOB varmuuskopion pakkaus. Säästämiseksi ja tietokanta ja kirjaa Vedostaa suositellaan varmuuskopion pakata. Katso lisätietoja SAP Huomautus [1588316] . Pakkaaminen varmuuskopioinnin on myös tärkeitä tietoja voi siirtää, jos aiot ladata varmuuskopioiden tai näennäiskiintolevyjen sisältävä varmuuskopion Vedostaa Azure virtuaalikoneen paikalliseen vähentää.

Käytä tietokannan tai loki dump kohde Azure AM temp tilaa /mnt tai /mnt/resource.

#### <a name="performance-considerations-for-backupsrestores"></a>Suorituskyvyn Huomioitavaa varmuuskopioida ja palauttaa
Paljas käyttöönotoissa, kuten varmuuskopiointi ja palauttaminen suorituskyky on riippuvainen määrät kuinka monta voi lukea rinnakkain ja siirtonopeuden nämä asemat ehkä. Lisäksi varmuuskopion pakkaamisen käyttämä suorittimen kulutus voi toistaa merkittäviä roolin VMs vain enintään 8 suorittimen viestiketjuissa siirtyminen kanssa. Tämän vuoksi jokin oletetaan, voit:

* Vähemmän näennäiskiintolevyjen voidaan tallentaa tietokannan-laitteet, sitä vähemmän yleinen siirtonopeuden lukutilassa
* Mitä pienempi suorittimen määrän viestiketjut AM, Lisää vakavia varmuuskopion pakkaamisen vaikutus
* Kirjoittaa varmuuskopion, pienempi siirtonopeuden vähemmän kohteet (Linux ohjelmisto RAID, näennäiskiintolevyjen)

Voit laajentaa kohdemäärä kirjoittaminen on kaksi asetukset joita voidaan käyttää/yhdistetyn tarpeidesi:

* Kohteen varmuuskopioinnin äänenvoimakkuuden striping päälle useita liitetyn näennäiskiintolevyjen IOPS siirtonopeuden mustat asemassa parantamiseksi
* Dump kokoonpanon luomisen SAP Ietokannan tasolla, joka käyttää useita kohdekansio vedostaminen voit kirjoittaa

Striping aseman päälle useita liitetyn näennäiskiintolevyjen on käsitelty aiemmassa tässä oppaassa. Lisätietoja käyttämällä useita kansioita SAP Ietokannan dump määrityksessä tutustumaan tallennettu toimintosarja sp_config_dump, jolla voidaan luoda dump määritykset [Sybase Infocenter](http://infocenter.sybase.com/help/index.jsp)ohjeet.

### <a name="disaster-recovery-with-azure-vms"></a>Azure VMs ja palauttaminen

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Tietojen replikoinnin SAP Sybase replikoinnin palvelimen kanssa
Mahdollistaa SAP Sybase replikoinnin Server (SRS) SAP-Ietokannan lämpimän valmius-ratkaisun siirtäminen tietokannan tapahtumia asynkronisesti kaukana sijaintiin. 

Asennus- ja SRS toiminto toimii myös toiminnallisesti AM, ylläpidettävä Azure virtuaalikoneen palveluita kuin se tekee paikallisen.

Ietokannan HADR SAP replikoinnin palvelimen kautta ei tueta tässä vaiheessa kohdassa kerralla. Se saattaa testattu ja julkaistu Microsoft Azure ympäristöjen tulevaisuudessa.

## <a name="specifics-to-oracle-database-on-windows"></a>Oracle-tietokantaan Windows yksityiskohtia
Oracle-ohjelmiston tukee midyear 2013, koska Oracle toimimaan Microsoft Windows Hyper-V ja Azure. Lue tämän artikkelin tarkempia Yleiset tuki Windows Hyper-V ja Azure Oracle: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

SAP-sovellusten hyödyntäminen Oracle-tietokantojen tietyn skenaarion tuetaan myös seuraavat yleiset tuki. Tiedot ovat nimeltään tämän asiakirjan osaan.

### <a name="oracle-version-support"></a>Oracle-Versiotuki
Oracle-versiot ja vastaavan OS versiot, jotka ovat tuettuja SAP-käyttöjärjestelmässä Oracle-Azuren näennäiskoneiden kaikki tiedot löytyvät seuraavat SAP Huomautus [2039619]

Yleisiä tietoja SAP Business Suite käytössä Oracle löytyy SCN: <https://scn.sap.com/community/oracle>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Oracle määritysten ohjeet Azure VMs SAP-asennukset

#### <a name="storage-configuration"></a>Tallennustilan kokoonpanon
Vain yhden esiintymän käyttämällä NTFS muotoiltu levyjen Oracle tuetaan. Kaikki tietokantatiedostot on tallennettu NTFS tiedostojärjestelmässä Näennäiskiintolevyn levyjen perusteella. Nämä näennäiskiintolevyjen ovat käytettävissä Azure AM ja perustuvat sivun Azure-BLOB-OBJEKTIEN tallennustilaan (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Kaikenlaista verkkoasemat tai jaettuja Azure tiedostopalvelut, kuten:
 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>
 
**ei** tueta Oracle-tietokantatiedostot!

Käytä Azure näennäiskiintolevyjen sivun Azure-BLOB-säiliö perusteella, lausunnot tässä asiakirjassa luvun [välimuisti VMs ja näennäiskiintolevyjen] [dbms-opas-2.1] ja [Microsoft Azuren tallennustilaan] [dbms-opas-2.3] käyttää ominaisuuksissa Oracle-tietokantaan.

Aiemmassa Yleiset asiakirjan osaan kuvatulla IOPS siirtonopeuden varten Azure näennäiskiintolevyjen kiintiöiden olemassa. Tarkka kiintiön riippuen AM käytetään. AM tyypit ja niiden tavoitteet luettelo löytyy [tähän][virtual-machines-sizes]

Laji Azure AM Tuetut tiedostotyypit, lue SAP Huomautus [1928533]

Kun täyttävät nykyisen IOPS kiintiön levyä kohti, on voi tallentaa kaikki yhden yksittäisen liitetyn Azure-Näennäiskiintolevyn DB tiedostot. 

Jos Lisää IOPS tarvitaan, on erittäin suositeltavaa käyttää ikkunan tallennustilan jakavat (vain käytettävissä Windows Server 2012: ssa tai uudempi versio) tai Windowsin raitasarjoittamista Windows 2008 R2: n avulla voit luoda useita liitetyn Näennäiskiintolevyn levyjä yhtä suuri loogisen laitteen. Katso myös tämä asiakirja ohjelmiston RAID [dbms-opas-2.2] luvun. Tämän menetelmän yksinkertaistaa hallinnan katseltavan hallittavan levytilaa ja voit jakaa tiedostoja manuaalisesti useita liitetyn näennäiskiintolevyjen vältetään.

#### <a name="backup--restore"></a>Voit varmuuskopioida / palauttaminen
Varmuuskopion / palauttaa toiminnot, SAP-BR * Tools for Oracle tuetaan samalla tavalla kuin Vakio Windows Server-käyttöjärjestelmät ja Hyper-V. Oracle palautus hallinta (RMAN) tukee myös levylle ja palauttaa levyltä varmuuskopiot.

#### <a name="high-availability"></a>Suuri käytettävyys
[Kommentti]: <>  (linkki viittaa ASM)
Oracle tietojen suojaa tuetaan suuren käytettävyyden ja tietojen palauttamista varten. Tiedot löytyvät [tämän] [ virtual-machines-windows-classic-configure-oracle-data-guard] ohjeissa.

#### <a name="other"></a>Muut
Kaikki muut yleisohjeet kuten Azure käytettävyys joukot tai SAP seurantaa koskevat kuvatulla tavalla kolme ensimmäistä luvut tämän asiakirjan versioiden VMs Oracle-tietokantaan.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>Tekniset tiedot Windows SAP MaxDB-tietokantaan

### <a name="sap-maxdb-version-support"></a>SAP MaxDB versio tuki
SAP tällä hetkellä SAP MaxDB Azure-tuotteiden SAP NetWeaver käytettäväksi 7.9 versio tukee. SAP-MaxDB palvelimen tai JDBC ja ODBC-ohjaimet, jota käytetään SAP NetWeaver perustuvien tuotteiden kaikki päivitykset ovat käytettävissä ainoastaan SAP-palvelun Marketplace <https://support.sap.com/swdc>.
Yleistietoja SAP NetWeaver käytössä SAP MaxDB tukikäytännöistä <https://scn.sap.com/community/maxdb>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Tuettu Microsoft Windows-versioiden ja Azure AM SAP MaxDB DBMS
Tuettu Microsoft Windows-versio löytämisestä SAP MaxDB DBMS-Azure-kohdassa:

* [SAP: N tuotteen saatavuus matriisi (PAM)][sap-pam]
* SAP: in Huomautus [1928533]

On erittäin suositeltavaa käyttää Microsoft Windowsin, joka on Microsoft Windows 2012 R2-käyttöjärjestelmän uusin versio.

### <a name="available-sap-maxdb-documentation"></a>Käytettävissä olevat SAP MaxDB dokumentaatio
Voit tarkistaa päivitetyt luettelo SAP MaxDB dokumentaatio seuraavat SAP Huomautus [767598]
    
### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP MaxDB määritysten ohjeet Azure VMs SAP-asennukset

#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Tallennustilan kokoonpanon
Azure-tallennustilan parhaat käytännöt SAP MaxDB noudata luvun RDBMS käyttöönoton rakenteen mainitusta Yleiset suosituksia [dbms-opas-2].

> [AZURE.IMPORTANT] Muut tietokannat, kuten SAP MaxDB on myös tietojen ja lokitiedostojen. SAP-MaxDB termejä oikean termin on kuitenkin "aseman" (ei "tiedosto"). Esimerkiksi ovat SAP MaxDB tietomääriä ja log asemat. Älä sekoita ne OS asemat. 

Lyhyessä on.

* Määritä Azure-tallennustilan tilin, joka sisältää SAP MaxDB tiedot ja kirjaudu asemat (eli tiedostot) **Paikallisen tarpeettomat Storage (LRS)** kuin määritetty luku [Microsoft Azuren tallennustilaan] [dbms-opas-2.3].
* SAP-MaxDB tietomääriä (eli tiedostot) IO-polkua erottaa IO polun log asemat (eli tiedostot). Tämä tarkoittaa, että SAP MaxDB tietomääriä (eli tiedostot) on asennettava looginen asemassa ja SAP MaxDB lokin asemat (eli tiedostot) sinulla on asennettava loogiseksi.
* Määritä oikea tiedoston tallentamisen välimuistiin kunkin Azure-blob-sen mukaan, käytätkö se SAP MaxDB tietojen tai loki levyasemien (eli tiedostot) ja Azure vakio tai Azure Premium tallennustilaa, käytätkö kuvatulla tavalla luvun [Tiedostovälimuistin VMs] [dbms-opas-2.1].
* Kun täyttävät nykyisen IOPS kiintiön levyä kohti, on mahdollista tallentaa kaikki tietomääriä yhden liitetyn Azure näennäiskiintolevyllä ja tallentaa myös kaikki tietokannan log asemat toiseen yhden liitetyn Azure-Näennäiskiintolevyn.
* Jos Lisää IOPS ja/tai tilaa tarvitaan, on erittäin suositeltavaa käyttää Microsoft ikkunan tallennustilan jakavat (vain käytettävissä Microsoft Windows Server 2012: ssa tai uudempi versio) tai Microsoft Windows raitasarjoittamista Microsoft Windows 2008 R2: n avulla voit luoda useita liitetyn Näennäiskiintolevyn levyjä yhtä suuret looginen laite. Katso myös tämä asiakirja ohjelmiston RAID [dbms-opas-2.2] luvun. Tämän menetelmän yksinkertaistaa hallinnan katseltavan hallittavan levytilaa ja vältetään työmäärään ja tiedostojen jakaminen manuaalisesti useita liitetyn näennäiskiintolevyjen yli.
* Suurin IOPS vaatimukset voit käyttää Azure Premium tallennusta, joka on saatavana DS-sarjan ja GS sarjan VMs.

![Azure IaaS-AM SAP MaxDB DBMS viittaus määrittäminen][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Varmuuskopiointi ja palauttaminen
Otettaessa SAP MaxDB kyselyjä Azure varmuuskopioinnin kehitysmenetelmistä on tarkistettava. Vaikka järjestelmä ei ole tehokkaasti järjestelmän, ylläpitää SAP MaxDB SAP-tietokanta on varmuuskopioida säännöllisesti. Azuren tallennustilaan säilyttää kolme kuvia, varmuuskopion on nyt vähemmän tärkeitä kannalta suojaaminen järjestelmän vastaan tallennustilan järjestelmävirheiden ja muita tärkeämpiä toiminnallisia tai järjestelmänvalvojan virheet. Ensisijainen syy ylläpito ERISNIMI varmuuskopiointi ja palauttaminen sopimus on niin, että voit hyvittää looginen tai Manuaalinen virheet ajankohta palautus ominaisuudet. Jotta tavoitteena on käyttää varmuuskopioiden Palauta tietokanta tietyssä vaiheessa ajassa tai määritä toisen järjestelmän kopioimalla olemassa olevan tietokannan Azure varmuuskopioista avulla. Esimerkiksi voit voi siirtää 2 tason SAP-kokoonpanon saman järjestelmän 3 järjestelmän määritykseen palauttamalla varmuuskopion.

Varmuuskopiointia ja palauttamista Azure-tietokantaan toimii samalla tavalla kuin se tekee paikallisen Systemsin, jotta voit käyttää vakio SAP MaxDB varmuuskopiointi ja palauttaminen Työkalut, jotka on kuvattu yksi SAP Huomautus [767598]luetellut SAP MaxDB dokumentaatio asiakirjoista. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Suorituskyvyn Huomioitavaa varmuuskopiointi ja palauttaminen
Paljas käyttöönotoissa, kuten varmuuskopiointi ja palauttaminen suorituskyky on riippumatta siitä, kuinka monta asemat voi lukea rinnakkain ja siirtonopeuden nämä asemat. Lisäksi varmuuskopion pakkaamisen käyttämä suorittimen kulutus toistettavissa merkittäviä roolin VMs enintään 8 suorittimen viestiketjuissa siirtyminen kanssa. Tämän vuoksi jokin oletetaan, voit:

* Mitä vähemmän näennäiskiintolevyjen voidaan tallentaa tietokannan laitteiden alaosassa yleinen luku siirtonopeuden.
* Mitä pienempi suorittimen määrän viestiketjut AM, Lisää vakavia varmuuskopion pakkaamisen vaikutus
* Vähemmän kohteet (raita kansioita, näennäiskiintolevyjen) kirjoittaa varmuuskopion, alaosassa siirtonopeuden

Kirjoittaminen kohdemäärä parantamiseksi on kaksi asetusta, voit käyttää, mahdollisesti yhdessä tarpeidesi:

* Osoitetaan erilliset asemat varmuuskopiointi
* Kohteen varmuuskopioinnin äänenvoimakkuuden striping useita liitetyn näennäiskiintolevyjen päälle IOPS siirtonopeuden mustat levyn asemassa parantamiseksi
* On erillinen oma loogisen levyn laitteet:
    * SAP MaxDB varmuuskopion asemat (eli tiedostot)
    * SAP MaxDB tietomääriä (eli tiedostot)
    * SAP MaxDB lokin asemat (eli tiedostot)

Striping aseman päälle useita liitetyn näennäiskiintolevyjen on käsitelty aiemmin luvussa ohjelmiston RAID [dbms-opas-2.2] tämän asiakirjan. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Muut
Kaikki muut yleisohjeet kuten Azure käytettävyys joukot tai SAP seurantaa koskevat myös kuvatulla tavalla kolme ensimmäistä luvut tämän asiakirjan versioiden VMs SAP MaxDB tietokannassa.
Muut SAP MaxDB kielikohtaiset asetukset eivät näy Azure VMs, jotka on kuvattu eri asiakirjoja, jotka on lueteltu SAP Huomautus [767598] ja SAP: N julkaisutiedot:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>SAP-liveCache Windows yksityiskohtia

### <a name="sap-livecache-version-support"></a>SAP-liveCache Versiotuki
Pieni versio SAP liveCache tuettu Azuren näennäiskoneiden on **SAP LC/LCAPPS 10.0 SP 25** **liveCache 7.9.08.31** sekä **LCA muodosta 25**, julkaistu **EhP** 2 SAP palvelujen ohjauksen hallinta 7.0 tai uudempi versio.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Tuettu Microsoft Windows-versioiden ja Azure AM SAP liveCache DBMS
Tuettu Microsoft Windows-versio löytämisestä Azure-liveCache SAP-kohdassa:

* [SAP: N tuotteen saatavuus matriisi (PAM)][sap-pam]
* SAP: in Huomautus [1928533]

On erittäin suositeltavaa käyttää Microsoft Windowsin, joka on Microsoft Windows 2012 R2-käyttöjärjestelmän uusin versio. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache kokoonpano Azure VMs SAP-asennukset

#### <a name="recommended-azure-vm-types"></a>Suositellut Azure AM tyypit
SAP liveCache on sovellus, joka suorittaa laskutoimituksia erittäin suuri, summa ja nopeuden RAM-Muistia ja suorittimen on suuri vaikutus SAP liveCache suorituskykyä. 

Azure AM sisältötyypeille tukemat SAP (SAP Huomautus [1928533]) kaikki virtual suorittimen resurssit varataan AM varmuuskopioidaan hypervisor erillinen fyysinen suorittimen resursseja mukaan. Ei ole overprovisioning (ja näin ollen ei ole kilpailua suorittimen resurssien) tapahtuu.

Tyypeissä Azure AM esiintymän tukemat SAP-AM muisti on vastaavasti 100 %: n yhdistetty muistin – overprovisioning (liiallinen sitoumus), esimerkiksi ei käytetä.

Tämä näkökulmasta on erittäin suositeltavaa käyttää uusi D-sarjan tai DS-sarjan (yhdessä Azure Premium tallennustilan kanssa) Azure AM tyyppi, hänellä on 60 % nopeampaa suoritinta kuin A-sarjan. Suurin RAM-Muistia ja suorittimen kuormituksen voit käyttää G-sarjan ja GS-sarjan (yhdessä Azure Premium tallennustilan kanssa) VMs uusimman Intel Xeon® suoritin E5-v3, perheen kanssa on kaksi kertaa muistin ja neljä kertaa tasainen tilan asema tallennustilaa (SSDs), D, DS-sarjan.

#### <a name="storage-configuration"></a>Tallennustilan kokoonpanon
Kun SAP liveCache perustuu SAP MaxDB tekniikka, Azure tallennustilan parhaat suosittelee mainitusta SAP MaxDB luvun [tallennustilan kokoonpanon] [dbms-opas-8.4.1] koskevat myös SAP liveCache. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Erillinen Azure-AM liveCache
Kun SAP liveCache käyttää eri kekovaihtoehtoja laskennallinen power, tehokkaasti käytön on erittäin suositeltavaa ottaa käyttöön erillinen Azure Virtual-koneen. 
 
![Varattu Azure AM tehokkaasti Käyttötapaus liveCache varten][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Varmuuskopiointi ja palauttaminen
Varmuuskopiointi ja palauttaminen, mukaan lukien suorituskykyyn liittyviä tietoja, ovat jo kuvattu asiaa SAP MaxDB luvut [varmuuskopiointi ja palauttaminen] [dbms-opas-8.4.2] ja [suorituskyvyn huomioon otettavia seikkoja varmuuskopiointi ja palauttaminen] [dbms-opas-8.4.3]. 

#### <a name="other"></a>Muut
Kaikki muut yleisohjeet on jo artikkelissa SAP-MaxDB [Tämä] [dbms-opas-8.4.4] luvussa. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>SAP: N sisällön palvelimen Windows yksityiskohtia
SAP-sisällön palvelimen on erillinen, palvelinpohjaiset osa tallentaa sisältöä, kuten sähköisten asiakirjojen muotoilua. SAP-sisällön palvelimen kehittäminen tekniikka tarjoaa ja on käytetty rajat-sovelluksen SAP-sovelluksia varten. Se on asennettu eri järjestelmään. Tyypillinen sisältö on koulutusmateriaali ja Knowledge varasto tai teknisiä piirustuksia mySAP PLM tiedostojen hallintajärjestelmän peräisin. 

### <a name="sap-content-server-version-support"></a>SAP: N sisällön palvelinversion tuki
SAP: in tukee tällä hetkellä:

* **SAP: N sisällön palvelimen** versio **6.50 (tai uudempi versio)**
* **SAP-MaxDB versio 7.9**
* **Microsoft IIS (Internet Information Server) version 8.0 (tai uudempi versio)**

On erittäin suositeltavaa käyttää SAP sisällön palvelin, joka kirjoittamisen tämän asiakirjan milloin **6.50 SP4**on uusin versio ja **Microsoft IIS 8,5**uusin versio. 

Valitse viimeisimmät tuetut versiot SAP Content Server ja Microsoft IIS- [SAP tuotteen käytettävyys matriisin (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Tuetut SAP sisällön palvelimen Microsoft Windowsin ja Azure AM tyypit
Tuetut Windows-version SAP sisällön Server Azure--artikkelissa:

* [SAP: N tuotteen saatavuus matriisi (PAM)][sap-pam]
* SAP: in Huomautus [1928533]

On erittäin suositeltavaa käyttää Microsoft Windowsin, joka aikaa kirjoittamisen tässä asiakirjassa on **Windows Server 2012 R2: n**uusin versio.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP Content Server Configuration ohjeet Azure VMs SAP-asennukset

#### <a name="storage-configuration"></a>Tallennustilan kokoonpanon 
Jos määrität SAP sisällön, johon haluat tallentaa tiedostot SAP MaxDB-tietokantaan, kaikki Azure tallennustilan parhaat käytännöt suositus mainitusta SAP MaxDB luvun tallennustilan kokoonpanon [dbms-opas-8.4.1] ovat myös voimassa SAP Content Server-skenaario. 

Jos määrität SAP sisällön, johon haluat tallentaa tiedostoja tiedostojärjestelmän, on suositeltavaa käyttää erillinen looginen asema. Tallennustilan välilyöntejä avulla voit tehostaa myös loogisen levyn koon ja siirtonopeuden IOPS kuvatulla tavalla luvun ohjelmiston RAID [dbms-opas-2.2]. 

#### <a name="sap-content-server-location"></a>SAP: N sisällön palvelimen sijainti
SAP: N sisällön palvelimessa on sama Azure alue-ja Azure VNET, jossa SAP-järjestelmää on otettu käyttöön. Voit vapaasti päätä, haluatko ottaa käyttöön sisällön SAP-palvelimen osat erillinen Azure-AM tai saman AM, jossa on käynnissä SAP-järjestelmää. 
 
![Erillinen Azure AM sisällön SAP-palvelin][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>SAP: in välimuistin palvelimen sijainti
SAP välimuistia ei palvelinpohjaiset lisäkomponentin antamaan paikallisesti (välimuistissa) tiedostojen käyttäminen. SAP-välimuistin palvelimen välimuistiin SAP sisällön palvelimeen asiakirjoja. Näin voit optimoida verkkoliikennettä, jos asiakirjat on noudetaan useammin kuin kerran eri paikoista. Yleinen sääntö on, että SAP-välimuistin palvelimen on oltava fyysisesti lähellä asiakkaan, joka käyttää SAP-välityspalvelin. 

Seuraavassa on kaksi vaihtoehtoa:

1. **Asiakas on SAP Taustajärjestelmä** Jos SAP Taustajärjestelmä on määritetty sisällön SAP-palvelinta, SAP järjestelmässä on asiakas. Kun SAP-järjestelmää ja SAP Content Server on otettu käyttöön sama Azure alueen – samaan Azure joten – ne ovat fyysisesti lähellä toisiinsa. Tämän vuoksi ei ole tarpeen on erillinen SAP-välityspalvelin. SAP-Käyttöliittymän asiakkaat (SAP Graafisen tai Internet-selaimen) käyttää SAP-järjestelmää suoraan ja SAP-järjestelmää hakee asiakirjojen sisällön SAP-palvelimesta.
1. **Asiakas on paikallisen web-selaimessa** SAP-sisällön palvelimen voi määrittää käyttää suoraan web-selaimessa. Tässä tapauksessa käynnissä ympäristöön – selain on SAP Content Server client. Paikallisen palvelinkeskuksen ja Azure palvelinkeskuksen sijoitetaan eri toimipisteitä (Ihannetapauksessa lähellä toisistaan). Paikallisen palvelinkeskuksen on yhdistetty Azure Azure sivusto sivusto VPN- tai ExpressRoute kautta. Vaikka molemmat vaihtoehdot tarjoaminen suojatun VPN-verkkoyhteyden Azure-sivusto sivusto verkkoyhteys ei tarjoa verkon kaistanleveys ja viive SLA paikallisen palvelinkeskuksen ja Azure palvelinkeskuksen välillä. Voit nopeuttaa access asiakirjoihin, tekemällä jokin seuraavista:
    1. Asenna paikallisesta välimuistista SAP-palvelimesta on, sulje paikallisen web-selaimessa ([this][dbms-guide-900-sap-cache-server-on-premises] kuva-vaihtoehto)
    1. Määritä Azure ExpressRoute, joka tarjoaa nopean ja pieni viive verkon paikallisen palvelinkeskuksen ja Azure palvelinkeskuksen välinen yhteys.
 
![SAP-välimuistin palvelimen paikallisen asennustoimintoa][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Voit varmuuskopioida / palauttaminen
Jos määrität SAP Content Server SAP MaxDB tietokannan tiedostojen tallentamiseen, varmuuskopiointi ja palauttaminen toimintosarja ja suorituskyvyn seikat ovat jo kuvattu SAP MaxDB luvun [varmuuskopiointi ja palauttaminen] [dbms-opas-8.4.2] ja [suorituskyvyn Huomioitavaa varmuuskopiointi ja palauttaminen] luvun [dbms-opas-8.4.3]. 

Jos määrität SAP Content Server tiedostojärjestelmän tiedostojen tallentamiseen, yksi vaihtoehto on suorittamaan manuaalinen varmuuskopiointi ja palauttaminen koko tiedoston rakenteen jossa tiedostot sijaitsevat. Samalla SAP MaxDB varmuuskopiointi ja palauttaminen, on suositeltavaa on erillinen levyaseman varmuuskopion tarkoitusta varten. 

#### <a name="other"></a>Muut
Muiden SAP sisällön palvelimen tietyt asetukset eivät näy Azure VMs ja on kuvattu eri asiakirjoja ja SAP-muistiinpanoja:

* <https://Service.SAP.com/contentserver> 
* SAP: in Huomautus [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>Tekniset tiedot, IBM DB2 Windows LUW varten
Microsoft Azure voit siirtää helposti aiemmin SAP-sovellusta käynnissä IBM DB2-Azuren näennäiskoneiden Linux, UNIX ja Windows (LUW). SAP-IBM DB2 LUW varten, jossa järjestelmänvalvojille ja kehittäjille edelleen käyttää samaa kehittäminen ja hallintatyökalut, jotka ovat käytettävissä paikallisen.
Yleisiä tietoja SAP Business Suite käytössä IBM DB2 varten LUW löytyy-SAP yhteisön verkon (SCN) osoitteessa <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Saat lisätietoja ja SAP-Azure LUW DB2-päivitykset-SAP Huomautus [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux, UNIX ja Windows-Versiotuki
SAP: in for Microsoft Azure virtuaalikoneen Services LUW IBM DB2-tuetaan DB2-versio 10.5 vuodesta.

Lisätietoja tuetuista SAP-tuotteet ja Azure AM tyypit tutustumaan SAP Huomautus [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX ja Windows-määritysten ohjeet Azure VMs SAP-asennukset

#### <a name="storage-configuration"></a>Tallennustilan kokoonpanon
Kaikki tietokantatiedostot on tallennettu NTFS tiedostojärjestelmässä Näennäiskiintolevyn levyjen perusteella. Nämä näennäiskiintolevyjen ovat käytettävissä Azure AM ja perustuvat sivun Azure-BLOB-OBJEKTIEN tallennustilaan (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Kaikenlaista verkkoasemat tai jaettuja esimerkiksi seuraavat Azure tiedostopalvelut ovat **ei** tueta tietokantatiedostoja: 

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>
 
Jos käytössäsi on Azure näennäiskiintolevyjen sivun Azure-BLOB-säiliö perusteella, tehdä tässä asiakirjassa luvun RDBMS käyttöönoton rakenteen [dbms-opas-2] lauseet koskevat myös ominaisuuksissa IBM DB2 LUW tietokannan kanssa. 

Aiemmassa Yleiset asiakirjan osaan kuvatulla IOPS siirtonopeuden varten Azure näennäiskiintolevyjen kiintiöiden olemassa. Tarkka kiintiön määräytyvät käytettävän AM tyyppi. AM tyypit ja niiden tavoitteet luettelo löytyy [tähän][virtual-machines-sizes].

Kun nykyinen IOPS kiintiön levyä kohden riittää, on mahdollista tallentaa kaikki tietokantatiedostot yhden yksittäisen liitetyn Azure-Näennäiskiintolevyn. 

Suorituskyvyn huomioon otettavia seikkoja viitata myös SAP asennuksen apuviivat luvun "Tietojen turvallisuus ja suorituskyvyn huomioon otettavia seikkoja, tietokannan kansioita".

Vaihtoehtoisesti voit Windows tallennustilan jakavat (vain käytettävissä Windows Server 2012: ssa tai uudempi versio) tai Windows-raitasarjoittamista Windows 2008 R2 luvun ohjelmiston RAID [dbms-opas-2.2] tämän artikkelin ohjeiden avulla voit luoda useita liitetyn Näennäiskiintolevyn levyjä yhtä suuri looginen laite.
Sisältävät sapdata ja saptmp kansioiden DB2 tallennustilan polut levyjen on määritettävä 512 kt fyysinen levy-sektorikokoa. Käytettäessä Windows tallennustilan jakavat, sinun on luotava manuaalisesti rivin komentoliittymä parametrilla kautta tallennustilan-jakavat "-LogicalSectorSizeDefault". Lisätietoja <https://technet.microsoft.com/library/hh848689.aspx>.

#### <a name="backuprestore"></a>Varmuuskopiointi ja palauttaminen
IBM DB2 LUW, varmuuskopiointi ja palauttaminen-toimintoja tuetaan samalla tavalla kuin Vakio Windows Server-käyttöjärjestelmät ja Hyper-V.

Sinun on tehtävä että sinulla on kelvollinen tietokannan varmuuskopioinnin toimintamallin paikassa. 

Paljas käyttöönotoissa, kuten varmuuskopiointi ja palauttaminen suorituskyky määräytyy määrät kuinka monta voi lukea rinnakkain ja siirtonopeuden nämä asemat ehkä. Lisäksi varmuuskopion pakkaamisen käyttämä suorittimen kulutus voi toistaa merkittäviä roolin VMs vain enintään 8 suorittimen viestiketjuissa siirtyminen kanssa. Tämän vuoksi jokin oletetaan, voit:

* Vähemmän näennäiskiintolevyjen voidaan tallentaa tietokannan-laitteet, sitä vähemmän yleinen siirtonopeuden lukutilassa
* Mitä pienempi suorittimen määrän viestiketjut AM, Lisää vakavia varmuuskopion pakkaamisen vaikutus
* Vähemmän kohteet (raita kansioita, näennäiskiintolevyjen) kirjoittaa varmuuskopion, alaosassa siirtonopeuden

Lisäämiseksi kohdemäärä kirjoittaminen kahdella eri tavalla voidaan käyttää/yhdistetyn tarpeidesi:

* Kohteen varmuuskopioinnin äänenvoimakkuuden striping päälle useita liitetyn näennäiskiintolevyjen IOPS siirtonopeuden mustat asemassa parantamiseksi
* Kirjoita varmuuskopion useita kohdekansio avulla

#### <a name="high-availability-and-disaster-recovery"></a>Suuri käytettävyys ja palauttaminen
Microsoft Klusterin Serverin (MSCS) ei tueta.

DB2 suuren käytettävyyden palauttaminen (HADR) tuetaan. Jos HA-määritys näennäiskoneiden on toimi nimenselvitys, Azure-asetuksissa ei poiketa määritys, joka on tehty paikallisen. Ei suositella riippuvaisia vain IP-tarkkuus.

Älä käytä Azure kaupan Geo-replikoinnin. Lisätietoja viittaavat luvun [Microsoft Azuren tallennustilaan] [dbms-opas-2.3] ja [suuren käytettävyyden ja palauttaminen ja Azure VMs] luvun [dbms-opas-3].

#### <a name="other"></a>Muut
Kaikki muut yleisohjeet kuten Azure käytettävyys joukot tai SAP seurantaa koskevat kuvatulla tavalla tämän asiakirjan versioiden VMs IBM DB2 LUW saat kanssa myös kolme ensimmäistä lukuihin. 

Myös katsoa luvun [yleiset SQL Server Azure yhteenveto-SAP: n] [dbms-opas-5.8].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>Tekniset tiedot, IBM DB2 LUW Linux varten
Microsoft Azure voit siirtää helposti aiemmin SAP-sovellusta käynnissä IBM DB2-Azuren näennäiskoneiden Linux, UNIX ja Windows (LUW). SAP-IBM DB2 LUW varten, jossa järjestelmänvalvojille ja kehittäjille edelleen käyttää samaa kehittäminen ja hallintatyökalut, jotka ovat käytettävissä paikallisen. Yleisiä tietoja SAP Business Suite käytössä IBM DB2 varten LUW löytyy-SAP yhteisön verkon (SCN) osoitteessa <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Saat lisätietoja ja SAP-Azure LUW DB2-päivitykset-SAP Huomautus [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux, UNIX ja Windows-Versiotuki
SAP: in for Microsoft Azure virtuaalikoneen Services LUW IBM DB2-tuetaan DB2-versio 10.5 vuodesta.

Lisätietoja tuetuista SAP-tuotteet ja Azure AM tyypit tutustumaan SAP Huomautus [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX ja Windows-määritysten ohjeet Azure VMs SAP-asennukset

#### <a name="storage-configuration"></a>Tallennustilan kokoonpanon
Kaikki tietokantatiedostot on tallennettava tiedostojärjestelmässä Näennäiskiintolevyn levyjen perusteella. Nämä näennäiskiintolevyjen ovat käytettävissä Azure AM ja perustuvat sivun Azure-BLOB-OBJEKTIEN tallennustilaan (<https://msdn.microsoft.com/library/azure/ee691964.aspx>).
Kaikenlaista verkkoasemat tai jaettuja esimerkiksi seuraavat Azure tiedostopalvelut ovat **ei** tueta tietokantatiedostoja:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

Jos käytössäsi on Azure näennäiskiintolevyjen sivun Azure-BLOB-säiliö perusteella, tehdä tässä asiakirjassa luvun RDBMS käyttöönoton rakenteen [dbms-opas-2] lauseet koskevat myös ominaisuuksissa IBM DB2 LUW tietokannan kanssa.

Aiemmassa Yleiset asiakirjan osaan kuvatulla IOPS siirtonopeuden varten Azure näennäiskiintolevyjen kiintiöiden olemassa. Tarkka kiintiön määräytyvät käytettävän AM tyyppi. AM tyypit ja niiden tavoitteet luettelo löytyy [tähän][virtual-machines-sizes].

Kun nykyinen IOPS kiintiön levyä kohden riittää, on mahdollista tallentaa kaikki tietokantatiedostot yhden yksittäisen liitetyn Azure-Näennäiskiintolevyn.

Suorituskyvyn huomioon otettavia seikkoja viitata myös SAP asennuksen apuviivat luvun "Tietojen turvallisuus ja suorituskyvyn huomioon otettavia seikkoja, tietokannan kansioita".

Vaihtoehtoisesti voit LVM (loogisen levyn hallinta)- tai MDADM luvun ohjelmiston RAID [dbms-opas-2.2] tämän artikkelin ohjeiden avulla voit luoda useita liitetyn Näennäiskiintolevyn levyjä yhtä suuri looginen laite.
Sisältävät sapdata ja saptmp kansioiden DB2 tallennustilan polut levyjen on määritettävä 512 kt fyysinen levy-sektorikokoa.

#### <a name="backuprestore"></a>Varmuuskopiointi ja palauttaminen
IBM DB2 LUW, varmuuskopiointi ja palauttaminen-toimintoja tuetaan samalla tavalla kuin Vakio Linux asennuksen paikallisen.

Sinun on tehtävä että sinulla on kelvollinen tietokannan varmuuskopioinnin toimintamallin paikassa.

Paljas käyttöönotoissa, kuten varmuuskopiointi ja palauttaminen suorituskyky määräytyy määrät kuinka monta voi lukea rinnakkain ja siirtonopeuden nämä asemat ehkä. Lisäksi varmuuskopion pakkaamisen käyttämä suorittimen kulutus voi toistaa merkittäviä roolin VMs vain enintään 8 suorittimen viestiketjuissa siirtyminen kanssa. Tämän vuoksi jokin oletetaan, voit:

* Vähemmän näennäiskiintolevyjen voidaan tallentaa tietokannan-laitteet, sitä vähemmän yleinen siirtonopeuden lukutilassa
* Mitä pienempi suorittimen määrän viestiketjut AM, Lisää vakavia varmuuskopion pakkaamisen vaikutus
* Vähemmän kohteet (raita kansioita, näennäiskiintolevyjen) kirjoittaa varmuuskopion, alaosassa siirtonopeuden

Lisäämiseksi kohdemäärä kirjoittaminen kahdella eri tavalla voidaan käyttää/yhdistetyn tarpeidesi:

* Kohteen varmuuskopioinnin äänenvoimakkuuden striping päälle useita liitetyn näennäiskiintolevyjen IOPS siirtonopeuden mustat asemassa parantamiseksi
* Kirjoita varmuuskopion useita kohdekansio avulla

#### <a name="high-availability-and-disaster-recovery"></a>Suuri käytettävyys ja palauttaminen
DB2 suuren käytettävyyden palauttaminen (HADR) tuetaan. Jos HA-määritys näennäiskoneiden on toimi nimenselvitys, Azure-asetuksissa ei poiketa määritys, joka on tehty paikallisen. Ei suositella riippuvaisia vain IP-tarkkuus.

Älä käytä Azure kaupan Geo-replikoinnin. Lisätietoja viittaavat luvun [Microsoft Azuren tallennustilaan] [dbms-opas-2.3] ja [suuren käytettävyyden ja palauttaminen ja Azure VMs] luvun [dbms-opas-3].

#### <a name="other"></a>Muut
Kaikki muut yleisohjeet kuten Azure käytettävyys joukot tai SAP seurantaa koskevat kuvatulla tavalla tämän asiakirjan versioiden VMs IBM DB2 LUW saat kanssa myös kolme ensimmäistä lukuihin.

Myös katsoa luvun [yleiset SQL Server Azure yhteenveto-SAP: n] [dbms-opas-5.8].