## getHistoryByAccount

```
getHistoryByAccount: (principal) -> (vec OpRecord__2) query;
```

### cmd

```
dfx --identity <identity> canister --network ic call <canister_id> getHistoryByAccount '(principal "<user_principal>")'
```

### Motoko

```
let user_principal_text : Text = "jnrdx-fw4lt-hgeaj-shxci-azalp-5zcy2-uwrji-eskcf-52t7i-2a3bj-fqe";
let user_principal : Principal = Principal.fromText(user_principal_text);

let op_records : [wicp.OpRecord__2] = await _wicp_storage_actor.getHistoryByAccount(user_principal);
```

