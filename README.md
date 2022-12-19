## Let the laptop battery appear as the one of an uninterruptible power supply, using NUT
by [Stephan&ThinSpace;K.H.&ThinSpace;Seidl](http://www.stfmc.de/aux/skhs.html)

Version 2, Sat, 15 Feb 2020 21:57:27 +0100

---

### Introduction
With the development of heavy middleware systems as the **Distributed Computing Environment** (**DCE**) with their appropriate desktops in the early 1990s, the **UNIX**-type operating systems got as user-friendly and, at the same time, as slow as the **Windows**-type operating systems are. Before DCE, there were essentially two efficient manners to work with UNIX, either with the **Emacs** editor alone or with the **Vi** editor under **X11** to have the mouse for **Cut/Copy and Paste**. Running both Emacs and X11 at the same time did not perform on normal machines in the past, and running the Vi on the command line alone is awful. The latter of the ancient, but still highly efficient working manners, Vi plus X11, is up to now very interesting, especially if X11 goes together with **Fvwm** as the virtual window manager. The pair X11 plus Fvwm runs well on skimpy hardware as the **EeePC 1000H**, for example, and logging from such a system into an even slower one, as a router, via **ssh**, and running a Vi editor there does not really degrade the efficiency anymore.

### The problem
Deselecting heavy desktops as **KDE** and **Gnome** in our days has, on the other hand, the disadvantage that basic software functionalities do not come automatically. One of those functionalities is proper laptop battery management. Proper battery management is of vital importance on laptops. So it has to be done by hand.

### The solution
The systems in question here run **Debian** 6 through 9 with kernels 2.6.34 through 4.9.0. and **nut** 2.4.3 through 2.7.4. It is important that the directories **/sys/class/power_supply/AC*** or **/sys/class/power_supply/?*[0-9]** are provided such that the files **type** and **uevent** there can be read by **/usr/sbin/laptopbat2ups**. The daemon /usr/sbin/laptopbat2ups updates [/var/tmp/laptop-battery-status](var/tmp/laptop-battery-status) once per 13 seconds. The file /var/tmp/laptop-battery-status is the input for the **dummy-ups** driver of nut. nut is configured in such a manner that the operating system automatically shuts down if the remaining battery capacity is less than 20&ThinSpace;% or if the remaining runtime on battery is less than 5 minutes. Here is the list of the files that have to be created or updated, respectively.

---

<pre>
-rwxr-xr-x ... root root ... <a href="etc/init.d/laptopbat2ups">/etc/init.d/laptopbat2ups</a>
-rw-r----- ... root nut  ... <a href="etc/nut/nut.conf">/etc/nut/nut.conf</a>
-rw-r----- ... root nut  ... <a href="etc/nut/ups.conf">/etc/nut/ups.conf</a>
-rw-r----- ... root nut  ... <a href="etc/nut/upsd.conf">/etc/nut/upsd.conf</a>
-rw-r----- ... root nut  ... <a href="etc/nut/upsd.users">/etc/nut/upsd.users</a>
-rw-r----- ... root nut  ... <a href="etc/nut/upsmon.conf">/etc/nut/upsmon.conf</a>
-rw-r----- ... root nut  ... <a href="etc/nut/upssched.conf">/etc/nut/upssched.conf</a>
-rwxr-xr-x ... root root ... <a href="sbin/upsschedcmdscript">/sbin/upsschedcmdscript</a>
-rwxr-xr-x ... root root ... <a href="usr/sbin/laptopbat2ups">/usr/sbin/laptopbat2ups</a>
-rwxr-xr-x ... root root ... <a href="usr/sbin/upsstatus">/usr/sbin/upsstatus</a>
</pre>

It should not be forgotten to create the symlinks if there is a file [/etc/init.d/.legacy-bootordering](etc/init.d/.legacy-bootordering) or to do whatever is necessary for any other init system.

**/usr/sbin/upsstatus** is a supporting script that shows the situation when on battery. When on line, the given remaining battery time is not more than a rough guess. An output of /usr/sbin/upsstatus might look as follows.

---

```
VGP-BPS13 on battery
Discharging battery
Battery charge 82 %
Battery runtime 210 min
Shutdown command after 159 min due to remaining capacity of 20 %
Load 100 %
Real output power 13.5 W
```

---

```
VGP-BPS13 on line
Charging battery
Battery charge 82 %
Battery runtime 200 min
Shutdown command after 151 min due to remaining capacity of 20 %
Load 100 %
Real output power 14.4 W
```

## Conclusion
For me, it works fine. Try it, if you want.

[Source](http://www.stfmc.de/misc/linuxhacks/laptopbat2ups/tlf.html)
