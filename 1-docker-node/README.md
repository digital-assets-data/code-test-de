## Cryptocurrency Node

The Dockerfile in this directory will build and run a [Parity](https://www.parity.io/) client and sync the [Expanse](https://coinmarketcap.com/currencies/expanse/) cryptocurrency blockchain. Expanse is a fork of Ethereum and only used here so you don't have to run and sync the full Ethereum blockchain, which is hundreds of gigabytes. Expanse is much smaller at around 5-6 GB. They are functionally the same.

You can find the docs for interacting with Parity [here](https://wiki.parity.io/JSONRPC-eth-module).

Alternatively, we have a hosted node that you may use. IP: 107.22.132.192, port is the same as in the Dockerfile.

Bonus: Run a different type of node (not Parity based) to pull data from.


## Run

Install docker (Ubuntu): `sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common vim && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" && sudo apt-get update && sudo apt-get install -y docker-ce`

Build image: `sudo docker build -t parity-exp:stable .`

Create data directory: `mkdir -p ~/data` (feel free to change this location)

Run container: `sudo docker run --restart=unless-stopped -p 8545:8545 -p 30303:30303 -d -v ~/data:/root/parity/data --name exp parity-exp:stable` (make sure data directory, `-v` flag, matches the directory you created)
