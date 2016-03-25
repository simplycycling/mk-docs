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

Remove Docker images that are more than 24 hours old

    docker inspect -f '{{.Id}},{{.State.Running}},{{.State.FinishedAt}}' $(docker ps -qa) | \
      awk -F, 'BEGIN { TIME=strftime("%FT%T.000000000Z",systime()-60*60*24); } $2=="false" && $3 < TIME {print $1;}' | \
      xargs --no-run-if-empty docker rm >/dev/null 2>/dev/null