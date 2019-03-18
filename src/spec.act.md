# ERC-20

## Approve

```
behaviour approve of ERC20
interface approve(address Guy, uint Wad)

storage

    #ERC20.allowance[CALLER_ID][Guy] |-> Allowance => Wad

iff

    VCallValue == 0

returns 1
```
