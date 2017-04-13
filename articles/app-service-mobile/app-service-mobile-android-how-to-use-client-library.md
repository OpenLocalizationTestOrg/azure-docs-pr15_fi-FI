<properties
    pageTitle="Miten Android Mobile-sovellusten asiakas-kirjasto"
    description="Voit käyttää Azure-mobiilisovellukset Android asiakkaan SDK."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>


# <a name="how-to-use-the-android-client-library-for-mobile-apps"></a>Käyttämisestä mobiilisovellukset Android asiakas-kirjasto

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Tämän oppaan avulla voit toteuttaa Yleisiä tilanteita, joissa, kuten Android asiakkaan SDK Mobile-sovellusten avulla:

- Kyselyistä (lisääminen, päivittäminen ja poistaminen).
- Todennus.
- Virheiden käsittely.
- Mukauttaminen asiakas. 

Se myös tekee ohjaamme käytetään useimmin mobiilisovellukset yleisiä asiakkaan koodiksi.

Tässä oppaassa keskitytään asiakkaan Android SDK-paketissa.  Saat lisätietoja palvelinpuolen SDK: T Mobile-sovellusten aiheessa [.NET Taustajärjestelmä SDK käsitteleminen] [ 10] tai [kuinka pystyt käyttämään Node.js Taustajärjestelmä SDK][11].

## <a name="reference-documentation"></a>Oppaat

Voit etsiä [Javadocs API-viiteopas] [ 12] GitHub Android asiakas-kirjastoon.

## <a name="supported-platforms"></a>Tuetut käyttöympäristöt

Azure Mobile sovellukset Android SDK tukee API tasot 19 – 24 (KitKat Nougat kautta).  

"Tiedonkulun server-todennus käyttää näkymä esitetty UI. Jos laite ei pysty esittämään näkymä-Käyttöliittymä, muista tavoista todennus on tarpeen tuotteen alueen ulkopuolella.  Tämä SDK ei sovellu Watch type tai vastaavasti rajoitettu laitteiden.

## <a name="setup-and-prerequisites"></a>Asennus- ja edellytykset

Suorita [mobiilisovellukset pikaopas](app-service-mobile-android-get-started.md) opetusohjelman.  Tehtävä varmistaa Azure Mobile-sovellusten kehittämiseen kaikki vaatimukset täyttyvät.  Pikaopas avulla voit määrittää tilin ja luo ensimmäinen Mobile-sovelluksen-Taustajärjestelmä.

Jos et halua suorittaa pikaopas opetusohjelman, suorita seuraavat toimet:

- [Luo Mobile-sovelluksen taustassa] [ 13] käyttäminen Android-sovelluksen.
- Android Studiossa, [Päivitä Gradle muodosta tiedostot](#gradle-build).
- [Ota käyttöön internet-käyttöoikeus](#enable-internet).

###<a name="gradle-build"></a>Päivitä Gradle koontiversion määritystiedoston

Muuta **build.gradle** tiedostot:

1. Lisää tämä koodi *tason **build.gradle** projektitiedoston *buildscript* tunnisteessa* :

        buildscript {
            repositories {
                jcenter()
            }
        }

2. Lisää tämä koodi *riippuvuuksien* tunnisteessa *moduulin app* tason **build.gradle** -tiedoston:

        compile 'com.microsoft.azure:azure-mobile-android:3.1.0'

    Uusimpaan versioon ei tällä hetkellä 3.1.0. Tuetut versiot on lueteltu [Tässä][14].

###<a name="enable-internet"></a>Ota käyttöön internet-käyttöoikeudet

Voit käyttää Azure, sovelluksen on käytössä INTERNET-oikeudet. Jos se ei ole jo käytössä, Lisää seuraava rivi koodin **AndroidManifest.xml** -tiedostoon:

    <uses-permission android:name="android.permission.INTERNET" />

## <a name="the-basics-deep-dive"></a>Perusteet perinpohjaisesti käsittelevään artikkeliin

Tässä osassa käsitellään joitakin pikaopas, joka liittyy Azure Mobile-sovellusten käyttämisestä sovelluksessa koodi.  

###<a name="data-object"></a>Asiakkaan tietoluokkien määrittäminen

Määritä asiakkaan tietoluokat, jotka vastaavat Mobile-sovelluksen taustatietokannan taulukot käyttämään tietoja SQL Azure-taulukosta. Tämän artikkelin Esimerkeissä oletetaan **ToDoItem**, jolla on seuraavat sarakkeet-taulukon:

- tunnus
- teksti
- Viimeistele

Asiakkaan objektin vastaavan kirjoitetaan:

    public class ToDoItem {
        private String id;
        private String text;
        private Boolean complete;
    }

Koodin sijaitsee **ToDoItem.java**-tiedostossa.

Jos SQL Azure-taulukko sisältää enemmän sarakkeita, Lisää vastaavien kenttien tähän luokkaan.  Esimerkiksi jos DTO (tietojen siirto objekti) oli aiemmin kokonaisluku prioriteetti-sarakkeeseen ja valitse voit lisätä tämän kentän sekä sen nopeamman ja navigoinnin tavoista:

    private Integer priority;

    /**
    * Returns the item priority
    */
    public Integer getPriority() {
        return mPriority;
    }
    
    /**
    * Sets the item priority
    *
    * @param priority
    *            priority to set
    */
    public final void setPriority(Integer priority) {
        mPriority = priority;
    }

Lisätietoja taulukoita luominen mobiilisovellukset Taustajärjestelmä on artikkelissa [miten: Määritä taulukon ohjauskoneen] [ 15] (.NET Taustajärjestelmä) tai [määrittää taulukoiden dynaamisen rakenteen avulla] [ 16] (Node.js Taustajärjestelmä). Node.js, taustassa voi käyttää myös **helposti taulukot** -asetus [Azure portal].

###<a name="create-client"></a>Toimintaohje: Luo asiakkaan yhteydessä

Tämä koodi luo **MobileServiceClient** objekti, jota käytetään käyttämään Mobile-sovelluksen Taustajärjestelmä. Koodin oleviin `onCreate` määritetyn *AndroidManifest.xml* **MAIN** -toiminto ja **ALOITUKSEN** luokan **toimintaa** luokan menetelmä. Pikaopas-koodi se menee **ToDoActivity.java** -tiedostossa.

        MobileServiceClient mClient = new MobileServiceClient(
            "MobileAppUrl", // Replace with the Site URL
            this)

Korvaa tämän koodin `MobileAppUrl` ja Mobile-sovelluksen Taustajärjestelmä URL-osoite, joka löytyy [Azure portal] -sivu for Mobile-sovelluksen Taustajärjestelmä. Rivin kääntää koodin myös lisättävä **Tuo** -lause:

    import com.microsoft.windowsazure.mobileservices.*;

###<a name="instantiating"></a>Toimintaohje: taulukkoviittauksen luominen

Kyselyn tai muokkaa sen tietoja taustaan helpoin tapa on käyttää *kirjoitettu ohjelmoinnin mallin*, koska Java on erittäin kirjoitettu kielellä. Tämä malli sisältää saumaton JSON Sarjatoiminto ja sarjoituksen käyttämällä [gson] [ 3] lähetettäessä asiakkaan objektit ja taulukoiden tietojen Taustajärjestelmä Azure SQL-kirjasto.

Jos haluat käyttää taulukon, luo ensin [MobileServiceTable] [ 8] objektin soittamalla **getTable** [MobileServiceClient]menetelmän[9].  Tässä menetelmässä on kaksi Osastollasi:

    public class MobileServiceClient {
        public <E> MobileServiceTable<E> getTable(Class<E> clazz);
        public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
    }

Seuraava koodi **mClient** on MobileServiceClient objektin viittaus.  Ensimmäinen liikaa on käytössä, johon luokkanimi ja taulukkonimi on sama, ja sitä käytetään pikaopas:

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);

Toisen liikaa käytetään, kun taulukkonimi on sama kuin luokkanimi: ensimmäinen parametri on taulukkonimi.

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

###<a name="binding"></a>Toimintaohje: tietojen sitoa käyttöliittymä

Tietojen sidonta on kolme osaa:

- Tietolähde
- Näyttöasettelun
- Sovittimen, joka sitoo kaksi toisiaan kohti.

Esimerkki-koodia on palauttaa tiedot Mobile-sovellusten SQL Azure-taulukosta **ToDoItem** taulukoksi. Tämä toiminto on sovelluksia yleistä mallia.  Tietokantakyselyt palauttaa usein kokoelma rivit, jotka asiakas saa luettelon tai matriisista. Tässä esimerkissä taulukko on tietolähde.

Koodi määrittää näytön-asettelu, joka määrittää näkymän näkyviä tietoja laitteeseen.  Kahden on sidottu ja sovittimen, joka tässä tunniste on, **ArrayAdapter&lt;ToDoItem&gt; ** luokan.

#### <a name="layout"></a>Toimintaohje: määrittää asettelu

Asettelun määritetään eri XML-koodin katkelmat. Valita rakennetta, seuraava koodi edustaa **Luettelonäkymä** haluamme täytä tiedot Microsoftin palvelimen kanssa.

    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>

Yllä olevaa koodia *listitem* -määrite määrittää yksittäisen rivin luettelossa asettelun tunnus. Tämä koodi määrittää valintaruudun ja sisältämä teksti ja saa vahvistaa kerran kunkin kohteen luettelosta. Tämä asettelu ei näy **tunnus** -kenttä ja monimutkaisia asettelu määrittää kyseiset kentät näkyviin. Koodi on **row_list_to_do.xml** -tiedostossa.

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">
        <CheckBox
            android:id="@+id/checkToDoItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/checkbox_text" />
    </LinearLayout>


#### <a name="adapter"></a>Toimintaohje: sovittimen määrittäminen

Koska Microsoftin Näytä tietolähde on matriisin **ToDoItem**on aliluokka Tutustu sovittimen kohteesta **ArrayAdapter&lt;ToDoItem&gt; ** luokan. Tämä aliluokka tuottaa näkymän jokaisen **ToDoItem** **row_list_to_do** -asettelun.

Koodia on määrittää seuraavat luokka, joka on laajennus **ArrayAdapter&lt;E&gt; ** luokan:

    public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {

Ohita sovittimet **getView** -menetelmää. Esimerkki:

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }

        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });


        return row;
    }

Luodaan tämän luokan esiintymä-tässä aktiviteetti seuraavasti:

    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);

Toisen parametrin ToDoItemAdapter konstruktorin on viittaus asetteluun. Syy vahvistaa **Luettelonäkymä** ja määrittää sovittimen **Luettelonäkymä**.

    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);

### <a name="api"></a>API-rakenne

Mobile-sovellusten taulukon toimintoja ja mukautetun API-kutsujen ovat asynkroninen. [Tulevaisuudessa] ja [AsyncTask] -objektien käyttäminen henkilöihin liittyvät kyselyt, Lisää, päivittää ja poistaa asynkroninen menetelmistä. Käyttämällä tulevaa Suorita useita taustan viestiketjun eikä sinun tarvitse käsitellä useita sisäkkäisiä takaisinkutsuja on helpompaa.

Tarkista esimerkki [Azure portal] Android pikaopas-projektin **ToDoActivity.java** -tiedosto.

#### <a name="use-adapter"></a>Toimintaohje: Käytä sovittimen

Olet nyt valmis käytettäväksi tietojen sidonta. Seuraava koodi näkyy hankkiminen kohteiden taulukon ja paikallisen sovittimen täyttää palautetut nimikkeet.

    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }

Soita sovittimen **ToDoItem** taulukon muokkaaminen milloin tahansa. Koska muutokset tehdään tietueen tietueen välein, käsitellä yksirivinen kokoelman sijaan. Kun lisäät kohteen, kutsua **Lisää** menetelmää sovittimen; Kun poistat, kutsua **poistaminen** -menetelmää.

##<a name="querying"></a>Toimintaohje: kyselyn Mobile-sovelluksen taustaan

Tässä osassa kuvataan, miten kyselyjen antamaan Mobile-sovelluksen taustatietokannan, joka sisältää seuraavat toimet:

- [Palauttaa kaikki kohteet]
- [Palautetut tietojen suodattaminen]
- [Palauttaa tietojen lajitteleminen]
- [Sivujen tietojen palauttaminen]
- [Valitse tietyt sarakkeet]
- [KETJUTA kyselyn menetelmät](#chaining)

### <a name="showAll"></a>Toimintaohje: Palauta kaikki kohteet taulukosta

Seuraava kysely palauttaa kaikki kohteet **ToDoItem** -taulukossa.

    List<ToDoItem> results = mToDoTable.execute().get();

*Tulokset* muuttujan palauttaa tulosjoukon, luettelona kyselystä.

### <a name="filtering"></a>Toimintaohje: Suodatin palautti tietoja

Seuraavat kyselyn suorituksen palauttaa kaikki kohteet **ToDoItem** taulukon, jossa **Valmis** on **Epätosi**.

    List<ToDoItem> result = mToDoTable.where()
                                .field("complete").eq(false)
                                .execute().get();

**mToDoTable** on mobiilipalvelu taulukko, joka on aiemmin luotu viittaus.

Määritä suodatin, **jossa** menetelmä puhelun käyttäminen taulukon viittauksissa. **Jossa** -menetelmä perässä on **kentän** menetelmän menetelmän, joka määrittää loogisen lause perään. Mahdollisia predikaatin menetelmiä ovat **eq** (yhtä suuri kuin), **ne** (eri suuri kuin), **gt** (suurempi kuin)- **sivu** (suurempi tai yhtä suuri kuin), **lt** (pienempi kuin)- **tiedostoon** (pienempi tai yhtä suuri kuin). Näistä tavoista avulla voit verrata numero ja merkkijonon kenttien arvoja.

Voit suodattaa päivämäärien. Seuraavilla tavoilla voit verrata päivämäärän osien tai koko päivämääräkenttä: **vuosi**, **kuukausi**, **päivä**, **Tunti**, **minuutti**ja **toinen**. Seuraava esimerkki Lisää kohteita, joiden *Määräpäivä* on sama kuin 2013 suodattimen.

    mToDoTable.where().year("due").eq(2013).execute().get();

Seuraavilla tavoilla tukevat monimutkaisia suodattimia merkkijonon kentissä: **startsWith**, **endsWith** **ketjutettu**, **alimerkkijono**, **indexOf**, **Korvaa**, **toLower**, **toUpper**, **Leikkaa**tai **pituus**. Seuraavassa esimerkissä suodattaa taulukon rivien, joissa *teksti* -sarakkeen alussa on "PRI0."

    mToDoTable.where().startsWith("text", "PRI0").execute().get();

Numerokentät tuetaan operaattori seuraavista menetelmistä: **Lisää**, **sub** **mul**, **jako**, **mod**, **floor**, **PYÖRISTÄ.KERR.ylös**tai **pyöristää**. Seuraavassa esimerkissä suodattaa taulukon rivien, jossa **kesto** on parillinen.

    mToDoTable.where().field("duration").mod(2).eq(0).execute().get();

Voit yhdistää predikaatit looginen seuraavista tavoista: **ja **tai** **ja **ei**. Seuraavassa esimerkissä yhdistää kaksi edellä olevaa esimerkkiä.

    mToDoTable.where().year("due").eq(2013).and().startsWith("text", "PRI0")
                .execute().get();

Ryhmittely ja sisäkkäiset loogisilla operaattoreilla:

    mToDoTable.where()
                .year("due").eq(2013)
                    .and
                (startsWith("text", "PRI0").or().field("duration").gt(10))
                .execute().get();

Lisätietoja keskustelun ja esimerkkejä suodatus on artikkelissa [he käyttäisivät Android asiakas-kyselyn mallin tutkiminen](http://hashtagfail.com/post/46493261719/mobile-services-android-querying).

### <a name="sorting"></a>Toimintaohje: Lajittele palautti tietoja

Seuraava koodi palauttaa kaikki kohteet **ToDoItems** taulukosta lajiteltu nousevaan järjestykseen *teksti* -kentän mukaan. *mToDoTable* on aiemmin luomasi Taustajärjestelmä taulukon viittaus:

    mToDoTable.orderBy("text", QueryOrder.Ascending).execute().get();

**Lajittelu** -menetelmän ensimmäinen parametri on merkkijono, joka on sama nimi kenttä, jonka haluat lajitella. Toisen parametrin käyttää **QueryOrder** luettelointi, haluatko lajitella nousevaan tai laskevaan järjestykseen.  Jos suodatat, ***jossa*** -menetelmällä, ***jossa*** -menetelmä on ladattava ennen ***Lajittelu*** -menetelmää.

### <a name="paging"></a>Toimintaohje:-sivujen tietojen palauttaminen

Ensimmäinen esimerkissä viisi ylimmän valitseminen taulukosta. Kysely palauttaa kohteet **ToDoItems**sisällysluettelon. **mToDoTable** on aiemmin luomasi Taustajärjestelmä taulukon viittaus:

    List<ToDoItem> result = mToDoTable.top(5).execute().get();


Kysely, joka ohittaa ensimmäiset viisi kohteet ja palauttaa sitten seuraavan viiden näin:

    mToDoTable.skip(5).top(5).execute().get();

### <a name="selecting"></a>Toimintaohje: Valitse tietyt sarakkeet

Seuraava koodi kuvataan, miten kaikki kohteet tuotto **ToDoItems**sisällysluettelon, mutta näyttää vain **valmiina** - ja **teksti** -kentät. **mToDoTable** on Taustajärjestelmä taulukko, joka on aiemmin luotu viittaus.

    List<ToDoItemNarrow> result = mToDoTable.select("complete", "text").execute().get();

Select-funktion parametrit ovat taulukon sarakkeiden, jonka haluat palauttaa merkkijonon nimet.

**Valitse** menetelmä on seurattava menetelmiä, kuten **missä** ja **Lajittelu**. Se voi seurata sivutus menetelmiä, kuten **ylä**.

### <a name="chaining"></a>Toimintaohje: KETJUTA kyselyn menetelmät

Kyselyt taustatietokannan taulukot käytettävistä menetelmistä on yhdistetään. Kyselyn menetelmiä ketjutus avulla voit valita tietyt sarakkeet, jotka on lajiteltu ja sivutettu suodatetut rivit. Voit luoda monimutkaisia looginen suodattimet.
Kummassakin menetelmässä kysely palauttaa kyselyobjektin. Voit lopettaa sarjan menetelmistä ja suorittaa kyselyä, soita **execute** -menetelmää. Esimerkki:

    mToDoTable.where()
        .year("due").eq(2013)
        .and().startsWith("text", "PRI0")
        .or().field("duration").gt(10)
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .top(20)
        .execute().get();

Ketjutetut kyselyn menetelmistä on tilattava seuraavasti:

1. Voit suodattaa (**jossa**) tavoista.
2. Lajittelu (**Lajittelu**) menetelmät.
3. Valinta (**Valitse**) menetelmät.
4. sivutuksen (**ohittaa** ja **top**) menetelmät.

##<a name="inserting"></a>Toimintaohje: tietojen lisääminen taustaan

Vahvistaa *ToDoItem* luokan esiintymän ja määritä sen ominaisuuksia.

    ToDoItem item = new ToDoItem();
    item.text = "Test Program";
    item.complete = false;

Objektin lisääminen **insert()** avulla:

    ToDoItem entity = mToDoTable.insert(item).get();

Palautetun kohteen vastineita taustatietokannan, taulukkoon tietojen mukana tunnus- ja muiden arvojen määrittäminen taustassa.

Mobile-sovellusten taulukot edellyttävät **tunnus**-nimisen perusavainsarakkeeksi. Oletusarvoisesti tämä sarake on merkkijono. Tunnussarake oletusarvo on GUID-tunnus.  Voit antaa muiden yksilöllisiä arvoja, kuten sähköpostiosoitteet tai käyttäjänimet. Kun tunnus arvoksi ei ole toimitettu lisätyn tietueen, taustaan Luo uuden GUID-tunnus.

Merkkijono tunnukset on seuraavia etuja:

+ Tunnukset voidaan luoda ilman, että matkassa tietokantaan.
+ Tietueet on helpompi yhdistää eri taulukoista tai tietokantoihin.
+ Tunnukset integroi paremmin sovelluksen logiikan.

Merkkijono tunnuksen arvot ovat **vaaditaan** offline-synkronoinnin tuki.

##<a name="updating"></a>Toimintaohje: mobiilisovelluksessa tietojen päivittäminen

Taulukon tietojen päivittämiseen välittää uuden objektin **Update ()** -menetelmää.

    mToDoTable.update(item).get();

Tässä esimerkissä *kohde* on viittaus rivin *ToDoItem* -taulukosta, jossa on ollut tehdä siihen muutoksia.
Rivi, jonka **tunnus** on päivitetty.

##<a name="deleting"></a>Toimintaohje: poistaa tietoja mobiilisovelluksessa

Seuraava koodi näytetään, miten voit poistaa taulukon tietoja määrittämällä tieto-objekti.

    mToDoTable.delete(item);

Voit myös poistaa kohteen määrittämällä, voit poistaa rivin **tunnus** -kenttä.

    String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
    mToDoTable.delete(myRowId);

##<a name="lookup"></a>Toimintaohje: tietyn kohteen hakeminen

Hakee kohteen tietyn **tunnus** -kentän kanssa käyttämällä **lookUp()** -menetelmää:

    ToDoItem result = mToDoTable
                        .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
                        .get();

##<a name="untyped"></a>Toimintaohje: tyypittämättömän tietojen käsitteleminen

Tyypittämättömän ohjelmoinnin mallin avulla voit JSON Sarjatoiminto tarkka hallintaoikeutta.  On havainnollistetaan Yleisiä tilanteita, joissa haluta käyttää tyypittämättömän ohjelmoinnin mallia. Jos esimerkiksi Taustajärjestelmä taulukko sisältää useita sarakkeita ja haluat viitata alijoukkoa sarakkeet.  Kirjoitettu mallia edellyttää, voit määrittää kaikki mobiilisovellukset taulukon sarakkeiden tiedot-luokka.  

Useimmat API tarjouspyyntöjen tietojen käyttäminen muistuttavat kirjoitetun ohjelmoinnin puhelut. Tärkein ero on se, että tyypittämättömän mallin suoritat menetelmiä **MobileServiceJsonTable** objektin **MobileServiceTable** objektin sijaan.

### <a name="json_instance"></a>Toimintaohje: tyypittämättömän taulukon luominen

Samoin kuin kirjoitettua mallin, voit aloittaa käytön taulukkoviittauksen, mutta tällöin **MobileServicesJsonTable** objekti on. Hanki viittaus soittamalla **getTable** asiakkaan esiintymä menetelmää:

    private MobileServiceJsonTable mJsonToDoTable;
    //...
    mJsonToDoTable = mClient.getTable("ToDoItem");

Kun olet luonut **MobileServiceJsonTable**esiintymä, siinä on lähes saman Ohjelmointirajapinnan käytettävissä kirjoitetun ohjelmoinnin malli. Joissakin tapauksissa menetelmiä kestää tyypittämättömän parametri, joka on kirjoitettu parametrin sijaan.

### <a name="json_insert"></a>Toimintaohje: Lisää tyypittämättömän taulukkoon

Seuraava koodi esitetään, kuinka voit tehdä insert. Ensimmäiseksi on luotava [JsonObject][1], joka on osa [gson] [ 3] kirjastoon.

    JsonObject jsonItem = new JsonObject();
    jsonItem.addProperty("text", "Wake up");
    jsonItem.addProperty("complete", false);

Tämän jälkeen tyypittämättömän objektin lisääminen taulukkoon **insert()** avulla.

    mJsonToDoTable.insert(jsonItem).get();

Jos sinun on hankittava lisätyn objektin tunnus, käytä **getAsJsonPrimitive()** -menetelmää.

    jsonItem.getAsJsonPrimitive("id").getAsInt());

### <a name="json_delete"></a>Toimintaohje: tyypittämättömän taulukon poistaminen

Seuraava koodi näytetään, miten voit poistaa erillisen, tässä tapauksessa **JsonObject** , joka on luotu *Lisää* edellisen esimerkin samassa esiintymässä. Koodi on sama kuin kirjoitettua tapaukseen, mutta menetelmä on eri allekirjoitusta, koska se viittaa **JsonObject**.

         mToDoTable.delete(jsonItem);

Voit myös poistaa erillisen suoraan sen tunnus avulla:

         mToDoTable.delete(ID);

### <a name="json_get"></a>Toimintaohje: kaikki rivit tuotto tyypittämättömäksi taulukoksi

Seuraava koodi esitetään, kuinka voit hakea koko taulukon. Jälkeen käytät JSON-taulukkoon, voit hakea vain joidenkin taulukon sarakkeiden valikoivasti.

    public void showAllUntyped(View view) {
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final JsonElement result = mJsonToDoTable.execute().get();
                    final JsonArray results = result.getAsJsonArray();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (JsonElement item : results) {
                                String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                                String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                                Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                                ToDoItem mToDoItem = new ToDoItem();
                                mToDoItem.setId(ID);
                                mToDoItem.setText(mText);
                                mToDoItem.setComplete(mComplete);
                                mAdapter.add(mToDoItem);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
    }

Suodattaminen samoja, suodattaminen ja sivutus menetelmiä, jotka ovat käytettävissä kirjoitetun mallin ovat käytettävissä myös tyypittämättömän malli.

##<a name="custom-api"></a>Toimintaohje: mukautetun Ohjelmointirajapinnan kutsu

Mukautetun API avulla voit määrittää mukautetun päätepisteet, joka näyttää server-toiminto, joka ei ole insert yhdistäminen, päivittäminen, poistaminen tai luku. Käyttämällä mukautettuja API voit määrittää tarkemmin messaging, mukaan lukien lukeminen ja määrittäminen HTTP viestiotsikot ja määrittämällä viestin tekstiosaan muodon kuin JSON.

Android-asiakasohjelma ja soita Soita mukautetun API päätepisteen **invokeApi** -menetelmää. Seuraavassa esimerkissä esitetään soittaminen päätepisteen API-niminen **completeAll**, joka palauttaa sivustokokoelman luokan nimeltä **MarkAllResult**.

    public void completeItem(View view) {

        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll", MarkAllResult.class );

            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }

                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();
                }
            });
        }

Asiakkaan, joka lähettää viestin uuden mukautetun Ohjelmointirajapinnan kutsutaan **invokeApi** -menetelmää. Mukautetun Ohjelmointirajapinnan palauttama tulos näkyy viesti-valintaikkuna, kun virheitä. Muut versiot **invokeApi** avulla voit myös lähettää pyynnön jotakin objektin, HTTP-laskentatavan ja Lähetä pyyntö kyselyparametrit. **InvokeApi** tyypittämättömän versioita ovat käytettävissä myös.

##<a name="authentication"></a>Toimintaohje: käyttöoikeuksien lisääminen sovelluksen

Opetusohjelmat jo kerrotaan yksityiskohtaisesti Lisää nämä ominaisuudet.

Sovelluksen palvelu tukee [todennustapa sovelluksen käyttäjille](app-service-mobile-android-get-started-users.md) , ulkoiset tunnistetietojen toimittajat erilaisilla: Facebook, Google, Microsoft-Account, Twitter ja Azure Active Directory. Voit määrittää käyttöoikeudet vain todennetut käyttäjät toiminnoista käytön taulukot. Voit käyttää myös todennettujen käyttäjien käyttäjätietojen toteuttavien oman Taustajärjestelmä käyttöoikeuksien myöntämissääntöjä.

Kaksi todennus kulkee tuetaan: **server** -kulun sekä **asiakas** -työnkulku. Palvelin-työnkulku on helpoin todennus-toiminto, kun se on riippuvainen tunnistetietojen toimittajat web-liittymän.  Ei ole muita SDK: T tarvitaan toteuttamisesta server työnkulku-todennusta. Palvelimen työnkulku todennus ei tarjoa laaja integroida mobiililaitteen ja suositellaan vain kuitti käsite skenaarioita.

Asiakas-työnkulun avulla tarkempaa laitekohtaiset-ominaisuudet, kuten single Sign-integrointia varten, kun se on riippuvainen SDK: T myöntämä tunnistetietojen toimittaja.  Voit esimerkiksi integroida Facebook-SDK mobiili-sovellukseen.  Mobiilisovelluksen vaihtaa Facebook-sovellukseen ja vahvistaa, että Sign ennen vaihtaminen takaisin mobile-sovellus.

Ottaa käyttöön sovelluksen todennuksen tarvitaan neljä vaihetta:

- Voit rekisteröidä todennustavaksi sovelluksen tunnistetietojen toimittaja.
- Määritä sovellus palvelun Taustajärjestelmä.
- Taulukon vain sovelluksen palvelun taustassa todennettujen käyttäjien käyttöoikeuksien rajaamiseen.
- Todennus-koodin lisääminen sovelluksen.

Voit määrittää käyttöoikeudet vain todennetut käyttäjät toiminnoista käytön taulukot. Voit myös muokata pyynnöt todennetun käyttäjän SUOJAUSTUNNUS.  Saat lisätietoja Tarkista [aloittaminen todennus] ja palvelimen SDK ohjeet ohjeissa.

### <a name="caching"></a>Toimintaohje: todennus-koodin lisääminen sovelluksen

Seuraava koodi alkaa Google-palvelun avulla palvelimen työnkulku kirjautuminen prosessi:

    MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google);

Hanki Käyttäjätunnus-kohdassa, **MobileServiceUser** **getUserId** -menetelmällä. Katso esimerkki käyttämisestä tulevaa Soita asynkroninen kirjautuminen API- [todennuksen käytön aloittaminen].

### <a name="caching"></a>Toimintaohje: välimuistiin todennus tunnusten

Välimuistin todennus tunnusten edellyttää tallentaa Käyttäjätunnus ja todennus tunnuksen paikallisesti laitteeseen. Sovellus käynnistyy, kun seuraavan kerran tarkistat välimuistin ja jos nämä arvot ovat näkyvissä, voit ohittaa lokin ohjeiden mukaisesti ja lisätä vettä ennen nauttimista asiakkaan näiden tietojen kanssa. Nämä tiedot on kuitenkin luottamuksellisia ja on tallennettava salattu turvallisuutta siltä varalta, että puhelimen saa varastamiselta.

Näet täydellisen esimerkki siitä, kuinka välimuistin todennus tunnusten [välimuistin todentaminen tunnusten]‑osassa voit[7].

Kun yrität käyttää vanhentuneet tunnuksen, saat *401-ei valtuuksia* vastausta. Voit käsitellä todennusvirheitä suodattimien avulla.  Suodattimien leikkauspiste App palvelun Taustajärjestelmä pyynnöt. Suodatinkoodi 401 vastaus Testaa, käynnistää kirjautumisen prosessin ja säilyy 401 luoneen pyynnön. 

## <a name="adal"></a>Toimintaohje: todentaa käyttäjät Active Directory käyttöoikeuksien-kirjaston kanssa

Voit kirjautua Azure Active Directoryn avulla sovelluksesi käyttäjät Active Directory käyttöoikeuksien kirjaston (ADAL). Asiakkaan työnkulku kirjautumisen käyttäminen on usein suositeltavampaa `loginAsync()` menetelmistä sellaisena kuin se sisältää Lisää alkuperäisen UX ilmeen ja muita mukauttamisen avulla.

1. Määritä [määrittäminen sovelluksen palvelun Active Directory-kirjautumiset](app-service-mobile-how-to-configure-active-directory-authentication.md) -opetusohjelma mobiilisovelluksen-taustatietokannan AAD kirjautumista varten. Varmista, että Viimeistele valinnainen vaihe native client-sovelluksen.

2. Asenna ADAL muokkaamalla build.gradle tiedosto sisältää seuraavat määritykset:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

3. Lisää seuraava koodi sovelluksen, että seuraavat korvauksia:

* Korvaa **Lisää MYÖNTÄJÄ-tähän** nimi, jossa sovellus on valmisteltu vuokraajan. Muoto on oltava https://login.windows.net/contoso.onmicrosoft.com. Tämän arvon voi kopioida [perinteinen Azure portaalissa] Azure Active Directory-toimialueen-välilehdestä.

* **Lisää-resurssi-tunnus-tähän** tilalle mobiilisovelluksen Taustajärjestelmä Asiakastunnus. Voit hankkia Asiakastunnus-portaalissa **Azure Active Directory-asetukset** -kohdassa **Lisäasetukset** -välilehti.

* **Lisää-asiakas-tunnus-tähän** tilalle alkuperäisen asiakassovellus kopioitu Asiakastunnus.

* **Lisää-REDIRECT-URI-tähän** tilalle sivuston _/.auth/login/done_ päätepisteen HTTPS-mallin avulla. Tämän arvon pitäisi olla samanlainen _https://contoso.azurewebsites.net/.auth/login/done_.

        private AuthenticationContext mContext;

        private void authenticate() {
            String authority = "INSERT-AUTHORITY-HERE";
            String resourceId = "INSERT-RESOURCE-ID-HERE";
            String clientId = "INSERT-CLIENT-ID-HERE";
            String redirectUri = "INSERT-REDIRECT-URI-HERE";
            try {
                mContext = new AuthenticationContext(this, authority, true);
                mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
            } catch (Exception exc) {
                exc.printStackTrace();
            }
        }

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    Log.d(TAG, "Cancelled");
                } else {
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    Log.d(TAG, "Token is empty");
                } else {
                    try {
                        JSONObject payload = new JSONObject();
                        payload.put("access_token", result.getAccessToken());
                        ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                            @Override
                            public void onFailure(Throwable exc) {
                                exc.printStackTrace();
                            }
                            @Override
                            public void onSuccess(MobileServiceUser user) {
                                Log.d(TAG, "Login Complete");
                            }
                        });
                    }
                    catch (Exception exc){
                        Log.d(TAG, "Authentication error:" + exc.getMessage());
                    }
                }
            }
        };

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
            }
        }

## <a name="how-to-add-push-notification-to-your-app"></a>Toimintaohje: push-ilmoituksen lisääminen sovelluksen

Voit [lukea yleiskatsaus] [ 6] , joka kuvaa, miten Microsoft Azure ilmoituksen keskittimet tukee monenlaisia push-ilmoitukset.  [Tässä]opetusohjelmassa[5], push-ilmoitus lähetetään kaikissa laitteissa aina, kun tietue lisätään.

## <a name="how-to-add-offline-sync-to-your-app"></a>Toimintaohje: offline-synkronoinnin lisääminen sovelluksen

Pikaopas-opetusohjelma on koodi, joka sisältää offline-synkronoinnin. Etsi etuliite kommentit koodi:

    // Offline Sync

Seuraavat koodin rivit uncommenting voit toteuttaa offline-synkronoinnin ja voit lisätä samalla koodin mobiilisovellukset muu koodi.

##<a name="customizing"></a>Toimintaohje: asiakkaan mukauttaminen

Voit mukauttaa asiakkaan oletustoiminnon useilla tavoilla.

### <a name="headers"></a>Toimintaohje: mukauttaminen pyytää otsikot

Määrittää mukautetun HTTP-otsikon lisääminen sivupyynnön **ServiceFilter** :

    private class CustomHeaderFilter implements ServiceFilter {

        @Override
        public ListenableFuture<ServiceFilterResponse> handleRequest(
                    ServiceFilterRequest request,
                    NextServiceFilterCallback next) {

            runOnUiThread(new Runnable() {

                @Override
                public void run() {
                    request.addHeader("My-Header", "Value");                    }
            });

            SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
            try {
                ServiceFilterResponse response = next.onNext(request).get();
                result.set(response);
            } catch (Exception exc) {
                result.setException(exc);
            }
        }

### <a name="serialization"></a>Toimintaohje: Sarjatoiminto mukauttaminen

Asiakkaan oletetaan, että taulukoiden nimet, sarakkeiden nimien ja tietojen kirjoituskentän taustassa kaikki vastaavat täsmälleen asiakkaan määritelty tieto-objektit. Voi olla useita syitä, miksi asiakkaan ja palvelimen nimet eivät ehkä vastaa. Käyttämässäsi skenaariossa haluat ehkä tehdä seuraavanlaisia mukautukset:

- Käyttää sovelluksen palvelun taulukon sarakkeiden nimet eivät vastaa asiakkaan käytät nimiä.
- Käytä sovelluksen Service-taulukko, jossa on eri kuin se yhdistää työasemaohjelmassa luokan nimi.
- Ota käyttöön automaattinen ominaisuuden kirjainkoon.
- Monimutkainen ominaisuuksien lisääminen objektiin.

### <a name="columns"></a>Toimintaohje: Yhdistä eri asiakkaan ja palvelimen nimet

Oletetaan, että Java käyttää **ToDoItem** objektin ominaisuudet, kuten seuraavat ominaisuudet Java-tyyppiseksi nimissä:

- mId
- mText
- mComplete
- Kesto.Muunn

Muuntaa asiakkaan nimen sarjaksi kyselyjä palvelimessa **ToDoItem** taulukon sarakenimet vastaavat JSON nimet. Seuraava koodi käyttää [gson] [ 3] kirjaston huomautuksia ominaisuudet:

    @com.google.gson.annotations.SerializedName("text")
    private String mText;

    @com.google.gson.annotations.SerializedName("id")
    private int mId;

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;

    @com.google.gson.annotations.SerializedName("duration")
    private String mDuration;

### <a name="table"></a>Toimintaohje: Yhdistä eri taulukoiden nimet asiakkaan ja taustaan välillä

Yhdistää taulukkonimeä eri mobile-palvelujen taulukon nimen käyttämällä [getTable()] ohitus[ 4] menetelmää:

    mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

### <a name="conversions"></a>Toimintaohje: automatisoida sarakkeiden nimien yhdistämismääritysten

Voit määrittää muuntaminen strategia, joka koskee jokaisessa sarakkeessa käyttämällä [gson] [ 3] API. Android asiakas-kirjasto käyttää [gson] [ 3] onnistu Java-objektien JSON tietoihin, ennen kuin tiedot lähetetään Azure App palvelun taustalla.  Seuraava koodi käyttää **setFieldNamingStrategy()** menetelmää voit määrittää strategia. Tässä esimerkissä poistetaan alkukirjain ("m"), ja valitse pienet seuraavan merkin jokaisen kentän nimi. Esimerkiksi se muuttuvat "mId" "tunnus"

    client.setGsonBuilder(
        MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(new FieldNamingStrategy() {
            public String translateName(Field field) {
                String name = field.getName();
                return Character.toLowerCase(name.charAt(1))
                    + name.substring(2);
                }
            });

Koodi on suoritettava, ennen kuin käytät **MobileServiceClient**.

### <a name="complex"></a>Toimintaohje: tallentaa objektin tai matriisista ominaisuuden lisääminen taulukkoon

Tutustu Sarjatoiminto esimerkkejä on tähän mennessä kuulunut primitiivityyppejä, kuten kokonaisluvut ja merkkijonoja.  Primitiivityyppejä onnistu helposti JSON kyselyjä.  Jos haluamme monimutkaisia objektia, jonka automaattisesti ei onnistu JSON-annettava antamaan JSON Sarjatoiminto-menetelmää.  Jos haluat nähdä, miten antamaan mukautetun JSON Sarjatoiminto, tarkista blogimerkinnän [mukauttaminen Sarjatoiminto Mobile Services Android-asiakasohjelmassa gson-kirjaston käytön][2].

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[The API structure]: #api
[How to: Query data from a mobile service]: #querying
[Palauttaa kaikki kohteet]: #showAll
[Palautetut tietojen suodattaminen]: #filtering
[Palauttaa tietojen lajitteleminen]: #sorting
[Sivujen tietojen palauttaminen]: #paging
[Valitse tietyt sarakkeet]: #selecting
[How to: Concatenate query methods]: #chaining
[How to: Bind data to the user interface]: #binding
[How to: Define the layout]: #layout
[How to: Define the adapter]: #adapter
[How to: Use the adapter]: #use-adapter
[How to: Insert data into a mobile service]: #inserting
[How to: update data in a mobile service]: #updating
[How to: Delete data in a mobile service]: #deleting
[How to: Look up a specific item]: #lookup
[How to: Work with untyped data]: #untyped
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching
[How to: Handle errors]: #errors
[How to: Design unit tests]: #tests
[How to: Customize the client]: #customizing
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next Steps]: #next-steps
[Setup and Prerequisites]: #setup

<!-- Images. -->

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
[Käyttöoikeuksien käytön aloittaminen]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[Tulevaisuudessa]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html