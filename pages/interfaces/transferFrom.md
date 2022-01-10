## transferFrom

```
transferFrom: (principal, principal, nat) -> (TxReceipt);
```

### cmd

```
dfx --identity <spender identity> canister --network ic call <canister id> transferFrom '(principal "<from principal>", principal "<to principal>", <amount>:nat)'

# example
dfx --identity wicp_spender_1 canister --network ic call 7xlb5-raaaa-aaaai-qa2ja-cai transferFrom '(principal "wo4ia-cbvf6-2mz2p-q3d5p-y46vw-hsidv-gpjey-r3zp5-w4q4z-3pgk5-lae", principal "pzkqc-kav3g-iojph-vlerb-2ozvj-lnt3s-cpd5v-3f5mi-p6xje-hy66x-uqe", 10:nat)'
```

### motoko

```
public func call_transfer_from() : async wicp.TxReceipt {
    let from_principal_text : Text = "wo4ia-cbvf6-2mz2p-q3d5p-y46vw-hsidv-gpjey-r3zp5-w4q4z-3pgk5-lae";
    let to_principal_text : Text = "pzkqc-kav3g-iojph-vlerb-2ozvj-lnt3s-cpd5v-3f5mi-p6xje-hy66x-uqe";
    let from_principal = Principal.fromText(from_principal_text);
    let to_principal = Principal.fromText(to_principal_text);
    let transfer_amount = 1;
    let response : wicp.TxReceipt = await _wicp_actor.transferFrom(from_principal, to_principal, transfer_amount);
    switch (response) {
        case (#ok transaction_index) {
            let transaction_index_text = Nat.toText(transaction_index);
            Debug.print(debug_show("Succeeded, transaction_index: " # transaction_index_text));
        };
        case (#err reason) {
            Debug.print("Error: " # debug_show(reason));
        };
    };
    return response;
};
```

