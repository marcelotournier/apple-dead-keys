# Remap keys in the keyboard in Ubuntu

Article from https://dev.to/0xbf/remap-keys-in-the-keyboard-in-ubuntu-5a36

We could use xmodmap to remap keys in ubuntu.

First, generate the current keycode map:

Then open the .Xmodmap file, find the key you want to remap. In my case, the PageUp key is too close to the left arrow key and the PageDown key is too close to the right arrow key. Many times when I want to press the left arrow key, I pressed the PageUp key by accident. And I barely use the PageUp and PageDown key, so I want to remap the PageUp to LeftArrow and PageDown to RightArrow.

Once you open the .Xmodmap file, you will see a list of keycode and their functions, for example:

And it is easy to find the keycode for the LeftArrow and RightArrow keys:

	keycode  83 = KP_Left KP_4 KP_Left KP_4
	keycode  85 = KP_Right KP_6 KP_Right KP_6

But I don't know the keycode for the PageUp and PageDown key, so I need to find that first.

To detect what the keycode is for specific key, use command:

In my system, when I press PageUp, it shows 112:

So we can find that in the .Xmodmap file:

	keycode 112 = Prior NoSymbol Prior
	keycode 117 = Next NoSymbol Next

Now to remap those keys, we just need to change them to the Left and Right arrow:

	keycode 112 = KP_Left KP_4 KP_Left KP_4
	keycode 117 = KP_Right KP_6 KP_Right KP_6

Now save the file, and activate the key map:

Now if you press the "PageUp" key, it will work like the LeftArrow key.

But this change will only work in this login session. After you reboot the system, the keyboard will be reset to the default keys. To make it work on every reboot, we need to set an command to the "Startup Application":

	$ xmodmap .Xmodmap
	$ xkbcomp $DISPLAY $HOME/.xkbmap

Now open "Startup Application":

Click Add, set name to "Remap Keys", and the command to:

	xkbcomp /home/user/.xkbmap ":0"

Make sure you replace the user above to your username. Then reboot your system, the remap should work on next boot.

## Reference
- https://medium.com/@saplos123456/persistent-keyboard-mapping-on-ubuntu-using-xmodmap-cd01ad828fcd
- https://unix.stackexchange.com/questions/49650/how-to-get-keycodes-for-xmodmap

