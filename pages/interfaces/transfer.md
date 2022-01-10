## transfer

```
batchTransfer: (vec principal, vec nat) -> (TxReceipt)
```

### cmd

```
dfx --identity <identity> canister --network ic call <canister id> transfer '(principal "<to principal>", <amount>:nat)'

# example
dfx canister --network ic call 7xlb5-raaaa-aaaai-qa2ja-cai transfer '(principal "7xlb5-raaaa-aaaai-qa2ja-cai", 100000000:nat)'
```

### motoko

```
public func call_transfer() : async wicp.TxReceipt {
    let to_principal_text : Text = "pzkqc-kav3g-iojph-vlerb-2ozvj-lnt3s-cpd5v-3f5mi-p6xje-hy66x-uqe";
    let to_principal = Principal.fromText(to_principal_text);
    let transfer_amount = 1;

    let response : wicp.TxReceipt = await _wicp_actor.transfer(to_principal, transfer_amount);
    switch (response) {
        case (#ok transaction_index) {
            let transaction_index_text = Nat.toText(transaction_index);
            Debug.print(debug_show("succeeded, transaction_index: " # transaction_index_text));
        };
        case (#err reason) {
            Debug.print("Error: " # debug_show(reason));
        };
    };
    return response;
};
```