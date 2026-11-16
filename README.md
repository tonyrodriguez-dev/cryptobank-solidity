# cryptobank-solidity

Educational Solidity ETH bank with per-user balances, a per-user deposit cap that only the admin can change, and step-by-step Remix tests.

## What it does

- `depositEther()` is `payable`: the caller's ETH is added to their own line in `userBalance`.
- `withdrawEther(amount_)` lets a user take out **only what they previously deposited**.
- `maxBalance` is the most ETH **one user** can have deposited at once. It is set in the constructor and changed with `modifyMaxBalance`, which only the `admin` can call.
- Every deposit and withdrawal emits an event (`EtherDeposit`, `EtherWithdraw`).
- `withdrawEther` follows the checks-effects-interactions order: it updates the balance **before** sending the ETH, so a malicious receiver cannot reuse the old balance.

## Try it in Remix

1. Compile [CryptoBank.sol](CryptoBank.sol) with **0.8.24** and select **Remix VM**.
2. Deploy with `maxBalance_` = `5000000000000000000` (5 ETH in wei) and `admin_` = the address of Account 1.
3. Account 2 sets **Value** to `3` ether and calls `depositEther`. `userBalance(Account 2)` shows 3 ETH.
4. Account 2 deposits 3 ETH again. It reverts with `MaxBalance reached` (3 + 3 is more than 5).
5. Account 3 deposits 2 ETH. It passes. The contract now holds 5 ETH.
6. Set **Value** back to `0`. Account 3 calls `withdrawEther` with `3000000000000000000`. It reverts with `Not enough ether`.
7. Account 2 withdraws `1000000000000000000`. `userBalance(Account 2)` becomes 2 ETH and the contract holds 4 ETH.
8. Account 2 calls `modifyMaxBalance`. It reverts with `Not allowed`. Account 1 calls it with `10000000000000000000` and it passes.

## Credits and license

Contract written in class for the Blockchain Accelerator course taught by José; this repository holds my copy of the exercise with added documentation. Released under **GPL-3.0**, matching the license header of the contract.

This is a learning exercise and has not been audited. Do not use it with real funds.
