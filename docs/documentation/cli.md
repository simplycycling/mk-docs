# Command Line Stuff

That's right, stuff. Good stuff, mainly.

### Bounce gpg-agent on OS X

    pgrep scdaemon
    pgrep gpg-agent
    kill -9 both processes
    sudo launchctl unload /System/Library/LaunchDaemons/com.apple.ifdreader.plist



