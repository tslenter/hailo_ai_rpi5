# Hailo basic commands in combination with duo PCI-E 3.0, NVME, Ai Hailo
```
Manual, bugs, and fixes
Selected hardware:
PCI-E 3.0 card: Seeed studio pcie 3.0 naar dual m.2 hat
NVME: Non-Volatile memory controller: Micron Technology Inc 2400 NVMe SSD (DRAM-less) (rev 03)
Hailo Ai card: Hailo Technologies Ltd. Hailo-8 AI Processor (rev 01)
```

## Install all software
```
sudo apt install hailo-all
```

## Check:
```
sudo dmesg | grep hailo
```

## Error:
```
[    1.762496] hailo_pci: loading out-of-tree module taints kernel.
[    1.767100] hailo: Init module. driver version 4.21.0
[    1.767454] hailo 0001:06:00.0: Probing on: 1e60:2864...
[    1.767458] hailo 0001:06:00.0: Probing: Allocate memory for device extension, 13192
[    1.767475] hailo 0001:06:00.0: enabling device (0000 -> 0002)
[    1.767481] hailo 0001:06:00.0: Probing: Device enabled
[    1.767493] hailo 0001:06:00.0: Probing: mapped bar 0 - 00000000ff0c4264 16384
[    1.767497] hailo 0001:06:00.0: Probing: mapped bar 2 - 00000000cbe562d8 4096
[    1.767501] hailo 0001:06:00.0: Probing: mapped bar 4 - 00000000826e6d72 16384
[    1.767504] hailo 0001:06:00.0: Probing: Force setting max_desc_page_size to 4096 (recommended value is 16384)
[    1.767512] hailo 0001:06:00.0: Probing: Enabled 64 bit dma
[    1.767515] hailo 0001:06:00.0: Probing: Using userspace allocated vdma buffers
[    1.767518] hailo 0001:06:00.0: Disabling ASPM L0s
[    1.767522] hailo 0001:06:00.0: Successfully disabled ASPM L0s
[    1.767583] hailo 0001:06:00.0: Failed to enable MSI -28
[    1.767585] hailo 0001:06:00.0: Failed enabling interrupts -28
[    1.767586] hailo 0001:06:00.0: Failed activating board -28
[    1.767594] hailo 0001:06:00.0: probe with driver hailo failed with error -28
```

## Fix:
```
sudo nano /boot/firmware/config.txt
dtparam=pciex1
dtparam=pciex1_gen=3
dtoverlay=pciex1-compat-pi5,no-mip
```

## Control check:
```
sudo dmesg | grep hailo
```

## Correct output:
```
[    1.644325] hailo: Init module. driver version 4.20.0
[    1.646343] hailo 0001:06:00.0: Probing on: 1e60:2864...
[    1.646348] hailo 0001:06:00.0: Probing: Allocate memory for device extension, 13184
[    1.646366] hailo 0001:06:00.0: enabling device (0000 -> 0002)
[    1.646373] hailo 0001:06:00.0: Probing: Device enabled
[    1.646392] hailo 0001:06:00.0: Probing: mapped bar 0 - 00000000461d0b2b 16384
[    1.646396] hailo 0001:06:00.0: Probing: mapped bar 2 - 0000000078e091ba 4096
[    1.646399] hailo 0001:06:00.0: Probing: mapped bar 4 - 00000000e7259883 16384
[    1.646403] hailo 0001:06:00.0: Probing: Force setting max_desc_page_size to 4096 (recommended value is 16384)
[    1.646412] hailo 0001:06:00.0: Probing: Enabled 64 bit dma
[    1.646415] hailo 0001:06:00.0: Probing: Using userspace allocated vdma buffers
[    1.646419] hailo 0001:06:00.0: Disabling ASPM L0s
[    1.646422] hailo 0001:06:00.0: Successfully disabled ASPM L0s
[    1.646498] hailo 0001:06:00.0: Writing file hailo/hailo8_fw.bin
[    1.725095] hailo 0001:06:00.0: File hailo/hailo8_fw.bin written successfully
[    1.725102] hailo 0001:06:00.0: Writing file hailo/hailo8_board_cfg.bin
[    1.725126] Failed to write file hailo/hailo8_board_cfg.bin
[    1.725128] hailo 0001:06:00.0: File hailo/hailo8_board_cfg.bin written successfully
[    1.725130] hailo 0001:06:00.0: Writing file hailo/hailo8_fw_cfg.bin
[    1.725137] Failed to write file hailo/hailo8_fw_cfg.bin
[    1.725138] hailo 0001:06:00.0: File hailo/hailo8_fw_cfg.bin written successfully
[    1.814318] hailo 0001:06:00.0: NNC Firmware loaded successfully
[    1.814325] hailo 0001:06:00.0: FW loaded, took 167 ms
[    1.850446] hailo 0001:06:00.0: Probing: Added board 1e60-2864, /dev/hailo0
```

## Check if correct detected:
```
hailortcli fw-control identify
```

## Correct output:
```
tom@ulrasp0001:~ $ hailortcli fw-control identify
Executing on device: 0001:06:00.0
Identifying board
Control Protocol Version: 2
Firmware Version: 4.20.0 (release,app,extended context switch buffer)
Logger Version: 0
Board Name: Hailo-8
Device Architecture: HAILO8L
Serial Number: SNSNSNSNSNSN
Part Number: PNPNPNPNPNPN
Product Name: HAILO-8L AI ACC M.2 B+M KEY MODULE EXT TMP
```
