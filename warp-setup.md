# SETUP Cloudflare VPN ON LINUX - Wireguard, Warp, Warp+
> Lasted guide (5/13/2021)

- [Install sevice](#Install)
  - [WireGuard](#WireGuard)
  - [WGCF](#WGCF)
  - [1.1.1.1](#1.1.1.1)
- [Config](#Config)
  - [Purchase Warp+ & Get license](#Purchase-Warp+-&-Get-license)
  - [Registry](#Registry)
- [Run & Verify](#Run-&-Verify)

## Install
### WireGuard
**Ubuntu:**

```
$ Sudo apt install wireguard
```
More platform [there](https://www.wireguard.com/install/)

### WGCF

```
cd /usr/local/bin
sudo curl -LJ -o wgcf https://github.com/ViRb3/wgcf/releases/download/v2.2.3/wgcf_2.2.3_linux_amd64
sudo chmod +x wgcf
```
:grey_exclamation: Check lasted version and LBS platform [there](https://github.com/ViRb3/wgcf/releases)

### 1.1.1.1
Download `1.1.1.1` on App Store or CH Play.

## Config

### Purchase Warp+ & Get license
> U can skip this if don't use warp+.

**Step 1:** Purchase Warp+ price 1$

**Step 2:** `1.1.1.1` \> `Setting` \> `Account` \> `Key` - save this ID.

### Registry
Run command:
```
cd /etc/wireguard
wgcf register
```

If success output can same like this:
```
2020/06/01 19:03:16 Device name: D86E69
2020/06/01 19:03:16 Device model: PC
2020/06/01 19:03:16 Device active: true
2020/06/01 19:03:16 Account type: free
2020/06/01 19:03:16 Warp+ enabled: false
2020/06/01 19:03:16 Premium data: 0
2020/06/01 19:03:16 Quota: 0
```

:no_entry: This step for config warp+ .
```
WGCF_LICENSE_KEY="ID-LICENSE" wgcf update
```
output same like this:
```
2020/06/01 19:03:16 Warp+ enabled: false
```

Next, run command:
```
wgcf generate
```
Output will be same like this:
```
2020/06/01 19:04:06 Account type: limited
2020/06/01 19:04:06 Warp+ enabled: true
2020/06/01 19:04:06 Premium data: 1623716000000000
2020/06/01 19:04:06 Quota: 1623613938856622
```

**All step created 2 files:

`wgcf-account.toml`

`wgcf-profile.conf`


## Run & Verify

Run command:
```
wg-quick up wgcf-profile
```
Verify:
```
wg show
```

![Screenshot from 2021-05-13 16-27-01](https://user-images.githubusercontent.com/82546097/118106699-15002080-b408-11eb-810c-f0ca3fbae4b1.png)

### Check device status & Verify Warp/ Warp+
```
wgcf status
wgcf trace
```
If you look at the last line, it should say `warp=on` or `warp=plus`, depending on whether you have Warp or Warp+ respectively.


## :postbox: Contact me
Fb: /giangsonahh
Gmail: Doanhoang277@gmail.com

## Reference
https://github.com/ViRb3/wgcf
