## Android-Application-Platform-Findings
----

| Findings | Description |Recommendations    |
| -------- | -------- | -------- |
| Database Stored in Android Device without Encryption | If an attacker tries to gain access to a victim's sensitive information, any encryption flaws will be immediately apparent. By connecting the mobile device directly to a computer and utilizing readily available software, an attacker will be able to obtain it. As a result, attackers have access to all third-party application directories containing sensitive information. By altering a genuine application or using malware, the attacker can easily steal this information. When an organization is attacked, it becomes vulnerable to identity theft and adverse publicity.The risk of this vulnerability is that it can result in data loss, in the best case for one user, and the worst case for many users. It may also result in the following technical impacts: extraction of the app’s sensitive information via mobile malware, modified apps, or forensic tools.  | 1. Developers can make use of SQLCiphers to encrypt the local database file. <br> 2. If encrypted SQLite databases are used, determine whether the password is hard-coded in the source, stored in shared preferences, or hidden somewhere else in the code or filesystem. <br> 3. Firebase Real-time Databases can be used. <br> 4. Realm Databases can be used. <br>Please refer to following link: https://mobile-security.gitbook.io/mobile-security-testing-guide/android-testing-guide/0x05d-testing-data-storage#firebase-real-time-databases | 
| Insecure Data Storage | Insecure data storage vulnerabilities occur when development teams assume that users or malware will not have access to a mobile device’s filesystem and subsequent sensitive information in data-stores on the device. Filesystems are easily accessible, Organizations should expect a malicious user or malware to inspect sensitive data stores. | 1. For local storage the enterprise android device administration API can be used to force encryption to local file-stores using “setStorageEncryption”.<br> 2. For SD Card Storage some security can be achieved via the ‘javax.crypto’ library. You have a few options, but an easy one is simply to encrypt any plain text data with a master password and AES 128.<br> 3. Ensure any shared preferences properties are NOT MODEWORLDREADABLE unless explicitly required for information sharing between apps. <br> 4. Consider providing an additional layer of encryption beyond any default encryption mechanisms provided by the operating system. <br> Please refer the following link: https://owasp.org/www-project-mobile-top-10/2016-risks/m2-insecure-data-storage |
| Root Detection not Implemented | The full system access means that malware could potentially exploit root access to do much more damage than it normally could. Once an app is granted root access, it can do anything – run a key logger in the background without telling you, extract your account information from other apps, or even mess up your device by deleting critical system files. If malicious user gets root access to application , the following things can be done by malicious user : <br>1. Access source code of application and modify it<br>2. Access the entire database of application and modify it.<br>3. Attackers can read and misuse the cookies and cache values. | 1. Perform root detection in application code.<br>2. Type “su” without the quotes. If the device has been rooted , the next line will display “#” signifying that root access and device is ready for super user commands. If it is not been rooted then terminal will not recognize “su” command.<br>3. Check if write permissions are there on directories. |
| Application Debuggable is Enabled | The android:debuggable attribute sets whether the application is debuggable. It is set for the application as a whole and can not be overridden by individual components. The attribute is set to false by default. |Android:debuggable flag attribute should always be set to "false" in AndroidManifest.xml of the application   |
| Application Data Backup is Enabled | This allows anyone with access to the device to make a copy of all of the application’s data. An example of when this could be dangerous is if an adversary with device access is able to download un unencrypted database. |In AndroidManifest.xml file under application tag, Change [android: allowBackup=true] from 'true' to 'false'.  |
| Cleartext Traffic is Enabled |Allowing cleartext network communications in an Android app means that anyone monitoring network traffic can see and manipulate the data that is being transmitted. This is a vulnerability if the transmitted data includes sensitive information such as passwords, credit card numbers, or other personal information. Regardless of if you are sending sensitive information or not, using cleartext can still be a vulnerability as cleartext / plaintext HTTP traffic can also be manipulated through network poisoning attacks such as ARP or DNS poisoning, thus potentially enabling attackers to influence the behavior of an app. |In AndroidManifest.xml file under application tag, Change [android:usesCleartextTraffic=true] from 'true' to 'false'. |
| Application Exported is enabled | The android:exported attribute sets whether a component (activity, service, broadcast receiver, etc.) can be launched by components of other applications:<br>If true, any app can access the activity and launch it by its exact class name.<br>If false, only components of the same application, applications with the same user ID, or privileged system components can launch the activity.<br>The logic behind the default value of this attribute changed over time and was different depending on the component types and Android versions. For example, on API level 16 (Android 4.1.1) or lower the value for <provider> elements is set to true by default. Not setting this attribute explicitly carries the risk of having different default values between some devices. |In AndroidManifest.xml file under application tag, Change [android: exported=true] from 'true' to 'false'|
| Insecure logging and Unintended Data Leakage | Insecure logging and unintended data leakage in Android apps can lead to privacy breaches and expose sensitive user information, Sensitive data such as User credentials, Secret key, MFA secret key and session id are getting logged in clear text in logcat. |1. Avoid READ_LOGS permission to the malicious user in the application.<br>2. If logs are sent over a network then ensure that they are sent over an SSL channel.<br>Please refer the following link: https://owasp.org/www-project-mobile-top-10/2014-risks/m4-unintended-data-leakage   |
| Successful Reverse Engineering | An attacker will typically download the targeted app from an app store and analyse it within their local environment using a suite of different tools.An attacker can perform an analysis of the final core binary to determine its original string table, source code, libraries, algorithms, and resources embedded within the app |1. Narrow down what methods / code segments to obfuscate.<br>2. Tune the degree of obfuscation to balance performance impact.<br>3. Withstand de-obfuscation from tools like Pro-Guard, IDA Pro and Hopper.<br>4. Obfuscate string tables as well as methods.   |







## Finding : Application Data Backup is Enabled
  - Decompile and open the [`AndroidManifest.xml`](https://developer.android.com/guide/topics/manifest/manifest-intro) file and Find this flag: [`android:allowBackup="true"`](https://developer.android.com/guide/topics/data/autobackup)
  - It must be false, If the value is set to `true` then it could be a Finding.
  - After executing all available app functions, attempt to back up via adb. If the backup is successful, inspect the backup archive for sensitive data. Open a terminal and run the following command:
  ```
  adb backup -apk -nosystem <package-name>
                  or
  adb backup "-apk -nosystem <package-name>"
  ```
  - ADB should respond now with "Now unlock your device and confirm the backup operation" and you should be asked on the Android phone for a password. Enter random one, For example here is: 123
  - Approve the backup from your device by selecting the Back up my data option. After the backup process is finished, the file .ab (ex: backup.ab) will be in your working directory. 
  - To convert the .ab file to tar. Download the [Android Backup Extractor](https://github.com/nelenkov/android-backup-extractor/releases) is another alternative backup tool. Run the following command to convert the tar file: _(If tool dosen't work, you have to download the Oracle JCE Unlimited Strength Jurisdiction Policy Files for JRE7 or JRE8 and place them in the JRE lib/security folder.)_
  `java -jar abe.jar abe unpack <backup.ab> <backup.tar> [password]`
  - `[password]` is the password when your android device asked you earlier. For example here is: 123
  ```
  java -jar abe.jar unpack backup.ab backup.tar 123
  ```
  - if it shows something like this, which means it has unpacked successfully.
  ```
 Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
Calculated MK checksum (use UTF-8: true): 916423B1691313FF3696A8DDC185E4AB9F557304B8DC70A3B0008D189C0E5161
0% 1% 2% 4% 5% 7% 9% 10% 15% 20% 25% 30% 35% 40% 45% 50% 55% 60% 65% 70% 75% 80% 85% 90% 99% 100% 
2769408 bytes written to backup.tar.
  ```
  - Extract the tar file to your working directory.
  ```
  tar -xvf backup.tar
  ```
  - Check into extracted folder for any files created by the application that contain sensitive data. If found then it's a Finding.

## Finding : Application UsesClearTextTraffic Enabled
  - Decompile and open the [`AndroidManifest.xml`](https://developer.android.com/guide/topics/manifest/manifest-intro) file and Find this flag: [`android:usesCleartextTraffic="true"`](https://developer.android.com/guide/topics/manifest/application-element#usesCleartextTraffic)
  - It must be false, If the value is set to `true` then it’s Finding.
  
## Finding : Application Exported is Enabled
  - Decompile and open the [`AndroidManifest.xml`](https://developer.android.com/guide/topics/manifest/manifest-intro) file and Find this flag: [`android:exported="true"`](https://developer.android.com/topic/security/risks/android-exported)
  - It must be false, If the value is set to `true` then it could be Finding then you need to use drozer to call that specific activity and check if it opens by drozer or not.
  - To check Use `Drozer` run this commands. (Drozer Agent app must be installed and ON in the device)
```
Starting a session:
$ adb forward tcp:31415 tcp:31415
$ drozer console connect                    : If device connected with USB
$ drozer console connect --server <IP>      : If device cnnected with Wifi_ADB

Retrieving package information:
dz> run app.package.list -f <app name>
dz> run app.package.info -a <package name>

Identifying the attack surface:
dz> run app.package.attacksurface <package name>

Exploiting Activities:
dz> run app.activity.info -a <package name> -u
dz> run app.activity.start --component <package name> <component name>
dz> run app.activity.start --component <package name> <component name> --extra <type> <key> <value>

Exploiting Content Provider (OPTIONAL):
dz> run app.provider.info -a <package name>
dz> run scanner.provider.finduris -a <package name>
dz> run app.provider.query <uri>
dz> run app.provider.update <uri> --selection <conditions> <selection arg> <column> <data>
dz> run scanner.provider.sqltables -a <package name>
dz> run scanner.provider.injection -a <package name>
dz> run scanner.provider.traversal -a <package name>

Exploiting Broadcast Receivers (OPTIONAL):
dz> run app.broadcast.info -a <package name>
dz> run app.broadcast.send --component <package name> <component name> --extra <type> <key> <value>
dz> run app.broadcast.sniff --action <action>

Exploiting Service (OPTIONAL):
dz> run app.service.info -a <package name>
dz> run app.service.start --action <action> --component <package name> <component name>
dz> run app.service.send <package name> <component name> --msg <what> <arg1> <arg2> --extra <type> <key> <value> --bundle-as-obj




Side channel attack
Tapjacking
Android biomentric bypass
IOS biomatric bypass

