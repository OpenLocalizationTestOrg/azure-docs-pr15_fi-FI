<properties 
    pageTitle="Silta Android näkymä alkuperäisen Mobile välitys Android SDK kanssa" 
    description="Kerrotaan, miten voit luoda sillan näkymä Javascript-ja alkuperäisen Mobile välitys Android SDK: N välillä"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Silta Android näkymä alkuperäisen Mobile välitys Android SDK kanssa

> [AZURE.SELECTOR]
- [Android-silta](mobile-engagement-bridge-webview-native-android.md)
- [iOS-silta](mobile-engagement-bridge-webview-native-ios.md)

Jotkin mobile-sovellukset on suunniteltu kuin hybrid app, jossa sovelluksessa on kehitetty alkuperäisen Android kehittäminen, mutta jotkin tai jopa kaikkien ikkunoiden hahmonnetaan sisällä Android näkymä. Voit edelleen tarjoaman Mobile välitys Android SDK: ssa tällaiset-sovelluksista ja tässä opetusohjelmassa kuvataan, miten voit vaihtaa tietoja tekoa. Seuraava näyte koodi perustuu Android ohjeet [tähän](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Artikkelissa kuvataan, miten kuvata tämän menetelmän voidaan toteuttaa saman Mobile välitys Android SDK's menetelmiä siten, että näkymä hybrid-sovelluksen voit myös aloittaa pyynnöt seuraa tapahtumia, työt, virheitä, sovelluksen tiedot aikana putkistot ne Microsoftin Android SDK kautta. 

1. Sinun on ensin, varmista, että ovat kadonneet tämän [opetusohjelman aloittaminen](mobile-engagement-android-get-started.md) integroiminen Mobile SDK välitys hybrid-sovelluksen Android kautta. Kun teet näin, että `OnCreate` tapa näyttää seuraavat.  
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }

2. Varmista, että hybrid sovelluksen on näyttö, jossa näkymä sitä. Se koodi on samankaltainen kuin seuraavat kohtaa, johon on ladataan paikallisen HTML-tiedoston **Sample.html** näkymä `onCreate` näytön-menetelmää. 

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }

        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }

3. Luo nyt silta tiedosto nimeltä **WebAppInterface** , joka luo jotkin tavallisimmat Mobile välitys Android SDK menetelmät käyttämällä päälle paketti `@JavascriptInterface` [Android ohjeissa](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript)kuvatun:

        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
        
        import com.microsoft.azure.engagement.EngagementAgent;
        
        import org.json.JSONArray;
        import org.json.JSONObject;
        
        public class WebAppInterface {
            Context mContext;
        
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
        
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
        
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
        
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
        
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  

4. Kun edellä kuvatun silta tiedoston luomaamme annettava varmistaa, että se on liitetty Microsoftin näkymä. Tämä tehtävä, sinun on muokattava oman `SetWebview` menetelmä niin, että se näyttää seuraavalta:

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }

5. Yllä koodikatkelman on nimeltään `addJavascriptInterface` Microsoftin silta luokan liitettävä Microsoftin näkymä ja luoda myös kutsutaan **EngagementJs** Soita menetelmiä silta tiedostosta kahvaa. 

6. Luo nyt nimeltään **Sample.html** kansio nimeltä **varat** joka on ladattu näkymä ja olemme mistä soittavat menetelmiä silta tiedostosta projektin seuraava tiedosto.

        <!doctype html>
        <html>
            <head>
                <style type='text/css'>
                    html { font-family:Helvetica; color:#222; }
                    h1 { color:steelblue; font-size:22px; margin-top:16px; }
                    h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
                </style>
        
                <script type="text/javascript">
        
                    window.onerror = function(err)
                    {
                      log('window.onerror: ' + err);
                    }
        
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
        
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
        
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
        
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
        
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
        
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
        
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
        
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>

8. Huomaa seuraavat seikat yllä HTML-tiedoston tietoja:

    -   Se sisältää syötteen ruudut, johon voidaan lisätä tietoja voidaan käyttää tapahtuman, työ, virhe, AppInfo nimet. Napsautettaessa-painikkeen vieressä puhelu soitetaan JavaScript-koodia, johon soittaa myöhemmin menetelmiä välittää tämän puhelun Mobile välitys Android SDK silta-tiedostosta. 
    -   Olemme tunnisteita, jotkin staattinen lisätiedot tapahtumia, työt ja näytetään, miten tämän voi tehdä myös virheitä. Tämä lisätiedot lähetetään JSON merkkijono, jos olet kohde `WebAppInterface` tiedoston, on jäsentää ja sijoittaa Android `Bundle` ja välitetään sekä lähettäminen tapahtumat-projektit-virheet. 
    -   Mobile välitys työ on poistettu nimellä määrittämäsi tekstiruudussa, suorita 10 sekuntia ja sulje. 
    -   Mobile välitys appinfo tai tunnisteen välitetään customer_name staattinen avain ja arvo, joka on lisätty syötteen arvona tunnisteen. 
 
9. Suorita sovellus ja näet seuraavasti. Nyt joitakin testi tapahtuma, kuten jompikumpi seuraavista nimi ja sitten **lähettää** sen alapuolella. 

    ![][1]

10. Nyt siirtyessäsi sovelluksen ja ulkoasua, valitse **Tapahtumat -> tietojen** **Näyttö** -välilehti, näet sekä staattinen sovelluksen-tiedot, jotka on lähettää näkyvät tapahtuman. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
