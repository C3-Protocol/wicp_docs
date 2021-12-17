## Mint

```
burn: (nat) -> (BurnResponse);
```

### cmd

```
dfx --identity <identity> canister --network ic call <canister_id> mint '(record {from_subaccount = opt vec{}; blockHeight = <block_height>:nat64})'
```

### Motoko

```
let request : wicp.ICPTransactionRecord = {
    from_subaccount = null;     // Not support sub-token
    blockHeight = 1586015;
};

let response : wicp.MintResponse = await _wicp_actor.mint(request);
switch (response) {
    case (#ok transaction_index) {
        let transaction_index_text = Nat.toText(transaction_index);
        Debug.print(debug_show("Transaction index: " # transaction_index_text));
    };
    case (#err error) {
        Debug.print("Mint error: " # debug_show(error));
    };
};
```

