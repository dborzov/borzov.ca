+++
date = "2016-03-04T08:36:54-07:00"
title = "Fixing jmeter's java.security.cert.CertificateException error"
+++

# Fixing jmeter's java.security.cert.CertificateException error

We were using [jmeter](http://jmeter.apache.org/) at work to test an external service. I installed `jmeter` on macOS with `brew` but when I tryed running the tests I had them failing with this the error message:

```
java.security.cert.CertificateException: Certificates does not conform to algorithm constraints
```

The sucky thing was that it was failing only on my machine, it was working fine for my teammates. The tested service was obviously up and it looked was that the issue was with some versioning divergence for some shared code or service. So I had to dig a little into what was up with that.

Here is what turned out to have happened. `jmeter` uses the JDK library to make the request. Since **Java 7 u16** (see the [release notes](http://www.oracle.com/technetwork/java/javase/6u17-141447.html)) the default support for some of the TLS protocols was turned off. The library just raises an exception when the library call is made to connect to endpoints that use it, ignoring the request.

So how to address this? The ideal solution is to use more secure and proven algorithms for your SSL.
If it is not an option, it is possible to turn off the checker at the java setting file.

Find `{JDK_HOME}/jre/lib/security/java.security` for the JRE installation that jmeter uses and comment the lines assigning the values for:

```

jdk.tls.disabledAlgorithms=SSLv3, RC4, MD5withRSA, DH keySize < 768
jdk.certpath.disabledAlgorithms=MD2, MD5, RSA keySize < 1024

```

# Where is the JDK that jmeter uses?

Apparently macOSX has several `JDK` distrubutions installed by different libraries and tools. I ran a global search for `java.security` file and found several  `{JDK_HOME}` installs. Which one does our jmeter use?

It turns out that in my case the file was at
```
 /Library/Java/JavaVirtualMachines/jdk1.8.0_74.jdk/Contents/Home/jre/lib/security/java.security
 ```

If that did not help you, you can just fix all the `java.security` files by finding them with something
like

```
 sudo find / -iname "java.security"
 ```

And that worked for me.

[Let me know](mailto:tihoutrom@gmail.com) if that helped you or not.
