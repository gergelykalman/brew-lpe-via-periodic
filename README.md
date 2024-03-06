# brew-lpe-via-periodic

Local Privilege Escalation on macOS if brew is installed in /usr/local

Since neither Apple nor the maintainers of homebrew particularly care
about exploits like this, I decided to go with Full Disclosure.

Homebrew on Intel (or with game porting toolkit) messes up the
permissions on macOS by allowing all admin users to write anything
under /usr/local/

Since macOS's periodic daemon will happily execute scripts from this
directory when it runs, we can trivially create a root-owned file via
MallocStackLogging and top. Periodic will run the script as the owner,
so we can escalate to root this way if we can control the contents.

To control the contents, we can simply set inheriting ACLs on the
parent directory, /usr/local/etc/ which allows us to edit the file
even though the standard UNIX permissions are restricted.

NOTE: This does NOT affect Apple Silicon macs, UNLESS they have game
porting toolkit installed, which I suspect will be a common
occurrence.

I wrote this exploit in 3 minutes, so don't expect miracles.

