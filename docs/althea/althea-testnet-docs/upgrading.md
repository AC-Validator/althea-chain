# Upgrading to althea-testnet2v2

althea-testnet2v2 is the successor to the v1 testnet we brought online two weeks ago.

If you are not a validator yet, please see [the instructions for setting up a validator](setting-up-a-validator.md)

When more than 66% of the voting power on Althea testnet returns the chain will start once again!

## Update gbt

The new gravity bridge tools binary is a single binary replacement for the previous five and has dramatic CLI simplifications for ease of use

In order to download ARM binaries change the name in the wget link from ‘gbt' to gbt-arm'

```

mkdir althea-bin
cd althea-bin

# Tools for the gravity bridge from the gravity repo

wget https://github.com/althea-net/althea-chain/releases/download/v0.2.2/gbt
chmod +x *
sudo mv * /usr/bin/

```

run `gbt --version` to check that you where successful, the current latest version is `gbt 0.5.3`

## Update your genesis file

This is the exported genesis file of the chain history, we'll import it into our new updated chain keeping all balances and state

```
wget https://github.com/althea-net/althea-chain/releases/download/v0.2.2/althea-testnet2v2-genesis.json
cp althea-testnet2v2-genesis.json $HOME/.althea/config/genesis.json
```

## Start the chain

Unsafe reset all will reset the entire blockchain state in .althea allowing you to start althea-testnet1v5 using only the state from the genesis file

```
althea unsafe-reset-all
althea start
```

## Restart your Orchestrator

The way orchestrators are launched has changed, you will need to run these commands to upgrade.

You should copy the arguments out of your existing start-orchestrator.sh. By the time we are done you won't even need a script as the startup command is radically simplified.

```
gbt init
gbt keys set-orchestrator-key -p "your orchestrator cosmos key"
gbt keys set-ethereum-key -k "your validators etheruem key"
```

Now that you have saved your keys you can start the orchestrator with

```
gbt -a althea orchestrator --fees 125000ufootoken
```

\*\*If you have set a minimum fee value in your `~/.althea/config/app.toml` modify the `--fees` parameter to match that value!

If your orchestrator is working correctly you'll see a message like this

```

[2021-03-02T21:38:47Z INFO gravity_utils::connection_prep] Cosmos node is syncing or waiting for the chain to start. Standing by

```

You may also want to check the status of your Geth node, no changes are required there, just make sure it's online.

If your Geth node is working correctly you'll see a message like this every few seconds

```

INFO [03-02|21:37:59.043] Imported new block headers count=1 elapsed="347.865µs" number=4376135 hash="e05085…22afab"

```

## Wait for it

wait at this step for the chain to finish starting up again

If you've done the upgrade right expect to see this line

```

11:20PM INF Inbound Peer rejected err="incompatible: peer is on a different network. Got althea-testnet2v1, expected althea-testnet2v2" module=p2p numPeers=11

```

You are expecting the updated version of the chain. If you see 'expected althea-testnet2v1' then you are still running the out of date software and should double check these instructions.

Congrats you've finished upgrading! Keep an eye out for the upgrade signing progress message. You can safely leave your node unattended and everything should start when the chain starts

Keep your eye out for lines like these, this line notes that 23% of the chain power is online and ready to start.

```

10:10PM INF Added to prevote module=consensus prevotes="VoteSet{H:208301 R:0 T:SIGNED*MSG_TYPE_PREVOTE +2/3:<nil>(0.23819055244195356) BA{15:x\***\*\_\_\_\_\*\***x*} map[]}"

```
