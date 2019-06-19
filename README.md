# Gods Unchained API

Public developer API documentation for [Gods Unchained](https://godsunchained.com), a trading card game on the Ethereum blockchain. 

This version of the API (```v0```) is in a limited public beta: if you discover a bug, or the api returns results contrary to the specification, report it [here](https://discord.gg/UUn3h45). Error specifications will be added soon. 

## Projects 

Here are some third-party tools built using these APIs, make sure to ask in our [Discord server](https://discord.gg/2FDZrh2) if you're looking for help or you're wondering what to build. 

- [Gods Unchained Info](https://godsunchained.info)
- [GUDecks](https://gudecks.com)

## General

### Base URL

The base url for all requests is:

```
https://api.godsunchained.com
```

This URL must be suffixed with the version being requested (current version: ```v0```).

```
https://api.godsunchained.com/v0/
```

### Duplicate Arguments

We support queries of the following form:

```
https://api.godsunchained.com/v0/card?god=nature&god=death
```

Duplicate argument keys will be interpreted disjunctively: this query will return cards where the god is either nature OR death. 

### Pagination

All requests which can return multiple objects can be shaped by the ```page``` and ```perPage``` parameters. 

```
https://api.godsunchained.com/v0/proto?page=3&perPage=20
```

All paginated endpoints return data in the following format:

```
{
    total: number
    page: number
    perPage: number
    records: Array<any>
}
```

Where ```total``` is the number of records discovered by this query. 

### Sorting 

Sorts are applied to paginated endpoints using the ```sort``` and ```order``` query parameters:

```
https://api.godsunchained.com/v0/card?sort=mana&order=asc
```

Range and number types can be ordered by ```order=asc``` and ```order=desc```, defaulting to ```asc```.

Multiple sort parameters can be applied in one query, and will be applied in order:

```
https://api.godsunchained.com/v0/card?sort=mana&order=asc&sort=health&order=desc
```

For queries without exact pairings of sort and order parameters (where multiple parameters are applied), it is necessary to mark the order as ```null```:

```
https://api.godsunchained.com/v0/card?sort=mana&order=asc&sort=god&order=null&sort=health&order=desc
```


### Rate Limits

Currently, no rate limits are applied to API usage: this may change in future. 

### Types

General types:

| Type          | Description  |
| :-------------: |:-------------:|
| ![string](https://img.shields.io/badge/-string-lightgrey.svg) | A url encoded string. |
| ![number](https://img.shields.io/badge/-number-lightgrey.svg) | A decimal number. |
| ![boolean](https://img.shields.io/badge/-boolean-lightgrey.svg) | ```true``` or ```false``` |

Custom API types:

| Type          | Description  | 
| :-------------: |:-------------:|
| ![address](https://img.shields.io/badge/-address-green.svg) | A hexadecimal Ethereum address, case insensitive. |
| ![range](https://img.shields.io/badge/-range-green.svg) | A specific number ```1000```, a range ```1000-2000```, a minimum ```1000-``` or a maximum ```-2000```. |

The valid options for the enumeration types in various apis are set out below:

| Type          | Options |
| :-------------: |:-------------:|
| ![God](https://img.shields.io/badge/-God-blue.svg) | light, death, nature, war, magic, deception |
| ![PackType](https://img.shields.io/badge/-PackType-blue.svg) | rare, epic, legendary, shiny | 
| ![Rarity](https://img.shields.io/badge/-Rarity-blue.svg) | common, rare, epic, legendary, mythic |
| ![Type](https://img.shields.io/badge/-Type-blue.svg) | creature, spell, weapon | 
| ![Tribe](https://img.shields.io/badge/-Tribe-blue.svg) | nether, aether, atlantean, viking, olympian, anubian, amazon |
| ![Quality](https://img.shields.io/badge/-Quality-blue.svg) | plain, shadow, gold, diamond |
| ![Format](https://img.shields.io/badge/-Format-blue.svg) | full, card |


### Concepts

There are several 'types' of card in Gods Unchained:

- **Token**: A full ERC721 token card which has been 'activated'.
- **Model**: The most common type of card: locked in, shown on frontend, but not yet converted to an ERC721 token to save gas. 
- **Centralized**: Cards which cannot be traded on the blockchain (part of the core set or an untradeable promotion card). 

Some of these endpoints return a combination of the above, while some do not: this is documented by the individual endpoints. In general, the default is to return only cards which can become ERC721 tokens (i.e. Token and Model cards). 

**Prototype** cards, or **protos**, contain the underlying stats for a class of card. 


### Summary

| Method        |  Description  | Status |
| :-------------| :-----| :-------: |
| ```/card/{id}``` | Get card | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/card```    | List cards| ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/proto/{id}``` | Get a proto | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/proto```    | List protos | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/factory/{address}``` | Get factory | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/factory```  | Get a list of factories | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/factory/{address}/purchase/{id}``` | Get purchase | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/purchase``` | List factories | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/factory/{address}/purchase/{id}/pack/{index}``` | Get pack | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/pack```  | List packs | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/referral``` | Get a list of referrals | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/image/{id}``` | Get image | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/user/{address}``` | Get user | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/ranking``` | List users ranked by cards owned | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/rarity``` | Get rarity statistics | ![Live](https://img.shields.io/badge/-Live-green.svg) |
| ```/user/{address}/inventory``` | Get a user's inventory | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/deck``` | Encode a deck into a deck string | ![Live](https://img.shields.io/badge/-Live-green.svg) | 
| ```/deck/{string}``` | Decode a deck from a deck string  | ![Live](https://img.shields.io/badge/-Live-green.svg) |

## Core API

### GET /card/{id}

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| id      | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | ERC721 id of the card |

Returns the token card with id ```id``` and appropriate metadata. Currently conforms to both the generic and Apollo metadata specifications. 

### GET /card ![paginated](https://img.shields.io/badge/-Paginated-orange.svg)

Returns a list of token and model cards. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```user ``` | ![address](https://img.shields.io/badge/-address-green.svg) | get cards owned by a specific address |
| ```rarity``` | ![Rarity](https://img.shields.io/badge/-Rarity-blue.svg) | get cards with a specific rarity |
| ```quality``` | ![Quality](https://img.shields.io/badge/-Quality-blue.svg) | get cards with a specific quality |
| ```god``` | ![God](https://img.shields.io/badge/-God-blue.svg) | get cards with a specific god |
| ```type``` | ![Type](https://img.shields.io/badge/-Type-blue.svg) | get cards with a specific type |
| ```tribe``` | ![Tribe](https://img.shields.io/badge/-Tribe-blue.svg) | get cards with a specific tribe |
| ```purity``` | ![range](https://img.shields.io/badge/-range-green.svg) | get cards with a particular purity |
| ```mana``` | ![range](https://img.shields.io/badge/-range-green.svg) | get cards with a specific mana |
| ```health``` | ![range](https://img.shields.io/badge/-range-green.svg) | get cards with a specific health |
| ```attack``` | ![range](https://img.shields.io/badge/-range-green.svg) | get cards with a specific attack |
| ```proto``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | get cards with a specific prototype id |

**Response Format**

```
{
    "total": 1000,
    "page": 1,
    "perPage": 1,
    "records": [
        {
            "id": {
                "Int64": 0,
                "Valid": false,
            }
            "proto": 319,
            "purity": 59,
            "user": "0xCb3562Dd15807e2BCF35092B1e873971AF0a51da"
        }
    ]
}
```

### GET /proto/{id}

Returns the prototype card with id ```id```. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```id``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | id of the prototype card |

**Response Format**

```
{
    "id":300,
    "name":"Guerilla Sabotage",
    "effect":"Deal 4 damage to a random enemy creature. Draw a card.",
    "god":"Nature",
    "rarity":"Common",
    "tribe":{"String":"","Valid":false},
    "mana":4,
    "attack":{"Int64":0,"Valid":false},
    "health":{"Int64":0,"Valid":false},
    "type":"Spell"
}
```

### GET /proto ![paginated](https://img.shields.io/badge/-Paginated-orange.svg)

Returns a list of prototype cards. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```god``` | ![God](https://img.shields.io/badge/-God-blue.svg) | get protos with a specific god |
| ```rarity``` | ![Rarity](https://img.shields.io/badge/-Rarity-blue.svg) | get protos with a specific rarity |
| ```type``` | ![Type](https://img.shields.io/badge/-Type-blue.svg) | get protos with a specific type |
| ```tribe``` | ![Tribe](https://img.shields.io/badge/-Tribe-blue.svg) | get protos with a specific tribe |
| ```mana``` | ![range](https://img.shields.io/badge/-range-green.svg) | get protos with a specific mana |
| ```health``` | ![range](https://img.shields.io/badge/-range-green.svg) | get protos with a specific health |
| ```attack``` | ![range](https://img.shields.io/badge/-range-green.svg) | get protos with a specific attack |

**Response Format**

```
{
    "total": 380,
    "page": 1,
    "perPage: 1,
    "records": [
        {
            "id":300,
            "name":"Guerilla Sabotage",
            "effect":"Deal 4 damage to a random enemy creature. Draw a card.",
            "god":"Nature",
            "rarity":"Common",
            "tribe":{"String":"","Valid":false},
            "mana":4,
            "attack":{"Int64":0,"Valid":false},
            "health":{"Int64":0,"Valid":false},
            "type":"Spell"
        }
    ]
}
```

### GET /factory/{address}

Returns the pack factory at address ```address```. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```address``` | ![address](https://img.shields.io/badge/-address-lightgrey.svg) | address of factory |

**Response Format**

```
{
    "address":"0x0777f76d195795268388789343068e4fcd286919",
    "type":"rare"
}
```

### GET /factory ![paginated](https://img.shields.io/badge/-Paginated-orange.svg)

Returns a list of pack factories. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```type``` | ![PackType](https://img.shields.io/badge/-PackType-blue.svg) | type of pack |

**Response Format**

```
{
    "total": 4,
    "page": 1,
    "perPage: 1,
    "records": [
        {
            "address":"0x0777f76d195795268388789343068e4fcd286919",
            "type":"rare"
        }
    ]
}

```

### GET /factory/{address}/purchase/{id}

Returns purchase ```id``` from the pack factory at ```address```. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```address``` | ![address](https://img.shields.io/badge/-address-lightgrey.svg) | address of factory |
| ```id``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | id of purchase within factory |

**Response Format**

```
{
    "id":0,
    "user":"0x3882C6ba6475165aC5257Ddc1D8d7782E7805c28",
    "count":1,
    "remaining":0,
    "factory":"0x000983ba1A675327F0940b56c2d49CD9c042DFBF",
    "txhash":"0xda2b2956588bd642bed4b0aa8f63c979f4893662dd31c237aa58b173bf4eb223",
    "type":"shiny"
}
```

### GET /purchase ![paginated](https://img.shields.io/badge/-Paginated-orange.svg)

Returns a list of purchases. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```type``` | ![PackType](https://img.shields.io/badge/-PackType-blue.svg)| get purchases from a specific pack type |
| ```user``` | ![address](https://img.shields.io/badge/-address-green.svg)| get purchases made by a specific user |
| ```factory``` | ![address](https://img.shields.io/badge/-address-green.svg)| get purchases made in a specific factory |
| ```remaining``` | ![range](https://img.shields.io/badge/-range-green.svg)| number of packs remaining to be activated from this purchase |
| ```count``` | ![range](https://img.shields.io/badge/-range-green.svg)| number of packs purchased in this purchase |

**Response Format**

```
{
    "total": 1000,
    "page": 1,
    "perPage: 1,
    "records": [
        {
            "id":0,
            "user":"0x3882C6ba6475165aC5257Ddc1D8d7782E7805c28",
            "count":1,
            "remaining":0,
            "factory":"0x000983ba1A675327F0940b56c2d49CD9c042DFBF",
            "txhash":"0xda2b2956588bd642bed4b0aa8f63c979f4893662dd31c237aa58b173bf4eb223",
            "type":"shiny"
        }
    ]
}

```

### GET /factory/{address}/purchase/{id}/pack/{index}

Returns the pack with index ```index``` from purchase ```id``` from the pack factory with address ```address```. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```address``` | ![address](https://img.shields.io/badge/-address-green.svg)| address of the pack factory |
| ```id``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg)| id of the purchase |
| ```index``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg)| index of the pack within the purchase |

**Response Format**

```
{
    "purchaseid":11665,
    "purchaseindex":0,
    "purchaseindices":[0,1,2,3,4],
    "user":"0x62ed0960478Cd1aAA29e9e94928107D7b1E2Cef8",
    "factory":"0x0777F76D195795268388789343068e4fCd286919",
    "opened":true,
    "cards":[
        {"proto":264,"purity":600},
        {"proto":38,"purity":990},
        {"proto":299,"purity":549},
        {"proto":347,"purity":275},
        {"proto":291,"purity":850}
    ],
    "type":"rare"
}
```

### GET /pack ![paginated](https://img.shields.io/badge/-Paginated-orange.svg)

Returns a list of packs.

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```type``` | ![PackType](https://img.shields.io/badge/-PackType-blue.svg) | get packs of a specific type |
| ```user``` | ![address](https://img.shields.io/badge/-address-green.svg) | get packs purchased by a specific user |
| ```factory``` | ![address](https://img.shields.io/badge/-address-green.svg) | get packs created by a specific factory |
| ```purchase``` | ![range](https://img.shields.io/badge/-range-green.svg) | get packs created in a specific purchase |
| ```opened``` | ![boolean](https://img.shields.io/badge/-boolean-lightgrey.svg) | whether these packs have been opened |
| ```fill``` | ![boolean](https://img.shields.io/badge/-boolean-lightgrey.svg) | whether to fill these packs with their cards |

**Response Format**

```
{
    "total": 1000,
    "page": 1,
    "perPage: 1,
    "records": [
        {
            "purchaseid":11665,
            "purchaseindex":0,
            "purchaseindices":[0,1,2,3,4],
            "user":"0x62ed0960478Cd1aAA29e9e94928107D7b1E2Cef8",
            "factory":"0x0777F76D195795268388789343068e4fCd286919",
            "opened":true,
            "cards":[
                {"proto":264,"purity":600},
                {"proto":38,"purity":990},
                {"proto":299,"purity":549},
                {"proto":347,"purity":275},
                {"proto":291,"purity":850}
            ],
            "type":"rare"
        }
    ]
}
```


### GET /referral ![paginated](https://img.shields.io/badge/-Paginated-orange.svg)

Returns a list of referrals.

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```type``` | ![PackType](https://img.shields.io/badge/-PackType-blue.svg) | get referrals with a specific rarity |
| ```referrer``` | ![address](https://img.shields.io/badge/-address-green.svg) | get referrals made by a specific user |
| ```purchaser``` | ![address](https://img.shields.io/badge/-address-green.svg) | get referrals made for a specific user |
| ```factory``` | ![address](https://img.shields.io/badge/-address-green.svg) | get referrals made in a particular factory |

**Response Format**

```
{
    "total": 1000,
    "page": 1,
    "perPage: 1,
    "records": [
        {
            "id":0,
            "referrer":"0xb08F95dbC639621DbAf48A472AE8Fce0f6f56a6e",
            "purchaser":"0xE4a8dfcA175cDcA4Ae370f5b7aaff24bD1C9C8eF",
            "factory":"0x1e891C587b345ab02A31b57c1F926fB08913d10D",
            "value":1746000000000000000,
            "count":0,
            "type":"shiny"
        }
    ]
}
```

### GET /image/{id}

Returns an image based on the card prototype with id ```id```. To get an image in its card form, use the ```format``` and ```quality``` parameters. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```format``` | ![format](https://img.shields.io/badge/-Format-blue.svg) |  the format in which the image should be presented |
| ```h``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) |  the height to which the image will be resized |
| ```w``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) |  the width to which the image will be resized |
| ```quality``` | ![Quality](https://img.shields.io/badge/-Quality-blue.svg) |  the quality of the card |


### GET /user/{address}

Get a user. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```address``` | ![string](https://img.shields.io/badge/-address-green.svg) | the Ethereum address of the user |

**Response Format**

```
{
    "username": "ender",
    "address": "0xC257274276a4E539741Ca11b590B9447B26A8051",
    "nonce": 0
}
```

## Helper APIs

To help build more effective applications for our ecosystem, we're also providing a couple of helpful API endpoints. These endpoints may be deprecated in future releases, as they are composable from existing endpoints, but provide a convenient interface during the development of nascent GU-focused applications. 

### GET /ranking ![paginated](https://img.shields.io/badge/-Paginated-orange.svg)

Returns an ordered list of users with the most cards which meet particular conditions. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```rarity``` | ![Rarity](https://img.shields.io/badge/-Rarity-blue.svg)| get rank of cards with a specific rarity |
| ```quality``` | ![Quality](https://img.shields.io/badge/-Quality-blue.svg) | get rank of cards with a specific quality |
| ```god``` | ![God](https://img.shields.io/badge/-God-blue.svg) | get rank of cards with a specific god |
| ```type``` | ![Type](https://img.shields.io/badge/-Type-blue.svg) | get rank of cards with a specific type |
| ```tribe``` | ![Tribe](https://img.shields.io/badge/-Tribe-blue.svg) | get rank of cards with a specific tribe |
| ```purity``` | ![range](https://img.shields.io/badge/-range-green.svg) |  get rank of cards with a minimum purity bound |
| ```mana``` | ![range](https://img.shields.io/badge/-range-green.svg) | get rank of cards with a specific mana |
| ```health``` | ![range](https://img.shields.io/badge/-range-green.svg) | get rank of cards with a specific health |
| ```attack``` | ![range](https://img.shields.io/badge/-range-green.svg) | get rank of cards with a specific attack |
| ```proto``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | get rank of cards with a specific prototype id |

**Response Format**

```
{
    "total": 10000,
    "page": 1,
    "perPage": 1,
    "records": [
        {
            "user": "0xa012623C2d4EB0cfe921Bd283bb1823370Ae2737",
            "count": 1585
        }
    ]
}
```

### GET /rarity ![paginated](https://img.shields.io/badge/-Paginated-orange.svg)

Returns rarity information about protos. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```user``` | ![address](https://img.shields.io/badge/-address-green.svg) | get rarity info about cards owned by a specific address |
| ```rarity``` | ![rarity](https://img.shields.io/badge/-Rarity-blue.svg) | get rarity info about cards with a specific rarity |
| ```quality``` | ![quality](https://img.shields.io/badge/-Quality-blue.svg) | get rarity info about cards with a specific quality |
| ```god``` | ![god](https://img.shields.io/badge/-God-blue.svg) | get rarity info about cards with a specific god |
| ```type``` | ![type](https://img.shields.io/badge/-Type-blue.svg) | get rarity info about cards with a specific type |
| ```tribe``` | ![tribe](https://img.shields.io/badge/-Tribe-blue.svg) | get rarity info about cards with a specific tribe |
| ```purity``` | ![range](https://img.shields.io/badge/-range-green.svg) | get rarity info about cards within a purity range |
| ```mana``` | ![range](https://img.shields.io/badge/-range-green.svg) | get rarity info about cards within a mana range |
| ```health``` | ![range](https://img.shields.io/badge/-range-green.svg) | get rarity info about cards within a health range |
| ```attack``` | ![range](https://img.shields.io/badge/-range-green.svg) | get rarity info about cards with an attack range |
| ```proto``` | `![number](https://img.shields.io/badge/-number-lightgrey.svg) | get rarity info about cards with a specific prototype id |

**Sort Options**

```proto```, ```plain```, ```shadow```, ```gold```, ```diamond```

**Response Format**

```
{
    "total": 380,
    "page": 1,
    "perPage": 1,
    "records": [
        {
            "proto": 1,
            "plain": 1325,
            "shadow": 72,
            "gold": 20,
            "diamond": 3
        }
    ]
}
```

### GET /user/{address}/inventory ![paginated](https://img.shields.io/badge/-Paginated-orange.svg)

Returns the inventory of the user with address ```address```, including token, shadow and centralized cards. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```rarity``` | ![rarity](https://img.shields.io/badge/-Rarity-blue.svg) | get cards with a specific rarity |
| ```quality``` | ![quality](https://img.shields.io/badge/-Quality-blue.svg) | get cards with a specific quality |
| ```god``` | ![god](https://img.shields.io/badge/-God-blue.svg) | get cards with a specific god |
| ```type``` | ![type](https://img.shields.io/badge/-Type-blue.svg) | get cards with a specific type |
| ```tribe``` | ![tribe](https://img.shields.io/badge/-Tribe-blue.svg) | get cards with a specific tribe |
| ```purity``` | ![range](https://img.shields.io/badge/-range-green.svg) | get cards within a purity range |
| ```mana``` | ![range](https://img.shields.io/badge/-range-green.svg) | get cards within a mana range |
| ```health``` | ![range](https://img.shields.io/badge/-range-green.svg) | get cards within a health range |
| ```attack``` | ![range](https://img.shields.io/badge/-range-green.svg) | get cards with an attack range |
| ```proto``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | get cards with a specific prototype id |

**Response Format**

```
{
    "total": 380,
    "page": 1,
    "perPage": 1,
    "records": [
        {
            "proto": 1,
            "purities": [
                "100", "200", "300", "2999"
            ]
        }
    ]
}
```

## DeckString APIs

[DeckStrings](https://github.com/fuelgames/deckstring) are a convenient standard for allowing applications to import and export decks. The following APIs provide a convenient interface for basic deck string operations. 

### POST /deck

Encodes a deck into a deck string. 

**Request Body** 

```
{
    "version": 1,
    "god": "deception",
    "protos": [
        290, 17, 201, 201, 80, 80, 93, 93, 64, 64, 185, 185, 55, 55, 97, 331, 281, 281, 252, 252, 330,
		330, 280, 202, 202, 265, 265, 37, 94, 94
    ]
}
```

**Response Format**

```
AQYBBhElYZgCogLLAgIMN0BQXV65AckBygH8AYkCmQLKAg==
```


### GET /deck/{string}

Decodes a deck from a deck string.  

**Parameters**


**Response Format** 

```
{
    "version": 1,
    "god": "deception",
    "protos": [
        290, 17, 201, 201, 80, 80, 93, 93, 64, 64, 185, 185, 55, 55, 97, 331, 281, 281, 252, 252, 330,
		330, 280, 202, 202, 265, 265, 37, 94, 94
    ]
}
```

### GET /mode 

Lists the game modes with some properties.  

**Parameters**

```
[
 {
  "id": 2,
  "name": "Constructed",
  "description": "Classic Contructed",
  "live": true,
  "required_level": 0,
  "properties": {
   "type": 4,
   "image_url": "https://images.godsunchained.com/misc/classic_constructed.webp"
  }
 }
]
```

### GET /match ![paginated](https://img.shields.io/badge/-Paginated-orange.svg)

Show the match results

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```start_time``` | ![range](https://img.shields.io/badge/-range-green.svg) | start time of the match (UNIX epoch format) |
| ```end_time``` | ![range](https://img.shields.io/badge/-range-green.svg) | end time of the match (UNIX epoch format) |


**Response Format**

```
{
 "total": 1447,
 "page": 1,
 "perPage": 20,
 "records": [
  {
   "player_won": 9127,
   "player_lost": 6008,
   "game_mode": 2,
   "game_id": "b64865e2-682b-4a23-af11-20aad0cfd47c",
   "start_time": 1560734177,
   "end_time": 1560734355,
   "player_info": [{"god":"nature","cards":[301,121,68,237,976,1000,973,523,910,385,494,467,905,519,907,507,919,916,906,442,386,537,471,928,475,906,454,909,945,920],"global":false,"health":30,"status":"connected","user_id":9127},{"god":"Magic","cards":[401,401,404,404,908,908,455,455,535,535,467,467,926,926,981,981,402,402,504,504,396,396,406,406,983,983,407,407,1002,1002],"global":true,"health":0,"status":"connected","user_id":6008}],
   "total_turns": 6
  }
 ]
}
```

### GET /rank ![paginated](https://img.shields.io/badge/-Paginated-orange.svg)

Show the rank of a player per game mode.

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```user_id``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | Apollo ID of the user |
| ```game_mode``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | Game mode of the rank |


**Response Format**

```
{
 "total": 543,
 "page": 1,
 "perPage": 20,
 "records": [
  {
   "user_id": 9115,
   "game_mode": 1,
   "rating": 952,
   "rank_level": 1,
   "win_points": 82.849302,
   "loss_points": 86.586029
  },
  {
   "user_id": 2317,
   "game_mode": 2,
   "rating": 875.627936,
   "rank_level": 1,
   "win_points": 48.249483,
   "loss_points": 89.55682
  }
 ]
}
```


### GET /properties ![paginated](https://img.shields.io/badge/-Paginated-orange.svg)

Show the properties of a player.

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```user_id``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | Apollo ID of the user |

**Response Format**

```
{
 "total": 8298,
 "page": 1,
 "perPage": 20,
 "records": [
  {
   "user_id": 612,
   "xp_level": 0,
   "total_xp": 0,
   "xp_to_next": 25,
   "won_matches": 0,
   "lost_matches": 0
  },
  {
   "user_id": 706,
   "xp_level": 36,
   "total_xp": 25850,
   "xp_to_next": 350,
   "won_matches": 51,
   "lost_matches": 40
  }
 ]
}
```


### GET /predict

Calculates the probability of a match based on the rating of the players (using Elo rating algorithm)

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| ```user_id``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | Apollo ID of the user |
| ```opponent_id``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | Apollo ID of the opponent |
| ```game_mode``` | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | the game mode of the match |

**Response Format**

```
0.6717130465747431
```