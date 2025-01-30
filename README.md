##################################
源码修改 
- 关闭电源: Turn off the usbpower:
-  echo '0' > /sys/class/gpio/usb3power/value
- 打开电源: Turn on the usbpower:
-  echo '1' > /sys/class/gpio/usb3power/value

━━━━━━━━━━━━━━━━━━━━━━━━

⒈"mt7621_xiaomi_nand_128m.dtsi" Add modifications:
#add usbpower on and off

	gpio_export {
		compatible = "gpio-export";
		#size-cells = <0>;

		usb3power {
			gpio-export,name = "usb3power";
			gpio-export,output = <1>;
			gpios = <&gpio 12 GPIO_ACTIVE_HIGH>;
		};

	};



 ━━━━━━━━━━━━━━━━━━━━━━━━
#修改后:

	gpio_export {
		compatible = "gpio-export";
		#size-cells = <0>;

		usb3power {
		gpio-export,name = "usb3power";
		gpio-export,output = <1>;
		gpios = <&gpio 12 GPIO_ACTIVE_HIGH>;
		};

	};
	reg_usb_vbus: regulator {
		compatible = "regulator-fixed";
		regulator-name = "usb_vbus";
		regulator-min-microvolt = <5000000>;
		regulator-max-microvolt = <5000000>;
		gpios = <&gpio 12 GPIO_ACTIVE_HIGH>;
		enable-active-high;
	};



━━━━━━━━━━━━━━━━━━━━━━━━


⒉创建目录和文件files/etc/uci-defaults/mi3g 
增加了第1次启动脚本mi3g:


#r3g 24.10主分支_master https://github.com/sxf2314/openwrt.git
#开WiFi 5g WiFi信道设置为国家_俄罗斯（俄罗斯没有任何限制）


	uci set wireless.radio0.disabled='0'
	uci set wireless.radio1.disabled='0'
	uci set wireless.radio1.channel='64'
	uci set wireless.radio1.country='RU'
	uci commit wireless
	#设置 usbwan usbwan6 
	#在这个版本的openwrt  手机的Usb共享 不是usb0 而是eth1
	uci set network.usbwan=interface
	uci set network.usbwan.proto='dhcp'
	uci set network.usbwan.device='eth1'
	uci set network.usbwan6=interface
	uci set network.usbwan6.proto='dhcpv6'
	uci set network.usbwan6.device='eth1'
	uci commit network
	echo "All done!"


##################################
固件设置
━━━━━━━━━━━━━━━━━━━━━━━━

时区选择:上海
usbwan usbwan6:加入防火墙
在防火墙下开启:硬件加速
Ipv6设置 
usbwan6 :设置成主接口 ,设置成中继 ,学习
        lan :都设置成中继 ,学习
##################################
━━━━━━━━━━━━━━━━━━━━━━━━

			time:2025.01.30
