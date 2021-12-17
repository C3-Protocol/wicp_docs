## Burn

```
burn: (nat) -> (BurnResponse);
```

### cmd

```
dfx --identity <identity> canister --network ic call <canister_id> burn '(1000000009:nat)'
```

### Motoko

```
let burn_amount = 100000000;
let response : wicp.BurnResponse = await _wicp_actor.burn(burn_amount);

switch (response) {
    case (#ok transaction_index) {
        let transaction_index_text = Nat.toText(transaction_index);
        Debug.print(debug_show("Transaction index: " # transaction_index_text));
    };
    case (#err error) {
        Debug.print("Burn error: " # debug_show(error));
    };
};
```

