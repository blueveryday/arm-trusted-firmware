编译 ATF (RAM Boot)的方法：

1、手动编译（Linux）
git clone -b mtksoc --single-branch https://github.com/mtk-openwrt/arm-trusted-firmware
cd arm-trusted-firmware

2、开启 RAM Boot 需要打个小补丁（修改路径在的文件Config-uart_dl.in）：

--- a/plat/mediatek/apsoc_common/bl2/Config-uart_dl.in
+++ b/plat/mediatek/apsoc_common/bl2/Config-uart_dl.in
@@ -10,7 +10,7 @@ config _RAM_BOOT_RAM_BOOT_UART_DL
 	bool "Enable RAM boot UART download support"
 	depends on _BOOT_DEVICE_RAM
 	depends on !_RAM_BOOT_DEBUGGER_HOOK
-	depends on _INTERNAL
+	# depends on _INTERNAL
 	default n
 
 # Makefile options


3、然后就可以选择构建目标了（以 MT7981 DDR3 内存为例）：

Boot device中选择 RAM；

在 Advanced boot device configuration 里面 选中 Enable RAM boot UART download support；

注意：MT7981B 需要在 Advanced DRAM configurations 里面选择：内存封装为 BGA，默认是 QFN，不改此项刷入 bl2 必砖

4、开始构建：
make CROSS_COMPILE=aarch64-linux-gnu-

5、目标文件：
build/mt7981/release/bl2.bin