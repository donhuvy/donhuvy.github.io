---
layout: post
title:  "Stanford NLP"
date:   2020-12-22 22:17:00 +0700
categories: Java NLP
---
Stanford NLP

Install

```
cd /d C:\Users\donhuvy\Downloads

mvn install:install-file -Dfile=stanford-corenlp-4.2.0-models.jar -DgroupId=edu.stanford.nlp -DartifactId=english-models -Dversion=4.2.0 -Dpackaging=jar
```



File `pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.example</groupId>
    <artifactId>accounting</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <name>accounting</name>
    <url>http://donhuvy.github.io</url>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>edu.stanford.nlp</groupId>
            <artifactId>stanford-corenlp</artifactId>
            <version>4.2.0</version>
        </dependency>
        <dependency>
            <groupId>edu.stanford.nlp</groupId>
            <artifactId>english-models</artifactId>
            <version>4.2.0</version>
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

File https://nlp.stanford.edu/software/stanford-corenlp-4.2.0-models.jar

Tune: `_JAVA_OPTIONS=-Xmx8g`

![result](/images/2020_12_22_heap.png)

Result

```
"C:\Program Files\Java\jdk1.8.0_271\bin\java.exe" -Xmx8g "-javaagent:D:\Program Files\JetBrains\IntelliJ IDEA 2020.2.3\lib\idea_rt.jar=49893:D:\Program Files\JetBrains\IntelliJ IDEA 2020.2.3\bin" -Dfile.encoding=UTF-8 -classpath "C:\Program Files\Java\jdk1.8.0_271\jre\lib\charsets.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\deploy.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\ext\access-bridge-64.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\ext\cldrdata.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\ext\dnsns.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\ext\jaccess.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\ext\jfxrt.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\ext\localedata.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\ext\nashorn.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\ext\sunec.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\ext\sunjce_provider.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\ext\sunmscapi.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\ext\sunpkcs11.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\ext\zipfs.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\javaws.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\jce.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\jfr.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\jfxswt.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\jsse.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\management-agent.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\plugin.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\resources.jar;C:\Program Files\Java\jdk1.8.0_271\jre\lib\rt.jar;E:\foo\accounting\target\classes;C:\Users\donhuvy\.m2\repository\edu\stanford\nlp\stanford-corenlp\4.2.0\stanford-corenlp-4.2.0.jar;C:\Users\donhuvy\.m2\repository\com\apple\AppleJavaExtensions\1.4\AppleJavaExtensions-1.4.jar;C:\Users\donhuvy\.m2\repository\de\jollyday\jollyday\0.4.9\jollyday-0.4.9.jar;C:\Users\donhuvy\.m2\repository\org\apache\commons\commons-lang3\3.3.1\commons-lang3-3.3.1.jar;C:\Users\donhuvy\.m2\repository\org\apache\lucene\lucene-queryparser\7.5.0\lucene-queryparser-7.5.0.jar;C:\Users\donhuvy\.m2\repository\org\apache\lucene\lucene-queries\7.5.0\lucene-queries-7.5.0.jar;C:\Users\donhuvy\.m2\repository\org\apache\lucene\lucene-sandbox\7.5.0\lucene-sandbox-7.5.0.jar;C:\Users\donhuvy\.m2\repository\org\apache\lucene\lucene-analyzers-common\7.5.0\lucene-analyzers-common-7.5.0.jar;C:\Users\donhuvy\.m2\repository\org\apache\lucene\lucene-core\7.5.0\lucene-core-7.5.0.jar;C:\Users\donhuvy\.m2\repository\javax\servlet\javax.servlet-api\3.0.1\javax.servlet-api-3.0.1.jar;C:\Users\donhuvy\.m2\repository\xom\xom\1.3.2\xom-1.3.2.jar;C:\Users\donhuvy\.m2\repository\xml-apis\xml-apis\1.3.03\xml-apis-1.3.03.jar;C:\Users\donhuvy\.m2\repository\xerces\xercesImpl\2.8.0\xercesImpl-2.8.0.jar;C:\Users\donhuvy\.m2\repository\xalan\xalan\2.7.0\xalan-2.7.0.jar;C:\Users\donhuvy\.m2\repository\joda-time\joda-time\2.10.5\joda-time-2.10.5.jar;C:\Users\donhuvy\.m2\repository\org\ejml\ejml-core\0.39\ejml-core-0.39.jar;C:\Users\donhuvy\.m2\repository\com\google\code\findbugs\jsr305\3.0.2\jsr305-3.0.2.jar;C:\Users\donhuvy\.m2\repository\org\ejml\ejml-ddense\0.39\ejml-ddense-0.39.jar;C:\Users\donhuvy\.m2\repository\org\ejml\ejml-simple\0.39\ejml-simple-0.39.jar;C:\Users\donhuvy\.m2\repository\org\ejml\ejml-fdense\0.39\ejml-fdense-0.39.jar;C:\Users\donhuvy\.m2\repository\org\ejml\ejml-cdense\0.39\ejml-cdense-0.39.jar;C:\Users\donhuvy\.m2\repository\org\ejml\ejml-zdense\0.39\ejml-zdense-0.39.jar;C:\Users\donhuvy\.m2\repository\org\ejml\ejml-dsparse\0.39\ejml-dsparse-0.39.jar;C:\Users\donhuvy\.m2\repository\org\ejml\ejml-fsparse\0.39\ejml-fsparse-0.39.jar;C:\Users\donhuvy\.m2\repository\org\glassfish\javax.json\1.0.4\javax.json-1.0.4.jar;C:\Users\donhuvy\.m2\repository\com\google\protobuf\protobuf-java\3.9.2\protobuf-java-3.9.2.jar;C:\Users\donhuvy\.m2\repository\javax\activation\javax.activation-api\1.2.0\javax.activation-api-1.2.0.jar;C:\Users\donhuvy\.m2\repository\javax\xml\bind\jaxb-api\2.4.0-b180830.0359\jaxb-api-2.4.0-b180830.0359.jar;C:\Users\donhuvy\.m2\repository\com\sun\xml\bind\jaxb-core\2.3.0.1\jaxb-core-2.3.0.1.jar;C:\Users\donhuvy\.m2\repository\com\sun\xml\bind\jaxb-impl\2.4.0-b180830.0438\jaxb-impl-2.4.0-b180830.0438.jar;C:\Users\donhuvy\.m2\repository\edu\stanford\nlp\english-models\4.2.0\english-models-4.2.0.jar;C:\Users\donhuvy\.m2\repository\org\slf4j\slf4j-api\2.0.0-alpha1\slf4j-api-2.0.0-alpha1.jar" org.example.App
Picked up _JAVA_OPTIONS: -Xmx8g
Hello World!
SLF4J: No SLF4J providers were found.
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#noProviders for further details.
---
entities found
	detected entity: 	Joe Smith	PERSON
	detected entity: 	Seattle	CITY
---
tokens and ner tags
(Joe,PERSON) (Smith,PERSON) (is,O) (from,O) (Seattle,CITY) (.,O)

Process finished with exit code 0
```

### VnCoreNLP

https://github.com/vncorenlp/VnCoreNLP

https://raw.githubusercontent.com/vncorenlp/VnCoreNLP/master/VnCoreNLP-1.1.1.jar

```
cd /d C:\Users\donhuvy\Downloads

mvn install:install-file -Dfile=VnCoreNLP-1.1.1.jar -DgroupId=VnCoreNLP -DartifactId=VnCoreNLP -Dversion=1.1.1 -Dpackaging=jar
```

```xml
<dependency>
    <groupId>VnCoreNLP</groupId>
    <artifactId>VnCoreNLP</artifactId>
    <version>1.1.1</version>
</dependency>
```


```java
public static void main(String[] args) throws IOException {

    // "wseg", "pos", "ner", and "parse" refer to as word segmentation, POS tagging, NER and dependency parsing, respectively.
    String[] annotators = {"wseg", "pos", "ner", "parse"};
    VnCoreNLP pipeline = new VnCoreNLP(annotators);

    String str = "Ngày hai mươi hai tháng mười hai năm hai không hai mươi, công ty trách nhiệm hữu hạn Tín Phát chuyển khoản hai mươi triệu thanh toán tiền mua hàng.";

    Annotation annotation = new Annotation(str);
    pipeline.annotate(annotation);

    System.out.println(annotation.toString());
    // 1    Ông                 Nc  O       4   sub
    // 2    Nguyễn_Khắc_Chúc    Np  B-PER   1   nmod
    // 3    đang                R   O       4   adv
    // 4    làm_việc            V   O       0   root
    // ...

    //Write to file
    PrintStream outputPrinter = new PrintStream("output.txt");
    pipeline.printToFile(annotation, outputPrinter);

    // You can also get a single sentence to analyze individually
    Sentence firstSentence = annotation.getSentences().get(0);
    System.out.println(firstSentence.toString());
}
```