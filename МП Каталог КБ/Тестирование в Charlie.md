Microsoft Windows [Version 10.0.19045.6456]
(c) Корпорация Майкрософт (Microsoft Corporation). Все права защищены.

C:\Users\Filippova.Elvina.RW>adb device
adb.exe: unknown command device

C:\Users\Filippova.Elvina.RW>adb devices
List of devices attached
0B62812I3110308E        unauthorized


C:\Users\Filippova.Elvina.RW>adb reverse tcp:7777 tcp:8888
adb.exe: device unauthorized.
This adb server's $ADB_VENDOR_KEYS is not set
Try 'adb kill-server' if that seems wrong.
Otherwise check for a confirmation dialog on your device.

C:\Users\Filippova.Elvina.RW>adb reverse tcp:7777 tcp:8888
adb.exe: device unauthorized.
This adb server's $ADB_VENDOR_KEYS is not set
Try 'adb kill-server' if that seems wrong.
Otherwise check for a confirmation dialog on your device.

C:\Users\Filippova.Elvina.RW>adb devices
List of devices attached
0B62812I3110308E        unauthorized


C:\Users\Filippova.Elvina.RW>adb reverse tcp:7777 tcp:8888
adb.exe: device unauthorized.
This adb server's $ADB_VENDOR_KEYS is not set
Try 'adb kill-server' if that seems wrong.
Otherwise check for a confirmation dialog on your device.

C:\Users\Filippova.Elvina.RW>adb reverse tcp:7777 tcp:8888
7777

C:\Users\Filippova.Elvina.RW>adb shell settings put global http_proxy localhost:7777

C:\Users\Filippova.Elvina.RW>> adb shell settings put global http_proxy :0 adb reverse --remove-all
"shell" не является внутренней или внешней
командой, исполняемой программой или пакетным файлом.

C:\Users\Filippova.Elvina.RW>>adb shell settings put global http_proxy :0 adb reverse --remove-all
"shell" не является внутренней или внешней
командой, исполняемой программой или пакетным файлом.

C:\Users\Filippova.Elvina.RW>adb shell settings put global http_proxy :0 adb reverse --remove-all
Argument expected to be 'default'

C:\Users\Filippova.Elvina.RW>adb shell settings put global http_proxy :0 adb reverse --remove-all
Argument expected to be 'default'

C:\Users\Filippova.Elvina.RW>
C:\Users\Filippova.Elvina.RW>adb devices
List of devices attached
0B62812I3110308E        device


C:\Users\Filippova.Elvina.RW>adb shell settings delete global http_proxy
adb.exe: no devices/emulators found

C:\Users\Filippova.Elvina.RW>adb devices
List of devices attached
0B62812I3110308E        device


C:\Users\Filippova.Elvina.RW>adb shell settings delete global http_proxy
Deleted 1 rows

C:\Users\Filippova.Elvina.RW>adb devices
List of devices attached


C:\Users\Filippova.Elvina.RW>adb reverse --remove-all

C:\Users\Filippova.Elvina.RW>adb reboot

C:\Users\Filippova.Elvina.RW>adb shell settings delete global captive_portal_http_url
adb.exe: device offline

C:\Users\Filippova.Elvina.RW>adb shell settings delete global captive_portal_https_url
adb.exe: device offline

C:\Users\Filippova.Elvina.RW>adb shell settings put global captive_portal_mode 1
adb.exe: device offline

C:\Users\Filippova.Elvina.RW>adb shell svc wifi disable
adb.exe: device offline

C:\Users\Filippova.Elvina.RW>adb shell svc wifi enable
adb.exe: device offline

C:\Users\Filippova.Elvina.RW>.\adb shell settings put global http_proxy :0
".\adb" не является внутренней или внешней
командой, исполняемой программой или пакетным файлом.

C:\Users\Filippova.Elvina.RW>adb shell settings put global http_proxy :0

C:\Users\Filippova.Elvina.RW>adb shell am kill ru.napoleonit.kb

C:\Users\Filippova.Elvina.RW>