# Bitwarden_Export

<img src="https://raw.githubusercontent.com/dani-garcia/vaultwarden/main/resources/vaultwarden-icon.svg" width="180">

**[Bitwarden_Export](https://github.com/0neTX/Bitwarden_Export)**: Bash docker image that exports your Bitwarden vault contents.

## Features

- [x] Export personal vault
- [x] Export all organizations in vault (if applicable)
- [x] Export file attachments (if applicable)

The docker image provides the choice of creating unencrypted export files or password-encrypted export files. Attachments are not encrypted.

## Running it

### Requirements

- [Docker](https://docker.com) (Obviously)
- Personal API Keys for CLI Authentication <a href="https://bitwarden.com/help/personal-api-key/" target="_blank">https://bitwarden.com/help/personal-api-key/</a>

### How install in Unraid

* Search `bw-export` in *Apps* top menu, select `Actions --> Install`
* Set the required enviroment variables.

### Configuration environment variables

You must report the following variables so that the container can authenticate itself in your vault. The API Keys for authentication can be obtained from Personal API Keys for CLI Authentication <a href="https://bitwarden.com/help/personal-api-key/" target="_blank">https://bitwarden.com/help/personal-api-key/</a>

- `BW_CLIENTID`:  It's `client_id` variable from your account
- `BW_CLIENTSECRET`:  It's `client_secret` variable from your account
- `BW_PASSWORD`:  Your Bitwarden/Vaultwarden password.
- `EXPORT_PASSWORD`:  Encryption password you have chosen for exporting then vault. if you choose to use password-encryption to store your export files (recommended) be sure that you use a strong and memorable password! (don't just store it inside Bitwarden, because if you get locked out of your account you won't be able to restore your exports)

**OPTIONALS VARIABLES:**

- `BW_ORGANIZATIONS_LIST`: If you want to make a backup of your organizations, set one or more organizations separated by comma. To obtain your organization_id value, open a terminal and type: `bw login #(follow the prompts); bw list organizations | jq -r '.[0] | .id'`. **If you don't provided any value, the container will export all organizations in your vault.**

**REQUIRED VOLUMENS:**

- `OUTPUT_PATH_VOLUME`: Mapping for exporting vault. Ussually: `/mnt/user/appdata/bw-export`
- `ATTACHMENTS_PATH_VOLUME`: Mapping for exporting attachments in vault. Ussually: `/mnt/user/appdata/bw-export`

![container config](container_config.png)

Run the contaneir and check the logs. *No sensitive information is traced in logs.*

![container logs](container_log.png)

And enjoy it!

## Advanced features

## PUID and PGID

Docker runs all of its containers under the `root` user domain because it requires access to things like network configuration, process management, and your filesystem. This means that the processes running inside your containers also run as `root`. This kind of elevated access is not ideal for day-to-day use, and potentially gives applications the access to things they shouldn't \(although, a strong understanding of volume and port mapping will help with this\).

Another issue is file management within the container's mapped volumes. If the process is running under `root`, all files and directories created during the container's lifespan will be owned by `root`, thus becoming inaccessible by you.

Using the `PUID` and `PGID` allows our containers to map the container's internal user to a user on the host machine. All of our containers use this method of user mapping and should be applied accordingly.

### Using the variables

Create `PUID` and `PGID` variables with your user values, to find yours use `id your_user` as below:

![PUID Variables](puid-variables.png)

It is most likely that you will use the `id` of yourself, which can be obtained by running the command below. The two values you will be interested in are the `uid` and `gid`.

```shell
id your_user
```

Example output:

```shell
uid=1000(your_user) gid=1000(your_user) groups=1000(your_user)
```

## Unraid Support Forum:
- [https://forums.unraid.net/topic/127999-support-onetx-template-support-thread/](https://forums.unraid.net/topic/127999-support-onetx-template-support-thread/)

## License

This project is licensed under the GPL 3.0 License. See the [LICENSE](..\LICENSE)
file for details.
