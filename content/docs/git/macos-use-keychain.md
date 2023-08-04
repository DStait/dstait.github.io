---
title: "Use MacOS keychain"
---
```bash
git config --global credential.helper osxkeychain
 
# Manually add to git credential helper
read -p "username: " NAME
read -s -p "password(token): " PASSWD
echo "\
protocol=https
host=github.com
username=$NAME
password=$PASSWD" | git credential-osxkeychain store
```