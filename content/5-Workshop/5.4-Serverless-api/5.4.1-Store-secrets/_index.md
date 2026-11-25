---
title : "Store secrets in SSM Parameter Store"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

Every SecureString under the path `/student-market/dev/api` becomes a configuration key of the API (the `/` separators map to `:`).

| Parameter name | Configuration key | Value |
|---|---|---|
| `/student-market/dev/api/ConnectionStrings/DefaultConnection` | `ConnectionStrings:DefaultConnection` | Supabase **transaction** pooler string (port 6543, `No Reset On Close=true`) |
| `/student-market/dev/api/Jwt/SecretKey` | `Jwt:SecretKey` | Random string, at least 32 characters |
| `/student-market/dev/api/OriginVerify/Secret` | `OriginVerify:Secret` | Random string shared with CloudFront (step 5.5) |
| `/student-market/dev/api/Email/Password` | `Email:Password` | Optional Gmail App Password for OTP emails |

#### Create the parameters

```powershell
function New-Secret { -join ((48..57 + 65..90 + 97..122) | Get-Random -Count 48 | ForEach-Object { [char]$_ }) }
$path = "/student-market/dev/api"

aws ssm put-parameter --name "$path/ConnectionStrings/DefaultConnection" --type SecureString `
  --value "<transaction pooler connection string>" --overwrite --profile student-market --region ap-southeast-1
aws ssm put-parameter --name "$path/Jwt/SecretKey" --type SecureString --value (New-Secret) --overwrite --profile student-market --region ap-southeast-1
$originSecret = New-Secret
aws ssm put-parameter --name "$path/OriginVerify/Secret" --type SecureString --value $originSecret --overwrite --profile student-market --region ap-southeast-1
```

Keep `$originSecret` in this terminal session; it is passed to the media stack in step 5.5.

#### Verify without revealing values

```powershell
aws ssm get-parameters-by-path --path $path --recursive --profile student-market --region ap-southeast-1 `
  --query "Parameters[].{Name:Name,Type:Type}" --output table
```

All entries must have type `SecureString`. Avoid `--with-decryption` unless a value really needs to be read.
