# Keychron Q2 Max for Homerow mode

## Background

홈로우(Homerow)는 키보드 맵핑 방법 중 하나로, asdfjkl; 키에 Ctrl, Alt, Shift, GUI 키를 할당하는 방식이다. 

- [A guide to home row mods](https://precondition.github.io/home-row-mods)

예를 들어, a 키를 누르고 있으면 Cmd 키가 눌린 것처럼 동작하며, 단순히 a를 빠르게 누르면 a가 입력된다. 
이 방법은 키보드를 사용할 때 손가락의 이동을 줄여주어 효율적인 타이핑을 가능하게 한다.

![](https://camo.githubusercontent.com/5efeb16db4e9aaaac5d160d6bc5c2c916a3cb80c305068714a4465744f9afe19/68747470733a2f2f63646e2e73686f706966792e636f6d2f732f66696c65732f312f303035392f303633302f313031372f66696c65732f4b65796368726f6e2d51322d4d61782d36355f2d4c61796f75742d576972656c6573732d437573746f6d2d4d656368616e6963616c2d4b6579626f6172642d436172626f6e2d426c61636b2e6a70673f763d31373033393137313133)

Q2 Max 키보드는 VIA를 통해 홈로우의 기본 설정을 지원하지만, VIA에서는 홈로우 모드를 완벽하게 설정할 수 없다. 
홈로우는 자주 사용하는 키의 누르는 시간에 따라 다른 키가 입력되는 방식이기 때문에, 시간 조절과 누르는 순서에 대한 모드 설정이 필요하다. 
그러나 VIA에서는 이러한 설정이 불가능하다. 
따라서, 홈로우를 제대로 사용하려면 펌웨어를 빌드하여 적용해야 한다.

Keychron Q2 Max 키보드에 홈로우 모드를 적용하는 과정을 기록한다.

> !주의
> 키보드이 펌웨어를 수정하고 업로드하는 과정은 키보드가 고장날 수 있으므로 주의해야 한다. 
> 그리고 이 과정은 키보드의 보증을 상실시킬 수 있다.
> 이 문서는 키보드 펌웨어를 수정하고 업로드하는 과정을 설명하며,
> 이 과정에서 발생하는 문제에 대해서는 각자 책임을 져야 한다.

## 빌드 준비

```bash
git clone https://github.com/byounghoonkim/keychron-q2-max-homerow.git
cd keychron-q2-max-homerow
git submodule update --init --recursive
```

## qmk environment 설정
[Setting Up Your QMK Environment | QMK Firmware](https://docs.qmk.fm/newbs_getting_started) 문서를 참고하여 QMK 환경을 설정한 후 
`qmk setup` 명령어를 사용하여 환경을 설정한다.

## Firmware 소스 수정

`keyboards/keychron/q2_max/ansi_encoder/config.h` 파일에 TAPPING_TERM과 PERMISSIVE_HOLD를 추가한다.
TAAPING_TERM은 키를 누르고 있는 시간을 설정하는 값이다. 이는 개인의 타이핑 속도에 따라 조절할 수 있다.
기본 값은 200이며, 150으로 설정하였다. PERMISSIVE_HOLD는 홈로우 모드를 사용할 때 필요한 설정이다.
PERMISSIVE_HOLD에 대한 자세한 내용은 [qmk_firmware/docs/tap_hold.md at master · byounghoonkim/qmk_firmware]를 참고한다.

'keyboards/keychron/q2_max/ansi_encoder/keymaps/via/keymap.c' 파일에 홈로우 모드를 위한 키도 적용한다.
이는 VIA 를 통해서도 설정할 수 있지만, keymap.c 를 수정하여 기본값을 수정하였다.

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
diff --git a/keyboards/keychron/q2_max/ansi_encoder/keymaps/via/keymap.c b/keyboards/keychron/q2_max/ansi_encoder/keymaps/via/keymap.c
index 286aaa1cc9..66b5c0aa85 100644
--- a/keyboards/keychron/q2_max/ansi_encoder/keymaps/via/keymap.c
+++ b/keyboards/keychron/q2_max/ansi_encoder/keymaps/via/keymap.c
@@ -25,12 +25,22 @@ enum layers {
     FN2,
 };
 
+#define KC_A_HR MT(MOD_LGUI, KC_A)
+#define KC_S_HR MT(MOD_LALT,KC_S)
+#define KC_D_HR MT(MOD_LSFT,KC_D)
+#define KC_F_HR MT(MOD_LCTL,KC_F)
+#define KC_G_HR MT(MOD_LCTL | MOD_LSFT | MOD_LALT,KC_G)
+#define KC_J_HR MT(MOD_LCTL | MOD_RCTL,KC_J)
+#define KC_K_HR MT(MOD_LSFT | MOD_RSFT,KC_K)
+#define KC_L_HR MT(MOD_LALT | MOD_RALT,KC_L)
+#define KC_SCLN_HR MT(MOD_LGUI | MOD_RGUI,KC_SCLN)
+
 // clang-format off
 const uint16_t PROGMEM keymaps[][MATRIX_ROWS][MATRIX_COLS] = {
     [MAC_BASE] = LAYOUT_ansi_67(
         KC_ESC,  KC_1,     KC_2,     KC_3,    KC_4,    KC_5,    KC_6,    KC_7,    KC_8,    KC_9,    KC_0,     KC_MINS,  KC_EQL,   KC_BSPC,          KC_MUTE,
         KC_TAB,  KC_Q,     KC_W,     KC_E,    KC_R,    KC_T,    KC_Y,    KC_U,    KC_I,    KC_O,    KC_P,     KC_LBRC,  KC_RBRC,  KC_BSLS,          KC_DEL,
-        KC_CAPS, KC_A,     KC_S,     KC_D,    KC_F,    KC_G,    KC_H,    KC_J,    KC_K,    KC_L,    KC_SCLN,  KC_QUOT,            KC_ENT,           KC_HOME,
+        KC_CAPS, KC_A_HR,  KC_S_HR,  KC_D_HR, KC_F_HR, KC_G_HR, KC_H,    KC_J_HR, KC_K_HR, KC_L_HR, KC_SCLN_HR,KC_QUOT,           KC_ENT,           KC_HOME,
         KC_LSFT,           KC_Z,     KC_X,    KC_C,    KC_V,    KC_B,    KC_N,    KC_M,    KC_COMM, KC_DOT,   KC_SLSH,            KC_RSFT, KC_UP,
         KC_LCTL, KC_LOPTN, KC_LCMMD,                            KC_SPC,                             KC_RCMMD, MO(MAC_FN1),MO(FN2),KC_LEFT, KC_DOWN, KC_RGHT),
 

```

## Firware 빌드
아래 명령으로 qmk firmware를 빌드한다.

```bash
qmk compile -kb keychron/q2_max/ansi_encoder -km via
```

> 위와 같은 과정으로 미리 빌드된 펌웨어 파일은 [./keychron_q2_max_ansi_encoder_via_homerow.bin](./keychron_q2_max_ansi_encoder_via_homerow.bin) 에서 다운로드 받을 수 있다.


## Firmware 적용하기
빌드한 firmware를 아래와 같은 순서로 적용한다.

1. [QMK Toolbox](https://qmk.fm/toolbox))를 실행한다. 
2. - [키크론 Q2 Max 펌웨어 업데이트 & 다운로드 (공장 초기화 방법 안내)](https://keychron.kr/firmware/?vid=118) 문서를 참고하여 키크론 Q2 Max 키보드를 DFU 모드로 변경한다.
3. `Open` 버튼을 클릭하여 빌드한 펌웨어 파일을 선택한다.
4. `Flash` 버튼을 클릭하여 펌웨어를 적용한다.
5. 펌웨어 적용이 완료되면 키보드를 재부팅한다.
6. 키보드가 정상적으로 동작하는지 확인한다.
7. VIA를 통해 설정을 확인한다.

![VIA 설정](./via.png)

## Reference

- [A guide to home row mods](https://precondition.github.io/home-row-mods)
- [qmk_firmware/docs/tap_hold.md at master · byounghoonkim/qmk_firmware](https://github.com/byounghoonkim/qmk_firmware/blob/master/docs/tap_hold.md)
- [Setting Up Your QMK Environment | QMK Firmware](https://docs.qmk.fm/newbs_getting_started)
- [Building Your First Firmware | QMK Firmware](https://docs.qmk.fm/newbs_building_firmware)
- [Flashing Your Keyboard | QMK Firmware](https://docs.qmk.fm/newbs_flashing)
- [How Keys Are Registered, and Interpreted by Computers | QMK Firmware](https://docs.qmk.fm/how_keyboards_work)
- [How a Keyboard Matrix Works | QMK Firmware](https://docs.qmk.fm/how_a_matrix_works)
- [Understanding QMK's Code | QMK Firmware](https://docs.qmk.fm/understanding_qmk)
- [키크론 Q2 Max 펌웨어 업데이트 & 다운로드 (공장 초기화 방법 안내)](https://keychron.kr/firmware/?vid=118)


