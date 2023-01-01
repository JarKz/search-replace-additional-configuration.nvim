# :monocle_face: search-replace.nvim

![a](https://user-images.githubusercontent.com/226654/210119753-8951db87-e7e1-48c7-a75d-e3c5f222d702.gif)

A Neovim search and replace plugin that builds on the native search and replace experience.

Neovim has an excellent search and replace system, however it can be slow to use due to the
number of key-strokes required to use it.

## :sparkles: Features

* Quick opening of `:%s///gcI`
* Quick opening of `:%s/<special selection>//` where `<special selection>` refers to a
  predefined selection under the cursor. Please see: [What are Special Selections?](https://github.com/roobert/search-replace.nvim#what-are-special-selection)
* A UI to preview the current special selections under the cursor
* Quick opening of `:%s/<visual selection>//gcI` where `<visual selection>` is a
  visual-charwise selection
* Configuration of the default flags passed to search and replace, e.g: `gcI` when
  searching across a buffer with `:%s///gcI`
* Support for search and replace over multiple buffers
* A command to search and replace over a visual-block/visual-linewise/visual-charwise
  selection
* Example key mappings

## :zap: What are Special Selections?

With the following example text:

``` lua
lvim.builtin.which_key.mappings["r"]["w"]
```

And the cursor position shown as `|`

The following examples are `true`.

### CWord

`CWord` is replaced with the `word` under the cursor (like `*`)

``` lua
# Selection:
lv|im.builtin.which_key.mappings["r"]["w"]
^^^^^
# Value:
lvim
```

``` lua
# Selection:
lvim.bui|ltin.which_key.mappings["r"]["w"]
     ^^^^^^^^
# Value:
builtin
```

``` lua
# Selection:
lvim.builtin.whi|ch_key.mappings["r"]["w"]
             ^^^^^^^^^^
# Value:
which_key
```

``` lua
# Selection:
lvim.builtin.which_key.mapp|ings["r"]["w"]
                       ^^^^^^^^^
# Value:
mappings
```

### CWORD

`CWORD` is replaced with the `WORD` under the cursor (like greedy `word`)

``` lua
# Selection:
lv|im.builtin.which_key.mappings["r"]["w"]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
# Value:
lvim.builtin.which_key.mappings["r"]["w"]
```

``` lua
# Selection:
lvim.builtin.whi|ch_key.mappings["r"]["w"]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
# Value:
lvim.builtin.which_key.mappings["r"]["w"]
```

### CExpr

`CExpr` is replaced with the `word` under the cursor, including more to form a C expression.

``` lua
# Selection:
lvim.bui|ltin.which_key.mappings["r"]["w"]
^^^^^^^^^^^^^
# Value:
lvim.builtin
```

``` lua
# Selection:
lvim.builtin.wh|ich_key.mappings["r"]["w"]
^^^^^^^^^^^^^^^^^^^^^^^
# Value:
lvim.builtin.which_key
```

``` lua
# Selection:
lvim.builtin.which_key.map|pings["r"]["w"]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
# Value:
lvim.builtin.which_key.mappings
```

### CFile

`CFile` is replaced with the path name under the cursor (like what `gf` uses)

``` lua
# Selection:
lvim.bui|ltin.which_key.mappings["r"]["w"]
^^^^^^^^^^^^^
# Value:
lvim.builtin
```

``` lua
# Selection:
lvim.builtin.wh|ich_key.mappings["r"]["w"]
^^^^^^^^^^^^^^^^^^^^^^^
# Value:
lvim.builtin.which_key
```

``` lua
# Selection:
lvim.builtin.which_key.map|pings["r"]["w"]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
# Value:
lvim.builtin.which_key.mappings
```

## :movie_camera: Demos

## :microscope: Available Commands

### Single Buffer Empty Search

* `SearchReplaceSingleBuffer`
* `SearchReplaceMultiBuffer`

### Single Buffer Special Key Search

* `SearchReplaceSingleBufferCWord`
* `SearchReplaceSingleBufferCWORD`
* `SearchReplaceSingleBufferCExpr`
* `SearchReplaceSingleBufferCFile`

### Multi Buffer Special Key Search

* `SearchReplaceMultiBufferCWord`
* `SearchReplaceMultiBufferCWORD`
* `SearchReplaceMultiBufferCExpr`
* `SearchReplaceMultiBufferCFile`

### Single/Multi Buffer UI Special Key Search Hinting

* `SearchReplaceSingleBufferSelections`
* `SearchReplaceMultiBufferSelections`

### Visual Charwise As Search

* `SearchReplaceSingleBufferWithinBlock`

### Visual (Blockwise/Linewise) Selection Search

* `SearchReplaceVisualSelection`
* `SearchReplaceVisualSelectionCWord`
* `SearchReplaceVisualSelectionCWORD`
* `SearchReplaceVisualSelectionCExpr`
* `SearchReplaceVisualSelectionCFile`

## :rocket: Usage

### Plugin Management

#### Lazy.nvim

``` lua
{
  "roobert/search-replace.nvim",
  config = function()
    require("search-replace").setup({
      -- optionally override defaults
      default_replace_single_buffer_options = "gcI",
      default_replace_multi_buffer_options = "egcI",
    })
  end,
}
```

### Key Bindings

#### Lunarvim / Which-Key

``` lua
keymap = lvim.builtin.which_key.mappings

keymap["r"] = { name = "SearchReplaceSingleBuffer" }

keymap["r"]["s"] =
  { "<CMD>SearchReplaceSingleBufferSelections<CR>", "SearchReplaceSingleBuffer [s]elction list" }
keymap["r"]["w"] = { "<CMD>SearchReplaceSingleBufferCWord<CR>", "[w]ord" }
keymap["r"]["W"] = { "<CMD>SearchReplaceSingleBufferCWORD<CR>", "[W]ORD" }
keymap["r"]["e"] = { "<CMD>SearchReplaceSingleBufferCExpr<CR>", "[e]xpr" }
keymap["r"]["f"] = { "<CMD>SearchReplaceSingleBufferCFile<CR>", "[f]ile" }

keymap["r"]["b"] = { name = "SearchReplaceMultiBuffer" }

keymap["r"]["b"]["s"] =
  { "<CMD>SearchReplaceMultiBufferSelections<CR>","SearchReplaceMultiBuffer [s]elction list" }
keymap["r"]["b"]["w"] = { "<CMD>SearchReplaceMultiBufferCWord<CR>", "[w]ord" }
keymap["r"]["b"]["W"] = { "<CMD>SearchReplaceMultiBufferCWORD<CR>", "[W]ORD" }
keymap["r"]["b"]["e"] = { "<CMD>SearchReplaceMultiBufferCExpr<CR>", "[e]xpr" }
keymap["r"]["b"]["f"] = { "<CMD>SearchReplaceMultiBufferCFile<CR>", "[f]ile" }

lvim.keys.visual_block_mode["<C-r>"] = [[<CMD>SearchReplaceSingleBufferVisualSelection<CR>]]
lvim.keys.visual_block_mode["<C-b>"] = [[<CMD>SearchReplaceWithinVisualSelectionCWord<CR>]]
```
