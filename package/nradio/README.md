# NRadio C8-660 packages

Imported from immortalwrt-mt798x-rebase dev/wt9103, commit
26d07ddc051edbf3c0f05df10f05c50a6cbcdd02, for the ImmortalWrt 25.12
filogic-mac80211 tree. Original runtime sources are retained, except the
user-approved removal of modeminit's unconditional Wi-Fi LED write.

- luci-app-Secondsystem660: original dual-system page/script, default C8 profile.
- luci-app-zmodem: optional modem page, selects the scripts and legacy SMS app.
- nradio-rm520n-scripts: optional OEM scripts and AArch64 userspace utilities.
- luci-app-sms-tool: legacy SMS dependency; readsms/sendsms templates use a
  private modem/nradio-sms directory to coexist with the original zmodem files.

Makefiles add actual runtime dependencies and persistent configuration paths.
The SMS post-install script skips execution against an image staging root;
on-device behavior is preserved. Existing sendat is reused from the packages
feed. The large 5g-modem directory and vendor Wi-Fi/HNAT packages are not imported.

Four missing runtime assets come from the unpacked OEM C8-660 4.6.8 image:
oem/c8_660_fw/sysupgrade-nradio_wt9103_No2/rootfs/usr/bin/.
The OEM luci-app-zmodem.list records their original paths; the assets are kept
with the scripts here so modeminit has its Python worker even without the UI.
Binary architecture/library checks do not establish device runtime success.

| Asset | SHA-256 |
| --- | --- |
| smstrun.py | 8811f881ba34e7984e862f8550e803045a4fb988cf00b09f1e22e5ec6a4871b4 |
| RMUnlock | bf63f9882ca421f4a4fb7f5095d41ab46279dcb012c5e089ea04c13b3bb85286 |
| sms_tool2 | 62874d2424de786b002c90a235b927b8741cb030e5341fe7cb2b94391c89019c |
| smstrun-title.conf | 416b33a75f053ceb9fa3e91badcad1503fcfc0980349d77cb899f5ca1756839a |

Known source gaps: /usr/share/modem/autoswitch.sh is absent from both the rebase
checkout and the OEM rootfs. /usr/bin/smstrun.conf is user-owned token data and
is not supplied by OEM; no token file is generated. Original calls remain.
Full hardware behavior, MTD/UBI numbering, SIM options and the OEM binaries
require device acceptance. See specs/004-wt9103-mac80211/ in the parent workspace.

The dev_wt9103 branch is dedicated to WT9103/C8-660. Its global
CONFIG_MTD_PARTITIONED_MASTER=y intentionally matches the rebase tree;
other Filogic boards are outside this branch's acceptance scope.

The missing autoswitch.sh is referenced by the original RM520N script for
scheduled cellular network-mode switching (automatic/4G/5G), configured by
switchNetwork, Autoswitchtime and smode2. These references were already present
in the initial rebase import fdf09a2c8f0347859dfe62f803c6ee7808385d0f.
The imported rm520n.sh matches the unpacked OEM file byte for byte, while neither
the OEM package file list nor the rebase file history provides autoswitch.sh.
The original references are retained; the reason the implementation is absent
cannot be established from the available sources.
