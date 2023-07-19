# Private-Ethereum-Blockchain

The primary goal of this project is to acquire practical knowledge and hands-on experience with Ethereum, a well-known Blockchain platform. Additionally, I aim to familiarize myself with concepts such as Public-Key signatures, hash functions, key management, and proof-of-work.
Through this project, my intention is to understand how cryptographic algorithms are employed in real-world applications and develop motivations to comprehend them effectively. I used the pre-compiled geth binary which I shared in this repo.
In order to execute the binary program "geth", you can extract it from the downloaded .gz file and place it in the current folder. To run it, simply use the command "./geth".

## Step by step setup (Private Ehtereum Network with a single node)

## Genesis file

Creating a genesis.json file describing the private Ethereum network. In order to establish a private network, it is necessary to furnish geth with essential information that is needed to generate the initial block. Every blockchain begins with a Genesis Block, which is the first block in the chain. To set up our private blockchain, we will generate a custom genesis block using a genesis file. Subsequently, we will instruct Geth to utilize this genesis file to create our unique genesis block. We can save the code below as genesis.json. The chainID cannot be 0.

```
{
"config": {
"chainId": 2023,
"homesteadBlock": 0,
"eip150Block": 0,
"eip155Block": 0,
"eip158Block": 0,
"byzantiumBlock": 0,
"constantinopleBlock": 0,
"petersburgBlock": 0,
"ethash": {}
},
"difficulty": "10",
"gasLimit": "8000000",
"alloc": {}
}
```

## Use the pre-compiled geth binary to init the private ethereum network

```
./geth --http -http.port "8085" --datadir data init genesis.json
```

## Run the private ethereum network

The "--nodiscover" option stop your node to connect to the public Ethereum network.

```
./geth --http -http.port "8085" --datadir data --networkid 2023 --nodiscover --allow-insecure-unlock
```

## Enable the Geth console

Open a new terminal window and run the snippet below to open the Geth console

```
./geth attach data/geth.ipc
```

## Creatig account

create at least two accounts.

```
personal.newAccount()
```

Enter personal passphrase for the account. Make sure you note down or remember the account address and password as it will be needed more often.

## Setting etherbase account

By default, the first created account is an etherbase account. Although, we can change the etherbase status of other accounts using **miner.setEtherbase("the account address")**. Etherbase account is used to receive token reward from mining. for example,

```
miner.setEtherbase(eth.accounts[0])
```

## Checking Etherbase account

```
eth.coinbase
```

## Checking the balance of the account

```
eth.getBalance(eth.accounts[0])
      or
eth.getBalance(eth.coinbase)
```

## Start and Stop Mining tokens

```
miner.start()
miner.stop()

```

We can check the balance weather blockchain grows or not, if it does not, terminate the geth console and private ethereum network. Then run the etherem network command and run geth console command on the new terminal window.

## Making transactions

To make a successful transaction initially we need to unlock the base account. In this example we transfer ether from eth.accounts[0] to eth.accounts[1] with value 500000.

```
personal.unlockAccount(eth.accounts[0])
```

Provide the passphrase. Then we can do the transaction

```
eth.sendTransaction({from: eth.accounts[0], to: eth.accounts[1], value: 500000})
```

If you check the balance of the second account which is eth.account[1] is still 0.

```
eth.getBalance(eth.accounts[1])
```

To make the transaction successful we need to start mining

```
miner.start()
```

Check the balance of the eth.account[1] the transaction occurred. The transaction will generate its own hash, we can view transaction information

```
eth.getTransaction("transaction hash value")
```
