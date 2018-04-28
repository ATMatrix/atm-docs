# chinb.js 参考

some samples here

## Account

Account intro

## Account.genKey

Generate private/public key.

```javascript
const Account = require('Account');

let key     = Account.genKey(password);
let pubKey  = key.getPubKey();
let privKey = key.getPrivKey(password);
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
password | null | Password used to encrypt generated private key before storing on client's device

<aside class="success">
Is `Account` a singleton class design?
</aside>