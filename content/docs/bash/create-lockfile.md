---
title: "Create lockfile for script" 
---

```bash
LOCKFILE=/var/run/script_name.lck
[ -w $LOCKFILE ] && echo "Already running.... exiting!" && exit 1 || touch $LOCKFILE
# end of script
rm $LOCKFILE
```