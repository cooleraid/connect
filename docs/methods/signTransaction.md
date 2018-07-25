## Sign transaction

Asks device to sign given
inputs and outputs of pre-composed transaction. User is asked to confirm all transaction
details on TREZOR.


ES6
```javascript
const result = await TrezorConnect.signTransaction(params);
```

CommonJS
```javascript
TrezorConnect.signTransaction(params).then(function(result) {

});
```

### Params 
[****Optional common params****](commonParams.md)
###### [flowtype](../../src/js/types/params.js#L137-L142)
* `inputs` - *obligatory* `Array` of [TransactionInput](../../src/js/types/trezor.js#L128-L139),
* `outputs` - *obligatory* `Array` of [TransactionOutput](../../src/js/types/trezor.js#L141-L153),
* `coin` - *obligatory* `string` Determines network definition specified in [coins.json](../../src/data/coins.json) file. Coin `shortcut`, `name` or `label` can be used.
* `push` - *optional* `boolean` Broadcast signed transaction to blockchain. Default is set to false

### Example
###### PAYTOADDRESS
```javascript
TrezorConnect.signTransaction({
    inputs: [
        {
            address_n: [44 | 0x80000000, 0 | 0x80000000, 2 | 0x80000000, 1, 0],
            prev_index: 0,
            prev_hash: 'b035d89d4543ce5713c553d69431698116a822c57c03ddacf3f04b763d1999ac'
        }
    ],
    outputs: [
        {
            address_n: [44 | 0x80000000, 0 | 0x80000000, 2 | 0x80000000, 1, 1],
            amount: 3181747,
            script_type: 'PAYTOADDRESS'
        }, {
            address: '18WL2iZKmpDYWk1oFavJapdLALxwSjcSk2',
            amount: 200000,
            script_type: 'PAYTOADDRESS'
        }
    ],
    coin: "btc"
});
```

###### SPENDP2SHWITNESS 
```javascript
TrezorConnect.signTransaction({
    inputs: [
        {
            address_n: [49 | 0x80000000, 0 | 0x80000000, 2 | 0x80000000, 1, 0],
            prev_index: 0,
            prev_hash: 'b035d89d4543ce5713c553d69431698116a822c57c03ddacf3f04b763d1999ac'
            amount: 3382047,
            script_type: 'SPENDP2SHWITNESS'
        }
    ],
    outputs: [
        {
            address_n: [49 | 0x80000000, 0 | 0x80000000, 2 | 0x80000000, 1, 1],
            amount: 3181747,
            script_type: 'PAYTOP2SHWITNESS'
        }, {
            address: '18WL2iZKmpDYWk1oFavJapdLALxwSjcSk2',
            amount: 200000,
            script_type: 'PAYTOADDRESS'
        }
    ],
    coin: "btc"
});
```



### Result
###### [flowtype](../../src/js/types/response.js#sign-transaction)
```javascript
{
    success: true,
    payload: {
        signatures: Array<string>, // Array of signer signatures
        serializedTx: string,        // serialized transaction
        txid?: string,             // broadcasted transaction id
    }
}
```
Error
```javascript
{
    success: false,
    payload: {
        error: string // error message
    }
}
```

### Migration from older version

version 4 and below
```javascript
var inputs = [{
    address_n: [44 | 0x80000000, 0 | 0x80000000, 2 | 0x80000000, 1, 0],
    prev_index: 0,
    prev_hash: 'b035d89d4543ce5713c553d69431698116a822c57c03ddacf3f04b763d1999ac'
}];
var outputs = [{
    address_n: [44 | 0x80000000, 0 | 0x80000000, 2 | 0x80000000, 1, 1],
    amount: 3181747,
    script_type: 'PAYTOADDRESS'
}, {
    address: '18WL2iZKmpDYWk1oFavJapdLALxwSjcSk2',
    amount: 200000,
    script_type: 'PAYTOADDRESS'
}];
TrezorConnect.setCurrency('BTC');
TrezorConnect.signTx(
    inputs,
    outputs,
    "example message",
    function(result) {
        result.signatures    // not changed
        result.serialized_tx // renamed to "serializedTx"
        // added "txid" field if "push" is set to true
    }, 
    "bitcoin"
);
```
version 5
```javascript
// params are key-value pairs inside Object
TrezorConnect.signTransaction({ 
    inputs: [{
        address_n: [44 | 0x80000000, 0 | 0x80000000, 2 | 0x80000000, 1, 0],
        prev_index: 0,
        prev_hash: 'b035d89d4543ce5713c553d69431698116a822c57c03ddacf3f04b763d1999ac'
    }],
    outputs: [{
        address_n: [44 | 0x80000000, 0 | 0x80000000, 2 | 0x80000000, 1, 1],
        amount: 3181747,
        script_type: 'PAYTOADDRESS'
    }, {
        address: '18WL2iZKmpDYWk1oFavJapdLALxwSjcSk2',
        amount: 200000,
        script_type: 'PAYTOADDRESS'
    }],
    coin: "btc"
}).then(function(result) {
    ...
})
```