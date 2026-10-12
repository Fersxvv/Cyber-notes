# 📝 Creating, Moving and Deleting Files

> `$ cat summary.txt`
> The basic file operations: make it, copy it, move it, delete it.

## Creating
| Command | What it does |
|---|---|
| `touch notes.txt` | Creates an empty file (or updates the timestamp if it exists) |
| `mkdir folder` | Creates a folder |
| `mkdir -p a/b/c` | Creates nested folders in one go |
| `echo "text" > file.txt` | Writes text to a file, **overwriting** it |
| `echo "text" >> file.txt` | Adds text to the end of a file |

## Reading
| Command | What it does |
|---|---|
| `cat file.txt` | Prints the whole file |
| `less file.txt` | Scrolls through a big file (`q` to quit) |
| `head file.txt` | First 10 lines |
| `tail file.txt` | Last 10 lines |
| `tail -f file.txt` | Watches a file live as it grows (great for logs) |

## Copying and moving
| Command | What it does |
|---|---|
| `cp a.txt b.txt` | Copies a file |
| `cp -r folder/ backup/` | Copies a folder and everything in it |
| `mv a.txt folder/` | Moves a file |
| `mv old.txt new.txt` | Renames a file (moving it to a new name) |

## Deleting
| Command | What it does |
|---|---|
| `rm file.txt` | Deletes a file |
| `rm -r folder/` | Deletes a folder and everything in it |
| `rmdir folder` | Deletes a folder only if it's empty |
| `rm -i file.txt` | Asks before deleting |

## ⚠️ Gotchas I hit
- **There is no recycle bin.** `rm` is permanent.
- `>` overwrites without warning. Mixing up `>` and `>>` wipes files.
- `cp` and `mv` overwrite existing files silently. Add `-i` to be asked first.
- Never run `rm -rf` on anything you haven't double-checked, especially with `sudo`.

[⬅ Back to Linux](./README.md)
