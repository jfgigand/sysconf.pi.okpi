# Sysconf profile for Okapi's Raspberry PI B+

## Services

### 42000/tcp: SSH

* Management of SSH authorized keys, for users
  [root](tree/etc/ssh/authorized_keys.root.d/) and
  [pi](tree/etc/ssh/authorized_keys.pi.d/) (one file per key, merged
  together by sysconf-etc.d)


### 4713/tcp: PulseAudio

[Configuration](tree/etc/pulse/system.pa) enables
_module-native-protocol-tcp_ and _module-zeroconf-publish_.

You should update the network address with your own. TODO: split
_etc/pulse/system.pa_ to _etc/pulse/system.pa.d/*.system.pa_
files to allow easier overriding.

[pulseaudio-mixer-cli.py](https://github.com/mk-fg/pulseaudio-mixer-cli),
which is a PulseAudio mixer written in Python, was imported
[here](tree/usr/bin/pulseaudio-mixer-cli.py) and can control the
volume levels with a curses-like interface.

The usual _pacmd_ command will of course provide complete management
of the PulseAudio system.

#### How to configure the PulseAudio client

* Install _paprefs_: ```apt-get install paprefs```
* Run _paprefs_ as the same user as the one running PulseAudio (the
one which play songs: usually not _root_)
* Check "Make discoverable PulseAudio network sound devices available
locally"

Done! It should work, because of ZeroConf. Maybe the PulseAudio
session should be restarted: run ```pulseaudio -k``` then
```pulseaudio --start``` or logout from X and login again.


### 6600/tcp: MPD (Music Player Daemon)

[MPD](http://www.musicpd.org/) runs as user _mpd_.
[Clients](http://mpd.wikia.com/wiki/Clients) can connect to it from
the local network.

MPD is currently [configured](tree/etc/mpd.conf) to use ALSA only for
sound output, which is a lot more stable than the direct PulseAudio
output (which was not usable in the first try, but's it's not the end
of the story :). ALSA is emulated by PulseAudio anyway.

#### About MPD clients

I have been using these clients (but there are
[many](http://mpd.wikia.com/wiki/Clients)):

* [mpc](http://www.musicpd.org/clients/mpc/) for easy _mpc update_ and
control from scripts (including window manager applets)

* [ncmpcpp](http://ncmpcpp.rybczak.net/) for a user friendly
curses-style interface

* [gmpc](http://gmpclient.org/) which is like _ncmpcpp_ but with a
GTK+ interface: this is a good choice for someone used to GUI player
like _Benshee_ or _Totem_.

* [MPDroid](https://github.com/abarisain/dmix) to control MPD from an
Android device, with a well-made interface.


## History

* 20151014 started by JF Gigand <jf@geonef.fr>
* 20151018 first working version, released on GitHub
