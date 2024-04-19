## Running Lite nodes using Docker

> Running docker on your machine will get you up and running quickly but should not be considered production level.
> This requires a lot more orchestration that is not in scope of this document.

There are docker images available at:
* `ghcr.io/aiblockofficial/network/node-user:latest`

Use the `node-user` image for a Lite node - we still have to update historic naming conventions.
We support `AMD` and `ARM` architectures.

You may also bake your own by running `docker build -t aiblock-node .` in the [`Network`](https://github.com/AIBlockOfficial/Network) repo root.
This image will support both node types which gets switched by passing a NODE_TYPE command `miner` or `user`.

### Settings for the nodes

AIBlock nodes use a `node_settings.toml` file for configuration.
This is located in `/etc/node_settings.toml` and has to be mounted as the container starts.

The sample settings file for a lite node can be found here:
* [user](node_settings_user.toml) 

They are currently referencing `testnet`

### Running docker directly

```bash
docker run -d -p --name aiblock-user 3000:3000 \
-v /tmp/user:/src \
-v ./node_settings_user.toml:/etc/node_settings_user.toml \
-e ADDRESS="127.0.0.1:12340" \
-e WITH_USER_ADDRESS="127.0.0.1:12350" \
ghcr.io/aiblockofficial/network/node-miner:latest user
```

* `-p 3000:3000` sets ports used for the miner's REST API.
* `-v /tmp/wallet:/src` mounts your local filesystem so the miner's wallet may be persisted.
* `-e ADDRESS` sets the ip:port of the node. If more than one node will be running from the same IP address, the port number has to be different for each node. 
* `-e WITH_USER_ADDRESS` also starts a user node with a wallet to hold the tokens.

You might also need to run with `--user` on some Linux systems for mounting permission purposes.

>
> Replace the `/tmp` directory with something more permanent - if you lose this, you lose all your tokens.
>

### Using docker-compose

It is probably easier using `docker-compose` to manage these.
Here is a simple example of a [`docker-compose.yaml`](docker-compose.yml) file
It references both node setting files as supplied above and will start up a Miner and Lite node to `testnet`

Run this by using `docker-compose up -d` for detached mode and inspect the logs using `docker-compose logs --follow`

>
> Once again, replace the `/tmp` directory with something more permanent - if you lose this, you lose all your tokens.
>