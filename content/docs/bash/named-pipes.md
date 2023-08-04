---
title: "Named pipes"
---

## Reader script
```bash
#!/bin/bash
 
pipe=/tmp/testpipe
 
trap "rm -f $pipe" EXIT
 
if [[ ! -p $pipe ]]; then
    mkfifo $pipe
fi
 
while true
do
    if read line <$pipe; then
        if [[ "$line" == 'quit' ]]; then
            break
        fi
        echo $line
    fi
done
echo "Reader exiting"
```

## Writer script
```bash
#!/bin/bash
 
pipe=/tmp/testpipe
 
if [[ ! -p $pipe ]]; then
    echo "Reader not running"
    exit 1
fi
 
 
if [[ "$1" ]]; then
    echo "$1" >$pipe
else
    echo "Hello from $$" >$pipe
fi
```