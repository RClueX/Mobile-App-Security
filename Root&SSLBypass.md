

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

```
Check Package name - > frida-ps -Ua
```


>adb shell getprop ro.product.cpu.abi

```
adb push frida-server-16.2.1-android-x86 /data/local/tmp
```

```
adb shell chmod 777 /data/local/tmp/frida-server-16.2.1-android-x86
```

```
adb push rootbypass.js /data/local/tmp
```

```
adb shell /data/local/tmp/frida-server-16.2.1-android-x86 &
```


```
frida -U -f (Target) -l rootbypass.js
```
