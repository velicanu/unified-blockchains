# Ethereum pipeline
data analysis toolkit for data across blockchains

## Prerequisites
1 instance with 1TB local storage space, we'll call this the "ethereum" instance

1 instance with 1TB storage for unified data, we'll call this the "unified" instance

## Setup
Install and run the ethereum daemon, which is needed to download the blockchain:
```bash
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
geth --rpc
```

Setup ssh config for connecting from "ethereum" instance to "unified" instance, for example if 10.0.0.10 was the unified instance then ~/.ssh/config would have this entry in it:
```b
Host 10.0.0.10
     User ubuntu
     IdentityFile ~/.ssh/velicanu-IAM-keypair.pem
```

Clone this repo
```bash
git clone https://github.com/velicanu/unified-blockchains.git
cd unified-blockchains/ethereum
```

Next we're going to configure and setup the instance. The unified server ip is the ip of "unified" instance, and s3 bucket name should be customized as per your setup, the rest can take default values.

```bash
./initial-config.sh
```

Next run the setup script, which will try to catch configuration errors.

```bash
./initial-setup.sh
```

If the setup succeeds, then you're ready to start the pipeline!

```bash
cd src/
./keep-updated.sh 0
```

The above command will start the pipeline and pull new data whenever it becomes available. The argument is at which block to start looking for new data from. If running on a cloud instance, it's best to run this in a screen session so it can continue running in the background.


## Tests

The tests/ directory has tests which validate the blockchain data. To run it the ```jq``` program needs to be installed, in order to parse the json files from the command line

```bash
sudo apt install jq
```

Then run the tests to see if the pipeline is working.

```bash
./tests/run_all.sh
```
