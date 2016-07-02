# Command Line Stuff

That's right, stuff. Good stuff, mainly.

### GPG stuff

Like many nerds, I'm a raging paranoid, especially when I read anything
about the NSA, the Supreme Court telling the FBI sure, you can hack anybody's
computer, without a warrant. No problem, have at it. So I use a Yubikey,
which is a hardware device that you can store your GPG keys on. This is
desirable, because you don't have to actually store your GPG keys on your
computer. But as with anything (especially gpg), there's a learning curve.
The following are a couple of tips that have helped me.

**Import GPG keys from a Yubikey on a fresh install**

This is basically a three step process:

Import your public key

    gpg --import < publickey.txt

Generate stubs

    gpg --card-status

and finally, if you are using your Yubikey for ssh authentication, create 
new .gpg-agent-info

    gpg-agent --daemon --enable-ssh-support --write-env-file ~/.gpg-agent-info

**Bounce gpg-agent on OS X**

Every now and then, when my laptop wakes from suspend, there is still a
gpg session cached, and it conflicts with the actual card. This requires
simply bouncing the service. 

    pgrep scdaemon
    pgrep gpg-agent
    kill -9 both processes
    sudo launchctl unload /System/Library/LaunchDaemons/com.apple.ifdreader.plist

After doing so, just use gpg for whatever you normally do, and you'll be
prompted for your pin, as you normally would.

For more info on setting up and using GPG keys on a Yubikey, I would 
strongly recommend the following site. It walks you through the setup from
beginning to end:

[Offline GnuPG Master Key and Subkeys on YubiKey NEO Smartcard](https://blog.josefsson.org/2014/06/23/offline-gnupg-master-key-and-subkeys-on-yubikey-neo-smartcard/)

### Tmux

(I have ` set as the prefix)

Split pane horizontally

    ` % (`+shift+5)

Split pane vertically 

    ` " (`+shift+')

Switch window

    ` directional arrow
    
Detach session

    ` d
    
Attach session

    tmux attach-session -t 1

### Miscellaneous 

Remove Docker images that are more than 24 hours old

    docker inspect -f '{{.Id}},{{.State.Running}},{{.State.FinishedAt}}' $(docker ps -qa) | \
      awk -F, 'BEGIN { TIME=strftime("%FT%T.000000000Z",systime()-60*60*24); } $2=="false" && $3 < TIME {print $1;}' | \
      xargs --no-run-if-empty docker rm >/dev/null 2>/dev/null