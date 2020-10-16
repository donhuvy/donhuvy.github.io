---
layout: post
title:  "Send email in Java"
date:   2020-10-15 15:38:00 +0700
categories: Java JavaEE
---
Send email in Java

File `pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.example</groupId>
	<artifactId>sendmail</artifactId>
	<version>1.0.0-SNAPSHOT</version>
	<name>sendmail</name>
	<url>https://donhuvy.github.io</url>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
	</properties>
	<dependencies>
		<dependency>
			<groupId>com.sun.mail</groupId>
			<artifactId>jakarta.mail</artifactId>
			<version>1.6.5</version>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
		</dependency>
	</dependencies>
	<build>
		<pluginManagement>
			<plugins>
				<plugin>
					<artifactId>maven-clean-plugin</artifactId>
					<version>3.1.0</version>
				</plugin>
				<plugin>
					<artifactId>maven-resources-plugin</artifactId>
					<version>3.0.2</version>
				</plugin>
				<plugin>
					<artifactId>maven-compiler-plugin</artifactId>
					<version>3.8.0</version>
				</plugin>
				<plugin>
					<artifactId>maven-surefire-plugin</artifactId>
					<version>2.22.1</version>
				</plugin>
				<plugin>
					<artifactId>maven-jar-plugin</artifactId>
					<version>3.0.2</version>
				</plugin>
				<plugin>
					<artifactId>maven-install-plugin</artifactId>
					<version>2.5.2</version>
				</plugin>
				<plugin>
					<artifactId>maven-deploy-plugin</artifactId>
					<version>2.8.2</version>
				</plugin>
				<plugin>
					<artifactId>maven-site-plugin</artifactId>
					<version>3.7.1</version>
				</plugin>
				<plugin>
					<artifactId>maven-project-info-reports-plugin</artifactId>
					<version>3.0.0</version>
				</plugin>
			</plugins>
		</pluginManagement>
	</build>
</project>
```
Must enable Gmail less secure app: [https://myaccount.google.com/lesssecureapps](https://myaccount.google.com/lesssecureapps)


```java
package com.example.sendmail;

import java.util.Properties;

import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.AddressException;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

public class App {

	public static void main(String[] args) throws AddressException, MessagingException {
		Properties properties = new Properties();
		properties.put("mail.smtp.auth", "true");
		properties.put("mail.smtp.starttls.enable", "true");
		properties.put("mail.smtp.host", "smtp.gmail.com");
		properties.put("mail.smtp.port", 587);
		Session session = Session.getInstance(properties, new javax.mail.Authenticator() {
			protected PasswordAuthentication getPasswordAuthentication() {
				return new PasswordAuthentication("donhuvy2014@gmail.com", "hoaxxxxxduxngxxxx");
			}
		});
		Message message = new MimeMessage(session);
		message.setFrom(new InternetAddress("donhuvy2014@gmail.com"));
		message.setRecipients(Message.RecipientType.TO, InternetAddress.parse("vydn@mpsolutions.io"));
		message.setSubject("tieu de email");
		message.setText("Dear Vy,");
		Transport.send(message);
	}

}
```

When check email `donhuvy2014@gmail.com` Folder `Sent` , and check email `vydn@mpsolutions.io` folder `Inbox`; you will see email message.

List properties of SMTP: [https://javaee.github.io/javamail/docs/api/com/sun/mail/smtp/package-summary.html](https://javaee.github.io/javamail/docs/api/com/sun/mail/smtp/package-summary.html)

List properties of IMAP: [https://javaee.github.io/javamail/docs/api/com/sun/mail/imap/package-summary.html](https://javaee.github.io/javamail/docs/api/com/sun/mail/imap/package-summary.html)

For get email from inbox server (AWS WorkMail)

```java
package com.example;

import java.util.Properties;

import javax.mail.Session;
import javax.mail.Store;
import javax.mail.URLName;

import com.sun.mail.imap.IMAPSSLStore;

public class TestMail {

	public static void main(String[] args) {
		Store store = null;
		try {
			Properties imapProps = new Properties();
			imapProps.setProperty("mail.imap.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
			imapProps.setProperty("mail.imap.socketFactory.fallback", "false");
			imapProps.setProperty("mail.imap.partialfetch", "false");
			imapProps.put("mail.smtp.connectiontimeout", "180000");
			imapProps.put("mail.smtp.timeout", "180000");
			URLName url = new URLName("IMAP", "imap.mail.us-east-1.awsapps.com", 993, "", "vydn@mpsolutions.io",
					"SecRET");
			Session session = Session.getInstance(imapProps, null);
			store = new IMAPSSLStore(session, url);
			store.connect();
			System.out.println("Thành công!");
		} catch (Exception e) {
			e.printStackTrace();
		}
		// return store;
	}

}
```