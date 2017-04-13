<properties
    pageTitle="Näytä SAML palauttama Access Control-palvelu (Java)"
    description="Lue, miten voit tarkastella SAML palauttama isännöimät Azure Java-sovellusten käyttö Control-palvelu."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>Voit tarkastella SAML palauttama Azure Access Control-palvelu

Tässä oppaassa kerrotaan, kuinka voit tarkastella pohjana suojaus vahvistus merkintä Language (SAML) palauttaa sovelluksen mukaan Azure Access ohjausobjektin Service (ACS). Oppaan muodostaa [todennusta ja Azure Access ohjausobjektin palvelun käyttämällä Pimennys käyttäjien poistamisesta][] , Aiheeseen antamalla koodi, joka näyttää SAML-tiedot. Valmiin sovelluksen näyttää seuraavankaltaiselta.

![Esimerkki SAML tulostus][saml_output]

Lisätietoja ACS-kohdassa [seuraavat vaiheet](#next_steps) .

> [AZURE.NOTE]
> Azure Access Services ohjausobjektin suodatin on yhteisön tekniikka esikatselu. Kuin esiversion ei varsinaisesti tukee sitä Microsoft.

## <a name="prerequisites"></a>Edellytykset

Tässä oppaassa tehtävien suorittaminen loppuun miten [todentaa Web käyttäjille, joilla Azure Access ohjausobjektin palvelun käyttämällä Pimennys][] otoksen ja käyttää sitä lähtökohtana opetusohjelmassa.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>Muodosta polku ja käyttöönoton kokoonpanon JspWriter kirjaston lisääminen

Lisää kirjastoon, joka sisältää muodosta polku ja käyttöönoton kokoonpanon **javax.servlet.jsp.JspWriter** -luokka. Jos käytössäsi on Tomcat-kirjasto on **jsp api.jar**, joka sijaitsee Apache **lib** -kansiossa.

1. Pimennys 's Project Explorer- **MyACSHelloWorld**hiiren kakkospainikkeella, valitse **Muodosta polku**, valitse **Määritä luominen polku**, **kirjastot** -välilehti ja valitse sitten **Lisää ulkoisen JARs**.
2. **JAR valinta** -valintaikkunassa tarvittavat PURKKIIN Siirry, valitse se ja valitse sitten **Avaa**.
3. **MyACSHelloWorld ominaisuudet** -valintaikkunaa on edelleen avoinna Valitse **Käyttöönoton kokoonpanon**.
4. **Web-käyttöönoton kokoonpano** -valintaikkunassa valitsemalla **Lisää**.
5. **Uuden kokoonpanon direktiivin** -valintaikkunassa valitse **Java luominen polku tapahtumat** ja valitse sitten **Seuraava**.
6. Valitse haluamasi kirjasto ja valitse **Valmis**.
7. Valitse **OK** ja sulje **MyACSHelloWorld ominaisuudet** -valintaikkunan.

## <a name="modify-the-jsp-file-to-display-saml"></a>Näytä SAML JSP-tiedoston muokkaaminen

Muokkaa **index.jsp** käyttämään seuraava koodi.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {
        
            try
            {
                String nodeName;
                int nChild, i;
                
                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");
                   
                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }
                   
                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                   {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    
    
                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        
                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }
                                            
                                     }
                                     out.println("<br>");
                                     
                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>
    
        <%
        try 
        {
            String data  = (String) request.getAttribute("ACSSAML");
            
            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();
            
            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA); 
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();
            
            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();
    
            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e) 
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }
        
        %>
    </body>
    </html>

## <a name="run-the-application"></a>Suorita sovellus

1. Sovellus on tietokoneen emulaattori tai Azure-ohjeiden mukaan miten [todennusta ja Azure Access ohjausobjektin palvelun käyttämällä Pimennys käyttäjien][]käyttöön.
2. Käynnistä selain ja avaa web-sovelluksen. Kun olet kirjautunut sisään sovelluksen, näkyviin tulee SAML tiedot, mukaan lukien tunnistetietojen toimittaja myöntämä suojauksen vahvistus.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisää Tutustu ACS's toiminnot ja kokeilla kehittyneempiä tilanteita, joissa on artikkelissa [Access ohjausobjektin palvelun 2.0][].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Access Control-palvelu 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Miten verkko-käyttäjät, joilla on Azure Access Control-palvelu Pimennys tarkistamiseen]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 