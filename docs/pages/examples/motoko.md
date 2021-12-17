```
import Nat "mo:base/Nat";
import Text "mo:base/Text";
import Blob "mo:base/Blob";
import Debug "mo:base/Debug";
import Result "mo:base/Result";
import Principal "mo:base/Principal";
import wicp "./wicp";

actor {
    private let _wicp_canister_id = "r3uro-piaaa-aaaai-abafa-cai";
    private let _wicp_actor : wicp.Self = actor(_wicp_canister_id);

    public func call_balance() : async Nat {
        let user_principal_text : Text = "wo4ia-cbvf6-2mz2p-q3d5p-y46vw-hsidv-gpjey-r3zp5-w4q4z-3pgk5-lae";
        let user_principal = Principal.fromText(user_principal_text);
        let request : wicp.BalanceRequest = {
            token = "";     // Not support sub-token
            user = #principal(user_principal);      // Only support Principal
        };
        let response : wicp.BalanceResponse = await _wicp_actor.balance(request);
        switch (response) {
            case (#ok balance) {
                let balance_text = Nat.toText(balance);
                Debug.print(debug_show("balance: " # balance_text));
            };
            case (#err common_error) {
                switch (common_error) {
                    case(#InvalidToken token){
                        Debug.print("Invalid token: " # token);
                    };
                    case(#Other reason){
                        Debug.print("Error: " # reason);
                    };
                };
            };
        };
        return 0;
    };

    public func call_approve() : async Nat {
        let spender_principal_text : Text = "yxshb-hrmwv-3vx7m-miuda-unbx2-jax5g-6ta2n-5gnri-gly7y-z6zyx-xqe";
        let spender_principal = Principal.fromText(spender_principal_text);
        let allowance_amount = 100;
        let request : wicp.ApproveRequest = {
            token = "";
            subaccount = null; 
            allowance = allowance_amount;
            spender = spender_principal;
        };
        await _wicp_actor.approve(request);
        Debug.print("succeeded");
        return 0;
    };

    public func call_transfer() : async Nat {
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
        return 0;
    };

    public func call_batch_transfer() : async Nat {
        let to_principal_text_1 : Text = "pzkqc-kav3g-iojph-vlerb-2ozvj-lnt3s-cpd5v-3f5mi-p6xje-hy66x-uqe";
        let to_principal_text_2 : Text = "ljpnx-urbj3-7r4xj-wczwq-jkywa-kavjg-7etiu-h6jga-fajx3-dpqp5-gae";
        let to_principal_1 = Principal.fromText(to_principal_text_1);
        let to_principal_2 = Principal.fromText(to_principal_text_2);
        let transfer_amount_1 = 11;
        let transfer_amount_2 = 22;
        let to_list = [to_principal_1, to_principal_2];
        let transfer_amount_list = [transfer_amount_1, transfer_amount_2];
        let response : wicp.TransferResponse = await _wicp_actor.batchTransfer(to_list, transfer_amount_list);
        switch (response) {
            case (#ok transaction_index) {
                let transaction_index_text = Nat.toText(transaction_index);
                Debug.print(debug_show("succeeded, transaction_index: " # transaction_index_text));
            };
            case (#err reason) {
                Debug.print("Error: " # debug_show(reason));
            };
        };
        return 0;
    };

    public func call_batch_transfer_from() : async Nat {
        let from_principal_text_1 : Text = "wo4ia-cbvf6-2mz2p-q3d5p-y46vw-hsidv-gpjey-r3zp5-w4q4z-3pgk5-lae";
        let from_principal_text_2 : Text = "qelq6-hkhf3-4nquw-ks2ag-o2sfs-gdor7-v2rc5-6g7py-u2nr2-kncwx-7qe";

        let to_principal_text_1 : Text = "pzkqc-kav3g-iojph-vlerb-2ozvj-lnt3s-cpd5v-3f5mi-p6xje-hy66x-uqe";
        let to_principal_text_2 : Text = "ljpnx-urbj3-7r4xj-wczwq-jkywa-kavjg-7etiu-h6jga-fajx3-dpqp5-gae";

        let to_principal_1 = Principal.fromText(to_principal_text_1);
        let to_principal_2 = Principal.fromText(to_principal_text_2);

        let transfer_amount_1 = 11;
        let transfer_amount_2 = 22;

        let to_list = [to_principal_1, to_principal_2];
        let transfer_amount_list = [transfer_amount_1, transfer_amount_2];
        let response : wicp.TransferResponse = await _wicp_actor.batchTransfer(to_list, transfer_amount_list);

        switch (response) {
            case (#ok transaction_index) {
                let transaction_index_text = Nat.toText(transaction_index);
                Debug.print(debug_show("succeeded, transaction_index: " # transaction_index_text));
            };
            case (#err reason) {
                Debug.print("Error: " # debug_show(reason));
            };
        };
        return 0;
    };

    public func call_allowance() : async Nat {
        let owner_principal_text : Text = "jnrdx-fw4lt-hgeaj-shxci-azalp-5zcy2-uwrji-eskcf-52t7i-2a3bj-fqe";
        let owner_principal : Principal = Principal.fromText(owner_principal_text);

        let spender_principal_text : Text = "yxshb-hrmwv-3vx7m-miuda-unbx2-jax5g-6ta2n-5gnri-gly7y-z6zyx-xqe";
        let spender_principal : Principal = Principal.fromText(spender_principal_text);

        let request : wicp.AllowanceRequest = {
            token = "";      // Not support sub-token
            owner = #principal(owner_principal);
            spender = spender_principal;
        };

        let result : Result.Result<wicp.Balance, wicp.CommonError> = await _wicp_actor.allowance(request);
        
        switch (result) {
            case (#ok allowance_balance) {
                let allowance_balance_text = Nat.toText(allowance_balance);
                Debug.print(debug_show("allowance_balance: : " # allowance_balance_text));
            };
            case (#err common_error) {
                Debug.print("Error: " # debug_show(common_error));
            };
        };
        return 0;
    };
};
```

