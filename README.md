# Dell XPS 13 7390 2 in 1

<img src="https://media.discordapp.net/attachments/1084252068711247965/1137134955575644160/IMG_6096.jpg?width=488&height=651">


<br>

-----------------------------------------


### Specs:

- Intel i7 1065G7 IceLake
- 16 GB 3733 MHz LPDDR4 RAM
- Intel Iris Plus Graphics
- Toshiba 512 GB NVMe SSD
- 13.3" 1920x1200
- Intel AX200 (soldered)
- Realtek ALC289, layout-id 11

<br>

-----------------------------------------


 ### Current Status:

 | **Feature**        | **Status**           
|--------------------|----------------------|
| WiFi               | Working              | 
| Bluetooth          | Working              | 
| Suspend / Sleep    | Working              | 
| Trackpad           | Working              | 
| Graphics Accel.    | Working              | 
| Internal Speakers  | Working              |   
| Keyboard backlight | Working              | 
| Keyboard           | Working              |
| Internal Storage   | Working              | 
| SD Card Reader     | Working              | 
| Headphone Jack     | Untested             | 
| HDMI Audio         | Untested             |
| HDMI Video         | Untested             |                                                                 
| USB Ports          | Working              | 
| Webcam             | Not Working          | 
| Internal Mic.      | Working              | 
| Logout / Lock      | Working              | 
| Shutdown / Restart | Working              | 
| Continuity         | Not Working          |     
| Power management   | Working              |
| Thunderbolt 3      | Disabled in BIOS     |
| Fingerprint Reader | Not working          |

<br>

-----------------------------------------

### CFG Lock

CFG Lock variable is found at 0x43 and can be disabled by putting 0x0 as value.
  
     setup_var CpuSetup 0x43 0x0

<br>

-----------------------------------------


### BIOS Settings

- System Configuration --> SATA Operation --> AHCI
- Security --> Absolute --> Disabled
- Security --> SMM Security Mitigation --> Disabled
- Security --> Intel SGX --> Disabled/Software Control
- Secure Boot --> Enable Secure Boot --> Toggle OFF
- Secure Boot --> Secure Boot Mode --> Deployed Mode
- POST Behavior --> Sign of Life --> Display Logo Sign of Life --> Toggle OFF

-----------------------------------------


### Enabling S3 Sleep

**warning:** This step is crucial. Your system will not sleep if you do not do this.

1. Compile this SSDT with iASL or equivalant:

```
    DefinitionBlock ("", "SSDT", 2, "meghn6", "S3Fix", 0x00001000)

    {

    External (_SB_.SS3, IntObj)
    
    Scope (\_SB)

    {

    	Name (SS3, One)
     

	}
    
 
}
  
```

2. Rename S0 to XS0: (Goes into ACPI --> Patch) 
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<array>
	<dict>
		<key>Base</key>
		<string></string>
		<key>BaseSkip</key>
		<integer>0</integer>
		<key>Comment</key>
		<string>_S0 to XS0 rename; Fixes sleep</string>
		<key>Count</key>
		<integer>0</integer>
		<key>Enabled</key>
		<true/>
		<key>Find</key>
		<data>WFMw</data>
		<key>Limit</key>
		<integer>0</integer>
		<key>Mask</key>
		<data></data>
		<key>OemTableId</key>
		<data>AAAAAA==</data>
		<key>Replace</key>
		<data>WFMw</data>
		<key>ReplaceMask</key>
		<data></data>
		<key>Skip</key>
		<integer>0</integer>
		<key>TableLength</key>
		<integer>0</integer>
		<key>TableSignature</key>
		<data>AAAAAA==</data>
	</dict>
</array>
</plist>
```
<br>

-----------------------------------------

### Audio:

This laptop uses ALC289, layout-id 11. 

<br>

-----------------------------------------


### iGPU Patching:

Besides the normal IceLake patches mentioned in the Dortania guide, the XPS 13 needs a few extra fixes:

1. `enable-backlight-smoother`
2. `enable-dbuf-early-optimizer`
3. `enable-backlight-registers-fix`
4. Use framebuffer `0200518A`

-----------------------------------------


### Misc:
- Add `-noDC9` to your boot-args to prevent panic after waking from sleep
- Dell's firmware is problematic with USB detection in F12 boot menu. To ensure a consistent experience, make sure the drive is partitioned as FAT32/GPT.
    - If you can't get it to work no matter what you try (flipping orientation, switching ports, etc etc) reset NVRAM.
- Make sure to toggle the "Advanced" switch in the BIOS.
- F12 for boot menu, F2 for BIOS

-----------------------------------------


### Undervolting:

- Please refer to [zearp's](https://github.com/zearp/Nucintosh#undervolting) undervolting guide.
- I found `./voltageshift offset -80 -75 -75` to work pretty well, YMMV and please be careful when doing this.




-----------------------------------------

#### Special Thanks:

- Dell for being a huge asshole 
- ExtremeXT for the S3 sleep SSDT and ACPI rename
- [Zearp's undervolting guide](https://github.com/zearp/Nucintosh#undervolting)
- [MVDB0110's XPS 13 7390 guide](https://github.com/MVDB0110/OC-XPS13-7390)
- [sambow23's XPS 13 non 2 in 1 guide](https://github.com/sambow23/Dell-XPS-13-7390-macOS)




