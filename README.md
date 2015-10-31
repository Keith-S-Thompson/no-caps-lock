no-caps-lock
============

Disabling Caps Lock for Microsoft Windows

See also my recent blog post [Where should the control key
be?](http://the-flat-trantor-society.blogspot.com/2013/12/where-should-control-key-be.html),
which covers Windows and other operating systems (mostly Linux).

Modern computer keyboards almost universally place the Caps Lock key
to the left of the **A** key, and the Control key down in the lower
left corner, below Shift.

I personally *hate* this layout. I prefer to have the Control key
immediately to the left of **A**, and Caps Lock (which I rarely if
ever use) either safely out of the way or disabled altogether.

I do not criticize those who prefer the "standard" layout; if it
works for you, that's great, and you don't need to read this.

[This question](http://superuser.com/questions/36920/how-can-i-remap-a-keyboard-key)
on [superuser.com](http://superuser.com/) discusses other
methods for remapping keys on MS Windows; most of them use
separate applications.

If you download the `nocapslock.reg` file (included in this repository)
and open it via Windows Explorer, assuming you have sufficient system
permissions to modify the registry, it will remap the Caps Lock key
to act as a Control key.

The changes won't take effect until after you reboot.

Unfortunately, there appears to be no way to make this change on
a per-user basis. I advise against using this on a shared Windows
computer unless you're sure that *all* the other users are ok with
this non-standard layout.

**DISCLAIMER :** I've used the `nocapslock.reg` file on a number of
Windows systems over the years, from Windows 98 or so up to Windows 10.
I've never had a problem with it, but I can accept no responsibility
for any damage it may do.  If you use this, you will be modifying
your system registry, which has the potential to do Very Bad Things
to your system.

You can also do the same thing by manually modifying the registry using
`regedit.exe`.  I do not advise this approach, since manually modifying
your registry is even more dangerous than opening a `.reg` file.
But you can try it if, for example, you want to swap the Control and
Caps Lock keys rather than disabling Caps Lock altogether; I don't
have a `.reg` file for that.

I do not remember where I originally got this information, or in
particular where I got the `nocapslock.reg` file.  I'll update this
with a proper attribution if I'm able to track it down.

The key you need to modify is:

    "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout\Scancode Map"
    Type = REG_BINARY

(Note: it's "`Keyboard Layout`", *not* "`Keyboard Layouts`".)

To swap left control and caps lock:

    Data = 00000000 00000000 0300000000 3A001D00 1D003A00 00000000
    0x00000000 Header: Version. Set to all zeroes. 
    0x00000000 Header: Flags. Set to all zeroes. 
    0x00000003 Three entries in the map (including null entry).
    0x001D003A Left CTRL key --> CAPS LOCK (0x1D --> 0x3A).
    0x003A001D CAPS LOCK --> Left CTRL key (0x3A --> 0x1D). 
    0x00000000 Null terminator. 

To map caps lock to control (no caps lock):

    Data = 00000000 00000000 0200000000 1D003A00 00000000
    0x00000000 Header: Version. Set to all zeroes. 
    0x00000000 Header: Flags. Set to all zeroes. 
    0x00000002 Two entries in the map (including null entry).
    0x003A001D CAPS LOCK --> Left CTRL key (0x3A --> 0x1D). 
    0x00000000 Null terminator. 
