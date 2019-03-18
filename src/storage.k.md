```
syntax Int ::= "#ERC20.allowance" "[" Int "]" "[" Int "]" [function]
// -----------------------------------------------
// doc: The allowance of `$1` for `$0`
rule #ERC20.allowance[A][B] => #hashedLocation("Solidity", 5, A B)
```
