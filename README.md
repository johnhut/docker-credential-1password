# README

To get going:

1. Put `bash/docker-credential-1password` in your path
2. Decide on the vault to use for your docker credential store
3. Populate 1password vault with required creds
4. Tell docker to use 1password

## Docker credential store vault

The script will default to using the "Docker" vault for storing your docker credentials.
If you wish to use the Docker vault, create it in 1password.

If you'd like to use a different vault create the `DOCKER_CREDENTIAL_1PASSWORD_VAULT`
environment variable, and assign it the name of your vault.

## Populating credentials in 1password

You can manually create an item, or use `bash/docker-credential-1password` to do it
as shown below:

```bash
docker-credential-1password store <<EOF
{
"ServerURL": "$(op read "op://current/path/to/url")",
"Username": "$(op read "op://current/path/to/username")",
"Secret": "$(op read "op://current/path/to/credential")"
}
EOF
```

For example, if you wish to access docker images in the github container registry:

Item       | Value
-----------|-----------------------
url        | ghcr.io
username   | [your github username]
credential | [PAT*1]

*1 The personal access token (PAT) e.g. a classic token with read:packets or
   write:packages permissions

## Tell docker to use 1password

The docker config is stored here: `$HOME/.docker/config.json`.  Edit this file to include
the following:

```json
{
    "credsStore": "1password"
}
```

The above json will tell docker to use our `docker-credential-1password` script instead of
directly placing credentials in this json file.

Note: docker will simply append the `credsStore` value to the end of the string `docker-credential-`
      in order to know which command to run.
