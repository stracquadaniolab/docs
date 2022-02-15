# Working remotely 

The cell cluster is fully capable of remote development using VSCode; this is
convenient as you will be able to use the computing power of the cluster, while
using the usual IDE for developing your code. Check the full guide
[here](https://code.visualstudio.com/docs/remote/remote-overview), particularly
you need **Connect to a remote host** part. 

These are the main steps:

1. You should install an SSH key on your cell account, such that you can access
  cell without password.
2. Open `vscode` and go to the `Remote explorer` and look for SSH Targets.
3. Select `cell01` from the list and connect.
4. VSCode will now connect to `cell01` and you will have access to the
   filesystem and computing resources from your local IDE.

## Remote terminal

Oftentimes, the VPN connection or your own wifi are not very stable and you
could end up losing a remote connection on the terminal in the midst of a test.
A solution for this  would be using tools such as
[tmux](https://github.com/tmux/tmux/wiki) or
[screen](https://www.networkworld.com/article/3441777/how-the-linux-screen-tool-can-save-your-tasks-and-your-sanity-if-ssh-is-interrupted.html).
We recommend installing tmux through conda.

## FAQs
