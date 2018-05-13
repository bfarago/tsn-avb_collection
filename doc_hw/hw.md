# TSN/AVB capable hw collection

Intel i210 chipset (complete)
[datasheet](i210-ethernet-controller-datasheet.pdf)

Atheros AR8031 PHY  (1588v2 ready) 802.3-100BaseT (1000Mbps over four twisted-pair)
[overview](IEEE1588_Phy_Atheros_AR8031.pdf)

Broadcom BCM89810 PHY  (1588v2 ready) 802.3-100BaseT1  (100Mbps over one twisted-pair)
[overview](IEEE1588_Phy_BroadR-89810-PB00-R.pdf)

Texas DP83630 PHY [datasheet](datasheet_TI_Phy_dp83630.pdf)

APPLE
-----

With macOS Sierra, I found that Mac Pro (2010) does not have AVB availability, because of the NIC does not support AVB.
On the other hand, MacBook Pro 13-inch (late 2011), MacBook Pro 15-inch (early 2011) have AVB availability.

XMOS
----

works:

XK-AUDIO-216-MC-AB : xCORE-200 Multichannel Audio Platform

XK-SK-AVB-DC: AVB Audio Daisy Chain

XK-AVB-LC-SYS : Low Cost AVB Audio Endpoint (not supported)

tricky:

xCORE General Purpose sliceKIT + XA-SK-ETH100 + 

Some ethernet slice-card use KSZ9031RNX GbPhy which is not 1588 capable.


MICROCHIP
---------
[Product catalog](prodcat_2016_microchip_00002285B.pdf)

Switch: KSZ9477 [repository](https://github.com/Microchip-Ethernet/EVB-KSZ9477) [datasheet](datasheet_microchip_KSZ9477S_00002392A.pdf)

Switch: KSZ9567R [datasheet](datasheet_microchip_KSZ9567R_00002329B.pdf)

MARVELL
-------

Broadcom
--------