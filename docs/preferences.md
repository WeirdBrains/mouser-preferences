# Preferences mode

`esc :p` opens the preferences mode (a built-in mode plugin, like commander
and chat; `mouser plugin disable preferences` turns it off). It lists every
setting in `$MOUSER_HOME/preferences.toml` by section: Appearance, Window,
Keys, Plugins, Chat, Models, Local model and MCP server. Plugins has
`plugins.max_restarts`: how many crashes within 30 s a plugin may have before
it stays off (default 3; 0 never restarts; see [Mode plugin protocol](https://github.com/WeirdBrains/mouser/blob/main/docs/mode-protocol.md)).

Keys (alt+key by default; remap them in `modes.toml` under `[preferences.keys]`):

- alt+↑ / alt+↓ move, alt+PgUp / alt+PgDn page;
- alt+← / alt+→ change the value: the previous/next choice, on/off, or ±1 for numbers;
- alt+Enter edits: on/off toggles, choices open a dropdown (↑/↓, Enter picks,
  Esc closes), other fields take typed text (Enter saves, Esc cancels);
- alt+r resets the row to its default (the key is removed from the file).

Rows are clickable: the value edits it, `reset` resets it.

Every value is validated before it is saved (ranges, choices, http(s) URLs),
and the file is only written when Mouser can still read it: the change goes
through `toml_edit`, so comments, ordering and keys Mouser doesn't know are
kept, and it is written atomically (temp file + rename). A preferences.toml that
doesn't parse is never overwritten; the mode says so instead.

Theme, light/dark, the Esc wait, chat and model settings apply at once; rows
marked "new windows" (font, sidebar, MCP server) apply to windows opened later.

Secrets (the local model server key) go only to the OS keychain; the file
keeps a `keychain:NAME` reference and the mode shows "set (keychain)". With no
keychain available the secret is not saved. The model folder is shown but
moved with `mouser model local move <dir>`.

## As a plugin program

`plugins/preferences` is the same mode as a separate program
(`mouser-preferences`, `transport = "mode"`), built only on `mouser-mode-api`.
Installed, it replaces the built-in preferences mode (`mouser plugin list`
marks it `[overrides built-in]`):

```
cargo build --release -p mouser-preferences
mkdir -p /tmp/prefs-plugin && cp plugins/preferences/plugin.toml target/release/mouser-preferences /tmp/prefs-plugin/
mouser plugin install /tmp/prefs-plugin
mouser plugin permissions preferences run=allow prefs.write=allow
```

It reads and writes through Mouser's `prefs.*` calls, so Mouser still does the
validation and the comment-keeping write. For a secret, Enter makes Mouser
show its own key popup. The key you type goes from that popup to the keychain,
and the plugin only hears that it was saved.

## Context menu

Right-click a setting (or `.` while preferences has the keyboard): edit (Enter),
previous / next value (← / →, for on/off, choices and numbers), reset to default
(`r`, greyed when nothing is set), copy value (never for a key; those stay in the
keychain), copy the setting's name, previous / next setting. Off the settings:
previous / next setting and page up / down.

## Panel size

Mouser 1.4 lets you size the mode panel (`F` fullscreen, `]` / `[` taller /
shorter while the mode has the keyboard, or drag its bottom edge); this plugin
lays its panel out to that size (`dock` in `view.resized`). Older Mouser
versions keep the plugin's own height.
