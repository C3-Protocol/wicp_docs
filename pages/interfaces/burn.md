## Burn

```
burn: (nat) -> (BurnResponse);
```

### cmd

```
dfx --identity <identity> canister --network ic call <canister_id> burn '(<amount>:nat)'

# example
dfx --identity default canister --network ic call 7xlb5-raaaa-aaaai-qa2ja-cai burn '(10000000:nat)'
```

### Motoko

```
public func call_burn() : async wicp.TxReceipt {
    let burn_amount = 10000000;
    let response : wicp.TxReceipt = await _wicp_actor.burn(burn_amount);

    switch (response) {
        case (#ok transaction_index) {
            let transaction_index_text = Nat.toText(transaction_index);
            Debug.print(debug_show("Transaction index: " # transaction_index_text));
        };
        case (#err error) {
            Debug.print("Burn error: " # debug_show(error));
        };
    };
    return response;
};
```

