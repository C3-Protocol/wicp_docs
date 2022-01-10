## batchTransfer

```
batchTransfer: (vec principal, vec nat) -> (TransferResponse);
```

### cmd

```
dfx --identity <identity> canister --network ic call <canister_id> batchTransfer '(vec{principal "<to_principal_1>";principal "<to_principal_2>";}, vec{<amount_1>:nat; <amount_2>:nat})'

# example
dfx --identity wicp_from_1 canister --network ic call 7xlb5-raaaa-aaaai-qa2ja-cai batchTransfer '(vec{principal "pzkqc-kav3g-iojph-vlerb-2ozvj-lnt3s-cpd5v-3f5mi-p6xje-hy66x-uqe";principal "ljpnx-urbj3-7r4xj-wczwq-jkywa-kavjg-7etiu-h6jga-fajx3-dpqp5-gae";}, vec{1:nat; 2:nat})'
```

### Motoko

```
public func call_batch_transfer() : async wicp.TxReceipt {
    let to_principal_text_1 : Text = "pzkqc-kav3g-iojph-vlerb-2ozvj-lnt3s-cpd5v-3f5mi-p6xje-hy66x-uqe";
    let to_principal_text_2 : Text = "ljpnx-urbj3-7r4xj-wczwq-jkywa-kavjg-7etiu-h6jga-fajx3-dpqp5-gae";
    let to_principal_1 = Principal.fromText(to_principal_text_1);
    let to_principal_2 = Principal.fromText(to_principal_text_2);
    let transfer_amount_1 = 11;
    let transfer_amount_2 = 22;
    let to_list = [to_principal_1, to_principal_2];
    let transfer_amount_list = [transfer_amount_1, transfer_amount_2];
    let response : wicp.TxReceipt = await _wicp_actor.batchTransfer(to_list, transfer_amount_list);
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

