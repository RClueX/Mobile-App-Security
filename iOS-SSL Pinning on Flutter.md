- Flutter application is proxy unware and we cannot redirect the traffic through WIFI proxy. In android, we can do it by ProxyDroid but, in iOS we need to do it through VPN.

```
sudo wget https://git.io/vpn -O openvpn-install.sh
```
```
sudo sed -i "$(($(grep -ni "debian is too old" openvpn-install.sh | cut  -d : -f 1)+1))d" ./openvpn-install.sh
```
```
sudo chmod +x openvpn-install.sh
```
```
sudo ./openvpn-install.sh
```

Enter below details after running the script
