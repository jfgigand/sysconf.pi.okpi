# Sysconf profile for Okapi's Raspberry PI B+

## Services

### 42000/tcp: SSH

* Management of SSH authorized keys, for users
  [tree/etc/ssh/authorized_keys.root.d/](root) and
  [/etc/ssh/authorized_keys.pi.d/](pi) (one file per key, merged
  together by sysconf-etc.d)


### 4713/tcp: PulseAudio

[tree/etc/pulse/system.pa](Configuration) enables
_module-native-protocol-tcp_ and _module-zeroconf-publish_.

You should update the network address with your own. TODO: split
_tree/etc/pulse/system.pa_ to _tree/etc/pulse/system.pa.d/*.system.pa_
files to allow easier overriding.

[https://github.com/mk-fg/pulseaudio-mixer-cli](pulseaudio-mixer-cli.py),
which is a PulseAudio mixer written in Python, was imported
[tree/usr/bin/pulseaudio-mixer-cli.py](here) and can control the
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

[http://www.musicpd.org/](MPD) runs as user _mpd_.
[http://mpd.wikia.com/wiki/Clients](Clients) can connect to it from
the local network.

MPD is currently [tree/etc/mpd.conf](configured) to use ALSA only for
sound output, which is a lot more stable than the direct PulseAudio
output (which was not usable in the first try, but's it's not the end
of the story :). ALSA is emulated by PulseAudio anyway.

#### About MPD clients

I have been using these clients (but there are
[http://mpd.wikia.com/wiki/Clients](many)):

* [http://www.musicpd.org/clients/mpc/](mpc) for easy _mpc update_ and
control from scripts (including window manager applets)

* [http://ncmpcpp.rybczak.net/](ncmpcpp) for a user friendly
curses-style interface

* [http://gmpclient.org/](gmpc) which is like _ncmpcpp_ but with a
GTK+ interface: this is a good choice for someone used to GUI player
like _Benshee_ or _Totem_.

* [https://github.com/abarisain/dmix](MPDroid) to control MPD from an
Android device, with a well-made interface.


## History

* Created on 20151014 by JF Gigand <jf@geonef.fr>
