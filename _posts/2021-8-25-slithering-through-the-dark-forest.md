---
layout: post
title: Slithering Through the Dark Forest
summary: Using a slither tool to unlock a contract with a hidden key. 
image: /images/slithering-through-the-dark-forest.jpg
---
Using a slither tool to unlock a contract with a hidden key. 

### A Coincidence of Wants

As I worked through the Ethernaut challenges, I started familiarizing myself with Trail of Bits' static analyzer [Slither](https://github.com/crytic/slither/). An [issue](https://github.com/crytic/slither/issues/793) on the Github repo gave me the idea to build a tool that reads storage from deployed smart contracts without needing to know the storage slot beforehand. This tool is a WIP.

`slither-read-storage` makes it quick and easy to find the storage slot and value of a variable. It works by using Slither's underlying parsing engine to determine the storage layout of a given contract. Then, it's trivial to retrieve the value by calling `web3.getStorageAt([address], [slot])` under the hood.

Incidentally, this turned solving the "Privacy" challenge on the [Ethernaut wargame](https://ethernaut.openzeppelin.com/) into an opportunity to contribute back to Slither and learn the inner workings of how Solidity handles storage.

### Solving the "Privacy" Ethernaut Challenge 

In order to win, we need to set `locked` to false by determing a value `_key` that will equal `data[2]`.

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Privacy {

  bool public locked = true;
  uint256 public ID = block.timestamp;
  uint8 private flattening = 10;
  uint8 private denomination = 255;
  uint16 private awkwardness = uint16(now);
  bytes32[3] private data;

  constructor(bytes32[3] memory _data) public {
    data = _data;
  }
  
  function unlock(bytes16 _key) public {
    require(_key == bytes16(data[2]));
    locked = false;
  }

  /*
    A bunch of super advanced solidity algorithms...

      ,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`
      .,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,
      *.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^         ,---/V\
      `*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.    ~|__(o.o)
      ^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'  UU  UU
  */
}
```

First, we pass the file of the target contract, the target address, the target variable name, and our rinkeby node's url. This effectively retrieve's the storage slot of `data[0]`.

```
$ slither-read-storage Privacy.sol 0xFdaF948A66903a0050ed051508c7C26D137E76C4 data --rpc-url https://rinkeby.infura.io/v3/{project-id} 

> Privacy.data type bytes32[3] evaluated to:
> 0x5ff6b6a895e912882837aab9887e5db421a2e29fd3f1603e22a98a6a92ac47ce
> in Privacy at storage slot:
> 3
```

Since we want `data[2]` and each element of the byte array fills its own slot, we also pass `--key 2`, traversing two indices forward in the array.

```
$ slither-read-storage Privacy.sol 0xFdaF948A66903a0050ed051508c7C26D137E76C4 data --rpc-url https://rinkeby.infura.io/v3/{project-id} --key 2

> Privacy.data type bytes32[3] evaluated to:
> 0x1ffbae565d970e3c29ccf51e52978efa673f5e0d64fee8d734d069beb97dac7c
> in Privacy at storage slot:
> 5
```

We have retrieved `data[2]` from the byte array which we must coerce from a byte32 value into a byte16 value as `unlock`'s parameter requires. Since the hexidecimal value has a length of 66, and the "0x" prefix helps the parser recognize hexidecmial, we will select the first 34 characters. 

```
$ "0x1ffbae565d970e3c29ccf51e52978efa673f5e0d64fee8d734d069beb97dac7c".slice(0,34)
> "0x1ffbae565d970e3c29ccf51e52978efa"
```

Now we invoke the contract's `unlock` method.

```
$ contract.unlock("0x1ffbae565d970e3c29ccf51e52978efa")
```

To verify we are [succesful](https://rinkeby.etherscan.io/tx/0x9d3cc85510b21d86953aab8d2b3d3dc5ab7dc52d12a2eaf608cec3b7ea2168c7#statechange), we check the value of `locked`.

```
$ await contract.locked()
> false
```

We've successfully unlocked the contract by reading the private variable with the help of Slither. In the dark forest of Ethereum, `private` variables are not, in fact, secret.

### Further Resources

To read more about how Ethereum storage works, check out [this section](https://docs.soliditylang.org/en/v0.8.7/internals/layout_in_storage.html) of the Solidity language documentation. For an alternative approach, I recommend checking out this [write-up](https://medium.com/coinmonks/ethernaut-lvl-12-privacy-walkthrough-how-ethereum-optimizes-storage-to-save-space-and-be-less-c9b01ec6adb6) (note: the Privacy.sol contract has a different storage layout since it contains constants).
