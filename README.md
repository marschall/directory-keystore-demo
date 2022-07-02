Directory Keystore Demo
=======================

Demo project for [marschall/directory-keystore](https://github.com/marschall/directory-keystore). The Directory Keystore library allows you to use the system certificates of a Linux distrubution as the truststore for Java applications without any code changes. To make this work you only need four things:

1. The library itself which is a single JAR without any dependencies, it runs on Java 8 and Java 11. It needs to the on the classpath or the modulepath.
1. A file containing the location of the directory where the certificates are located. For example `/etc/ssl/certs/` or `/etc/pki/tls/certs`.
1. A properties file to append the provider to the list of the installed providers.
1. Three additional JVM options . These are:
   1. `java.security.properties` this file is used to add the security provider to the exising providers. Pay attention to use only a single `=`.
   1. `javax.net.ssl.trustStore` this file contains the location of the directory with the certificates. This indirection is necessary due to the way Java loads keystores.
   1. `javax.net.ssl.trustStoreType` this must have the value `directory`

Putting this all togehter your JVM options should something like this:

```
-Djava.security.properties=${basedir}/src/test/resources/additional.java.security \
-Djavax.net.ssl.trustStore=${basedir}/src/test/resources/etcsslcerts \
-Djavax.net.ssl.trustStoreType=directory
```

The code should work with any JVM and has been tested with OpenJ9.

