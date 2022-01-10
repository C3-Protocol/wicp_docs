## Swap(mint)

```
swap: (ICPTransactionRecord) -> (MintResponse)
```

### cmd

```
dfx --identity <spender identity> canister --network ic call <canister id> getReceiveICPAcc

dfx --identity <spender identity> ledger --network ic transfer <receive address> --amount <amount> --memo <memo>

dfx --identity <spender identity> canister --network ic call <canister id> swap '(record {blockHeight = <block height>:nat64})'

# example
dfx --identity default canister --network ic call 7xlb5-raaaa-aaaai-qa2ja-cai getReceiveICPAcc
(vec { "ee23aeae43e99756de3ef2ea6501b9b3521f478d072089295fcd3a6407ae961e" })

dfx --identity default ledger --network ic transfer ee23aeae43e99756de3ef2ea6501b9b3521f478d072089295fcd3a6407ae961e --amount 0.0002 --memo 0
Transfer sent at BlockHeight: 1938251

dfx --identity default canister --network ic call 7xlb5-raaaa-aaaai-qa2ja-cai swap '(record {blockHeight = 1938251:nat64})'
(variant { 24_860 = 19_359 : nat })

```

### Motoko

```
private func _get_receive_icp_address() : async wicp.AccountIdentifier {
    let account_identifier_array : [wicp.AccountIdentifier] = await _wicp_actor.getReceiveICPAcc();
    assert (account_identifier_array.size() > 0);
    return account_identifier_array[0];
};

private func _transfer_to_wicp_receive_address(to_account : wicp.AccountIdentifier) : async Nat64 {
    let fee_ : ledger.ICPTs = {
        e8s = 10000;
    };
    let now = Time.now();
    let now_nat64 = Nat64.fromIntWrap(now);
    let nowTimestamp : ledger.TimeStamp = { timestamp_nanos = now_nat64 };
    let send_args : ledger.SendArgs = {
        to = to_account;
        fee = fee_;
        memo = 0;
        from_subaccount = null;
        created_at_time = ?nowTimestamp;
        amount = {e8s=1000};
    };

    let block_height : ledger.BlockHeight = await _ledger_actor.send_dfx(send_args);
    return block_height;
};

public func call_swap() : async wicp.MintResponse {
    let to_account = await _get_receive_icp_address();
    let block_height = await _transfer_to_wicp_receive_address(to_account);

    let request : wicp.ICPTransactionRecord = {
        from_subaccount = null;     // Not support sub-token
        blockHeight = block_height;
    };

    let response : wicp.MintResponse = await _wicp_actor.swap(request);
    switch (response) {
        case (#ok transaction_index) {
            let transaction_index_text = Nat.toText(transaction_index);
            Debug.print(debug_show("Transaction index: " # transaction_index_text));
        };
        case (#err error) {
            Debug.print("Mint error: " # debug_show(error));
        };
    };
    return response;
};
```

