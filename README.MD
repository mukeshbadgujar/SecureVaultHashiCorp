# Start hashicorp
# Complete Guide to Hashicorp Vault by _**Mukesh Badgujar**_
## heare i will let you know how to start hashicorp vault, we will use in python.

### Step 1: Install Hashicorp Vault on linux / windows
- Install Vault from the official HashiCorp website.
- https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-install
- choose your OS and download the vault.

### Step 2: Verifying the Installation
- After installing Vault, verify the installation by checking the version of Vault.
- $ vault --version

### Step 3: Start the Vault Server 
- Start the Vault server in development mode.
- $ `vault server -help`
- $ `vault server -dev`
- $ `export VAULT_ADDR='http://127.0.0.1:8200' `
- after that you can see the vault server is running on http://127.0.0.1:8200
- you can access the vault server using the browser.
- Set the VAULT_TOKEN environment variable value to the generated Root Token value displayed in the terminal output.
- $ `export VAULT_TOKEN="hvs.<key>"`
- see you have unseal key and root token in the terminal.
- take that and save it in the safe place.
- go to browser and use Root token to login.
- verify the vault server is running or not. use the below command.
- $ `vault status`

## till now if you are able to see the vault server is running then you are good to go.

# Your First Secret
- Now, let's write our first secret to Vault. follow the below steps.
1. keep the vault server running.
2. open the terminal and run the below command.
3. $ `vault kv put secret/hello foo=world` (this command will write the secret to the vault server)
4. you will get the below output.
```
vault kv put secret/hello foo=world
== Secret Path ==
secret/data/hello

======= Metadata =======
Key                Value
---                -----
created_time       2024-12-16T15:35:01.220330374Z
custom_metadata    <nil>
deletion_time      n/a
destroyed          false
version            1
```
5. you can see the secret is written to the vault server.

1. another way to write the secret to the vault server.
2. $ `vault kv put secret/hello foo=world2 excited=yes` (this command will write the secret to the vault server)
3. excite=yes is the another key value pair.
4. you will get the below output.
```
vault kv put secret/hello foo=world2 excited=yes
== Secret Path ==
secret/data/hello

======= Metadata =======
Key                Value
---                -----
created_time       2024-12-16T15:37:39.520303151Z
custom_metadata    <nil>
deletion_time      n/a
destroyed          false
version            2
```
**Note: you can use files to write the secrets to the vault server. it is more secure way to write the secrets.**

# Read the Secret
- Now, let's read the secret from the vault server. follow the below steps.
- $ `vault kv get secret/hello` (this command will read the secret from the vault server)
- you will get the below output.
```
vault kv get secret/hello
== Secret Path ==
secret/data/hello

======= Metadata =======
Key                Value
---                -----
created_time       2024-12-16T15:37:39.520303151Z
custom_metadata    <nil>
deletion_time      n/a
destroyed          false
version            2

===== Data =====
Key        Value
---        -----
excited    yes
foo        world2
```
- you can get value of the secret by key.
- $ `vault kv get -field=excited secret/hello`
- you will get the below output.
```
vault kv get -field=excited secret/hello
yes
```
- you can see the value of the key excited is yes.
- you can read the secret by key.
- $ `vault kv get -field=foo secret/hello`
- you will get the below output.
```
vault kv get -field=foo secret/hello
world2
```
- you can see the value of the key foo is world2.

## also you can read the secret with json format.
- $ `vault kv get -format=json secret/hello`
- you will get the below output.
```
vault kv get -format=json secret/hello
{
  "request_id": "60365a64-ddd0-f863-d2f6-de02b16161c8",
  "lease_id": "",
  "lease_duration": 0,
  "renewable": false,
  "data": {
    "data": {
      "excited": "yes",
      "foo": "world2"
    },
    "metadata": {
      "created_time": "2024-12-16T15:37:39.520303151Z",
      "custom_metadata": null,
      "deletion_time": "",
      "destroyed": false,
      "version": 2
    }
  },
  "warnings": null,
  "mount_type": "kv"
}
```
- you can see the secret in json format.

### you can also extract the value of the key from the json format with jq command.
- $ `vault kv get -format=json secret/hello | jq -r .data.data.excited`
- you will get the below output.
```
vault kv get -format=json secret/hello | jq -r .data.data.excited
yes
```

## **you can also see the keys in server**
![img.png](img.png)


# Delete the Secret
- Now, let's delete the secret from the vault server. follow the below steps.
- $ `vault kv delete secret/hello`
- you will get the below output.
```
vault kv delete secret/hello
Success! Data deleted (if it existed) at: secret/data/hello
```


# Secrets engines
- Vault secrets engines are components that store, generate, or encrypt data.
- A secrets engine is enabled at a specific path in the Vault.
- what is the use of secrets engines?
  - Secrets engines are responsible for managing the lifecycle of secrets. 
  - They are responsible for reading, writing, and deleting secrets.
  - They are responsible for generating dynamic secrets.
  - They are responsible for encrypting and decrypting data.
  - They are responsible for rotating credentials.
  - They are responsible for managing access to secrets.
  - They are responsible for managing leases. leases are the time duration for which the secret is valid.
  - They are responsible for managing revocation of secrets. revocation is the process of invalidating the secret.
  - They are responsible for managing audit logs. audit logs are the logs that are generated when the secret is accessed.
  - They are responsible for managing policies. policies are the rules that are defined for accessing the secrets.
  - They are responsible for managing authentication methods. authentication methods are the methods that are used to authenticate the user.

# Enable the secrets engine
- Now, let's enable the secrets engine in the vault server. follow the below steps.
- $ `vault secrets enable -path=kv kv`
- you will get the below output.
```
vault secrets enable -path=kv kv
Success! Enabled the kv secrets engine at: kv/
```
- list the secrets engines.
- $ `vault secrets list`
- you can disable the secrets engine. if you want no one can access the secrets. or not required.
- $ `vault secrets disable kv`
- you will get the below output.
```
vault secrets disable kv
Success! Disabled the secrets engine (if it existed) at: kv/
```

- now if you read the secrets you will get the below output.
- $ `vault kv get kv/hello`
```
vault kv get kv/hello
Error making API request.

URL: GET http://127.0.0.1:8200/v1/sys/internal/ui/mounts/kv/hello
Code: 403. Errors:

* preflight capability check returned 403, please ensure client's policies grant access to path "kv/hello/"
```

# Enable the secrets engine
- Now, let's enable the secrets engine in the vault server. follow the below steps.
- $ `vault secrets enable -path=kv kv`
- you will get the below output.
```
vault secrets enable -path=kv kv
Success! Enabled the kv secrets engine at: kv/
```


# Dynamic Secrets
- Dynamic secrets are generated on-demand and are unique to the client.
- Dynamic secrets are generated for a specific time duration.
- Dynamic secrets are generated for a specific purpose.
- Dynamic secrets are generated for a specific client.
- Dynamic secrets are generated for a specific path.
- Dynamic secrets are generated for a specific role.
- Dynamic secrets are generated for a specific lease.

## now we using aws secrets engine.
- $ `vault secrets enable -path=aws aws`
- you will get the below output.
```
vault secrets enable -path=aws aws
Success! Enabled the aws secrets engine at: aws/
```

# Configure the AWS secrets engine
- Now, let's configure the AWS secrets engine in the vault server. follow the below steps.
- $ `vault write aws/config/root access_key=AKIAIOSFODNN7EXAMPLE secret_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`
- you will get the below output.
```
Success! Data written to: aws/config/root
```

# Create the AWS role
- Now, let's create the AWS role in the vault server. follow the below steps.
- $ `vault write aws/roles/my-role credential_type=iam_user policy_document=-<<EOF
heredoc> {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:*",
      "Resource": "*"
    }
  ]
}
- you will get the below output.
```
Success! Data written to: aws/roles/my-role
```

# Generate the AWS credentials
- Now, let's generate the AWS credentials in the vault server. follow the below steps.
- $ `vault read aws/creds/my-role`
- you will get the below output.
```
Key                Value
---                -----
lease_id           aws/creds/my-role/1e1b1b1b-1b1b-1b1b-1b1b-1b1b1b1b1b1b
lease_duration     768h
lease_renewable    true
access_key         AKIAIOSFODNN7EXAMPLE
secret_key         wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
security_token     <nil>
```

# Revoke the AWS credentials
- Now, let's revoke the AWS credentials in the vault server. follow the below steps.
- $ `vault lease revoke aws/creds/my-role/1e1b1b1b-1b1b-1b1b-1b1b-1b1b1b1b1b1b`
- you will get the below output.
```
All revocation operations queued successfully!
```



# Authentication methods

## Token authentication
- Token authentication is the default authentication method in the vault server.
- Token authentication is automatically enabled. When you started the dev server, the output displayed a root token. The Vault CLI read the root token from the $VAULT_TOKEN environment variable. This root token can perform any operation within Vault because it is assigned the root policy. One capability is to create new tokens.

- to create the token.
- $ `vault token create`
- you will get the below output.
```
vault token create
Key                  Value
---                  -----
token                hvs.<key>
token_accessor       9jcs0fqEQLJGDXYcKDHCgy2c
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
```

- login with the token.
- $ `vault login`
- you will get the below output.
```
Token (will be hidden):
Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.
Key                  Value
---                  -----
token                hvs.<key>
token_accessor       <key>
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
```

- Create another token.
- $ `vault token create`
- you will get the below output.
```
vault token create
Key                  Value
---                  -----
token                hvs.<key>
token_accessor       9jcs0fqEQLJGDXYcKDHCgy2c
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
```
- The token is created and displayed here as s.TsKT5ubouZ7TF26Eg7wNIl3k. Each token that Vault creates is unique.
- Revoke the first token you created.
- $ `vault token revoke hvs.<key>`
- you will get the below output.
```
Success! Revoked token (if it existed)
```

# GitHub authentication
- GitHub authentication is a method of logging into Vault using GitHub credentials.

- Enable the GitHub authentication method.
- $ `vault auth enable github`
- you will get the below output.
```
Success! Enabled github auth method at: github/
```

- Configure the GitHub authentication method.
- $ `vault write auth/github/config organization=hashicorp`
- you will get the below output.
```
Success! Data written to: auth/github/config
```

- Display all the authentication methods that Vault has enabled.
- $ `vault auth list`
- you will get the below output.
```
Path       Type      Accessor               Description
----       ----      --------               -----------
github/    github    auth_github_1b1b1b1b    n/a
token/     token     auth_token_1b1b1b1b     token based credentials
```

- Since you will attempt to login with an auth method, you should ensure that the VAULT_TOKEN environment variable is not set for this shell session since its value will take precedence over any token you obtain from Vault.
- $ `unset VAULT_TOKEN`
- you will get the below output.
```
unset VAULT_TOKEN
```

- Login with the GitHub authentication method.
- $ `vault login -method=github`
- you will get the below output.
```
GitHub Personal Access Token (will be hidden):
Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.

Key                    Value
---                    -----
token                  s.DNtKCjVQ1TxAzgMqtDuwjjC2
token_accessor         e7zLJuPg2tLpav66ZSu5AyDC
token_duration         768h
token_renewable        true
token_policies         [default applications]
token_meta_org         hashicorp
token_meta_username    my-user
```

- Log back in with the root token.
- $ `vault login`

- Revoke all tokens generated the github auth method.
- $ `vault token revoke -mode=path auth/github`
- All tokens generated by logins to the path auth/github are revoked.

- vault auth disable github
- Disable the GitHub authentication method.
- $ `vault auth disable github`
- you will get the below output.
```
Success! Disabled the auth method (if it existed) at: github/
```


# Policy Format

- Policies are written in HashiCorp Configuration Language (HCL).
- Policies are written in JSON format, YAML format, or HCL format.

- The policy format is as follows:
```
path "path" {
  capabilities = ["capabilities"]
}
```

- The policy format is as follows:
```
path "secret/data/*" {
  capabilities = ["create", "update"]
}

path "secret/data/foo" {
  capabilities = ["read"]
}
```

- View the default policy.
- $ `vault policy read default`

- Create a policy.
- $ `vault policy write -h`

- Create the policy named my-policy with the contents from stdin.
- $ `vault policy write my-policy - <<EOF
- you will get the below output.
```
vault policy write my-policy - << EOF
# Dev servers have version 2 of KV secrets engine mounted by default, so will
# need these paths to grant permissions:
path "secret/data/*" {
  capabilities = ["create", "update"]
}

path "secret/data/foo" {
  capabilities = ["read"]
}
EOF
Success! Uploaded policy: my-policy
```

- View the policy.
- $ `vault policy read my-policy`
- you will get the below output.
```
# Dev servers have version 2 of KV secrets engine mounted by default, so will
# need these paths to grant permissions:
path "secret/data/*" {
  capabilities = ["create", "update"]
}

path "secret/data/foo" {
  capabilities = ["read"]
}
```

- for more information you can visit the below link.
- https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-policies


# Most important topic : Deploy Vault


- Unset the VAULT_TOKEN environment variable to prevent any value used earlier from use this time.
- $ `unset VAULT_TOKEN`

- Configure Vault
- create config.hcl file
- $ `vi config.hcl`
- add the below content in the file.
```
storage "raft" {
  path    = "./vault/data"
  node_id = "node1"
}

listener "tcp" {
  address     = "127.0.0.1:8200"
  tls_disable = "true"
}

api_addr = "http://127.0.0.1:8200"
cluster_addr = "https://127.0.0.1:8201"
ui = true
```

## Start the server
- Start the Vault server with the configuration file.
- $ `mkdir -p ./vault/data`
- $ `vault server -config=config.hcl`

- Initialize the server
- $ `export VAULT_ADDR='http://127.0.0.1:8200'`
- $ `vault operator init`

## Seal/Unseal
- Unseal the server
- $ `vault operator unseal`
- Every initialized Vault server starts in the sealed state. From the configuration, Vault can access the physical storage, but it can't read any of it because it doesn't know how to decrypt it. The process of teaching Vault how to decrypt the data is known as unsealing the Vault.
- now login with the root token.
- $ `vault login`

- kill the Vault process from a command.
- $ `kill -9 $(lsof -t -i:8200)` 
- or 
- $ `pgrep -f vault | xargs kill`

- Delete the /vault/data directory which stores the encrypted Vault data.
- $ `rm -rf ./vault/data`

# You can get more information from the below link.
https://developer.hashicorp.com/vault/tutorials/get-started

