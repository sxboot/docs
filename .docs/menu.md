# Menu

The menu is visible after *sxboot* is done initializing. It shows a list of all configured entries in the configuration file


## Main Menu Controls

### Selecting and booting an entry
You can use the *UP* and *DOWN* arrow keys to select an entry. If you attempt to go below the last entry, you will get the option to create a new entry.
Note that any new entries are temporary and will not be saved to the configuration file.

The selected entry is booted by pressing *ENTER*.  
Alternatively, you can wait for the amount of time specified in the configuration file (10 seconds by default), after which the first entry will be started automatically.
This countdown will be stopped when any key is pressed.

### Command Line
By pressing *C*, you will enter the [Command Line](Command_Line). You can return to the menu by typing *menu* and pressing *ENTER*, or boot an entry from there.

### Editing an entry
When pressing *E*, a list of all configured options of the selected entry will be shown. See **Runtime Configuration** below.

You can also create a new temporary menu entry by going beyond the last listed entry and pressing *E*.


## Runtime Configuration
After pressing *E* on a menu entry, a list of all configured options will be shown.

Here, you can use the arrow keys to select an option or create a new option by going below the last entry. Press *ENTER* to edit the selected option or press *ESCAPE* to return to the main menu.

A screen will be shown where you can edit the name ('key') or the value of the option. Select which one to edit by using the *UP* and *DOWN* arrow keys.
When finished editing, press *ENTER* to save the changes or press *ESCAPE* to cancel.

**Note:** You cannot edit the key for the option 'type' as that option is required for all entries and has a special meaning.  
**Note:** Any changes made using this method are not saved to the configuration file and will only stay until the next reboot.
