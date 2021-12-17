## balance

```
balance: (BalanceRequest) -> (BalanceResponse) query;
```

### cmd

```
dfx --identity <identity> canister --network ic call <canister id> balance '(record {user = variant {"principal"=principal "<user pricipal>"}; token="";})'
```

### motoko

```
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
```

