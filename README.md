slock - simple screen locker
============================
simple screen locker utility for X.


Requirements
------------
In order to build slock you need the Xlib header files.


Installation
------------
Edit config.mk to match your local setup (slock is installed into
the /usr/local namespace by default).

Afterwards enter the following command to build and install slock
(if necessary as root):

    make clean install

Dwmlogo patch
-------------
This build contains the dwmlogo patch (look at the date to see which one is the latest), which instead of using the colors for the whole screen, it draws the dwm logo which changes color based on the state.

### Logodwm in combination with other patches
I have not tested the logodwm patch with all other patches, so if there is some incompatibility, make an issue or create a pull request.


### Dwmlogo patch conflict with dpms patch
With the `dpms` patch there is a conflict in the `main` function. Edit the `main` function so it looks like this, for it to work properly:

    main(int argc, char **argv){
        ...
            XFreeGC(dpy, locks[s]->gc);
        }

        /* reset DPMS values to inital ones */
        DPMSSetTimeouts(dpy, standby, suspend, off);
        XSync(dpy, 0);
        XCloseDisplay(dpy);

        return 0;
    }


Running slock
-------------
Simply invoke the 'slock' command. To get out of it, enter your password.
