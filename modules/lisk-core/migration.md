# Lisk Core Migration

> Migration of a Lisk Node is only necessary during a hard fork.
> For normal software updates that don't invoke a hard fork, please perform a normal upgrade, see [Upgrade vs Migration](introduction.md#upgrade-vs-migration).

## What happens during migration

The migration is a special case of upgrading your Lisk Core node. A migration is needed if the new version of the software is not compatible with older Lisk Core versions.

As a consequence, the majority of the network needs to do this software update simultaneously to stabilize the forked chain and as a result, make it grow faster as the outdated version of the chain.

To achieve this, a height is picked to proceed with a simultaneous global upgrade. 
When the blockheight of the network reaches the predefined height, it will invoke the update on all nodes in the network at the same time. 
This way the probabilities of creating forks are lower. 

Besides the normal update script, the **Lisk Bridge** script might include additional scripts, e.g. if the new update includes changes in the structure of the configuration file, the script would try to reorganize your existing config to the new structure and then run the normal update script afterward.
If you don't want to use the automated script, you can read a [detailed explanation](#migrate-manually) of how to migrate manually below. 

The following section describes how to use the automated migration script for binary distributions of Lisk Core, to make your migration as seamless as possible. Additionally, you can find information below on how to perform the migration process manually.

The **recommended** migration path is as follows:
- [Automated Migration](#automated-migration)
  - [Prepare your workspace](#prepare-your-workspace)
  - [Lisk Bridge : Automate the upgrade](#lisk-bridge--automated-lisk-core-migration)
  - [What if I miss the block height or if Lisk Bridge script fails](#what-if-i-miss-the-block-height-or-if-lisk-bridge-script-fails)
  - [Lisk Bridge Command Reference](#lisk-bridge-command-reference)
  - [Migration Notes Lisk Core 0.9 -> 1.0](#migration-notes-lisk-core-0.9-1.0)

The above should be enough to complete the migration. For more curious users, we've included a few more advanced sections:
- [Migrate manually](#migrate-manually)
  - [Utility scripts](#utility-scripts)
    - [Generate Config](#generate-config)
    - [Update Config](#update-config)
    - [Console](#console)

## Automated Migration

### Prepare your workspace
We assume that you have already installed Lisk Core and are familiar with the application. Nevertheless, we want to highlight a couple of things to double-check:
- Revisit the [setup](introduction.md#lisk-core-distributions) steps before continuing.
- Verify that the following ports are open, depending on the network:

| Network | httpPort(HTTP) | wsPort(TCP) |
| -----------|-------------|-------------|
| Mainnet | 8000         | 8001        |
| Testnet   | 7000         | 7001        |

- Make a backup of `config.json`. While this process does make a backup automatically in a `backup/` directory, it's recommended to make one yourself as you can never be too careful. At the end of this tutorial, we include some advanced cases to guide you.
- The next section introduces a tool we have implemented to make a gapless migration.

### Lisk Bridge: Automated Lisk Core Migration
We introduced a tool to perform the migration called **Lisk Bridge**, available at [downloads.lisk.io](https://downloads.lisk.io/lisk/).
Here are the direct download links depending on the network your node is connected to:
- [Lisk Bridge for Testnet](https://downloads.lisk.io/lisk/test/lisk_bridge.sh)
- [Lisk Bridge for Mainnet](https://downloads.lisk.io/lisk/main/lisk_bridge.sh)

It's a wrapper script that invokes `installLisk.sh upgrade` at a specific block height - and thus is intended for users using the [Binary installation](setup/binary.md).

To upgrade your node on a specific network height, you should download `lisk_bridge.sh` to where you would normally download and run installLisk.sh. 
In other words, it should be 1 directory up from where the lisk application is stored. 
For example, if you're currently running `Lisk Core 0.9.16` as a user called `lisk`, `Lisk Core 0.9.16` is most likely installed in `/home/lisk/lisk-main` and `installLisk.sh` is stored in `/home/lisk`, the lisk user's home directory. 
As the lisk user then, you could run the following:

```bash
su - lisk # change to lisk user
rm -f lisk_bridge.sh # remove old versions of lisk_bridge.sh
wget https://downloads.lisk.io/lisk/{network}/lisk_bridge.sh
```
Where `<network>` can either be `main` for Mainnet or `test` for Testnet.

Then run the script with the desired parameters:
```bash
bash lisk_bridge.sh -n <network> -h <blockheight>
```
Where `<network>` can either be `main` for Mainnet or `test` for Testnet.
`<blockheight>` is the before announced blockheight, when the hard fork is going to happen.

The bridge script will run and wait for the specified height of the network and upon reaching this height, will invoke `installLisk.sh` to update the code, migrate the database to the new model and update the config files.

If you're doing a fully automated migration, you could run `lisk_bridge.sh` inside of tmux, screen, byobu or another terminal multiplexer and detach your session even days ahead of time, though you'd want to minimize this lead time if you're exporting `LISK_MASTER_PASSWORD`.

> **Important note for delegates:** After automated upgrade, you still need to enable forging again manually, like described in [configuration section](configuration.md#enable-disable-forging)

We have prepared a small clip showing the expected output from the script execution.
You can watch it to verify your migration was completed as expected: https://www.youtube.com/watch?v=Zy9gyH-toBM

### Lisk Bridge Command Reference
For reference, here is the lisk_bridge.sh usage help:
```bash
Usage: bash lisk_bridge.sh <-h <BLOCKHEIGHT>> [-s <DIRECTORY>] [-n <NETWORK>]
-h <BLOCKHEIGHT> -- specify blockheight at which bridging will be initiated
-f <TARBALL>     -- specify path to local tarball containing the target release
-s <DIRECTORY>   -- Lisk home directory
-n <NETWORK>     -- choose main or test

Example: bash lisk_bridge.sh -h 50000000 -n test -s /home/lisk/lisk-test
Set the LISK_MASTER_PASSWORD environment variable if you want to do secrets migration in non-interactive mode
```

### What if I miss the block height or if Lisk Bridge script fails?

Don't panic!
Counting from the migration height, you have 2 full forging rounds time to upgrade your node manually by following the steps described in [Migrate manually](#migrate-manually).
If 2 full forging rounds have already passed since migration, your Node will be probably on a fork after the upgrade.
To resolve this, rebuild your version of the blockchain [from snaphot](introduction.md#snapshots) or [from genesis block](administration/binary.md#rebuild-from-the-genesis-block).

### Migration Notes Lisk Core 0.9 -> 1.0

#### Neccessary utility scripts

The following utility scripts are run by `lisk_bridge.sh` :

- [update_config.js](#update-config): migrates config to new structure

During the execution of `lisk_bridge.sh`, it will prompt you asking for a password in the case where it finds a passphrase.
It will encrypt and migrate that passphrase to the new format.
If you want to avoid this prompt and make a full-automated migration, add the next environment variable to your system:

```bash
export LISK_MASTER_PASSWORD=XXXXXXXX
``` 

## Migrate manually

To migrate a Lisk node manually, do the following steps:

1. Backup your data.
2. Run the necessary [utility scripts](#utility-scripts). 
These scripts prepare the Lisk node for the migration and are required before the upgrade script can run successfully. 
The utility scripts that need to be run can vary depending on the migration.
3. Go through the default [upgrade process](introduction.md#upgrade-vs-migration).

## Utility Scripts

You don't need to run these script if you have run `lisk_bridge.sh` before as it is automatically executed there.

There are a couple of command line scripts that facilitate users of lisk to perform handy operations.

All scripts are located under `./scripts/` directory and can be executed directly by `node scripts/<file_name>`.

### Generate Config

This script will help you to generate a unified version of the configuration file for any network. Here is the usage of the script:

```bash
Usage: node scripts/generate_config.js [options]

Options:

-h, --help               output usage information
-V, --version            output the version number
-c, --config [config]    custom config file
-n, --network [network]  specify the network or use LISK_NETWORK
```

Argument `network` is required and can by `devnet`, `testnet`, `mainnet` or any other network folder available under `./config` directory.

### Update Config

This script keeps track of all changes introduced in Lisk over time in different versions. 
If you have one config file in any of specific version and you want to make it compatible with other versions of the Lisk, this scripts will do it for you.

```bash
Usage: node scripts/update_config.js [options] <input_file> <from_version> [to_version]

Options:

-h, --help               output usage information
-V, --version            output the version number
-n, --network [network]  specify the network or use LISK_NETWORK
-o, --output [output]    output file path
```

As you can see from the usage guide, `input_file` and` from_version` are required.
If you skip `to_version` argument changes in config.json will be applied up to the latest version of Lisk Core.
If you do not specify `--output` path the final config.json will be printed to stdout.
If you do not specify `--network` argument you will have to load it from `LISK_NETWORK` env variable.
