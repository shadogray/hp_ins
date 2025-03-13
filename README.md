# hp_ins
KeyMapper for HP Elitebook OffHook/OnHook keys

## Source
<https://alter.org.ua/soft/linux/hp_ins/>

# Original Description
I've bought HP Elitebook 745 g5 notebook. It has all Arrows, PgUp/PgDn and Home/End buttons (not trivail in out time). But I've missed to check for Insert. There are 3 multimedia buttons Display, Answer, Hangup instead of NumLock, Break and Insert. I don't use NumLock and Break, but Insert is vitally required. Usually it is not a problem to remap keys with input-remapper, but not in this case. Keyboard sends the following sequences for single key press:

|Key| Scan code sequence | Mapping |
|----|-------------------|---------|
|Screen|	Ctrl_L + Alt_L + Screen|	ScrollLock|
|Answer|	Ctrl_L + Answer + Meta_L|	Pause/Break|
|Hangup|	Ctrl_L + Alt_L + Hangup|	Insert|

So, maping Hangup to Insert gives Ctrl_L + Alt_L + Insert. It's not what I want. If I map entire Ctrl_L + Alt_L + Insert to Insert there is no way to use Ctrl + Insert or Alt + Insert sequences. It is not acceptable too. I've inspected actual key code sequences with evtest and found that the only difference between normal (user-psessed) and keyboard-generated Ctrl+Alt sequence is timing. Keyboards sends codes with delay about 0.1ms, while humans press buttons significantly slower. I've found Disable CapsLock filter and adopted it for my case. Quickly received key code sequences corresponding to Display, Answer, Hangup are mapped, while human-presses Ctrl, Alt and Meta are kept untouched. And Hangup button is now correcetly mapped to Insert

Since v1.0-2 added mapping for ScrollLock and Pause/Break

https://github.com/Alter-1/hp_ins
Install
Download and unpack archive hp-ins-1.0-3.amd64.tar.gz

cd hp-ins-1.0-3
less /proc/bus/input/devices
### Identify your keyboard, smth. like this:
```
I: Bus=0011 Vendor=0001 Product=0001 Version=ab41
N: Name="AT Translated Set 2 keyboard"
P: Phys=isa0060/serio0/input0
S: Sysfs=/devices/platform/i8042/serio0/input/input4
U: Uniq=
H: Handlers=sysrq kbd event4 leds
```
It means that your event device is /dev/input/event4

```
### Install 
sudo ./install.sh /dev/input/event4
sudo systemctl status hp_ins
Note: configuration is stored in /etc/hp_ins.conf
```

### Uninstall
sudo ./uninstall.sh
    or
sudo systemctl disable hp_ins
sudo systemctl stop hp_ins
Debugging
You can just run sudo ./hp_ins -v /dev/input/event4 to see if everything is ok before installing.

Native HP solution
FN+R = Break
FN+S = Sys Rq
FN+C = Scroll Lock
FN+E = Insert
FN+W = Pause
