# Keychron Q2 Max for Homerow mode

## Background

홈로우(Homerow)는 키보드 맵핑 방법 중 하나로, asdfjkl; 키에 Ctrl, Alt, Shift, GUI 키를 할당하는 방식이다. 
예를 들어, a 키를 누르고 있으면 Cmd 키가 눌린 것처럼 동작하며, 단순히 a를 빠르게 누르면 a가 입력된다. 
이 방법은 키보드를 사용할 때 손가락의 이동을 줄여주어 효율적인 타이핑을 가능하게 한다.

![](https://camo.githubusercontent.com/5efeb16db4e9aaaac5d160d6bc5c2c916a3cb80c305068714a4465744f9afe19/68747470733a2f2f63646e2e73686f706966792e636f6d2f732f66696c65732f312f303035392f303633302f313031372f66696c65732f4b65796368726f6e2d51322d4d61782d36355f2d4c61796f75742d576972656c6573732d437573746f6d2d4d656368616e6963616c2d4b6579626f6172642d436172626f6e2d426c61636b2e6a70673f763d31373033393137313133)

Q2 Max 키보드는 VIA를 통해 홈로우의 기본 설정을 지원하지만, VIA에서는 홈로우 모드를 완벽하게 설정할 수 없다. 
홈로우는 자주 사용하는 키의 누르는 시간에 따라 다른 키가 입력되는 방식이기 때문에, 시간 조절과 누르는 순서에 대한 모드 설정이 필요하다. 
그러나 VIA에서는 이러한 설정이 불가능하다. 
따라서, 홈로우를 제대로 사용하려면 펌웨어를 빌드하여 적용해야 한다.

Keychron Q2 Max 키보드에 홈로우 모드를 적용하는 과정을 기록한다.

## Firmware 소스 수정



```diff
diff --git a/keyboards/keychron/q2_max/ansi_encoder/config.h b/keyboards/keychron/q2_max/ansi_encoder/config.h
index 1de8bdc5ca..8d99266558 100644
--- a/keyboards/keychron/q2_max/ansi_encoder/config.h
+++ b/keyboards/keychron/q2_max/ansi_encoder/config.h
@@ -16,6 +16,9 @@
 
 #pragma once
 
+#define TAPPING_TERM 150
+#define PERMISSIVE_HOLD
+
 #ifdef RGB_MATRIX_ENABLE
 /* RGB Matrix driver configuration */
 #    define DRIVER_COUNT 2
```

## Settings via VIA

## Reference

- [A guide to home row mods](https://precondition.github.io/home-row-mods)
- [qmk_firmware/docs/tap_hold.md at master · byounghoonkim/qmk_firmware](https://github.com/byounghoonkim/qmk_firmware/blob/master/docs/tap_hold.md)
- [Setting Up Your QMK Environment | QMK Firmware](https://docs.qmk.fm/newbs_getting_started)
- [Building Your First Firmware | QMK Firmware](https://docs.qmk.fm/newbs_building_firmware)
- [Flashing Your Keyboard | QMK Firmware](https://docs.qmk.fm/newbs_flashing)
- [How Keys Are Registered, and Interpreted by Computers | QMK Firmware](https://docs.qmk.fm/how_keyboards_work)
- [How a Keyboard Matrix Works | QMK Firmware](https://docs.qmk.fm/how_a_matrix_works)
- [Understanding QMK's Code | QMK Firmware](https://docs.qmk.fm/understanding_qmk)


