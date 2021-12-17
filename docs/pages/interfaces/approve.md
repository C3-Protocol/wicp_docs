## approve

```
approve: (ApproveRequest) -> ();
```

### cmd

```
dfx --identity <identity> canister --network ic call <canister id> approve '(record {allowance = <amount>:nat; spender = principal "<spender principal>" ;subaccount=opt vec{};token="";})'
```

### motoko

```
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
```