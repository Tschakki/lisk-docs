# Lisk Elements API Client: Accounts Resource

This is a resource for interacting with the `accounts` endpoint provided by the Lisk public API. Each of the following methods can be accessed via the `accounts` property of an `APIClient` instance.

### `get`

Searches for matching accounts in the system.

#### Syntax

```js
get([options])
```

#### Parameters

`options`: See options in the [Core API documentation](/lisk-core/user-guide/api/1-0/1-0.json).

#### Return value

`Promise`: Resolves to an API response object.

#### Examples

```js
client.accounts.get({ username: 'oliver' })
    .then(res => {
        console.log(res.data);
    })
```

### `getMultisignatureGroups`

Searches for the specified account in the system and responds with a list of the multisignature groups that this account is a member of.

#### Syntax

```js
getMultisignatureGroups(address, [options])
```

#### Parameters

`address`: A Lisk address string.

`options`: See options in the [Core API documentation](/lisk-core/user-guide/api/1-0/1-0.json).

#### Return value

`Promise`: Resolves to an API response object.

#### Examples

```js
client.accounts.getMultisignatureGroups('15434119221255134066L')
    .then(res => {
        console.log(res.data);
    })
```

### `getMultisignatureMemberships`

Searches for the specified multisignature group and responds with a list of all members of this particular multisignature group.

#### Syntax

```js
getMultisignatureMemberships(address, [options])
```

#### Parameters

`address`: A Lisk address string.

`options`: See options in the [Core API documentation](/lisk-core/user-guide/api/1-0/1-0.json).

#### Return value

`Promise`: Resolves to an API response object.

#### Examples

```js
client.accounts.getMultisignatureMemberships('15434119221255134066L')
    .then(res => {
        console.log(res.data);
    })
```
