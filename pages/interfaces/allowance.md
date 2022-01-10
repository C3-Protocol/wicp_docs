## allowance

```
allowance: (principal, principal) -> (nat) query
```

### cmd

```
dfx canister --network ic call 7xlb5-raaaa-aaaai-qa2ja-cai allowance '(principal "<onwer principal>", principal "<spender principal>")'

# example
dfx canister --network ic call 7xlb5-raaaa-aaaai-qa2ja-cai allowance '(principal "wo4ia-cbvf6-2mz2p-q3d5p-y46vw-hsidv-gpjey-r3zp5-w4q4z-3pgk5-lae", principal "hiaj7-oiaaa-aaaai-abeaq-cai")'
```

### Motoko

```
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
```

