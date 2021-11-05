# github-actions-rsync-demo

**Rsync + Husarnet + GitHub Actions**

In this example using [Husarnet Action for GitHub Actions](https://github.com/marketplace/actions/husarnet) we reach remote device with `my-raspberry` hostname and copy `hello_world.txt` file there.

> **Note**
>
> That remote device need to be in the same Husarnet network as the GitHub workflow. Find installation methods [here](https://husarnet.com/docs/). Sample command to connect the remote device to Husarnet network:
>
> ```bash
> sudo husarnet join $HUSARNET_JOINCODE my-raspberry
> ```

## GitHub repository secrets

In your own copy of this repo, go to `Settings` > `Secrets` and create the following secrets with `New repository secret` button:

### 🔐 `SSH_PRIVATE_KEY`

Setup SSH key pair **(with NO passphrase!!!)**:

```bash
ssh-keygen -t rsa -b 4096 -C "GitHub-action" -f ga-ssh-key
```

That command will generate 2 files:

| File | How to use it? |
| - | - |
| `ga-ssh-key` | Copy the `ga-ssh-key` file content here to `SSH_PRIVATE_KEY` GitHub secret|
| `ga-ssh-key.pub` | Inside `my-raspberry` shell execute: `cat ga-ssh-key.pub >> /root/.ssh/authorized_keys` (or paste the file content manually to the new line at the end of `authorized_keys` file in e.g. vim if you generated keys on other host than `my-raspberry`) |

### 🔐 `KNOWN_HOSTS`

Now log into `my-raspberry` device and execute:

```bash
ssh-keyscan -t rsa $(hostname)
```

Copy output from that command to `KNOWN_HOSTS` GitHub secret.

### 🔐 `HUSARNET_TARGET_HOSTNAME`

In this example we named our device `my-raspberry`, so just paste this hostname to `HUSARNET_TARGET_HOSTNAME` secret.

### 🔐 `HUSARNET_JOINCODE`

The Join Code for your Husarnet network. Find it on your user account at https://app.husarnet.com. It looks like this: `fc94:b01d:1803:8dd8:b293:5c7d:7639:932a/eh2yAFK57bM2g8aeR49e3q`

## Triggering the workflow

After defining the GitHub secrets you can trigger the workflow manually from GitHub, or by pushing changes to the repo.

After the workflow finish its job, you should find `/root/files/hello_world.txt` file on `my-raspberry` device.