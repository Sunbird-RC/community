# Setting Up A Registry Instance

Now that you have the Registry CLI installed, we can create a new instance of a
registry!

Run the following command in a directory where you wish to setup the registry.
For this example, we will use `~/Registries/example/`. (`~` is short form for
the user's home directory).

> Before creating the registry, add the following line at the end of your hosts
> file if you haven't already: `127.0.0.1 kc`. The hosts file is `/etc/hosts` on
> Linux/MacOS and on Windows, it is `c:\windows\system32\drivers\etc\hosts`.

```sh
# Create and move into the ~/Registries/example directory
$ mkdir -p ~/Registries/example
$ cd ~/Registries/example
# Create a registry instance
$ registry init
```

> Don't copy-paste the `$` signs, they indicate that what follows is a terminal
> command

This will setup and start a registry using Docker on your machine.

> The default setup files used to create a registry can be found
> [here](https://github.com/gamemaker1/registry-setup-files). If you wish to use
> a different set of files to setup your registry, specify the remote git repo's
> URL using the `config` command before running the `init` command:
>
> ```
> $ registry config setup.repo <url-to-setup-repo>
> ```
>
> You will also need to change the container names and images if you have
> changed them in `docker-compose.yaml`:
>
> ```
> $ registry config container.images <comma-separated-image-names>
> $ registry config container.names <comma-separated-container-names>
> ```
