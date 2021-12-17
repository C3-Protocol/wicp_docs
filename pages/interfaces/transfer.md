## transfer

```
transfer: (TransferRequest) -> (EXTTransferResponse);
```

### cmd

```
dfx --identity <identity> canister --network ic call <canister id> transfer '(record {amount = <amount>:nat;from = variant {"principal"=principal "<from principal>"};memo=<memo>"\n":blob;notify=false;subaccount=opt vec{};to= variant {"principal"=principal "<to principal>"};token="";})'
```

### motoko

```
let from_principal_text : Text = "wo4ia-cbvf6-2mz2p-q3d5p-y46vw-hsidv-gpjey-r3zp5-w4q4z-3pgk5-lae";
let to_principal_text : Text = "pzkqc-kav3g-iojph-vlerb-2ozvj-lnt3s-cpd5v-3f5mi-p6xje-hy66x-uqe";
let from_principal = Principal.fromText(from_principal_text);
let to_principal = Principal.fromText(to_principal_text);
let transfer_amount = 1;
let memo_text = "test";
let memo_blob = Text.encodeUtf8(memo_text);
let memo = Blob.toArray(memo_blob);
let request : wicp.TransferRequest = {
    to = #principal(to_principal);      //Only support Principal now
    token = "";
    notify = false;
    from = #principal(from_principal);      //Only support Principal now
    memo = memo;
    subaccount = null;      //Not support sub-acccount now
    amount = transfer_amount;
};
let response : wicp.EXTTransferResponse = await _wicp_actor.transfer(request);
switch (response) {
    case (#ok transaction_index) {
        let transaction_index_text = Nat.toText(transaction_index);
        Debug.print(debug_show("succeeded, transaction_index: " # transaction_index_text));
    };
    case (#err reason) {
        Debug.print("Error: " # debug_show(reason));
    };
};
```