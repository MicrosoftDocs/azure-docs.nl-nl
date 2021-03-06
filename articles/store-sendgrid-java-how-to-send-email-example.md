---
title: Store-sendgrid-Java-How-to-Send-e-mail-voor beeld
description: E-mail verzenden met SendGrid van java in een Azure-implementatie. De resulterende toepassing vraagt de gebruiker om e-mail waarden.
services: ''
documentationcenter: java
author: thinkingserious
manager: sendgrid
editor: mollybos
ms.assetid: af096a73-6985-4350-92e4-ce1722c8d72f
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/30/2014
ms.author: erikre
ms.reviewer: vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com
ms.custom: devx-track-java
ms.openlocfilehash: 7a857b887b53ccb2288507742ca52d72d90ff548
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2021
ms.locfileid: "102214351"
---
# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>E-mail verzenden met SendGrid vanuit java in een Azure-implementatie
In het volgende voor beeld ziet u hoe u SendGrid kunt gebruiken om e-mail berichten te verzenden van een webpagina die wordt gehost in Azure. De resulterende toepassing vraagt de gebruiker om e-mail waarden, zoals in de volgende scherm afbeelding wordt weer gegeven.

![E-mail formulier][emailform]

De resulterende e-mail ziet er ongeveer uit als in de volgende scherm afbeelding.

![E-mail bericht][emailsent]

U moet de volgende stappen uitvoeren om de code in dit onderwerp te gebruiken:

1. Verkrijg de javax. mail potten, bijvoorbeeld van <https://www.oracle.com/technetwork/java/javamail/index.html> .
2. Voeg de potten aan uw Java-build-pad toe.
3. Als u een eclips gebruikt om deze Java-toepassing te maken, kunt u de SendGrid-bibliotheken in uw toepassings implementatie bestand (WAR) met behulp van de implementatie assemblage functie van eclips toevoegen. Als u geen gebruik maakt van eclips om deze Java-toepassing te maken, moet u ervoor zorgen dat de bibliotheken zijn opgenomen in dezelfde Azure-rol als uw Java-toepassing en worden toegevoegd aan het pad naar de klasse van uw toepassing.

U moet ook uw eigen SendGrid-gebruikers naam en-wacht woord hebben om het e-mail bericht te kunnen verzenden. Zie [e-mail verzenden met SendGrid vanuit Java](store-sendgrid-java-how-to-send-email.md)om aan de slag te gaan met SendGrid.

Daarnaast is het raadzaam om de informatie over het [maken van een Hallo wereld-toepassing voor Azure in een eclips](/azure/developer/java/toolkit-for-eclipse/create-hello-world-web-app)of met andere technieken voor het hosten van Java-toepassingen in azure te gebruiken als u geen gebruik maakt van eclips.

## <a name="create-a-web-form-for-sending-email"></a>Een webformulier voor het verzenden van e-mail maken
De volgende code laat zien hoe u een webformulier maakt om gebruikers gegevens op te halen voor het verzenden van e-mail. Voor doel einden van deze inhoud heeft het JSP-bestand de naam **emailform.jsp**.

```html
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "https://www.w3.org/TR/html4/loose.dtd">
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
```

## <a name="create-the-code-to-send-the-email"></a>De code maken om het e-mail bericht te verzenden
Met de volgende code, die wordt aangeroepen wanneer u het formulier in emailform.jsp voltooit, wordt het e-mail bericht gemaakt en verzonden. Voor doel einden van deze inhoud heeft het JSP-bestand de naam **sendemail.jsp**.

```java
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "https://www.w3.org/TR/html4/loose.dtd">
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
```

Naast het verzenden van het e-mail bericht levert emailform.jsp een resultaat voor de gebruiker. een voor beeld is de volgende scherm afbeelding:

![Resultaat van e-mail verzenden][emailresult]

## <a name="next-steps"></a>Volgende stappen
Implementeer uw toepassing in de compute-emulator en voer de volgende handelingen uit in een browser emailform.jsp, geef waarden op in het formulier, klik op **Dit e-mail bericht verzenden** en Bekijk de resultaten in sendemail.jsp.

Deze code is opgenomen om u te laten zien hoe u SendGrid in Java kunt gebruiken in Azure. Voordat u naar Azure implementeert in productie, wilt u mogelijk meer fout afhandeling of andere functies toevoegen. Bijvoorbeeld: 

* U kunt Azure Storage-blobs of SQL Database gebruiken om e-mail adressen en e-mail berichten op te slaan in plaats van een webformulier te gebruiken. Zie [How to use the Blob Storage service from Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/)(Engelstalig) voor meer informatie over het gebruik van Azure Storage-blobs in Java. Zie [using SQL database in Java](./azure-sql/database/connect-query-java.md)(Engelstalig) voor meer informatie over het gebruik van SQL database in Java.
* Zie [e-mail verzenden met SendGrid vanuit Java](store-sendgrid-java-how-to-send-email.md)voor meer informatie over het gebruik van SendGrid in Java.

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg