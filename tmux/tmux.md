Installing Tmux
You can easily install Tmux using the package manager of your distro.

Installing Tmux on Ubuntu and Debian 
------------------------------------
    $ sudo apt install tmux

Installing Tmux on CentOS and Fedora
------------------------------------
    $ sudo yum install tmux

Installing Tmux on macOS
------------------------
    $ brew install tmux
    
    
 Starting Your First Tmux Session
 --------------------------------
To start your first Tmux session, simply type tmux in your console:
        
        $tmux
        
        
 Get a list of commands
 -----------------------
 You can now run your first Tmux command. For example, to get a list of all commands, you would type:
 
        Ctrl+b ?
        
   Creating Named Tmux Sessions 
   ----------------------------
     $tmux new -s session_name
     
     
   Detaching from Tmux Session
   ---------------------------
    $Ctrl+b d
    
  Re-attaching to Tmux Session
  ----------------------------
    tmux ls
 The name of the session is the first column of the output.

    0: 1 windows (created Sat Sep 15 09:38:43 2018) [158x35]
    my_named_session: 1 windows (created Sat Sep 15 10:13:11 2018) [78x35]
    
 For example, to attach to session 0, you would type:
    
     $tmux attach-session -t 0
     
     
 Working with Tmux Windows and Panes 
 -----------------------------------
    Ctrl+b c Create a new window (with shell)
    Ctrl+b w Choose window from a list
    Ctrl+b 0 Switch to window 0 (by number )
    Ctrl+b , Rename the current window
    Ctrl+b % Split current pane horizontally into two panes
    Ctrl+b " Split current pane vertically into two panes
    Ctrl+b o Go to the next pane
    Ctrl+b ; Toggle between the current and previous pane
    Ctrl+b x Close the current pane
