

**Drozer Shortcut**
---
`
adb connect IP
`

`
adb shell am start -n com.absli/.ui.view.base.SplashActivity
`

`
am broadcast -a theBroadcast -n com.android.insecurebankv2/com.android.insecurebankv2.MyBroadCastReceiver –es phonenumber 5554 –es newpass Dinesh@123!
`

`
 Adb backup –apk –shared (apk name)
 `

 `
Cat backup.ab zlib-flate -uncompress > backup_compressed.tar
`


---
---


**Root Detection & SSL Pinning Bypass**
---

`
adb connect IP
`

```
Check Package name - > frida-ps -Ua
```

[ For checking system configuration i.e : x86 or 64 bit ]

>adb shell getprop ro.product.cpu.abi        

```
adb push frida-server-16.2.1-android-x86 /data/local/tmp
```

```
adb shell chmod 777 /data/local/tmp/frida-server-16.2.1-android-x86
```

```
adb push rootbypass.js /data/local/tmp         (Optional Step)
```
`For ssl Pinning Bypass -`
> adb push burp.crt /data/local/tmp

> adb push sslbypassscript.js /data/local/tmp 
```
adb shell /data/local/tmp/frida-server-16.2.1-android-x86 &
```


```
frida -U -f (Target Package Name) -l rootbypass.js or sslbypassscript.js
```
