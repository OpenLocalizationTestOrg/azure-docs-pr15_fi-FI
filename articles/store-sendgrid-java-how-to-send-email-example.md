<properties 
    pageTitle="Store-sendgrid-Java-How-to-send-email-example" 
    description="Azure-ympäristön SendGrid Java-sähköpostin lähettäminen" 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com"/>

# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Miten sähköpostin lähettäminen käyttämällä SendGrid Java-Azure käyttöönotto

Seuraavassa esimerkissä kerrotaan, miten SendGrid avulla voit lähettää sähköpostiviestejä verkkosivulta ylläpidettävä Azure. Tuloksena oleva sovellus kysyy käyttäjältä sähköpostin arvolla, koodin seuraavassa kuvassa esitetyllä tavalla.

![Sähköposti-lomake][emailform]

Tuloksena olevat sähköpostin näyttävät seuraavista näyttökuvan.

![Sähköpostiviesti][emailsent]

Sinun on käytettävä koodi tämän ohjeaiheen seuraavasti:

1. Hanki javax.mail tölkki, kuten <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. Lisää tölkki Java-muodosta polku.
3. Jos käytät Pimennys Java-sovelluksen luominen, voit lisätä SendGrid kirjastojen Pimennys 's käyttöönoton kokoonpanon ominaisuudella sovelluksen käyttöönotto tiedoston (SODAN). Jos et käytä Pimennys Java-sovelluksen luominen, varmista kirjastot on sisällyttävä sama Azure rooli Java-sovellus ja sovelluksesi luokkaa polkuun.


Sinulla on myös omat SendGrid käyttäjänimi ja salasana, voivat lähettää sähköpostiviestin. Aloita SendGrid, katso, [miten voit sähköpostin lähettäminen käyttämällä Java-SendGrid](store-sendgrid-java-how-to-send-email.md).

Lisäksi kannattaa tietojen [luodaan Hei maailma sovellusta varten Pimennys Azure](http://msdn.microsoft.com/library/windowsazure/hh690944)tai muita menetelmiä isännöimiseen Java-sovellusten Azure-tietokannassa, jos et käytä Pimennys, on erittäin suositeltavaa.

## <a name="create-a-web-form-for-sending-email"></a>Sähköpostin lähettäminen web-lomakkeen luominen

Seuraava koodi näkyy hakemaan käyttäjätiedot sähköpostin lähettämisestä Internet-lomakkeen luominen. Sisältötyypin tarkoituksiin JSP-tiedoston nimeksi **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-send-the-email"></a>Lähetä sähköpostiviesti koodin luominen

Seuraava koodi, jota kutsutaan, kun täytät lomakkeen emailform.jsp, Luo sähköpostiviesti ja lähettää sen. Sisältötyypin tarkoituksiin JSP-tiedoston nimeksi **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%
     
     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");
     
     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;
          
            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {
         
         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";
    
         Properties properties;
        
         properties = new Properties();
         
         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");
         
         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");
    
         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();
         
         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);
         
         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);
    
         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 
         
         message = new MimeMessage(mailSession);
         
         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            
    
         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);
         
         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");
    
         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");
         
         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();
     
        out.println("<p>Email processing completed.</p>");
         
     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>
    
    </body>
    </html>

Lisäksi lähettäminen sähköpostin emailform.jsp tarjoaa tuloksen käyttäjän; koodin seuraavassa kuvassa on esimerkki:

![Lähetä sähköposti tulos][emailresult]

## <a name="next-steps"></a>Seuraavat vaiheet

Laske emulaattori sovellusta käyttöönotto ja selaimessa Suorita emailform.jsp, kirjoittaa arvoja lomakkeen, napsauttamalla **tätä sähköpostin lähettäminen**ja nähdä sendemail.jsp tulokset.

Kuinka pystyt käyttämään SendGrid Java-Azure antamien koodi. Ennen kuin otat käyttöön Azure tuotannon, haluat ehkä lisätä Lisää Virheenkäsittely tai muita ominaisuuksia. Esimerkki: 

* Voit käyttää tallennustilan Azure-BLOB- tai SQL-tietokanta voidaan tallentaa sähköpostiosoitteet ja sähköpostiviestien verkkolomakkeen sijaan. Saat lisätietoja Java-tallennustilan Azure-BLOB- [ohjeet Java-Blob-objektien tallennustila-palvelun käyttöä varten](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). Katso lisätietoja SQL-tietokannan Java- [SQL-tietokannan käyttäminen Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* Voit käyttää `RoleEnvironment.getConfigurationSettings` noutamiseen koko käyttöönotto asetusten sijaan web-lomakkeen avulla voit hakea kyseiset arvot SendGrid käyttäjänimi ja salasana. Lisätietoja `RoleEnvironment` luokka on artikkelissa [Azure palvelun suorituksenaikainen kirjastossa JSP avulla](http://msdn.microsoft.com/library/windowsazure/hh690948) ja Azure-palvelun suorituksenaikainen paketin documentation <http://dl.windowsazure.com/javadoc>.
* Tutustu lisätietoja SendGrid Java- [Sähköpostin lähettäminen käyttämällä Java-SendGrid](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
