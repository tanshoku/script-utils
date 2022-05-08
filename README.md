# shell utils

A bunch of little utilities I've been writing in shell script to make my life with \*nix terminal better. Pretty much all of those scripts were only tested on my machine, with Debian Buster, so I can't tell if they'll run correctly for you or not. If you find a problem with any of these scripts, please open an issue and I might try to solve it if it's within my abilities.


### automount & autoumont

Utility to mount and umount luks, extX, fat, vfat and iso9660 partitions semi-autoomatically, without needing a ton of daemons in memory such as gvfs.
It can be called in several different ways:

- `automount` will give you a list of partitions you can mount
- `automount sdXN` will mount the supplied partition
- `autoumount` will give you a list of partitions you can umount
- `autoumount sdXN` will umount the supplied partition

The options `-cd` will change the directory of the running shell to the mountpoint, and `-fm` will open a new tab on thunar with the mounted partition, using the script called thunartab.

**Known bugs:**
- if a LUKS partition is already unlocked and you try to mount it, it will fail in some weird ways.

**TODO:**
- add support for more partition types


### backup

A backup script I wrote to replace deja-dup & co after it mangled a backup and made it unrecoverable. It requires `zsh`, `awk` and `rsync` for basic functionality, plus `ssh`, `nmap` and `parallel` for the remote options. Usage is quite straightforward:

- `backup` or `backup backup` will backup to a local drive;
- `backup remote` will backup to a remote machine (requires a running ssh server and rsync on the destination machine as well);
- `backup remote check` will check the integrity of remote files against local onss;
- `backup restore` and `backup remote restore` will restore the files to their origin (needs fixing);
- appending `backup remote` with `suspend` will suspend the destination server after the backup is finished (requires that systemctl suspend is allowed on sudoers on the server)

There is an internal setup script to generate the configuration file on the first run.

**Known bugs:**
- restore functions have been broken since the current backup was reduced to a file pointing to what needs to be kept from the incremental backup


### bigtimer & timer

Shows a countdown timer on your terminal. Usage goes like `timer 1d 23h 45m 50s` or `timer 3600`


### ccache

Cache the output of any command you wish! Compresses the data with gzip and encodes it with base64 for easier storage! Use `ccache -h` for more info. Plese check if your command runs correctly before caching it, or you'll end up with a corrupt cache file. Also notice that this only caches stdout.


### colortest, colortest-readable & colortest-small

Once I saw a really cool colortest script on a forum, but never found it online, so I wrote my own, algorithmically. The `-readable` version is basically the same, but the code is more legible, and the `-small` version simply prints the 16 terminal colours as 4x1 boxes.


### colourpicker

A wrapper script for `grabc`, it also prints the colour on your terminal, if it supports 24-bit colours, the html and rgb values, and copies the html/hex value to the clipboard.


### converts

Converts raw second values into more human-readable formats. Usage is as simple as `converts 3600` or `converts -l 1652032992` for a long version.


### debppa

Wrapper to automate installing Ubuntu PPAs on Debian. Use  `sudo debppa -a ppa` to add a PPA, or `sudo debppa -r ppa` to remove it.


### download_zip

Dowloads and unzips a ZIP file to a directory under the pwd.


### factorial

Calculate the factorial of N using recursive functions on zsh. Can't go over 31 as far as I know.


### finddupimg

A script that attempts to find similar/duplicate images from their content. It does so by calculating the sum of all R, G and B pixels on an image, and checking that against the R, G, B sums of the other JPEG and PNG images on the current directory, and if the difference is less than a given threshold, it will move both images to a new directory called  `tempdup/[first image name]`.

Usage is as simple as `finddupimg 2048`. Recommended threshold values are between 32 and 8192; below 32 it is too strict and will only match exact or near exact duplicates, and above 8192 or so it will start matching images with similar colours. You should try for yourself and see what value best suits your current set of images.

The purpose of this program is to find duplicate images along large collections of files, such as that it would be to exhausting and pointless for a user to do the search themselves, where the images can't be matched 1:1 and searched with simple checksums, such as when there are copies of the same image in different formats, copies with slightly different cropping or dimensions, or where lossy compression has changed the file ever so slightly that it cannot be matched through cryptographic hashes, such as md5 and sha sums.

The second purpose this program is to create a proof of concept of how such processes, such as reverse image search, can be achieved through well-crafted algortihms, instead of the regular and costly solutions of AI and machine learning.


### flac2ogg, flacalbum2ogg, mp32ogg

Useful scripts to convert music in flac and mp3 to ogg using `ffmpeg`. `flacalbum2ogg` also requires `mp3splt`.


### get-here

Downloads something from an url that's on the clipboard to the current directory, using wget. Multiple files can be separated by line breaks. Can be used with file managers such as Thunar, Dolphin or from the terminal. 


### hiragana2katakana

Translate hiragana characters to their katakana counterparts. Usage is `hiragana2katakana ひらがな`


### hopper

Half-Life 2 hopper mine noises. `hopper t` places a mine, `hopper n 10` activates the proximity beep and beeps 10 times.


### i3lock-color

Wrapper script for [i3lock-color](https://github.com/Raymo111/i3lock-color) and [corrupter](https://github.com/r00tman/corrupter). Automagically grabs colours from X resources, so it matches your terminal theme and so on. Also requires `scrot` and `imagemagick`.


### krita_recorder

A script to record timelapse videos by taking one screenshot a second. Can also compensates for various sources of lag by changing the delay by a small amount. Requires `xdotool`, `scrot` or `main`. Options can be adjusted inside the script.


### lapse

Make timelapse videos more easily from the command-line. I made this since I find it quite annoying and slow to work with the point-and-click, drag-and-drop interfaces of video editors, and I often need to do a timelapse video with something like ten separate files. Run `lapse -h` to find out more about how to use this script. Optionally requires [mikmod2wav](https://github.com/laomaiweng/mikmod2wav) to convert tracker/module music files to something that can be embedded into an MP4


### magic_suspend

Locks the screen and suspends after N seconds, and sends a notification of how much time is left. Can also be interrupted by calling the same script again. Usage is `magic_suspend [seconds]`. Obviously requires something to lock the screen, like i3lock or xscreensaver. Intended to be called from a keyboard shortcut from a window manager.


### mkpdf

PDF maker. Usage is `mkpdf page1.png page2.png pageN.png`. Output is YYMMDD.pdf. Please notice that the pages must be all the same size and orientation, or else some of them will end up cropped.


### morsebeep

Beep morse code messages through the internal speaker. Usage is `morsebeep "your message here"`. Requires bsdgames/morse and beep.


### mpd_nodaemon & ncmpcpp_start

Run Music Player Daemon without being a daemon 24/7. This program achieves this by spawning MPD and related programs, then closing them after nothing has been played for a set amount of time (default 10 minutes), and secured that your preffered MPD client is closed. It also optionally respawns polybar, which tends to die if you have the MPD widget on it and you kill the MPD process. You can use the script ncmpcpp_start to see how you can start this daemon alongside your preffered MPD client. 


### musicon

Control multiple music players without having to change your keybindings every time.


### noises

Telephone call and message noises. Originally intended to be used with IM clients such as the old Skype for GNU/Linux.


### palette

Generate palettes algorithmically. Inspired by a concept by viznut/pwp. Try `palette --help` for usage info. Requires zsh, imagemagick and optionally a 24-bit colour capable terminal emulator.


### passave

Secure (more-or-less) password manager. As a bonus, it saves your encrypted passwords as a cool-looking PNG image. Try `passave --help` for usage info or `passave -b` for an interactive mode. Requires imagemagick, aspell, aspell-en, zsh, and awk/gawk.


### permutate

Prints all possible permutations for the characters on a word. Usage is `permutate word`. Please notice that it gets exponentially complex the more characters you add.


### scan

Wrapper for scanimage. Use `scan -?` for usage info. Please notice you might need to change the variables in the file for it to work correctly with your particular scanner model.


### sd_sleep

A small daemon to make one or more drives sleep using hdparm, since `hdparm -s` doesn't always work. Requires zsh, hdparm, dstat and proc. Configuration is inside the script itself, there are two arrays, interval and disks; the position of the elements on the interval arrays are the amount of seconds until the drive on the same position on the disks array is put to sleep if no activity has happened.


### shot

Wrapper script for `maim` and `scrot` that allows you to copy the resulting screenshot to the clipboard, save it and so on. Use `shot -h` for usage info.

I'm currently looking for a way to make it only use either `maim` or `scrot` to avoid unneeded dependencies, but `scrot` sucks at selections and leaves garbage on the screen when you're not using a compositor, when you call it from scripts with a keyboard shortcut it dies because it captures the keypresses you used to call the script, and when it works, it doesn't show the selection crosshair, which is confusing because you can't know the screenshooter is running; on the other hand, `maim` can't capture window borders without some cumbersome hacks that break it on different WMs or themes.


### shwll

Copycat of the popular 5-letter word game. Grabs daily words procedurally from aspell dictionaries. Tries its best to keep you from cheating. Saves score data. Copies emojis to your clipboard when you're finished. Press ESC or ^C to quit. Except for grabbing the words from the dictionary, everything else is pure ZSH. All of this in less than 100 lines of code.


### spurdify

Mäke your text loök like this :-DDD Usage is as simple as `spurdify "sentence"` or `spurdify < file`. Written in plain ZSH. Yes, it supports having text piped into it, as well. Excerpt of the first couple stanzas of Edgar Allan Poe's "The Raven" after being destroyed by this script:

```
Once upon a midnighd dreaary :-DDD wwhile ponndered weak annd weary :-DDDDD
Over many a quaintt  antcurious volume of förgootteloree :-DDDDDDD
Vhilee I nödded :-DD nearlly nappiing :-DDDD suddenly there cäme a dapping :-DDDDDDDDD
As fo some one gently rapping :-DDDDrappig at my chamber door :-DDDDD
`'Tis some visitor :-DDD' I muuttered :-DDDDDDDDDDD `tapping  at my chamber  ddoor -
Only this :-DDDDDDDDDD and nothing mmore :-DDD'

Ah :-DDDDD ditncdly I remember it was iin the bleak Decemper :-DDD
Ant eachhseparade dying emmber wrought its  ghost ubon the flöoorr :-DDDDDDDDD
Eagerrly I wished the morrow :-DDDDDDDDD - vainly I  hadd sought o bborrow
Froöm my books surcease of orrow - srrow or the losd Lenore -
Fo rdhe rare and adiant maiden whom the angel snamed Lenore -
Namelss here for evermore :-DDD
```


### startfbt

Wrapper script for fbterm, sets a background and a colour scheme. Requires imagemagick and zsh. You also need to add the following near the end of your .zshrc.local if you want it to work automatically and start tmux:

```zsh
# Quick and dirty hack for fbterm+tmux
fbterm_pls=0
if [[ "$TERM" = "linux" ]] && [[ "$fbterm_pls" = "1" ]]; then
  if [[ -z $FBTERM ]]; then
    clear
    ~/.bin/startfbt main ~/Pictures/a-background.png
    exit 0
  elif [[ "$FBTERM" = "1" ]]; then
    PATH="$HOME/.bin:$HOME/.local/bin:$PATH"
    startfbt coloursetup
    tmux
    exit 0
  fi
fi
```


### stopwatch

A simple stopwatch for your terminal.


### stresst

Simple CPU stress tester based on running many md5sum instances in parallel. Uses bash so it's more portable.


### sync-nitrogen-wmaker

Synchronize wallpapers set with nitrogen to the Window Maker settings.


### tmux_start

Connect to an existing tmux session or creates a new one if there are none, using your preffered terminal emulator. Intended to be called from a keyboard shortcut inside a window manager.


### toggle-presentation-mode

Toggle presentation mode on Xfce4 with xfce4-power-manager running and shows a notification. Intended to be called from a keyboard shortcut.


### volume-set

Set main pulseaudio volume. Usage is `volume-set [+|-][percentage]%`


### wallpaper_cycler

Cycle wallpapers from a list on `~/.config/wallpaper_list`, which is a list of paths to files separated by new lines, e.g:

```
/home/liz/Pictures/squidfingers/pattern_001.gif
/home/liz/Pictures/squidfingers/pattern_002.gif
/home/liz/Pictures/squidfingers/pattern_003.gif
/home/liz/Pictures/squidfingers/pattern_004.gif
/home/liz/Pictures/squidfingers/pattern_005.gif
/home/liz/Pictures/squidfingers/pattern_006.gif
/home/liz/Pictures/squidfingers/pattern_007.gif
/home/liz/Pictures/squidfingers/pattern_008.gif
/home/liz/Pictures/squidfingers/pattern_009.gif
/home/liz/Pictures/squidfingers/pattern_010.gif
... etc
```

This is intended to be called from crontab, during X login, or from a keyboard shortcut. Requires zsh, grep and nitrogen.


### wallchange

Similar to `wallpaper_cycler`, but just sets a random wallpaper with hsetroot.


### webm2gif

Convert webm animations to GIF.


### window_center

Center the currently active window. Currently a dirty collection of hacks, but I intend to make it work properly with any number and setup of monitors some time in the future.


### wttr

Wrapper for wttr.in and graph.no. Requires `ccache` from this repo to cache data so your IP doeesn't get banned from pinging the services every time you run the script. Use `wttr -h` for options. The name of your city can be set on an environment variable called `WTTRCITY`, or on the beginning of this script.


### xrdb-colour-preview

Preview the colour scheme from xresources files. Basically `palette` and `colortest` cobbled together. Requires a 24-bit colour capable terminal emulator.
