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

## TransferFrom

`transferFrom` is pretty complicated and requires a few specs to cover it completely. We need to
define behaviour for the following cases:

- `src =/= dst`:
  - `src == msg.sender` or `allowance[src][msg.sender] == uint(-1)` (no checks on allowance)
  - `src =/= msg.sender` and `allowance[src][msg.sender] =/= uint(-1)`

- `src == dst`:
  - `src == msg.sender` or `allowance[src][msg.sender] == uint(-1)` (no checks on allowance)
  - `src =/= msg.sender` and `allowance[src][msg.sender] =/= uint(-1)`

### `src =/= dst`

First we specify the case where `src == msg.sender` or `allowance[src][msg.sender] == uint(-1)`:

```
behaviour transferFrom of ERC20
interface transferFrom(address Src, address Dst, uint Wad)

types

    Wad:       uint256
    SrcBal:    uint256
    DstBal:    uint256
    Allowance: uint256

storage

    #ERC20.balanceOf[Src] |-> SrcBal => SrcBal - Wad
    #ERC20.balanceOf[Dst] |-> DstBal => DstBal + Wad
    #ERC20.allowance[Src][CALLER_ID] |-> Allowance

iff in range uint256

    SrcBal - Wad
    DstBal + Wad

iff

    SrcBal >= Wad
    VCallValue == 0
    ((Src == CALLER_ID) orBool (Allowance == maxUInt256))

if

    Src =/= Dst

returns 1
```

Next we define the case where `src =/= msg.sender` and `allowance[src][msg.sender] =/= uint(-1)`.

```
behaviour transferFrom of ERC20
interface transferFrom(address Src, address Dst, uint Wad)

types

    Wad:       uint256
    SrcBal:    uint256
    DstBal:    uint256
    Allowance: uint256

storage

    #ERC20.balanceOf[Src]            |-> SrcBal    => SrcBal - Wad
    #ERC20.balanceOf[Dst]            |-> DstBal    => DstBal + Wad
    #ERC20.allowance[Src][CALLER_ID] |-> Allowance => Allowance - Wad

iff

    Src        =/= CALLER_ID
    Allowance  =/= maxUInt256

    SrcBal     >= Wad
    Allowance  >= Wad
    VCallValue == 0

iff in range uint256

    SrcBal    - Wad
    DstBal    + Wad
    Allowance - Wad

if

    Src =/= Dst

returns 1
```

### `src == dst`

First we define the case where `src == msg.sender` or `allowance[src][msg.sender] == uint(-1)`:

```
behaviour transferFrom of ERC20
interface transferFrom(address Src, address Dst, uint Wad)

types

    Wad:       uint256
    SrcBal:    uint256
    Allowance: uint256

storage

    #ERC20.balanceOf[Src] |-> SrcBal
    #ERC20.allowance[Src][CALLER_ID] |-> Allowance

iff

    SrcBal >= Wad
    VCallValue == 0
    ((Src == CALLER_ID) orBool (Allowance == maxUInt256))

if

    Src == Dst

returns 1
```

Finally we define the case where `src =/= msg.sender` and `allowance[src][msg.sender] =/= uint(-1)`:

```
behaviour transferFrom of ERC20
interface transferFrom(address Src, address Dst, uint Wad)

types

    Wad:       uint256
    SrcBal:    uint256
    Allowance: uint256

storage

    #ERC20.balanceOf[Src]            |-> SrcBal
    #ERC20.allowance[Src][CALLER_ID] |-> Allowance => Allowance - Wad

iff

    Src        =/= CALLER_ID
    Allowance  =/= maxUInt256

    SrcBal     >= Wad
    Allowance  >= Wad
    VCallValue == 0

if

    Src == Dst

returns 1
```
