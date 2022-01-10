## approve

```
approve: (principal, nat) -> (TxReceipt);
```

### cmd

```
dfx --identity <identity> canister --network ic call <canister id> approve '(principal "<spender principal>", <amount>:nat)'

# example
dfx --identity wicp_from_1 canister --network ic call 7xlb5-raaaa-aaaai-qa2ja-cai approve '(principal "pzkqc-kav3g-iojph-vlerb-2ozvj-lnt3s-cpd5v-3f5mi-p6xje-hy66x-uqe", 100:nat)'
```

### motoko

```
public func call_approve() : async wicp.TxReceipt {
    let spender_principal_text : Text = "yxshb-hrmwv-3vx7m-miuda-unbx2-jax5g-6ta2n-5gnri-gly7y-z6zyx-xqe";
    let spender_principal = Principal.fromText(spender_principal_text);
    let allowance_amount = 100;

    let response: wicp.TxReceipt = await _wicp_actor.approve(spender_principal, allowance_amount);
    switch(response){
        case(#ok transaction_index){
            let transaction_index_text = Nat.toText(transaction_index);
            Debug.print(debug_show("Succeeded, transaction_index: " # transaction_index_text));
        };
        case(#err reason){
            Debug.print("Error: " # debug_show(reason));
        };
    };
    return response;
};
```