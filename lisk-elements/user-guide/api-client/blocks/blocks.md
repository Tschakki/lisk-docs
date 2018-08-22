Author: diego

----

Created: 2018-04-25

----

Updated: 2018-06-25

----

Metadescription: This Lisk Elements user guide is your resource for interacting with the `blocks` endpoint provided by the Lisk public API.

----

Metakeywords: lisk elements blocks

----

Title: Blocks Resource

----

Opengraphtitle: Lisk Elements API: Blocks Resource

----

Opengraphimage: 

----

Opengraphdescription: 

----

Content: 

# Lisk Elements API Client: Blocks Resource

This is a resource for interacting with the `blocks` endpoint provided by the Lisk public API. Each of the following methods can be accessed via the `blocks` property of an `APIClient` instance.

### `get`

Searches for a specified block in the system.

#### Syntax

```js
get([options])
```

#### Parameters

`options`: See options in the [Core API documentation](/documentation/lisk-core/user-guide/api/1-0).

#### Return value

`Promise`: Resolves to an API response object.

#### Examples

```js
client.blocks.get({ blockId: '17572751491778765213' })
    .then(res => {
        console.log(res.data);
    })
```

----

Htmltitle: Lisk Elements API Client - Blocks Resource  | Lisk Documentation