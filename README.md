# Wireguard-VPN-server-client-setup : راه اندازی سرور وی پی ان وایرگارد.

###### وی پی ان شخصی با آی پی ثابت
###### برای اینکار بهتره از سرور دبین استفاده کنید، روی باقی توزیع های لینوکسها هم جواب میده ولی خب سازگاری بهتری با دبین داره.
###### درصورتی که درحالت روت هستید نیازی نیست از سودو استفاده کنید.

###### لینک ویدیو :
```
https://youtu.be/Fp0zazxtaIs
```

###### خرید دامنه از نیم چیپ: 
```
https://namecheap.pxf.io/BX7m6W
```
###### خرید دامنه سایت ایرانی: 
```
https://dashboard.azaronline.com/order/?aff=790&p=domain
```
###### خرید سرور از دیجیتال اوشن : 
```
https://m.do.co/c/0fb522deafa4
```

###### خرید سرور از سایت ایرانی : 
```
https://dashboard.azaronline.com/order/?aff=790&p=vps
```

**If you think this project is helpful to you, you may wish to give a** 🌟

**Feel Free To Donation :** ❤️

>TRC20: ```TGTyqv2MH7dZztMvaP5PKuS9Bma8RY5Pk8```

>ETH: ```0x5b5202a54e5ce4fb25f0d886254eeb07bb088614```

#### EU-SERVER

###### Add REP. اضافه کردن ریپازتوری
```
echo 'deb http://ftp.debian.org/debian buster-backports main' | sudo tee /etc/apt/sources.list.d/buster-backports.list
```

###### Update server and Wirguard instalation. آپدیت سرور و نصب وایرکارد
```
sudo apt update
```
```
sudo apt install wireguard
```
```
sudo apt install iptables
```

###### Generate Private and Public keys. ساخت کلید عمومی و خصوصی سرور
```
wg genkey | sudo tee /etc/wireguard/privatekey | wg pubkey | sudo tee /etc/wireguard/publickey
```
###### Find Privarekey. دیدن کلید خصوصی
```
cat privatekey
```

###### Create Server Configuration. ساخت فایل کانفیگ سرور
```
sudo nano /etc/wireguard/wg0.conf
```
###### Paste following to the file. کدها مورد نیاز کانفیگ
```
[Interface]
Address = 10.0.0.1/24
SaveConfig = true
ListenPort = 51820
PrivateKey = SERVER_PRIVATE_KEY
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
```
###### Save the File (in Nano editore use Ctrl+x)
###### اگه کنجکاوید بدونید چی میشه این کدارو ران کنید. 
###### در غیر اینصورت برید به [فوروارد IPV4](https://github.com/pouramin/Wireguard-VPN-server-client-setup/new/main?readme=1#allow-ipv4-forward-%D9%81%D9%88%D8%B1%D9%88%D8%A7%D8%B1%D8%AF-%D8%A2%DB%8C%D9%BE%DB%8C-%D9%88%D8%B1%DA%98%D9%86-4)
###### Start the wg config.
```
sudo wg-quick up wg0
```
###### Output should be:
```
[#] ip link add wg0 type wireguard
[#] wg setconf wg0 /dev/fd/63
[#] ip -4 address add 10.0.0.1/24 dev wg0
[#] ip link set mtu 1420 up dev wg0
[#] iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o ens3 -j MASQUERADE
```
###### See more details:
```
sudo wg show
interface: wg0
public key: +Vpyku+gjVJuXGR/OXXt6cmBKPdc06Qnm3hpRhMBtxs=
private key: (hidden)
listening port: 51820
```

###### See the ip for the vpn.
```
ip a show wg0
```
```
4: wg0: <POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1420 qdisc noqueue state UNKNOWN
group default qlen 1000
link/none
inet 10.0.0.1/24 scope global wg0
valid_lft forever preferred_lft forever
```

###### Allow ipv4 forward. فوروارد آیپی ورژن 4
```
sysctl net.ipv4.ip_forward=1
```

###### Enable to start with system. استارت آپ وایرگارد
```
sudo systemctl enable wg-quick@wg0
```

###### Server publickey. دیدن کلید عمومی سرور
```
cat publickey
```

###### Create client config. ساخت یوزر
```
nano /etc/wireguard/wg1.conf
```

###### Paste the following to the file. کدهای مورد نیاز ساخت یوزر
```
#Change the lines to your keys and ip addr!
[Interface]
PrivateKey = CLIENT_PRIVATE_KEY
Address = 10.0.0.2/24
DNS = 1.1.1.1, 8.8.8.8
[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = SERVER_IP_ADDRESS:51820
AllowedIPs = 0.0.0.0/0
```

###### Save the file and add the client public key with following command. اضافه کردن یوزر به کانفیگ
```
sudo wg set wg0 peer CLIENT_PUBLIC_KEY allowed-ips 10.0.0.2
```
```
Watch wg show
```

###### Connect to the vpn from PC/Phone

###### TO export the config to a qr-code install the following. ساخت کیو آر کد برای یوزر

```
sudo apt install qrencode
cd /etc/wireguard/
```
###### then use the command (You need to be in the same dir as the conf file /etc/wireguard)
```
qrencode -t ansiutf8 < wg1.conf
```

###### Scan the QR code from your phone.


###### Softs. نرم افزارها 


###### WinSCP. نرم افزار دیدن فایلای لینوکس
```
https://soft98.ir/internet/ftp-tools/748-winscp.html
```

###### WireGuard Soft. نرم افزار وایرگارد برای دیوایس های مختلف

###### Win 32. ویندوز 32 بیتی
```
https://www.uplooder.net/files/be3aae21a20551904422ee1a170747ec/wire32.zip.html
```

###### win 64. ویندوز 64 بیتی
```
https://www.uplooder.net/files/6ed0a416f8d537332c710a5664e889dd/wire64.zip.html
```

###### Android. اندروید
```
https://www.uplooder.net/files/8a52533bbf7bec80224b2f8955795273/android.zip.html
```

###### IOS-Mac. آیفون و مک
```
https://www.uplooder.net/files/37a0e4fc0e7d0404595ca31591ddfb66/IOS-Mac.zip.html
```
