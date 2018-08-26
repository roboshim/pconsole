pconsole
========
by Walter de Jong <walter@heiho.net> (C) 2001

pconsole COMES WITH NO WARRANTY. pconsole IS FREE SOFTWARE.
pconsole is distributed under terms described in the GNU General Public
License.

This is pconsole, the parallel console tool. pconsole was meant as an
interactive administrative shell tool for clusters.

pconsole allows you to connect to each node of your cluster simultaneously,
and you can type your administrative commands in a specialized window that
'multiplies' the input to each of the connections you have opened.
pconsole is best run from within X Windows, although it is possible to
employ it without X (in console mode) as well.
You need to install pconsole on only 1 machine in the cluster, this would
usually be your central administrative node.


MAKE AND INSTALL
----------------
Do a `./configure` and `make install` as root. If you first only want to
try pconsole, I suggest you do a `./configure --prefix=.` and `make install`
instead of just configure and make.
Try running `bin/pconsole.sh`. Or `bin/pconsole.sh machine1 machine2 machine2`.

pconsole was made to compile and run on any Unix-like platform. If you
encounter problems, check the list of possible problems at the end of
this README.

To reconfigure, do a `make mrproper` and `./configure` before typing
`make` again.

If you decide to make pconsole setuid root (as suggested), mind to
restrict access via a special group (such as 'admin' or 'operator') or
put it in a directory which the normal user doesn't have access to.
pconsole is a very powerful tool that should not fall into the wrong
hands. So, as root, execute:

```
    # chown root.admin pconsole
    # chmod 4110 pconsole
```

pconsole drops its root privileges when they're not needed, so the
program maintains maximum security.


HOW DOES IT WORK?
-----------------
pconsole does not work through daemons. The pconsole.sh script opens up a
number of connections to each node of your cluster of workstations. Then,
the pconsole binary attaches to the tty devices that these windows are
using, and it copies the input you type to all open connections.


TWEAKING AND TUNING
-------------------
pconsole.sh requires the `xterm` command. If you do not like xterm, you can
either edit the script or put the environment variable `P_TERM` in your
environment. Example:

```
    $ P_TERM=rxvt ; export P_TERM     # for sh

    > setenv P_TERM rxvt              # for csh
```

By default, the window geometry is 40x12 with font size 5x7. This is very
small. To change, either edit the script or use `P_TERM_OPTIONS` in your
environment. `P_TERM_OPTIONS` can have any options to the `P_TERM` command
you like. Example:

```
    $ P_TERM_OPTIONS="-geometry 80x25 -fn 10x20 -rv +sb"
    $ export P_TERM_OPTIONS
```

There is also a `P_CONSOLE_OPTIONS` environment variable, which specifies the
geometry of the main pconsole window.

By default, pconsole tries to use SSH to make connections. If you do not have
`ssh`, it will try to use `telnet`. If you do have ssh but pconsole fails to
find it, you should edit the script `ssh.sh` and adjust the line containing
`PATH`.
pconsole.sh uses the `P_CONNECT_CMD` environment variable (by default set
to 'ssh.sh'). Example of changing it:

```
    $ P_CONNECT_CMD=rlogin ; export P_CONNECT_CMD
```

If you want to use pconsole to connect your serial console lines, you should
probably set `P_CONNECT_CMD` to 'telnet', because serial console lines usually
do not support the SSH protocol.


USING PCONSOLE
--------------
Fire up pconsole.sh. Everything you type can and will be used against you.
Hit <Ctrl-A> to enter command mode. Type 'help' (or just 'h') for help.
Type 'c' to reconnect and leave command mode.
Hit <Ctrl-A> to re-enter command mode and type 'l' to list all connections.
You can use 'detach' to detach from a particular tty or host. If you have
multiple windows open to a particular host and enter 'detach <hostname>',
it will detach all ttys for that host.
Open up a new window on your local machine (same machine where you started
pconsole in the first place). Type the 'tty' command. It gives you the
name of the tty that the window is using. In the pconsole main window,
type 'attach <tty name>' or 'attach yourmachine#<tty name>'. Now type
'list' to see if the connection shows up.
Type 'quit' or 'exit' to quit pconsole.

If you own a serial port multiplexer that allows you to telnet to specific
port numbers on the IP of the multiplexer, you can have pconsole connect
to '<mux-IP>:<port number>'.


TIPS & TRICKS: TYPING PASSWORDS
-------------------------------
When logging in, you may need to enter a password. The pconsole main window
echoes everything you type, as it cannot 'know' that you are at a password
prompt. To keep the password from being visible, you should turn off the
echo. You can do this by typing 'echo off' in command mode, or you can
use the handy short-cut <Ctrl-S> when not in command mode. With echo
turned off, you can safely enter your password and hit <Ctrl-S> again
to toggle echoing back on.


TIPS & TRICKS: PCONSOLE FOR MANY MACHINES
-----------------------------------------
In theory, pconsole can work with an infinite number of connections, but
it's just not very practical to have more than 16 open windows on your
screen. If you want to run pconsole on many machines, try adding the
'-iconic' option to xterm:

```
    $ P_TERM_OPTIONS="-iconic" ; export P_TERM_OPTIONS
```

This will start all xterms in iconized mode, now just click one open
and let the others iconized, and you can work nice and easy.


IF IT DOESN'T WORK
------------------

- pconsole needs root privs. So, are you root?
  Or, is the pconsole binary setuid root?
- The pconsole.sh script uses 'ps' to find out on which tty the terminal
  processes are. You may need to edit this script and change the line
  containing 'ps'. If you don't know how to write shell scripts, ask a
  friend.
- Check the PATH variables in the top of the scripts pconsole.sh and
  ssh.sh. Most common paths are listed, but you may have your stuff in
  unusual directories.
- Read this README for more hints (see above).
- If all else fails, throw this package in the dumpster..!


LAST NOTES
----------

- You can get OpenSSH from http://www.openssh.org/
- The GNU General Public License is at http://www.fsf.org/copyleft/gpl.html
- The pconsole main distribution page is at https://walterdejong.github.io/pconsole/

I hope you'll enjoy using pconsole.


![Screenshot](/images/screenshot.jpg?raw=true "Screenshot")

