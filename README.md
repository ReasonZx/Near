# Backend Contract Development in AssemblyScript


### Installing modules:
```
$yarn global add near-cli
$yarn global add assemblyscript
$yarn global add asbuild

$yarn add -D near-sdk-as
$yarn add -D --ignore-scripts near-sdk-as
```

(Implement contract in assembly/index.ts and model.ts)


## 1. Compile the contract
```
$yarn asb
```
It will go to ./build/release/near-marketplace-contract.wasm




# Backend Contract Development in Rust


### Installing modules:

```
$yarn global add near-cli
$curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
$source $HOME/.cargo/env
$rustup target add wasm32-unknown-unknown

$cargo new near-marketplace-contract --lib
$cd near-marketplace-contract
```
Update Cargo.toml file
```
$cargo update
```
(Implement contract in src/lib.rs)


## 1. Compile the contract
```
$RUSTFLAGS='-C link-arg=-s' cargo build --target wasm32-unknown-unknown --release
```
It will go to target/wasm32-unknown-unknown/release/near_marketplace_contract.wasm



# Common Steps



## 2. Deploy the contract
(create subaccount: marketplace.reasonzx.testnet - see point 5)
```
$near deploy --accountId=marketplace.reasonzx.testnet --wasmFile=build/release/near-marketplace-contract.wasm
```
(Init the contract - Rust)
```
$near call marketplacerust.reasonzx.testnet init --accountId=reasonzx.testnet
```

## 3. Interacting with the contract
```
$near call marketplace.reasonzx.testnet setProduct '{"product": {"id": "0", "name": "BBQ", "description": "Grilled chicken and beef served with vegetables and chips.", "location": "Berlin, Germany", "price": "1000000000000000000000000", "image": "https://i.imgur.com/yPreV19.png"}}' --accountId=myaccount.testnet

$near view marketplace.reasonzx.testnet getProduct '{"id": "0"}'
```

## 4. Redeploy the contract
```
$near deploy --accountId=marketplace.reasonzx.testnet --wasmFile=build/release/near-marketplace-contract.wasm
```

## 5. Create a sub account
```
$near create-account buyeraccount.reasonzx.testnet --masterAccount reasonzx.testnet --initialBalance 6
```

## 6. Call contract with token transfer
```
near call mycontract.myaccount.testnet buyProduct '{"productId": "0"}' --depositYocto=1000000000000000000000000 --accountId=buyeraccount.myaccount.testnet
```



# Frontend Project

Install node
```
$npm install react-scripts@4.0.3
$npm install near-api-js
$npm install uuid
```

(Change first line of ./near-marketplace/src/utils/config.js to the smartcontract name)
```
$npm start
```