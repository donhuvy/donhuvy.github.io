---
layout: post
title:  "Java socket programming"
date:   2020-09-30 16:41:00 +0700
categories: java programming
---
Java socket programming.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.example</groupId>
    <artifactId>socket</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>socket</name>
    <!-- FIXME change it to the project's website -->
    <url>http://www.example.com</url>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.7</maven.compiler.source>
        <maven.compiler.target>1.7</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
            <plugins>
                <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
                <plugin>
                    <artifactId>maven-clean-plugin</artifactId>
                    <version>3.1.0</version>
                </plugin>
                <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
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
                <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
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
{% endhighlight %}


{% highlight java %}
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class ServerProgram {

    public static void main(String args[]) throws IOException {
        ServerSocket listener = null;
        System.out.println("Server is waiting to accept user...");
        int clientNumber = 0;
        // Mở một ServerSocket tại cổng 7777.
        // Chú ý bạn không thể chọn cổng nhỏ hơn 1023 nếu không là người dùng
        // đặc quyền (privileged users (root)).
        try {
            listener = new ServerSocket(7777);
        } catch (IOException e) {
            System.out.println(e);
            System.exit(1);
        }
        try {
            while (true) {
                // Chấp nhận một yêu cầu kết nối từ phía Client.
                // Đồng thời nhận được một đối tượng Socket tại server.
                Socket socketOfServer = listener.accept();
                new ServiceThread(socketOfServer, clientNumber++).start();
            }
        } finally {
            listener.close();
        }
    }

    private static void log(String message) {
        System.out.println(message);
    }

    private static class ServiceThread extends Thread {
        private int clientNumber;
        private Socket socketOfServer;

        public ServiceThread(Socket socketOfServer, int clientNumber) {
            this.clientNumber = clientNumber;
            this.socketOfServer = socketOfServer;
            // Log
            log("New connection with client# " + this.clientNumber + " at " + socketOfServer);
        }

        @Override
        public void run() {
            try {
                // Mở luồng vào ra trên Socket tại Server.
                BufferedReader is = new BufferedReader(new InputStreamReader(socketOfServer.getInputStream()));
                BufferedWriter os = new BufferedWriter(new OutputStreamWriter(socketOfServer.getOutputStream()));
                while (true) {
                    // Đọc dữ liệu tới server (Do client gửi tới).
                    String line = is.readLine();
                    // Ghi vào luồng đầu ra của Socket tại Server.
                    // (Nghĩa là gửi tới Client).
                    os.write(">> " + line);
                    // Kết thúc dòng
                    os.newLine();
                    // Đẩy dữ liệu đi
                    os.flush();
                    // Nếu người dùng gửi tới QUIT (Muốn kết thúc trò chuyện).
                    if (line.equals("QUIT")) {
                        os.write(">> OK");
                        os.newLine();
                        os.flush();
                        break;
                    }
                }
            } catch (IOException e) {
                System.out.println(e);
                e.printStackTrace();
            }
        }
    }

}
{% endhighlight %}

{% highlight java %}
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.Socket;
import java.net.UnknownHostException;
import java.util.Date;

public class ClientDemo {

    public static void main(String[] args) {
        // Địa chỉ máy chủ.
        final String serverHost = "localhost";
        Socket socketOfClient = null;
        BufferedWriter os = null;
        BufferedReader is = null;
        try {
            // Gửi yêu cầu kết nối tới Server đang lắng nghe
            // trên máy 'localhost' cổng 7777.
            socketOfClient = new Socket(serverHost, 7777);
            // Tạo luồng đầu ra tại client (Gửi dữ liệu tới server)
            os = new BufferedWriter(new OutputStreamWriter(socketOfClient.getOutputStream()));
            // Luồng đầu vào tại Client (Nhận dữ liệu từ server).
            is = new BufferedReader(new InputStreamReader(socketOfClient.getInputStream()));
        } catch (UnknownHostException e) {
            System.err.println("Don't know about host " + serverHost);
            return;
        } catch (IOException e) {
            System.err.println("Couldn't get I/O for the connection to " + serverHost);
            return;
        }
        try {
            // Ghi dữ liệu vào luồng đầu ra của Socket tại Client.
            os.write("HELO! now is " + new Date());
            os.newLine(); // kết thúc dòng
            os.flush();  // đẩy dữ liệu đi.
            os.write("I am Tom Cat");
            os.newLine();
            os.flush();
            os.write("QUIT");
            os.newLine();
            os.flush();
            // Đọc dữ liệu trả lời từ phía server
            // Bằng cách đọc luồng đầu vào của Socket tại Client.
            String responseLine;
            while ((responseLine = is.readLine()) != null) {
                System.out.println("Server: " + responseLine);
                if (responseLine.indexOf("OK") != -1) {
                    break;
                }
            }
            os.close();
            is.close();
            socketOfClient.close();
        } catch (UnknownHostException e) {
            System.err.println("Trying to connect to unknown host: " + e);
        } catch (IOException e) {
            System.err.println("IOException:  " + e);
        }
    }

}
{% endhighlight %}

{% highlight java %}
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class SimpleServerProgram {

    public static void main(String args[]) {
        ServerSocket listener = null;
        String line;
        BufferedReader is;
        BufferedWriter os;
        Socket socketOfServer = null;
        // Mở một ServerSocket tại cổng 9999.
        // Chú ý bạn không thể chọn cổng nhỏ hơn 1023 nếu không là người dùng
        // đặc quyền (privileged users (root)).
        try {
            listener = new ServerSocket(9999);
        } catch (IOException e) {
            System.out.println(e);
            System.exit(1);
        }
        try {
            System.out.println("Server is waiting to accept user...");
            // Chấp nhận một yêu cầu kết nối từ phía Client.
            // Đồng thời nhận được một đối tượng Socket tại server.
            socketOfServer = listener.accept();
            System.out.println("Accept a client!");
            // Mở luồng vào ra trên Socket tại Server.
            is = new BufferedReader(new InputStreamReader(socketOfServer.getInputStream()));
            os = new BufferedWriter(new OutputStreamWriter(socketOfServer.getOutputStream()));
            // Nhận được dữ liệu từ người dùng và gửi lại trả lời.
            while (true) {
                // Đọc dữ liệu tới server (Do client gửi tới).
                line = is.readLine();
                // Ghi vào luồng đầu ra của Socket tại Server.
                // (Nghĩa là gửi tới Client).
                os.write(">> " + line);
                // Kết thúc dòng
                os.newLine();
                // Đẩy dữ liệu đi
                os.flush();
                // Nếu người dùng gửi tới QUIT (Muốn kết thúc trò chuyện).
                if (line.equals("QUIT")) {
                    os.write(">> OK");
                    os.newLine();
                    os.flush();
                    break;
                }
            }
        } catch (IOException e) {
            System.out.println(e);
            e.printStackTrace();
        }
        System.out.println("Sever stopped!");
    }

}
{% endhighlight %}

{% highlight java %}
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.Socket;
import java.net.UnknownHostException;

public class SimpleClientDemo {

    public static void main(String[] args) {
        // Địa chỉ máy chủ.
        final String serverHost = "localhost";
        Socket socketOfClient = null;
        BufferedWriter os = null;
        BufferedReader is = null;
        try {
            // Gửi yêu cầu kết nối tới Server đang lắng nghe
            // trên máy 'localhost' cổng 9999.
            socketOfClient = new Socket(serverHost, 9999);
            // Tạo luồng đầu ra tại client (Gửi dữ liệu tới server)
            os = new BufferedWriter(new OutputStreamWriter(socketOfClient.getOutputStream()));
            // Luồng đầu vào tại Client (Nhận dữ liệu từ server).
            is = new BufferedReader(new InputStreamReader(socketOfClient.getInputStream()));
        } catch (UnknownHostException e) {
            System.err.println("Don't know about host " + serverHost);
            return;
        } catch (IOException e) {
            System.err.println("Couldn't get I/O for the connection to " + serverHost);
            return;
        }
        try {
            // Ghi dữ liệu vào luồng đầu ra của Socket tại Client.
            os.write("HELO");
            os.newLine(); // kết thúc dòng
            os.flush();  // đẩy dữ liệu đi.
            os.write("I am Tom Cat");
            os.newLine();
            os.flush();
            os.write("QUIT");
            os.newLine();
            os.flush();
            // Đọc dữ liệu trả lời từ phía server
            // Bằng cách đọc luồng đầu vào của Socket tại Client.
            String responseLine;
            while ((responseLine = is.readLine()) != null) {
                System.out.println("Server: " + responseLine);
                if (responseLine.indexOf("OK") != -1) {
                    break;
                }
            }
            os.close();
            is.close();
            socketOfClient.close();
        } catch (UnknownHostException e) {
            System.err.println("Trying to connect to unknown host: " + e);
        } catch (IOException e) {
            System.err.println("IOException:  " + e);
        }
    }

}
{% endhighlight %}