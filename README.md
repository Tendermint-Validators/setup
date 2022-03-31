# Tendermint installation script
This script installs and configures a Tendermint node. It removes the hazzle of manually
having to go through the installation steps to set up a node.

The script installs support scripts that can be used to create a wallet address and
promote the node to a validator.

The script supports the following networks:
- Vidulum
- Vidulum Testnet
- BeeZee
- BeeZee Testnet

## Requirements
Minimum hardware requirements for x86_64 (Intel/AMD) and arm64 (ARM)
Device    | Requirement
--------- | -----------
CPU Cores | 1
RAM       | 1 GB
HDD OS    | 20 GB
HDD Data  | >100 GB

Prefered hardware requirements for x86_64 (Intel/AMD) and arm64 (ARM)
Device    | Requirement
--------- | -----------
CPU Cores | 2
RAM       | 4 GB
HDD OS    | 60 GB
HDD Data  | >100 GB

Storage requirements for the data volume are highly dependant on the type of installation.
Please ensure that you can easily extend the data volume when needed.

Operating System: Ubuntu GNU/Linux 20.4.4 LTS

## Installation
Install a new server for your validator. Ensure that the server is up to date before starting
the installation.

Clone the repository to your server.
```
git clone git@github.com:Tendermint-Validators/setup.git
cd setup
```

Customize the validator options in the file settings.json to meet your requirements.
```
"validator": {
  "amount": 2500000,
  "commission_max_change_rate": 0.30,
  "commission_max_rate": 0.30,
  "commission_rate": 0.05,
  "details": "YOUR VALIDATOR DESCRIPTION",
  "fees": "20000",
  "keyname": "VALIDATOR_KEY",
  "moniker": "YOUR_VALIDATOR_NAME",
  "security_contact": "YOUR@EMAIL.ADDRESS",
  "website": "https://YOUR.WEBSITE.URL/"
},
```

Run the installation script to install your Tendermint validator.
```
./install_tendermint.sh
```

After the installation the Tendermint service should be running and syncing with the network.
Login as the Tendermint user and use the logs-indexed helper script to track the status.

Example for Vidulum:
```
su - vidulum
logs-indexed
```

## Configuration
The installer creates helper scripts that can be used to create a wallet address and promote
the node to validator.

In this example we use Vidulum. Please ensure that you use the correct Tendermint user account
for your installation.

Login as the Tendermint user.
```
su - vidulum
```

Create a wallet address. This will ask you to create a password. Make sure that your keep this
password in a safe place and never share this password with others. A mnemonic is also generated.
You should store this in a safe place as well as your need the mnemonic incase you have to restore
your wallet. Never store secrets in plaintext files. Use a password manager like KeePass to keep
your secrets safe.

```
create_wallet
```

You will need to transfer the collateral to the wallet address. By default we have set this to
2.5 UNITS (2.5VDL in our example). It is up to you to set a proper value. It is not possible to
promote a validator with less then 1.0 UNIT, and its not smart to do with 1.0 UNIT as the first
slashing event will cause your validator to go offline. Ensure that you lock up enough funds to
overcome slashing events.

Promote the node to validator.
```
create_validator
```

Your node has now been promoted to validator and should be visible on the explorer.
[https://explorer.rpc.erialos.me/](https://explorer.rpc.erialos.me/)
