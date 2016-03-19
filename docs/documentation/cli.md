# Command Line Stuff

That's right, stuff. Good stuff, mainly.

### GPG stuff

**Bounce gpg-agent on OS X**

    pgrep scdaemon
    pgrep gpg-agent
    kill -9 both processes
    sudo launchctl unload /System/Library/LaunchDaemons/com.apple.ifdreader.plist

**Create new .gpg-agent-info (Linux)**

    gpg-agent --daemon --enable-ssh-support --write-env-file ~/.gpg-agent-info

