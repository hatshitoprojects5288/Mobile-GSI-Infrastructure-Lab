# Partition Architecture & AVB Logic

Focus MediaTek A/B Slot & Dynamic Partition Systems.

The Dynamic Partition Paradigm (Super)Modern ARMv8 Redmi devices no longer utilize physical partitions for **system, vendor, and product.** Instead, they employ a Dynamic Partition scheme encapsulated within a physical super block.

Because standard bootloader **fastboot** cannot parse logical volumes inside the **super** block, GSI deployment requires rebooting into userspace **fastbootd**

------------
# The Execution Logic:

fastboot reboot fastboot

----
# Wipe logical partitions to ensure clean allocation
fastboot delete-logical-partition product_a

fastboot delete-logical-partition product_b

----
# Flash Generic System Image to the active logical slot

fastboot flash system system.img

----
# 2. Disabling Android Verified Boot (AVB)
3.
4. MediaTek platforms strictly enforce dm-verity. If a generic system image is flashed over the OEM vendor partition, the cryptographic hash mismatch will trigger an immediate kernel panic **(bootloop)**.
5.
6. The **Solution:** Before flashing the OS, the **vbmeta** partition must be overwritten with disable flags. This instructs the bootloader to ignore signature verification for all subsequent logical partitions.

-----
8. # The Execution Logic:

fastboot --disable-verity --disable-verification flash vbmeta vbmeta.img

-----
# 3. Seamless Updates (A/B Slots)

Since the device utilizes A/B slots for seamless updates, you must verify which slot the bootloader is currently prioritizing to avoid flashing a payload into a dormant slot.Command: fastboot getvar current-slot

**Command:** fastboot getvar current-slot

-----
