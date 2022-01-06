https://www.youtube.com/watch?v=Lqehvpe_djs&list=PLidcsTyj9JXKPJk1X3eKquMfXShMzpOfI&index=1

---

create tmux session:
- ``` tmux new -s HTB ```

prefix key:
- ``` Ctrl + B ```
- PK

Create new window:
- PK, press ``` c ```

Swith window:
- PK, press window number

Tmux config:
- https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/

Session:
- ``` tmux ls ``` show sessions
- ``` tmux attach -t {session_name} ``` attach session
- PK, ``` d ``` detach session

Copy mode, search
- PK, ``` [ ```
- Ctrl + s



---

Commands:

- ``` C-b ```         Send the prefix key (C-b) through to the application.
- ``` C-o ```         Rotate the panes in the current window forwards.
- ``` C-z ```         Suspend the tmux client.
- ``` ! ```           Break the current pane out of the window.
- ``` " ```           Split the current pane into two, top and bottom.
- ``` # ```           List all paste buffers.
- ``` $ ```           Rename the current session.
- ``` % ```           Split the current pane into two, left and right.
- ``` & ```           Kill the current window.
- ``` ' ```           Prompt for a window index to select.
- ``` ( ```           Switch the attached client to the previous session.
- ``` ) ```           Switch the attached client to the next session.
- ``` , ```           Rename the current window.
- ``` - ```           Delete the most recently copied buffer of text.
- ``` . ```           Prompt for an index to move the current window.
- ``` 0 to 9 ```      Select windows 0 to 9.
- ``` : ```           Enter the tmux command prompt.
- ``` ; ```           Move to the previously active pane.
- ``` = ```           Choose which buffer to paste interactively from a list.
- ``` ? ```           List all key bindings.
- ``` D ```           Choose a client to detach.
- ``` L ```           Switch the attached client back to the last session.
- ```[ ```            Enter copy mode to copy text or view the history.
- ``` ] ```           Paste the most recently copied buffer of text.
- ``` c ```           Create a new window.
- ``` d ```           Detach the current client.
- ``` f ```           Prompt to search for text in open windows.
- ``` i ```           Display some information about the current window.
- ``` l ```           Move to the previously selected window.
- ``` m ```           Mark the current pane (see select-pane -m).
- ``` M ```           Clear the marked pane.
- ``` n ```           Change to the next window.
- ``` o ```           Select the next pane in the current window.
- ``` p ```           Change to the previous window.
- ``` q ```           Briefly display pane indexes.
- ``` r ```           Force redraw of the attached client.
- ``` s ```           Select a new session for the attached client interactively.
- ``` t ```           Show the time.
- ``` w ```           Choose the current window interactively.
- ``` x ```           Kill the current pane.
- ``` z ```           Toggle zoom state of the current pane.
- ``` { ```           Swap the current pane with the previous pane.
- ``` } ```           Swap the current pane with the next pane.
- ``` ~ ```           Show previous messages from tmux, if any.


