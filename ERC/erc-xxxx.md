---
eip: XXXX
title: Pay with USSD
description: Offline payment transactions via USSD.
author: sirawt (@MASDXI), Paramet Kongjaroen (@parametprame), et al.
discussions-to: https://ethereum-magicians.org/t/eip-xxxx/
status: Draft
type: Standards Track
category: ERC
created: yyyy-mm-dd
---

<!-- 
requires: 681
-->

<!-- what's about ERC-4337 and ERC-7702 -->

## Motivation
In scenarios with limited or unreliable internet access, traditional digital payment methods may not be feasible. Unstructured Supplementary Service Data (USSD) provides a vital solution by enabling offline transactions like payments, transfers, and balance inquiries without requiring internet connectivity. Its simplicity and broad compatibility with basic mobile devices and Electronic Draft Capture (EDC) device make USSD an essential tool for mobile payments, particularly in underserved areas, supporting financial inclusion and broader access to digital financial services.

Use cases for offline payments including
- Credit
- e-Money
- Insurance
- Loyalty program
- Retail Central Bank Digital Currency (rCBDC)

## Specification

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “NOT RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119 and RFC 8174.

### Custodian Wallet `sendTransaction`

With custodian wallet, allowing token movement even when the user lacks direct synchronization with the blockchain network.

``` json
// custodian wallet
"payload": {
    "network": "<STRING>",
    "sub_id": "<HEX_VALUE>",
    "recipient": "<STRING>",
    "value": "<HEX_VALUE>",
    "currency": "<STRING>",
    "pin": "<STRING>",
}
```

``` json
"response": {
    "balance": "<HEX_VALUE>",
    "currency": "<STRING>",
    "transaction_hash": "<HEX_STRING>",
}
```

When transferring native tokens, the `currency` key **MUST** be `0` or `null`.  
The callback **MUST** return the latest `balance` of the specified `currency` and a `transactionHash`
The `currency` **SHOULD** be represented by the token's `symbol`, which is mapped to a `number`, allowing the user to select the token they wish to send.
If the transaction is successful, these details are returned to the `sender`; otherwise, a `error` message is provided.

### Non-Custodian Wallet `sendTransaction`

<!-- TODO -->

``` json
// non-custodian wallet
"payload": {
    "network": "<STRING>",
    "sub_id": "<HEX_VALUE>",
    "signature": "<STRING>",
}
```

``` json
"response": {
    "balance": "<HEX_VALUE>",
    "currency": "<STRING>",
    "transaction_hash": "<HEX_STRING>",
}
```
Service Provider **MUST** decode the raw transaction and retrieve the token symbol from the smart contract.
The callback **MUST** return the latest `balance` of the specified `currency` and a `transactionHash`

### Others Method

- getBalance
  
``` json
"payload": {
    "network": "<STRING>",
    "sub_id": "<HEX_VALUE>",
    "currency": "<STRING>",
}
```

``` json
"response": {
    "balance": "<HEX_VALUE>",
    "currency": "<STRING>",
}
```

When get balance of native tokens, the `currency` key **MUST** be `0` or `null`.
The callback **MUST** return the latest `balance` of the specified `currency`.

> **MUST** support to pay with `address` **OPTIONAL** `ens`,`phone number`, `email`, `username` or `unique Id`  
not covering transaction to smart contract with USSD cause application **MAY** update the data frequently.  

- getTransactionByHash
  
``` json
"payload": {
    "network": "<STRING>",
    "sub_id": "<HEX_VALUE>",
    "transaction_hash": "<HEX_VALUE>"
}
```

``` json
"response": {
    // transaction information see eth_getTransactionByHash and eth_getTransactionReceipt
}
```

## Rationale

<!-- TODO -->

## Backwards Compatibility

No backward compatibility issues found.

## Reference Implementation

<!-- TODO -->
<!-- TODO example implementation source code   -->
<!-- some useful source see:    -->
<!-- https://github.com/krypt007/kotanipay-USSD.git -->
<!-- https://docs.oracle.com/communications/F83448_01/doc.1500/ccc_ussd_gw_tg.pdf   -->
<!-- https://help.webexconnect.io/docs/sending-and-receiving-sms-using-sandbox   -->
<!-- https://github.com/SedemQuame/fido-ussd-app   -->
<!-- Mapped phone number to public address see: https://github.com/camaraproject/BlockchainPublicAddress   -->
<!-- ERC-7798 see: https://hackmd.io/VyxIMlk1SvCOpBpS6a_2uA?both   -->

## Security Considerations

<!-- TODO  -->
<!-- USSD risk see: https://blog.aujas.com/mitigating-ussd-security-risks -->
<!-- potential quantum resistance solution see: https://www.itu.int/dms_pub/itu-t/opb/tut/T-TUT-PROTO-2021-PDF-E.pdf -->


## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).