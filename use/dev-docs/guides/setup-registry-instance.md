# Setting Up A Registry Instance

Now that you have the Registry CLI installed, we can create a new instance of a
registry!

Run the following command in a directory where you wish to setup the registry.
For this example, we will use `~/Registries/example/`. (`~` is short form for
the user's home directory).

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

## Next Steps

Now that you have a registry instance up and running, you can jump straight into
[using the APIs](using-the-apis.md).
