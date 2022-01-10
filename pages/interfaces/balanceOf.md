## balance

```
  balanceOf: (principal) -> (nat) query;
```

### cmd

```
dfx canister --network ic call <canister id> balanceOf '(principal "<user principal>")'

# example
dfx canister --network ic call 7xlb5-raaaa-aaaai-qa2ja-cai balanceOf '(principal "ici4c-dgxam-uzc5b-rqbkm-pcfwc-hpyek-6vsom-u5rhm-cjsh7-6n64i-bae")'
```

### motoko

```
public func call_balance() : async Nat {
    let user_principal_text : Text = "wo4ia-cbvf6-2mz2p-q3d5p-y46vw-hsidv-gpjey-r3zp5-w4q4z-3pgk5-lae";
    let user_principal = Principal.fromText(user_principal_text);
    let balance : Nat = await _wicp_actor.balanceOf(user_principal);
    let balance_text = Nat.toText(balance);
    Debug.print("Balance: " # balance_text);
    return balance;
};
```

