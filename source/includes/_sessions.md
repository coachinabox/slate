# Controller **`/sessions`**

## **`POST`** new HTTP Session creation (`/sessions/create`)

> Sample call:

```shell
curl --include --request POST --data="{'t': JWT_TEXT_TOKEN}" \
     "<TSv2_API_HOST>/sessions/create"

# Alternatively:
curl --include --request POST  \
     -H "{'Authorization': 'Bearer JWT_TEXT_TOKEN'}" \
     "<TSv2_API_HOST>/sessions/create"
```

> Response (301, 'text/html')
> Location will be redirected to the actual endpoint upon success.


Starts (or continues) a new HTTP session authorized by a dynamic JWT associated to a specific CardSort.

The `sessions` controller will redirect to the correct endpoint if the JWT is accepted, or it will redirect to an error page otherwise.


### HTTP Request

`POST <TSv2_API_HOST>/sessions/create`


### Body Parameters

| _Parameter_ | _Type_ | _Required?_ | _Description_ |
| :--- | :---: | :---: | :--- |
| `t` | String | yes | JWT text token; its payload must contain the field `card_sort` pointing to an existing TSv2 CardSort ID |

The JWT text token can also be specified as a "Authorization" `Bearer`-type of header in the request (instead of using it as the `t` request payload).


---

# Controller **`/card_sort`**

## **`GET`** connect to an existing Card Sort (`/card_sort/continue`)

> Sample call:

```shell
curl --include --data="{'t': JWT_TEXT_TOKEN}" \
     "<TSv2_API_HOST>/card_sort/continue"

# Alternatively:
curl --include -H "{'Authorization': 'Bearer JWT_TEXT_TOKEN'}" \
     "<TSv2_API_HOST>/card_sort/continue"
```

> Response (301, 'text/html')
> Location will be redirected to the actual endpoint upon success.


Continues (connects to) an existing card sort process, given its associated JWT.

The `card_sort` controller will redirect to the correct endpoint if the JWT is accepted, or it will redirect to an error page otherwise.


### HTTP Request

`GET <TSv2_API_HOST>/card_sort/continue`


### Body Parameters

| _Parameter_ | _Type_ | _Required?_ | _Description_ |
| :--- | :---: | :---: | :--- |
| `t` | String | yes | JWT text token; its payload must contain the field `card_sort` pointing to an existing TSv2 CardSort ID |

<aside class="notice">
The JWT text token can also be specified as an "Authorization" <code>Bearer</code>-type of header in the request (instead of using it as the <code>t</code> request payload).
</aside>



## **`GET`** display completed Card Sort results (`/card_sort/show`)

> Sample call:

```shell
curl --include --data="{'t': JWT_TEXT_TOKEN}" \
     "<TSv2_API_HOST>/card_sort/show"

# Alternatively:
curl --include -H "{'Authorization': 'Bearer JWT_TEXT_TOKEN'}" \
     "<TSv2_API_HOST>/card_sort/show"
```

> Response (301, 'text/html')
> Location will be redirected to the actual endpoint upon success.


Displays the end result report of a completed Card Sort, given its associated JWT.
The Card Sort must be actually completed in order to have data to be displayed.

The `card_sort` controller will redirect to the correct endpoint if the JWT is accepted, or it will redirect to an error page otherwise.


### HTTP Request

`GET <TSv2_API_HOST>/card_sort/show`


### Body Parameters

| _Parameter_ | _Type_ | _Required?_ | _Description_ |
| :--- | :---: | :---: | :--- |
| `t` | String | yes | JWT text token; its payload must contain the field `card_sort` pointing to an existing TSv2 CardSort ID |

<aside class="notice">
The JWT text token can also be specified as an "Authorization" <code>Bearer</code>-type of header in the request (instead of using it as the <code>t</code> request payload).
</aside>
