allowance

```
oldAllowance: (principal, principal) -> (nat) query;
```

cmd

```
dfx --identity <identity> canister --network dev call WICP_motoko allowance '(record {token=""; owner = variant {"principal" = principal "wo4ia-cbvf6-2mz2p-q3d5p-y46vw-hsidv-gpjey-r3zp5-w4q4z-3pgk5-lae"}; spender = principal "ljpnx-urbj3-7r4xj-wczwq-jkywa-kavjg-7etiu-h6jga-fajx3-dpqp5-gae"})'
```

Motoko

```
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
```

