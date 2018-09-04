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

Ethereum addresses are all stored as lower-case in our database: all parameters passed to this API will be converted to lower-case.  

The valid options for the enumeration types in various apis are set out below:

- God: light, death, nature, war, 
- PackType: rare, epic, legendary, shiny
- Rarity: common, rare, epic, legendary, mythic
- Type: creature, spell, weapon
- Tribe: nether, aether, atlantean, viking, olympian, anubian, amazon
- Quality: common, shadow, gold, diamond

### Ranges

Parameters marked as rangeable take in 

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

Returns the token card with id ```id``` and appropriate metadata. Currently conforms to both the generic and Apollo metadata specifications. 

### GET /card 

Returns a list of cards. By default, this returns all types of cards: token, shadow and centralized. 

Parameters

- user ```string```: get cards owned by a specific address
- rarity ```string```: get cards with a specific rarity
- quality ```string```: get cards with a specific quality
- purity ```number```: get cards with a particular purity
- mana ```number```: get cards with a specific mana
- health ```number```: get cards with a specific health
- attack ```number```: get cards with a specific attack
- god ```string```: get cards with a specific god
- type ```string```: get cards with a specific type
- tribe ```string```: get cards with a specific tribe
- proto ```number```: get cards with a specific prototype id

Response Format

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

Response Format

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

Parameters

- mana ```number```: get protos with a specific mana
- health ```number```: get protos with a specific health
- attack ```number```: get protos with a specific attack
- god ```string```: get protos with a specific god
- type ```string```: get protos with a specific type
- tribe ```string```: get protos with a specific tribe

Response Format

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

### GET /pack

Returns a list of packs.

Parameters

- rarity ```string```: get packs with a specific rarity
- user ```string```: get packs purchased by a specific user
- factory ```string```: get packs created by a specific factory
- purchase ```number```: get packs created in a specific purchase

Response Format

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

### GET /purchase

Returns a list of purchases. 

Parameters

- rarity ```string```: get purchases with a specific rarity
- user ```string```: get purchases made by a specific user
- factory ```string```: get purchases made in a specific factory
- remaining ```number```: get number of packs remaining to be activated from this purchase

Response Format

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

Parameters

- rarity ```string```: get referrals with a specific rarity
- referrer ```string```: get referrals made by a specific user
- purchaser ```string```: get referrals made for a specific user
- factory ```string```: get referrals made in a particular factory


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

Parameters

- format ```string```: the format in which the image should be presented 

Returns the full image of the card prototype with id ```id```.

Currently does not support returning the image inside the card front - this is planned for a future release, and is likely to become the default. 

## Helper APIs

To help build more effective applications for our ecosystem, we're also providing a couple of helpful API endpoints. These endpoints may be deprecated in future releases, as they are composable from existing endpoints, but provide a convenient interface during the development of nascent GU-focused applications. 

### GET /ranking

Returns an ordered list of users with the most cards which meet particular conditions. 

Parameters

- user ```string```: get cards owned by a specific address
- rarity ```string```: get cards with a specific rarity
- shine ```string```: get cards with a specific shine
- purity ```number```: get cards with a minimum purity bound
- mana ```number```: get cards with a specific mana
- health ```number```: get cards with a specific health
- attack ```number```: get cards with a specific attack
- god ```string```: get cards with a specific god
- type ```string```: get cards with a specific type
- tribe ```string```: get cards with a specific tribe
- proto ```number```: get cards with a specific prototype id

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

Parameters

- user ```string```: get rarity info about cards owned by a specific address
- rarity ```string```: get rarity info about cards with a specific rarity
- quality ```string```: get rarity info about cards with a specific quality
- purity ```number```: get rarity info about cards with a minimum purity bound
- mana ```number```: get rarity info about cards with a specific mana
- health ```number```: get rarity info about cards with a specific health
- attack ```number```: get rarity info about cards with a specific attack
- god ```string```: get rarity info about cards with a specific god
- type ```string```: get rarity info about cards with a specific type
- tribe ```string```: get rarity info about cards with a specific tribe
- proto ```number```: get rarity info about cards with a specific prototype id

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
