### Configuration du BQEEL R2 MAX
## Activer le server TCP adb au démarage
Installer une apk shell sur le BQEEL, passer root sur le shell 
```bash
su
setprop service.adb.tcp.port 5555
stop adbd
start adbd
```
Se connecter avec adb sur la BQEEL
```bash
adb connect 192.168.0.46:5555
adb shell
mount -o rw,remount /system
```

Créer un script myboot.sh pour lancer le service adb en tcp dans /system/etc/, faire un push pour pousser le fichier si aucun éditeur est présent
```bash
#!/bin/sh
setprop service.adb.tcp.port 5555
stop adbd
start adbd
```
Le rendre executable
```bash
chmod 755 /system/etc/myboot.sh
```
Créer un fichier myboot.rc de lancement dans /system/etc/init
```bash
on property:sys.boot_completed=1 && property:wifi.interface=* && property:sys.wifitracing.started=1
    exec u:r:su:s0 root root -- /system/etc/myboot.sh
```
Au prochain redémarrage le service adb tcp devrait être disponnible
## Frida server
Installer le frida-server dans /bin (passer le fs system en rw).
Lancer le frida server en tcp.
```bash
/bin/frida-server -l 192.168.0.46
```
Pour se connecter avec frida à l'app
```bash
 ~/.local/bin/frida  -H 192.168.0.46 -f tw.com.letswin.vdartsgame -l base_address.js
 ```
