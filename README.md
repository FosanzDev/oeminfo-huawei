###  oeminfo-huawei
  
- Used to modify the oeminfo file/storage region on Huawai devices which controls things like the model number and region
- 2022 fork of https://github.com/Iceows/oeminfo-huawei deleting symlinks (Not necessary for Windows)
- 2021 fork of https://github.com/penn5/oeminfo-huawei fixing bugs in the pack function and adding a usage examples.
- Original code from https://forum.xda-developers.com/p9-plus/how-to/mod-oeminfo-structure-t3446382


```shell
oeminfo_huawei.py [-h] {extract,pack,replace} ...

positional arguments:
  {extract,pack,replace}

optional arguments:
  -h, --help            show this help message and exit
```


#### Detailed Usage example:

 - You must have an unlocked bootloader and Magisk installed to modify the oeminfo 
 - Script sucessfully tested on Ubuntu with python3 on a CMR-W09 running EMUI 9.1.0.332 to change the region from C567 to C432 and hw/usa to hw/eu
 
```shell
user@ubuntu:~/oem_info$ adb shell
su
dd if=/dev/block/platform/hi_mci.0/by-name/oeminfo of=/storage/sdcard1/oeminfo.img
adb pull /storage/emulated/0/oeminfo.img eminfo.emmc.win
```

```shell
user@ubuntu:~/oem_info$ python3 oeminfo_huawei.py extract -i ./oeminfo.emmc.win
hdr:OEM_INFO age:  7 id:    8  
hdr:OEM_INFO age:  1 id:   12 Region 
hdr:OEM_INFO age:  1 id:   42  
hdr:OEM_INFO age:  5 id:   43 Root Type (info) 
...
CMR-W09#hw-usa#CMR-W09 8.0.0.200(C567)
```

- Edit files within the CMR-W09#hw-usa#CMR-W09 8.0.0.200(C567) directory to change the region using a hex editor like GHex (Hexplorer for Win users)

```shell
user@ubuntu:~/oem_info$ python3 oeminfo_huawei.py pack -i CMR-W09#hw-usa#CMR-W09\ 8.0.0.200\(C567\)/ -o ./test2.img
None
```


```shell
user@ubuntu:~/oem_info$ fastboot flash oeminfo test2.img 
target reported max download size of 471859200 bytes
sending 'oeminfo' (65536 KB)...
OKAY [  2.264s]
writing 'oeminfo'...
OKAY [  0.478s]
finished. total time: 2.742s
```
