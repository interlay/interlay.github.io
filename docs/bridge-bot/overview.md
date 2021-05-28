# Bot

TypeScript utility to load test cross-chain systems based on XCLAIM (currently only targeting PolkaBTC).

This bot is in a very early stage, expect crashes. Bug reports, fixes and suggestions are welcome!

You can run the bot with the following options:
```
  --help                     Show help                                 [boolean]
  --version                  Show version number                       [boolean]
  --heartbeats               Try to issue and redeem slightly more than the
                             redeem dust value with every registered vault.
                             Mutually exclusive with the
                             `execute-pending-redeems` flag.
                                                       [boolean] [default: true]
  --per-hour                 Frequency of issuing and redeeming with each vault
                             in the system. Example: 0.5 => issue and redeem
                             every two hours.           [number] [default: 0.33]
  --execute-pending-redeems  Try to execute redeem requests whose BTC payment
                             has already been made. Mutually exclusive with the
                             `heartbeats` flag.       [boolean] [default: false]
```