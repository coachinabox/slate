# Group **`/api/v2/`**

## **`GET`** current API status (`/api/v2/status`)

> Sample call:

```shell
curl -H "Accept: application/json" -i <TSv2_API_HOST>/api/v2/status
```

```javascript
$.ajax({
  url: "<TSv2_API_HOST>/api/v2/status",
  method: "get",
  success: function(result) {
    console.log( result.msg );
    console.log( result.version );
  },
  error: function(result) {
    console.log( 'API error!' );
  },
  dataType: "json"
});
```

> Response (200, 'application/json'):

```json
{
  "msg": "Ok: reachable and running",
  "version": "2.00"
}
```

Retrieves the API server endpoint status.
No parameters or authorization are required.


### HTTP Request

`GET <TSv2_API_HOST>/api/v2/status`


---


## **`POST`** new CardSort start (`/api/v2/card_sort/start`)

> Sample call:

```shell
curl -X POST -H 'Accept: application/json' \
     -i <TSv2_API_HOST>/api/v2/card_sort/start \
     --data 'source_id=1&deck=1&brand=1&source=sort1&pathway_id=1&token=STATIC_AUTH_TOKEN'
```

```javascript
$.ajax({
  url: "<TSv2_API_HOST>/api/v2/card_sort/start",
  method: "post",
  data: {
    source_id:  1,
    deck:       1,
    brand:      1,
    source:     'sort1',
    token:      STATIC_AUTH_TOKEN
  },
  success: function(result) {
    console.log( result.msg );
    console.log( result.jwt );
  },
  error: function(result) {
    console.log( 'API error!' );
  },
  dataType: "json"
});
```

> Response (200, 'application/json'):

```json
{
  "msg": "Successful",
  "jwt": JWT_TEXT_TOKEN
}
```

Creates a new CardSort given the parameters.

The `STATIC_AUTH_TOKEN` can be customized by editing the `config/secrets.yml` file or by setting a dedicated `ENV` variable upon each deploy


### HTTP Request

`POST <TSv2_API_HOST>/api/v2/card_sort/start`


### Body Parameters

| _Parameter_ | _Type_ | _Required?_ | _Description_ |
| :--- | :---: | :---: | :--- |
| `source_id` | Integer | yes | CiaB's Source/User ID |
| `deck` | Integer | yes |  CiaB's Deck ID; This will be used also to retrieve the deck details internally with a call to DoC API |
| `brand` | Integer | yes |  CiaB's Brand ID |
| `token` | String | yes | static auth token string (typically set by deploy config) |
| `source` | String | - |  CiaB's source or Card Sort name; when not given, a card sort name will be generated using the specified source, deck & brand IDs |
| `pathway_id` | Integer | - | CiaB's pathway ID |


---


## **`POST`** create or renew JWT session (`/api/v2/session`)

> Sample call:

```shell
curl -X POST -H 'Accept: application/json' \
     -i <TSv2_API_HOST>/api/v2/session \
     --data 'source_id=1&card_sort=1&token=STATIC_AUTH_TOKEN'
```

```javascript
$.ajax({
  url: "<TSv2_API_HOST>/api/v2/session",
  method: "post",
  data: {
    source_id:  1,
    card_sort:  1,
    token:      STATIC_AUTH_TOKEN
  },
  success: function(result) {
    console.log( result.msg );
    console.log( result.jwt );
  },
  error: function(result) {
    console.log( 'API error!' );
  },
  dataType: "json"
});
```

> Response (200, 'application/json'):

```json
{
  "msg": "Successful",
  "jwt": JWT_TEXT_TOKEN
}
```

Allows to re-create or renew a JWT given a `source_id` and a TSv2 `card_sort` ID, that can be extracted by decoding the JWT from a previous session or by choosing one from the reported list of existing card sorts.


### HTTP Request

`POST <TSv2_API_HOST>/api/v2/session`


### Body Parameters

| _Parameter_ | _Type_ | _Description_ |
| :--- | :---: | :--- |
| `source_id` | Integer | CiaB's Source/User ID |
| `card_sort` | Integer | TwelveShifts V2 internal Card Sort ID; can be extracted by decoding the JWT or by requesting a list of all the available card sorts |
| `token` | String | static auth token string (typically set by deploy config) |


---


## **`POST`** list CardSorts for a user (`/api/v2/card_sort/for_user`)

> Sample call:

```shell
curl -X POST -H 'Accept: application/json' \
     -i <TSv2_API_HOST>/api/v2/card_sort/for_user \
     --data 'source_id=1&source_type=CiaB&pathway_id=1&token=STATIC_AUTH_TOKEN'
```

```javascript
$.ajax({
  url: "<TSv2_API_HOST>/api/v2/card_sort/for_user",
  method: "post",
  data: {
    source_id:    1,
    source_type:  'CiaB',
    pathway_id:   1,
    token:        STATIC_AUTH_TOKEN
  },
  success: function(result) {
    console.log( result.msg );
    // Array of JSON Objects:
    console.log( result.card_sorts );
  },
  error: function(result) {
    console.log( 'API error!' );
  },
  dataType: "json"
});
```

> Response (200, 'application/json')

```json
        {
          "msg": "Successful. Card sorts found: 1.",
          "card_sorts": [
            {
              "id":                   1,
              "name":                 "card sort name",
              "deck_id":              1,
              "current_stage_id":     0,
              "phase_num":            0,
              "current_position":     0,
              "created_at":           "2018-11-18 11:30",
              "updated_at":           "2018-11-18 11:30",
              "source_id":            1,
              "brand_id":             1,
              "deck_name":            "deck.name",
              "brand_name":           "brand.name",
              "last_intro_shown":     0,
              "pile_changes_phase1":  0,
              "pile_changes_phase2":  0,
              "pile_changes_phase3":  0,
              "jwt":                  JWT_TEXT_TOKEN
            }
          ]
        }
```

Returns the list of all the existing card sorts for a specified user (`source_id`), with several data fields for each card sort, as well as a new dynamic JWT token for each one that can be used to connect to its specific card sort process.
It's also possible to specify an additional Pathway ID parameter (optional), in order to better filter out the required results.


### HTTP Request

`POST <TSv2_API_HOST>/api/v2/card_sort/for_user`


### Body Parameters

| _Parameter_ | _Type_ | _Required?_ | _Description_ |
| :--- | :---: | :---: | :--- |
| `source_id` | Integer | yes | CiaB's Source/User ID |
| `source_type` | String | yes |  CiaB's Source type |
| `token` | String | yes | static auth token string (typically set by deploy config) |
| `pathway_id` | Integer | - | CiaB's pathway ID |

### Response fields

| _Parameter_ | _Type_ | _Description_ |
| :--- | :---: | :---: | :--- |
| `id` | Integer | CardSort instance ID |
| `name` | String |  CardSort name |
| `deck_id` | Integer | TSv2 internal Deck ID (not CiaB's) |
| `current_stage_id` | Integer | fine-grained internal stages (~0..6) |
| `phase_num` | Integer | phase or "logic stage" number; 0 (not-yet-started) .. 3 (end phase) |
| `current_position` | Integer | currently browsed pile or position; 0 (unsorted) .. 3 (top pile)|
| `created_at` | Integer | creation timestamp |
| `updated_at` | Integer | latest update timestamp |
| `source_id` | Integer | CiaB user ID |
| `source` | String | CiaB source |
| `brand_id` | Integer | CiaB brand ID |
| `deck_name` | String | CiaB Deck name |
| `brand_name` | String | CiaB Brand name |
| `last_intro_shown` | Integer | last intro screen shown (0: none, 1: phase 1, 2: phase 2, >2: done) |
| `pile_changes_phase1` | Integer | tot. pile revisions made in phase #1 |
| `pile_changes_phase2` | Integer | tot. pile revisions made in phase #2 |
| `pile_changes_phase3` | Integer | tot. pile revisions made in phase #3 |
| `jwt` | String | fine-grained internal stages (~0..6) |


#### Possible values for `phase_num`

These are high-level states that reflect the main Business-Logic of the Application.
The term "phase" in the source code always refers to these high-level states. Seldom the term "step" is used as a synonym.

| `phase_num` | _Description_ |
| :---: | :--- |
| 0 | card sort not started yet |
| 1 | Business-Logic phase #1 (default name: "Strengths Sort") |
| 2 | Business-Logic phase #2 (default name: "Context Sort") |
| 3 | Business-Logic phase #3 (default name: "Results" or "Highlighting stage") |


#### Possible values for `current_stage_id`

These are more fine-grained internal logic states.
The term "stage" in the source code always refers to these low-level states.

| `current_stage_id` | _Internal constant_ | _Description_ |
| :---: | :--- | :--- |
| 0 | `STAGE_BEGIN` | Initial stage code for a freshly created CardSort process (not-yet started) |
| 1 | `STAGE_1` | Stage 1 ends when all the cards have an assigned kind "strength" (that is: they have been processed at least once) |
| 1 | `STAGE_1_END` | Review of Stage 1 ends when the user clicks on the "go to the next stage" button |
| 2 | `STAGE_2` | Stage 2 ends when all the cards have been processed at least once |
| 2 | `STAGE_2_END` | 'Review' of Stage 2 ends when the user clicks on the "go to the next stage" button |
| 3 | `STAGE_3` | Stage 3 ends when all the cards have been processed at least once |
| 3 | `STAGE_3_END` | final stage code for a completed CardSort process (Logic state currently _NOT USED_) |


#### Possible values for `current_position`

Each stage has 4 possible "piles"/destination positions for each card. The terms "pile" and "position" are used interchangeably throughout all the source code.

| `current_position` | _Internal constant_ | _Description_ |
| :---: | :--- | :--- |
| 0 | `POS_UNSORTED` | Unsorted Pile/Position (depending on phase: "Unsorted", "Unprioritized" or "Highlighted") |
| 1 | `POS_TOP` | Top Pile/Position (depending on phase: "Strengths", "Important" or "Shifts") |
| 2 | `POS_MIDDLE` | Middle Pile/Position (depending on phase: "Neither", "Less important" or "Strengths") |
| 3 | `POS_BOTTOM` | Bottom Pile/Position (depending on phase: "Dev. Area", "Unimportant" or "Others") |


---


## **`POST`** list CardSorts for a cohort of Pathways (`/api/v2/card_sort/for_cohort`)

> Sample call:

```shell
curl -X POST -H 'Accept: application/json' \
     -i <TSv2_API_HOST>/api/v2/card_sort/for_cohort \
     --data 'pathway_ids=1%2C2%2C5%2C7&token=STATIC_AUTH_TOKEN'
```

```javascript
$.ajax({
  url: "<TSv2_API_HOST>/api/v2/card_sort/for_user",
  method: "post",
  data: {
    pathway_ids:  '1,2,5,7',
    token:        STATIC_AUTH_TOKEN
  },
  success: function(result) {
    console.log( result.msg );
    // Array of JSON Objects:
    console.log( result.card_sorts );
  },
  error: function(result) {
    console.log( 'API error!' );
  },
  dataType: "json"
});
```

> Response (200, 'application/json')

```json
        {
          "msg": "Successful. Card sorts found: 4.",
          "card_sorts": "(...same array of JSON objects as before...)"
        }
```

Same as `/api/v2/card_sort/for_user` endpoint, but returns the list of all the existing card sorts for a cohort of Pathway IDs (`pathway_ids`), returning a JSON response identical in structure but simply filtered on a different data set.

In case no CardSort matching the specified Pathway IDs will be found, an `error` JSON field will be returned.


### HTTP Request

`POST <TSv2_API_HOST>/api/v2/card_sort/for_cohort`


### Body Parameters

| _Parameter_ | _Type_ | _Description_ |
| :--- | :---: | :--- |
| `pathway_ids` | String/list | A comma-separated list of CiaB's Pathway IDs (E.g.: "1,2,5,7") |
| `token` | String | static auth token string (typically set by deploy config) |

### Response fields

See the `/api/v2/card_sort/for_user` example.


---
