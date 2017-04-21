
### <a name="wordpress-and-azure-app-service"></a>WordPress ja Azure sovelluksen-palvelu
   
- [Mikä on WordPress?](https://wordpress.org/)
- [Yritysluokan WordPress web App-sovelluksen määrittäminen](../articles/app-service-web/web-sites-php-enterprise-wordpress.md)
- [Voit ostaa ClearDB jaettu MySQL isännöinnin WordPress-sovelluksen](http://blog.syntaxc4.net/post/2012/12/03/provisioning-a-mysql-database-from-the-windows-azure-store.aspx)
- [Miten haluat hankkiminen ClearDB erillinen MySQL klusterin WordPress-sovelluksen](https://azure.microsoft.com/blog/announcing-new-mysql-premium-tiers-from-cleardb/)
- [Vahvistettu MySQL replikoinnin klusterin WordPress web-sovelluksen käyttöönotto](https://azure.microsoft.com/documentation/templates/wordpress-mysql-replication/)
- [Luo omia perustyyli perustyyli MySQL-klusterin käyttämällä Percona klusterin](https://azure.microsoft.com/documentation/templates/mysql-ha-pxc/) ja [Lue lisää siitä, miten voit hallita klusterin](https://github.com/fanjeffrey/axiom.articles/tree/master/pxc)
- [Ottaa käyttöön WordPress palautettua MySQL replikoinnin klusterin perustyyli toissijaisen määritysten mukaan](https://azure.microsoft.com/documentation/templates/mysql-replication/)
- [SQL Azure DB ProjectNami hallitsee palautettua WordPress sovelluksen käyttöönotto](https://azure.microsoft.com/marketplace/partners/projectnami/projectnami/)
- [Miten voit luoda WordPress Multisite verkkosovellukseen](../articles/app-service-web/web-sites-php-convert-wordpress-multisite.md)


###<a name="porting-your-application-to-app-service"></a>Sovittaminen sovelluksesi sovelluksen-palveluun 
- [Miten Azure App palvelun Valmistele](https://azure.microsoft.com/blog/how-to-plan-your-migration-to-azure-websites/)
- [Voit siirtää luotu WordPress sivuston Azure web Apps-sovellusten](https://sunithamk.wordpress.com/2013/11/06/migrate-your-existing-wordpress-site-to-windows-azure/)
- [Voit siirtää koodiin Linux Azure verkkosovelluksissa](https://www.movemetothecloud.net/LinuxMigration)
- [Siirtyminen live Azure Web Apps-sovellusten tarkistusluettelo](https://sunithamk.wordpress.com/2015/10/27/azure-web-apps-basic-operations-checklist/)
- [PHP määrittäminen](../articles/app-service-web/web-sites-php-configure.md)
- [Useita kehittäminen ympäristöissä WordPress-sovelluksen määrittäminen](../articles/app-service-web/app-service-web-staged-publishing-realworld-scenarios.md)

###<a name="troubleshooting-wordpress-application"></a>WordPress sovelluksen vianmääritys
- [WordPress sovelluksen vianmääritys](https://sunithamk.wordpress.com/2014/09/04/wordpress-troubleshooting-techniques-on-azure-websites/)
- [Kerää käyttö telemetriatietojen Azure sovelluksen tiedot-palvelun avulla](https://azure.microsoft.com/blog/usage-analytics-for-wordpress-with-azure-app-insights/)
- [Suorita Zend Zray Profilerin koodiin ongelmat ja suorituskyvyn vianmääritys](https://sunithamk.wordpress.com/2015/08/04/profiling-php-application-on-azure-web-apps/)
- [Käyttää Kudu tuki-portaalin ja ehkäistä ongelmat reaaliajassa](https://sunithamk.wordpress.com/2015/11/04/diagnose-and-mitigate-issues-with-azure-web-apps-support-portal/)
- [Käytä eri automaattinen-heal säännöt automatisoida selvitettäessä reaaliaikainen tapaukset](http://microsoftazurewebsitescheatsheet.info/#auto-heal)
- [Voit varmuuskopioida koodiin](../articles/app-service-web/web-sites-backup.md) ja [palauttaminen web Appissa](../articles/app-service-web/web-sites-restore.md)

###<a name="performance"></a>Suorituskyky
- [Voit nopeuttaa WordPress web Appissa](https://sunithamk.wordpress.com/2014/08/01/10-ways-to-speed-up-your-wordpress-site-on-azure-websites/)
- [Miten käytössä Redis.txt välimuistin](../articles/redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md) käyttämällä [redis välimuistiin-laajennusten](https://wordpress.org/plugins/wp-redis/)
- Käyttämällä [memcached laajennuksen](https://wordpress.org/plugins/memcached/) [WordPress memcached objektivälimuistin ottaminen käyttöön](../articles/app-service-web/web-sites-connect-to-redis-using-memcache-protocol.md)
- [Wincache kanssa W3 yhteensä välimuistiin-laajennusten ottaminen käyttöön](https://wordpress.org/plugins/w3-total-cache/)
- [Opit käyttämään supercache laajennuksen WordPress app nopeuttaminen](http://ruslany.net/2008/12/speed-up-wordpress-on-iis-70/)
- [Kuinka palvelimeen välimuistiin käyttämällä IIS tulosteen välimuistiin tallentaminen](http://blogs.msdn.com/b/brian_swan/archive/2011/06/08/performance-tuning-php-apps-on-windows-iis-with-output-caching.aspx)
- [Miten käytössä selaimen välimuistiasetukset staattiseksi sisällöksi](http://www.iis.net/configreference/system.webserver/staticcontent)
