```
import Nat "mo:base/Nat";
import Text "mo:base/Text";
import Blob "mo:base/Blob";
import Time "mo:base/Time";
import Nat64 "mo:base/Nat64";
import Debug "mo:base/Debug";
import Result "mo:base/Result";
import Principal "mo:base/Principal";

import wicp "./wicp";
import wicp_storage "./wicp_storage";
import ledger "./ledger";

actor {
    private let _wicp_canister_id = "7xlb5-raaaa-aaaai-qa2ja-cai";      // Test canister
    private let _wicp_actor : wicp.Self = actor(_wicp_canister_id);

    private let _wicp_storage_canister_id = "6bazw-eqaaa-aaaai-qa2ma-cai";      // Test canister
    private let _wicp_storage_actor : wicp_storage.Self = actor(_wicp_storage_canister_id);

    private let _ledger_canister_id = "ryjl3-tyaaa-aaaaa-aaaba-cai";
    private let _ledger_actor : ledger.Self = actor(_ledger_canister_id);

    public func call_balance_of() : async Nat {
        let user_principal_text : Text = "wo4ia-cbvf6-2mz2p-q3d5p-y46vw-hsidv-gpjey-r3zp5-w4q4z-3pgk5-lae";
        let user_principal = Principal.fromText(user_principal_text);
        let balance : Nat = await _wicp_actor.balanceOf(user_principal);
        let balance_text = Nat.toText(balance);
        Debug.print("Balance: " # balance_text);
        return balance;
    };

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

    public func call_batch_transfer_from() : async wicp.TxReceipt {
        let from_principal_text : Text = "wo4ia-cbvf6-2mz2p-q3d5p-y46vw-hsidv-gpjey-r3zp5-w4q4z-3pgk5-lae";
        let from_principal : Principal = Principal.fromText(from_principal_text);

        let to_principal_text_1 : Text = "pzkqc-kav3g-iojph-vlerb-2ozvj-lnt3s-cpd5v-3f5mi-p6xje-hy66x-uqe";
        let to_principal_text_2 : Text = "ljpnx-urbj3-7r4xj-wczwq-jkywa-kavjg-7etiu-h6jga-fajx3-dpqp5-gae";

        let to_principal_1 : Principal = Principal.fromText(to_principal_text_1);
        let to_principal_2 : Principal = Principal.fromText(to_principal_text_2);

        let transfer_amount_1 = 11;
        let transfer_amount_2 = 22;

        let to_list = [to_principal_1, to_principal_2];
        let transfer_amount_list = [transfer_amount_1, transfer_amount_2];
        let response : wicp.TxReceipt = await _wicp_actor.batchTransferFrom(from_principal, to_list, transfer_amount_list);

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

    public func call_allowance() : async Nat {
        let owner_principal_text : Text = "jnrdx-fw4lt-hgeaj-shxci-azalp-5zcy2-uwrji-eskcf-52t7i-2a3bj-fqe";
        let owner_principal : Principal = Principal.fromText(owner_principal_text);

        let spender_principal_text : Text = "yxshb-hrmwv-3vx7m-miuda-unbx2-jax5g-6ta2n-5gnri-gly7y-z6zyx-xqe";
        let spender_principal : Principal = Principal.fromText(spender_principal_text);

        let allowance_amount : Nat = await _wicp_actor.allowance(owner_principal, spender_principal);
        let allowance_amount_text = Nat.toText(allowance_amount);
        Debug.print("Allowance amount: " # allowance_amount_text);
        return allowance_amount;
    };

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

    public func call_get_history_by_account() : async [wicp_storage.OpRecord] {

        let user_principal_text : Text = "jnrdx-fw4lt-hgeaj-shxci-azalp-5zcy2-uwrji-eskcf-52t7i-2a3bj-fqe";
        let user_principal : Principal = Principal.fromText(user_principal_text);

        let op_records : [wicp_storage.OpRecord] = await _wicp_storage_actor.getHistoryByAccount(user_principal);

        return op_records;
    };
};
```

