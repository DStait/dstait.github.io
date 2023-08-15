---
title: "Iterating over files"
---


### Iterating over ls output is fragile. Use globs.

[SC2045](https://www.shellcheck.net/wiki/SC2045)
```bash
for f in *.wav
do
  [[ -e "$f" ]] || break  # handle the case of no *.wav files
  echo "$f"
done
```
