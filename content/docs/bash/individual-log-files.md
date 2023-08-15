---
title: "Individual log files" 
---

### Log output and errors to invidual files and still display all messages
```bash
#!/usr/bin/env bash

SUCCESS_LOG_NAME="success.log"
ERROR_LOG_NAME="error.log"

echo "hello World" > >(tee -a "$SUCCESS_LOG_NAME") 2> >(tee -a "$ERROR_LOG_NAME" >&2)
```
