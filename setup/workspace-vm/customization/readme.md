# Customizations for the Workspace VM

---

## Prerequisites

* The Workspace VM (set up one [here](../readme.md))

---

## Terminal

1. Open up the Terminal.
2. Go to `File` > `Preferences...`.
3. Verify and configure the following settings:
  * `Appearance`
    * `Scrollbar position`: `No scrollbar`
    * `Keyboard cursor shape`: `IBeamCursor`
    * Options (checked only):
      * `Show a border around the current terminal`
      * `Show close button on each tab`
      * `Enable bi-directional text support`
  * `Behavior`
    * Options (checked only):
      * `Unlimited History`
  * `Shortcuts` (update only; leave the rest unchanged)
    * `Copy Selection`: `Alt+C`
    * `Find`: `F3`
    * `Move Tab Left`: `Ctrl+Shift+PgUp`
    * `Move Tab Right`: `Ctrl+Shift+PgDown`
    * `Paste Clipboard`: `Alt+V`
    * `Paste Selection`: `Alt+Shift+V`
    * `Preferences`: `Ctrl + ,`
4. Click `OK` to save changes.
5. Go to `~/.config/qterminal.org`, and open `qterminal.ini` using a GUI text editor.
6. Find and update the following lines:
  * `Split%20Terminal%20Horizontally=Alt+Shift+Left|Alt+Shift+Right`
  * `Split%20Terminal%20Vertically=Alt+Shift+Up|Alt+Shift+Down`
7. Save the changes, and exit the text editor.

---

## 
