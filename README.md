# capuk-infra
Personal Infrastructure Repo
### Pre-steps
1. Backup cal/todo items manually
2. Backup dns records and volume
3. Shutdown old instance and remove DNS records and detach volume
4. Get secrets: s3 access/secret, api key for linode, add to .env
5. Add "StackScript" to a stackscript on linode account and get stackscript id
6. Deploy a new Ubuntu 24.04 image and attach data volume to it.

``` bash
linode-cli linodes create \
  --label nextcloud \
  --root_pass ${ROOT_PASS} \
  --booted true \
  --stackscript_id ${SS_ID} \
  --stackscript-data '{
    "user_name": "${USER}",
    "disable_root": "Yes",
    "token_password": "${TOKEN}",
    "subdomain": "${SUBDOMAIN}",
    "domain": "${DOMAIN}",
    "s3_access_key": "${ACCESS_KEY}",
    "s3_secret_key": "${SECRET_KEY}",
    "soa_email_address": "${SOA_EMAIL}"
  }' \
  --region us-east \
  --type g6-standard-16 \
  --authorized_keys "${PUBKEY}"
  --authorized_users "${USER}"
  --volumes ${VOLUME} \ 
```

#### TODO
- set all UDFs to a common file and ansible encrypt it [here](https://github.com/christinepuk/capuk-infra/blob/main/nextcloud/group_vars/linode/secret_vars.yml)

