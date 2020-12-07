# BEI API Usage

Link to the API documentation: https://bei.anchainai.com/api/doc

Every request needs an API key. Please visit [this page](https://www.anchain.ai/bei) if you need one.

## Response Body

#### status

- 200: api successfully retrieves information and returns
- 4xx: api finds no information or the usage is forbidden
- 5xx: api path or input parameters are invalid or internal error

#### data

- is_address_valid: input address is valid or not
- self
  - category: address category
  - detail: address details including entity name
- risk
  - level: risk level classification from 1 to 4 ascendingly
  - score: risk score in range 0-100
  - verdict_time: timestamp for querying risk info
- activity
  - suspicious_activity
    - aggr_type: type of activity aggregation
    - category: activity category
    - description: activity summary
    - entity: related entity
    - txn_count: count of suspicious transactions
    - txn_direct: direction of transaction
    - txn_hashes: list of transaction hashes
    - txn_vol: transaction volume
  - suspicious_activity_declare: declaration
  - verdict_time: timestamp for querying risk info

## APIs

Note: the API key for BEI demo server is ```demo_api_key```.

#### address_info

###### cURL

```bash
curl -XGET 'https://bei-demo.anchainai.com/api/address_info?proto=<PROTO>&address=<ADDR>&apikey=<APIKEY>'
```

###### Python

```python
import requests

url = 'https://bei-demo.anchainai.com/api/address_info'
payload = {
  'proto': '<PROTO>',
  'address': '<ADDR>',
  'apikey': '<APIKEY>'
}
res = requests.get(url=url, params=payload)
```

##### parameters

- proto(required): blockchain protocol, current available options: btc, eth
- address(required): blockchain address
- apikey(required): [get your free BEI API key today!](https://bei.anchainai.com/pricing)

##### sample response

```json
{
    "data": {
        "12t9YDPgwueZ9NyMgw519p7AA8isjr6SMw": {
            "is_address_valid": true,
            "self": {
                "category": [
                    "ransomware",
                    "abuse"
                ],
                "detail": [
                    "ransomware:WannaCry"
                ]
            }
        }
    },
    "err_msg": "",
    "status": 200
}
```

#### address_risk_score

###### cURL

```bash
curl -XGET 'https://bei-demo.anchainai.com/api/address_risk_score?proto=<PROTO>&address=<ADDR>&apikey=<APIKEY>'
```

###### Python

```python
import requests

url = 'https://bei-demo.anchainai.com/api/address_risk_score'
payload = {
  'proto': '<PROTO>',
  'address': '<ADDR>',
  'apikey': '<APIKEY>'
}
res = requests.get(url=url, params=payload)
```

##### parameters

- proto(required): blockchain protocol, current available options: btc, eth
- address(required): blockchain address
- apikey(required): [get your free BEI API key today!](https://bei.anchainai.com/pricing)

##### sample response

```json
{
    "data": {
        "12t9YDPgwueZ9NyMgw519p7AA8isjr6SMw": {
            "is_address_valid": true,
            "risk": {
                "level": 4,
                "score": 100,
                "verdict_time": 1605920588
            },
            "self": {
                "category": [
                    "abuse",
                    "ransomware"
                ],
                "detail": [
                    "ransomware:WannaCry"
                ]
            }
        }
    },
    "err_msg": "",
    "status": 200
}
```

#### address_risk_info

###### cURL

```bash
curl -XGET 'https://bei-demo.anchainai.com/api/address_risk_info?proto=<PROTO>&address=<ADDR>&apikey=<APIKEY>'
```

###### Python

```python
import requests

url = 'https://bei-demo.anchainai.com/api/address_risk_info'
payload = {
  'proto': '<PROTO>',
  'address': '<ADDR>',
  'apikey': '<APIKEY>'
}
res = requests.get(url=url, params=payload)
```

##### parameters

- proto(required): blockchain protocol, current available options: btc, eth
- address(required): blockchain address
- apikey(required): [get your free BEI API key today!](https://bei.anchainai.com/pricing)

##### sample response

```json
{
    "data": {
        "37pfy4LbsNhjz7myQoZpHWD4TVvSCnioBi": {
            "activity": {
                "suspicious_activity": [],
                "suspicious_activity_declare": "Suspicious Activity is summarized based on all historical transaction",
                "verdict_time": 1605920819
            },
            "is_address_valid": true,
            "risk": {
                "level": 2,
                "score": 50,
                "verdict_time": 1605920819
            },
            "self": {
                "category": [
                    "unaffiliated"
                ],
                "detail": []
            }
        }
    },
    "err_msg": "",
    "status": 200
}
```


