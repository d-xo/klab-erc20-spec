```
behaviour approve of ERC20
interface approve(address Guy, uint Wad)

types

    Approval : uint256

storage

    #hashedLocation("Solidity", 5, CALLER_ID Guy) |-> Allowance => Wad

iff

    VCallValue == 0

returns True
```
