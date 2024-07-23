# Foundry Cheatsheet

## Project Initialization

```shell
forge init
```

Description: Initialize a new Foundry project.

## Compilation

```shell
forge compile
```

Description: Compile Solidity contracts.

```shell
forge build
```

Description: Build the project (compile and generate artifacts).

## Local Blockchain

```shell
anvil
```

Description: Start a local Ethereum node for development.

## Environment Setup

```env
# .env.example
RPC_URL=http://127.0.0.1:8545
CHAIN_ID=31337
```

Description: Example environment variables for local development.

```shell
source .env
```

Description: Load environment variables from .env file.

## Contract Deployment

### Basic Deployment

```shell
forge create <ContractName> --rpc-url $RPC_URL --private-key $PRIVATE_KEY
```

Description: Deploy a contract to the specified RPC URL using a private key.

### Interactive Deployment

```shell
forge create <ContractName> --rpc-url $RPC_URL --interactive
```

Description: Deploy a contract interactively (prompts for private key).

### Deployment using Scripts

```shell
forge script script/<ScriptName>.s.sol
```

Description: Run a deployment script without broadcasting (simulates deployment).

```shell
forge script script/<ScriptName>.s.sol --rpc-url $RPC_URL --private-key $PRIVATE_KEY --broadcast
```

Description: Run a deployment script and broadcast the transaction.

## Utility Commands

### Hex to Decimal Conversion

```shell
cast --to-base 0x714e1 dec
```

Description: Convert hexadecimal to decimal.

## Wallet Management

### Import Private Key

```shell
cast wallet import defaultKey --interactive
```

Description: Import a private key interactively and encrypt it.

### List Wallets

```shell
cast wallet list
```

Description: List all imported wallets.

### Deploy with Encrypted Key

```shell
forge script script/<ScriptName>.s.sol --rpc-url $RPC_URL --account defaultKey --sender <PublicKeyAddress> --broadcast -vvvv
```

Description: Deploy using an encrypted key (prompts for password).

### Deploy with Password File

```shell
forge script script/<ScriptName>.s.sol --rpc-url $RPC_URL --account defaultKey --password-file .password --broadcast -vvvv
```

```shell
forge script script/<ScriptName>.s.sol --rpc-url $RPC_URL --account defaultKey --password-file .password --sender <PublicKeyAddress>  --broadcast -vvvv
```

Description: Deploy using an encrypted key with password stored in a file.

## Contract Interaction

### Send Transaction

```shell
cast send <ContractAddress> "functionName(type1,type2)" param1 param2 --rpc-url $RPC_URL --private-key $PRIVATE_KEY
```

```shell
cast send <ContractAddress> "functionName(type1,type2)" param1 param2 --rpc-url $RPC_URL --account defaultKey --password-file .password
```

Description: Send a transaction to interact with a contract function.

### Call View Function

```shell
cast call <ContractAddress> "functionName()"
```

Description: Call a view function on a contract (no state change).

## Code Formatting

```shell
forge fmt
```

Description: Format Solidity code in the project.

## ZKSync Specific Commands

### Install ZKSync Foundry

```shell
foundryup-zksync
```

Description: Install ZKSync-specific version of Foundry.

### Build for ZKSync

```shell
forge build --zksync
```

Description: Build the project for ZKSync.

### Switch back to Regular Foundry

```shell
foundryup
```

Description: Switch back to the regular version of Foundry.

### Deploy to ZKSync Local Node

```shell
forge create <ContractName> --rpc-url <ZKSyncRpcUrl> --zksync --legacy
```

Description: Deploy a contract to a ZKSync local node.

### Start ZKSync Local Node

```shell
npx zksync-cli dev config
# enter
# enter
```

```shell
npx zksync-cli dev start
```

Description: Start a local ZKSync node for development.

### Deploy to Local ZKSync Chain

```shell
forge create src/<ContractName>.sol:<ContractName> --rpc-url http://127.0.0.1 --private-key <PrivateKey> --zksync --legacy
```

Description: Deploy a contract to a local ZKSync chain.

## Additional Notes

1. When deploying contracts with a payable constructor, you can specify the value to send:

   ```solidity
   SimpleStorage ss = new SimpleStorage{value: 10 ether}();
   ```

2. For Sepolia testnet deployment with verification:

   ```shell
   forge script script/<ScriptName>.s.sol --rpc-url $SEPOLIA_RPC_URL --account defaultKey --sender <PublicKeyAddress> --verify --etherscan-api-key $ETHERSCAN_API_KEY --broadcast -vvvv
   ```

3. Always ensure that sensitive files like `.password` are included in your `.gitignore` file.

4. The encrypted version of your private key is stored in `~/.foundry/keystores/`.

5. ZKSync scripting support is limited. Check the latest documentation for updates.

6. When using `cast call` for view functions, the returned value is often in hex format. Use `cast --to-base` to convert it to a readable format if needed.
