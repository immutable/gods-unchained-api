# Gods Unchained API

Public developer API documentation for Gods Unchained. 

This API is in beta and is subject to change in future versions. 

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
BASE/proto?page=3?perPage=20
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

Currently, no rate limits are applied to API usage: this is likely to change in future. 

### Types

Ethereum addresses are all stored as lower-case in our database. 

### Concepts

There are several 'types' of card:

Token Card: A full ERC721 token card which has been activated.
Shadow Card: The most common type of card: locked in, shown on frontend, but not yet converted to ERC721 token to save gas. 
Centralized Card: Cards which cannot be traded, only stored by Fuel Games. 

Some of these endpoints return a combination of the above, while some do not: this is marked by the individual endpoint.

## API

### GET /card/{id}

Returns the token card with id ```id``` and appropriate metadata. Currently conforms to both the generic and Apollo metadata specifications. 

### GET /card 

Returns a list of cards. By default, this returns both token and shadow cards, but not centralized cards. 

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

- mana ```number```: get cards with a specific mana
- health ```number```: get cards with a specific health
- attack ```number```: get cards with a specific attack
- god ```string```: get cards with a specific god
- type ```string```: get cards with a specific type
- tribe ```string```: get cards with a specific tribe

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

