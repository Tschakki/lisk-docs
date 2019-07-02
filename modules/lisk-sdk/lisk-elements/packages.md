# Lisk Elements Packages

This section details how to use Lisk Elements, once you have successfully installed it.

**Usage**
  - [Node.js](#nodejs)
  - [Browser](#browser)
  
**Packages**
  - [@liskhq/lisk-api-client](packages/api-client.md): An API client for the Lisk network.
  - [@liskhq/lisk-client](packages/client.md): A default set of Elements for use by clients of the Lisk network.
  - [@liskhq/lisk-constants](packages/constants.md): General constants for use with Lisk-related software.
  - [@liskhq/lisk-cryptography](packages/cryptography.md): General cryptographic functions for use with Lisk-related software.
  - [@liskhq/lisk-passphrase](packages/passphrase.md): Mnemonic passphrase helpers for use with Lisk-related software.
  - [@liskhq/lisk-transactions](packages/transactions.md): Everything related to transactions according to the Lisk protocol.

## Usage

### Node.js

Simply import (or require) the package and access its functionality according to the relevant namespace.

**Example for client package**
```js
import lisk from '@liskhq/lisk-client';
//or
const lisk = require('@liskhq/lisk-client');
const transaction = lisk.transaction.transfer({
    amount: '100000000',
    recipientId: '15434119221255134066L',
    passphrase: 'robust swift grocery peasant forget share enable convince deputy road keep cheap',
});
```

**Example for sub packages**
```js
import * as transactions from '@liskhq/lisk-transactions';

transactions.transfer({
    amount: '123000000',
    recipientId: '12668885769632475474L',
});
```

### Browser

Load the Lisk Elements script via our CDN. For example to load the minified version 1.1.0 of Lisk Elements include the following script, which will then expose the `lisk` variable:

```html
<script src="https://js.lisk.io/lisk-client-1.1.0.min.js"></script>
<script>
    const transaction = lisk.transaction.transfer({
        amount: '100000000',
        recipientId: '15434119221255134066L',
        passphrase: 'robust swift grocery peasant forget share enable convince deputy road keep cheap',
    });
</script>
```
