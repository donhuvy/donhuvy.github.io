---
layout: post
title:  "New features in Java 11"
date:   2020-10-02 21:45:00 +0700
categories: java blog
---
New features in Java 11:

<img src="https://raw.githubusercontent.com/donhuvy/donhuvy.github.io/master/images/java_roadmap.png" alt="Java roadmap" class="inline"/>

[### `var` keyword](#var)

Old way

{% highlight java %}
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.URL;
import java.net.URLConnection;

public class Java10varOld {

    public static void main(String[] args) throws IOException {
        URL mp = new URL("https://donhuvy.github.io/");
        URLConnection connection = mp.openConnection();
        Reader reader = new BufferedReader(new InputStreamReader(
            connection.getInputStream()));
    }

}
{% endhighlight %}

New way

{% highlight java %}
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.URL;

public class Java10varNew {

    public static void main(String[] args) throws IOException {
        var mp = new URL("https://donhuvy.github.io/");
        var connection = mp.openConnection();
        var reader = new BufferedReader(new InputStreamReader(
            connection.getInputStream()));
    }

}
{% endhighlight %}

[### Cryptography - ChaCha20-Poly1305](#chacha20)

{% highlight java %}
import javax.crypto.KeyGenerator;
import javax.crypto.SecretKey;
import java.nio.ByteBuffer;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;

public class TestChaCha {

    // // 96 bits, 12 bytes.
    private static final int NONCE_LEN = 12;
    // 128 bits, 16 bytes.
    private static final int MAC_LEN = 16;                      

    public static void main(String[] args) throws Exception {
        String input = "Java & ChaCha20-Poly1305.";
        ChaCha20Poly1305 cipher = new ChaCha20Poly1305();
        // 256-bit secret key (32 bytes).
        SecretKey key = getKey();
        System.out.println("Input                  : " + input);
        System.out.println("Input             (hex): " + convertBytesToHex(input.getBytes()));
        System.out.println("\n---Encryption---");
        byte[] cText = cipher.encrypt(input.getBytes(), key);   // encrypt
        System.out.println("Key               (hex): " + convertBytesToHex(key.getEncoded()));
        System.out.println("Encrypted         (hex): " + convertBytesToHex(cText));
        System.out.println("\n---Print Mac and Nonce---");
        ByteBuffer bb = ByteBuffer.wrap(cText);
        // This cText contains chacha20 ciphertext + poly1305 MAC + nonce
        // ChaCha20 encrypted the plaintext into a ciphertext of equal length.
        byte[] originalCText = new byte[input.getBytes().length];
        byte[] nonce = new byte[NONCE_LEN];     // 16 bytes , 128 bits
        byte[] mac = new byte[MAC_LEN];         // 12 bytes , 96 bits
        bb.get(originalCText);
        bb.get(mac);
        bb.get(nonce);
        System.out.println("Cipher (original) (hex): " + convertBytesToHex(originalCText));
        System.out.println("MAC               (hex): " + convertBytesToHex(mac));
        System.out.println("Nonce             (hex): " + convertBytesToHex(nonce));
        System.out.println("\n---Decryption---");
        System.out.println("Input             (hex): " + convertBytesToHex(cText));
        byte[] pText = cipher.decrypt(cText, key);              // decrypt
        System.out.println("Key               (hex): " + convertBytesToHex(key.getEncoded()));
        System.out.println("Decrypted         (hex): " + convertBytesToHex(pText));
        System.out.println("Decrypted              : " + new String(pText));
    }

    private static String convertBytesToHex(byte[] bytes) {
        StringBuilder result = new StringBuilder();
        for (byte temp : bytes) {
            result.append(String.format("%02x", temp));
        }
        return result.toString();
    }

    /**
     * A 256-bit secret key (32 bytes).
     *
     * @return
     * @throws NoSuchAlgorithmException
     */
    private static SecretKey getKey() throws NoSuchAlgorithmException {
        KeyGenerator keyGen = KeyGenerator.getInstance("ChaCha20");
        keyGen.init(256, SecureRandom.getInstanceStrong());
        return keyGen.generateKey();
    }

}
{% endhighlight %}


[### Unicode 10](#unicode10)

{% highlight java %}
public class Unicode10App {

	public static void main(String[] args) {
		String bear = "🐻";
		// Nếu dòng trên không hiển thị trên trình soạn thảo,
		// có thể dùng cách
		// String bear = "\ud83d\udc3b";
		int bearCodepoint = bear.codePointAt(bear.offsetByCodePoints(0, 0));
		int something = bearCodepoint + 1;
		char mysteryAnimal[] = { Character.highSurrogate(something), 
            Character.lowSurrogate(something) };
		System.out.println("Vườn thú Thủ lệ không có con này: " + 
            String.valueOf(mysteryAnimal));
	}

}
{% endhighlight %}
