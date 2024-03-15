![110445358](https://github.com/RClueX/Mobile-App-Security-Testing/assets/110445358/42d0bb5f-00e8-4c96-86c3-6def922972c2)

---

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


![110445358](https://github.com/RClueX/Mobile-App-Security-Testing/assets/110445358/6972b11f-6291-4fb7-84c5-f53a893c540e)





> Install Python for windows from here.
> https://www.python.org/downloads/windows/

---


**We need to install some python packages for frida server. For this enter following command in terminal:**


```
python -m pip install Frida
python -m pip install objection
python -m pip install frida-tools
or
pip install Frida
pip install objection
pip install frida-tools
```


```
adb connect IP
```


[ For checking system configuration i.e : x86 or 64 bit ]

>adb shell getprop ro.product.cpu.abi

Download Frida server -

> https://github.com/frida/frida/releases   

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
Check Package name - > frida-ps -Ua
```

```
frida -U -f (Target Application) -l rootbypass.js or sslbypassscript.js
```
