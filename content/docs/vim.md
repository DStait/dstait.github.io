---
title: "VIM"
---


## Misc
### Delete up to the next character
```
dt" # To the next "
dt' # To the next '
```

### Delete in character
```
di" # Delete everything in the next ""
di{ # Delete everything in the next {}
```

### Delete without overwriting clipboard
Prefix command with underscore

    _dd

## Panes
Action | Command | Example
---|---|---
Split horizontal | `:sp filename or  Ctrl+W, s` |
Split Vertical | `:vsp filename or Ctrl+W, v ` |
Swap places | `ctrl+x` |  
Quit pane | `ctrl+q` | 
Increase/decrease height | `Ctrl+W +/-` |  `20<C-w>+`
Increase/decrease width | `Ctrl+W >/< ` | `30<C-w><`
Set height | `Ctrl+W _` | `50<C-w>_` |
Set width | `Ctrl+W \|` | `50<C-w>\|` |
Equalize width and height of all windows | `Ctrl+W =` |
Change to horizontal split | `Ctrl+w t Ctrl+K ` |
Change to vertical split | `Ctrl+w t Ctrl+H ` |


## Files / buffers

Action | Command 
---|---
Go to first file. | `:bf` 
Go to last file | `:bl` 
Go to next file. | `:bn` 
Go to previous file. | `:bp` 
Close file. | `:bw` 
open a new file | `:e filename`  
Cycle through files | `CTRL+Shift Up/Down` 
Open new file | `:tabe/tabnew`


## Folding
Action | Command
---|---
Fold | `zc` 
unfold | `zo` 
unfold everything in section | `zR` 
Fold Everything | `zC` 
Fold in all sections | `zM` 

## Tabs
Action | Command 
---|---
Go to next tab | `gt`
Go to previous tab | `gT`
Go to tab in position i | `{i}gt`
