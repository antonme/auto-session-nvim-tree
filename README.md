# Auto Session Nvim Tree Plugin

Synchronize [`auto-session`](https://github.com/rmagatti/auto-session) and [`nvim-tree`](https://github.com/nvim-tree/nvim-tree.lua)

### Description

This fixes a lot of problems with integrating [`nvim-tree`](https://github.com/nvim-tree/nvim-tree.lua) and [`auto-session`](https://github.com/rmagatti/auto-session). Both plugins try to control/listen for changes in the `cwd`, which causes infinite loops and corrupted session data.

This feature synchronizes the `cwd` between `auto-session` and the file tree plugin, which turns out to be powerful and fun to use.

##### :warning: This plugin is still experimental. Bugs to be expected.

### Do you really need this?

No. You may prefer to just change your `nvim-tree` settings to use the [`cd` instead of `lcd`](https://benatkin.com/2011/11/04/vims-lcd-command/):

```lua
  sync_root_with_cwd = true,
  actions = {
    change_dir = {
      global = true
    }
  },
```

and add a hook to your `auto-session` settings to open/close `nvim-tree` upon session restore/saving. This will *always* open `nvim-tree` though, whether or not you had it opened before.

```lua
local function close_nvim_tree()
  require('nvim-tree.view').close()
end

local function open_nvim_tree()
  require('nvim-tree').open()
end
require("auto-session").setup {
  log_level = "error",
  pre_save_cmds = {close_nvim_tree},
  post_save_cmds = {open_nvim_tree},
  post_open_cmds = {open_nvim_tree},
  post_restore_cmds = {open_nvim_tree},
  cwd_change_handling = {
    restore_upcoming_session = true, -- <-- THE DOCS LIE!! This is necessary!!
  },
}
```

However, I try to provide a more elegant solution here

### 📦 Installation/Setup

Any plugin manager should do, e.g. [Packer.nvim](https://github.com/wbthomason/packer.nvim)

Then add the following to your `init.lua`

```lua
local auto_session = require("auto-session")
local auto_session_nvim_tree = require("auto-session-nvim-tree")

auto_session.setup {
  log_level = "error",
  cwd_change_handling = {
    restore_upcoming_session = true, -- This is necessary!!
  },
}

auto_session_nvim_tree.setup(auto_session)
```
