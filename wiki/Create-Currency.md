---
layout: page
title: Create Currency
wikiPageName: Create-Currency
menu: wiki
---

This document is a reference for the `CreateCurrency` transaction type. This transaction is used for listing a new token on the blockchain.

## Binary encoding structure 
The following table illustrates the binary encoding format of an `CreateCurrency` transaction. All fields are in Big Endian byte order.

| Field name               | Size            | Description                                                           |
| -------------------------|-----------------| ----------------------------------------------------------------------|
| Transaction type(8)      | 8bits           | The type of the transaction                                           |
| Fee length               | 8bits           | The length of the fee field                                           |
| Precision                | 8bits           | The number of decimals that the new currency can handle               |
| Coin supply              | 64bits          | The coin supply of the new currency                                   |
| Creator                  | 33bytes         | The creator of the new currency                                       |
| Receiver                 | 33bytes         | The address of the receiver of the issued stock                       |
| Currency hash            | 32bytes         | The hash of the newly created currency                                |
| Fee hash                 | 32bytes         | The hash of the currency that the fee is being paid in                |
| Hash                     | 32bytes         | The hash of the transaction                                           |
| Signature                | 65bytes         | The signature of the transaction                                      |
| Fee                      | Variable length | The transaction fee                                                   |
