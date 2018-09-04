# Gods Unchained API

Public developer API documentation for Gods Unchained. 

This API is in public beta and is subject to change in future versions. 

This specification is for version 0. 

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

### Pagination

All requests which can return multiple objects can be shaped by the ```page``` and ```perPage``` parameters. 

```
BASE/proto?page=3&perPage=20
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

### Rate Limits

Currently, no rate limits are applied to API usage: this may change in future. 

### Types

General types:

| Type          | Description  |
| :-------------: |:-------------:|
| ![string](https://img.shields.io/badge/-string-lightgrey.svg) | A url encoded string. |
| ![number](https://img.shields.io/badge/-string-lightgrey.svg) | A decimal number. |

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
| ![Quality](https://img.shields.io/badge/-Quality-blue.svg) | common, shadow, gold, diamond |

### Duplicate Arguments

As is standard with REST APIs, we support queries of the following form:

```
https://api.godsunchained.com/card?god=nature&god=death
```

Duplicate argument keys will be interpreted disjunctively: this query will return cards where the god is either nature OR death. 

### Concepts

There are several 'types' of card in Gods Unchained:

- Token Card: A full ERC721 token card which has been activated.
- Shadow Card: The most common type of card: locked in, shown on frontend, but not yet converted to ERC721 token to save gas. 
- Centralized Card: Cards which cannot be traded on the blockchain (part of the core set or an untradeable promotion card). 

Some of these endpoints return a combination of the above, while some do not: this is documented by the individual endpoints.

## Core API

### GET /card/{id}

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| id      | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | ERC721 id of the card |

Returns the token card with id ```id``` and appropriate metadata. Currently conforms to both the generic and Apollo metadata specifications. 

### GET /card 

Returns a list of cards. By default, this returns all types of cards: token, shadow and centralized. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| user | ![address](https://img.shields.io/badge/-address-green.svg) | get cards owned by a specific address |
| rarity | ![Rarity](https://img.shields.io/badge/-Rarity-blue.svg) | get cards with a specific rarity |
| quality | ![Quality](https://img.shields.io/badge/-Quality-blue.svg) | get cards with a specific quality |
| god | ![God](https://img.shields.io/badge/-God-blue.svg) | get cards with a specific god |
| type | ![Type](https://img.shields.io/badge/-Type-blue.svg) | get cards with a specific type |
| tribe | ![Tribe](https://img.shields.io/badge/-Tribe-blue.svg) | get cards with a specific tribe |
| purity | ![range](https://img.shields.io/badge/-range-green.svg) | get cards with a particular purity |
| mana | ![range](https://img.shields.io/badge/-range-green.svg) | get cards with a specific mana |
| health | ![range](https://img.shields.io/badge/-range-green.svg) | get cards with a specific health |
| attack | ![range](https://img.shields.io/badge/-range-green.svg) | get cards with a specific attack |
| proto | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | get cards with a specific prototype id |

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
| id | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | id of the prototype card |

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

### GET /proto

Returns a list of prototype cards. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| god | ![God](https://img.shields.io/badge/-God-blue.svg)| get protos with a specific god |
| type | ![Type](https://img.shields.io/badge/-Type-blue.svg)| get protos with a specific type |
| tribe | ![Tribe](https://img.shields.io/badge/-Tribe-blue.svg)| get protos with a specific tribe |
| mana | ![range](https://img.shields.io/badge/-range-green.svg)| get protos with a specific mana |
| health | ![range](https://img.shields.io/badge/-range-green.svg)| get protos with a specific health |
| attack | ![range](https://img.shields.io/badge/-range-green.svg)| get protos with a specific attack |

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

### GET /factory/{address}/purchase/{id}/purchase/{index}

Returns the pack with index ```index``` from purchase ```id``` from the pack factory with address ```address```. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| address | ![address](https://img.shields.io/badge/-address-green.svg)| address of the pack factory |
| id | ![number](https://img.shields.io/badge/-number-lightgrey.svg)| id of the purchase |
| index | ![number](https://img.shields.io/badge/-number-lightgrey.svg)| index of the pack within the purchase |

**Response Format**

```
{
    "purchaseid":11665,
    "purchaseindex":0,
    "purchaseindices":[0,1,2,3,4],
    "user":"0x62ed0960478Cd1aAA29e9e94928107D7b1E2Cef8","factory":"0x0777F76D195795268388789343068e4fCd286919",
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

### GET /pack

Returns a list of packs.

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| type | ![PackType](https://img.shields.io/badge/-PackType-blue.svg) | get packs of a specific type |
| user | ![address](https://img.shields.io/badge/-address-green.svg) | get packs purchased by a specific user |
| factory | ![address](https://img.shields.io/badge/-address-green.svg) | get packs created by a specific factory |
| purchase | ![range](https://img.shields.io/badge/-range-green.svg) | get packs created in a specific purchase |

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
            "user":"0x62ed0960478Cd1aAA29e9e94928107D7b1E2Cef8","factory":"0x0777F76D195795268388789343068e4fCd286919",
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

### GET /factory/{address}/purchase/{id}

Returns purchase ```id``` from the pack factory at ```address```. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| address | ![address](https://img.shields.io/badge/-address-lightgrey.svg) | address of factory |
| id | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | id of purchase within factory |

**Response Format**

```
{
    "id":0,
    "user":"0x3882C6ba6475165aC5257Ddc1D8d7782E7805c28",
    "count":1,
    "remaining":0,
    "factory":"0x000983ba1A675327F0940b56c2d49CD9c042DFBF","txhash":"0xda2b2956588bd642bed4b0aa8f63c979f4893662dd31c237aa58b173bf4eb223",
    "type":"shiny"
}
```

### GET /purchase

Returns a list of purchases. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| type | ![PackType](https://img.shields.io/badge/-PackType-blue.svg)| get purchases from a specific pack type |
| user | ![address](https://img.shields.io/badge/-address-green.svg)| get purchases made by a specific user |
| factory | ![address](https://img.shields.io/badge/-address-green.svg)| get purchases made in a specific factory |
| remaining | ![range](https://img.shields.io/badge/-range-green.svg)| get number of packs remaining to be activated from this purchase |

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
            "factory":"0x000983ba1A675327F0940b56c2d49CD9c042DFBF","txhash":"0xda2b2956588bd642bed4b0aa8f63c979f4893662dd31c237aa58b173bf4eb223",
            "type":"shiny"
        }
    ]
}

```

### GET /referral

Returns a list of referrals.

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| type | ![PackType](https://img.shields.io/badge/-PackType-blue.svg) | get referrals with a specific rarity |
| referrer | ![address](https://img.shields.io/badge/-address-green.svg) | get referrals made by a specific user |
| purchaser | ![address](https://img.shields.io/badge/-address-green.svg) | get referrals made for a specific user |
| factory | ![address](https://img.shields.io/badge/-address-green.svg) | get referrals made in a particular factory |

**Response Format**

```
{
    "total": 1000,
    "page": 1,
    "perPage: 1,
    "records": [
        {
            "id":0,
            "referrer":"0xb08F95dbC639621DbAf48A472AE8Fce0f6f56a6e","purchaser":"0xE4a8dfcA175cDcA4Ae370f5b7aaff24bD1C9C8eF","factory":"0x1e891C587b345ab02A31b57c1F926fB08913d10D",
            "value":1746000000000000000,
            "count":0,
            "type":"shiny"
        }
    ]
}
```

### GET /image/{id}

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| format | ![string](https://img.shields.io/badge/-string-lightgrey.svg) |  the format in which the image should be presented |

Returns the full image of the card prototype with id ```id```.

Currently does not support returning the image inside the card front - this is planned for a future release, and is likely to become the default. 

## Helper APIs

To help build more effective applications for our ecosystem, we're also providing a couple of helpful API endpoints. These endpoints may be deprecated in future releases, as they are composable from existing endpoints, but provide a convenient interface during the development of nascent GU-focused applications. 

### GET /ranking

Returns an ordered list of users with the most cards which meet particular conditions. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| user | ![address](https://img.shields.io/badge/-address-green.svg)| get rank of cards owned by a specific address |
| rarity | ![Rarity](https://img.shields.io/badge/-Rarity-blue.svg)| get rank of cards with a specific rarity |
| quality | ![Quality](https://img.shields.io/badge/-Quality-blue.svg) | get rank of cards with a specific quality |
| purity | ![range](https://img.shields.io/badge/-range-green.svg) |  get rank of cards with a minimum purity bound |
| god | ![God](https://img.shields.io/badge/-God-blue.svg) | get rank of cards with a specific god |
| type | ![Type](https://img.shields.io/badge/-Type-blue.svg) | get rank of cards with a specific type |
| tribe | ![Tribe](https://img.shields.io/badge/-Tribe-blue.svg) | get rank of cards with a specific tribe |
| mana | ![range](https://img.shields.io/badge/-range-green.svg) | get rank of cards with a specific mana |
| health | ![range](https://img.shields.io/badge/-range-green.svg) | get rank of cards with a specific health |
| attack | ![range](https://img.shields.io/badge/-range-green.svg) | get rank of cards with a specific attack |
| proto | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | get rank of cards with a specific prototype id |

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

### GET /rarity

Returns rarity information about protos. 

**Parameters**

| Name        | Type          | Description  |
| :-------------: |:-------------:| :-----:|
| user | ![address](https://img.shields.io/badge/-address-green.svg) | get rarity info about cards owned by a specific address |
| rarity | ![rarity](https://img.shields.io/badge/-Rarity-blue.svg) | get rarity info about cards with a specific rarity |
| quality | ![quality](https://img.shields.io/badge/-Quality-blue.svg) | get rarity info about cards with a specific quality |
| god | ![god](https://img.shields.io/badge/-God-blue.svg) | get rarity info about cards with a specific god |
| type | ![type](https://img.shields.io/badge/-Type-blue.svg) | get rarity info about cards with a specific type |
| tribe | ![tribe](https://img.shields.io/badge/-Tribe-blue.svg) | get rarity info about cards with a specific tribe |
| purity | ![range](https://img.shields.io/badge/-range-green.svg) | get rarity info about cards within a purity range |
| mana | ![range](https://img.shields.io/badge/-range-green.svg) | get rarity info about cards within a mana range |
| health | ![range](https://img.shields.io/badge/-range-green.svg) | get rarity info about cards within a health range |
| attack | ![range](https://img.shields.io/badge/-range-green.svg) | get rarity info about cards with an attack range |
| proto | ![number](https://img.shields.io/badge/-number-lightgrey.svg) | get rarity info about cards with a specific prototype id |

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
