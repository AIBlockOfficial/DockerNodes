## A miner connected to a lite node

To connect a miner with a lite node:

1. Both nodes need to be in the `node_settings.toml`
2. The miner needs to start up with `WITH_USER_INDEX` referencing the index in `node_settings.toml`. In this example `0`.
3. **If you're on Linux**, you'll need to uncomment lines 21 and 37 in the `docker-compose.yml` to avoid a `"Permission denied"` error when Docker sets up the node DBs
