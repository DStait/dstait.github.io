---
title: "Set AWS Credentails from aws sts assume-role" 
---

Get the credentials
```bash
creds=$(aws sts assume-role --role-arn arn:aws:iam::000000000000:role/My_Role --role-session-name Assuming_My_Role)
```

Set the AWS environment variables so that the role is used when making api calls. 
```bash
eval $(jq -r '.Credentials | with_entries(.key |= ( gsub( "(?<a>[a-z])(?<A>[A-Z])"; "\(.a)_\(.A)") | ascii_upcase | "AWS_\(.)" ) ) | to_entries[] | "export \(.key)=\(.value)"' <<< ${creds})
```