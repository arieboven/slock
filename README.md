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
This build contains the dwmlogo patch, which instead of using the colors for the whole screen, it draws the dwm logo which change color based on the state.

### Logodwm in combination with other patches
I have not tested the logodwm patch with all other patches, so if there is some incompatibility, make an issue or create a pull request.


### Dwmlogo patch conflict with dpms patch
Although it gives not an error in the `main` function while patching with `dpms` it does not work properly. To fix it edit the `main` function so it looks like this:

    main(int argc, char **argv){
        ...

        /* did we manage to lock everything? */
        if (nlocks != nscreens)
            return 1;

        /* code from dpms patch */
        ...

        XSync(dpy, 0);

        ...

        /* everything is now blank. Wait for the correct password */
        readpw(dpy, &rr, locks, nscreens, hash);
        for (nlocks = 0, s = 0; s < nscreens; s++) {
            XFreePixmap(dpy, locks[s]->drawable);
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
