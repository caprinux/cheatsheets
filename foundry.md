## forge

### compile a contract

`forge build` -- builds all `.sol` files in `./src/` directory

### deploy a contract

```
forge create --rpc-url INSERT_RPC_API_ENDPOINT \
--constructor-args 100 \
--private-key INSERT_YOUR_PRIVATE_KEY \
src/MyToken.sol:MyToken
```

## cast

### get wallet address from private key

`cast wallet address PRIVATE_KEY`

### get balance of wallet at given address (in ether)

`cast balance -e WALLET_ADDRESS`

### dry run call a contract function

`cast call address 'functionName()'`

### transact and call a contract function

`cast send --private-key PRIVATE_KEY address 'functionName()'`
