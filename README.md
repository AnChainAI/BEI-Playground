# BEI API Usage

Link to the API documentation: https://bei.anchainai.com/docs

Every request needs an API key. Please visit [this page](https://www.anchain.ai/bei) if you need one.

Get started with our [demo domain](https://demo.anchainai.com/) to experience our BEI API with API key: ```demo_api_key```.

## Use Cases

#### Block the transaction if the receiver is high-risk

```python
import requests

def check_receiver_risk_score(proto, receiver_addr): # get address risk score from BEI API demo domain
  res = requests.get(f'https://demo.anchainai.com/api/address_risk_score?proto={proto}&apikey=demo_api_key&address={receiver_addr}')
  risk_score = res.json()['data'][receiver_addr]['risk']['score'] if res.status_code == 200 else -1
  return True if 0 <= risk_score <= 50 else False # if the input address is valid and safe

is_safe = check_receiver_risk_score('btc', '12t9YDPgwueZ9NyMgw519p7AA8isjr6SMw') # boolean indicating if address is safe
```

#### Alert the receiver if the sender is high-risk

```python
import requests

def check_sender_risk_score(proto, sender_addr): # get address risk score from BEI API demo domain
  res = requests.get(f'https://demo.anchainai.com/api/address_risk_score?proto={proto}&apikey=demo_api_key&address={sender_addr}')
  risk_score = res.json()['data'][sender_addr]['risk']['score'] if res.status_code == 200 else -1
  return True if 0 <= risk_score <= 50 else False # if the input address is valid and safe

is_safe = check_sender_risk_score('btc', '12t9YDPgwueZ9NyMgw519p7AA8isjr6SMw') # boolean indicating if address is safe
```

## Demo Addresses

| Protocol | Address                                    |
| -------- | ------------------------------------------ |
| BTC      | 12QtD5BFwRsdNsAZY76UVE1xyCGNTojH9h         |
| BTC      | 1H2zrVQxU3ymunr9CunjoActooLW2ryQK7         |
| BTC      | 12t9YDPgwueZ9NyMgw519p7AA8isjr6SMw         |
| BTC      | 1Lud76Q98VRHCUiyK7XUs7AgFofrqXeP78         |
| BTC      | 1NDyJtNTjmwk5xPNhjgAMu4HDHigtobu1s         |
| BTC      | 185TLkMZYwgcGjGMcWpYukdVd1b6HRs7kA         |
| ETH      | 0xd882cfc20f52f2599d84b8e8d58c7fb62cfe344b |
| ETH      | 0x6e1db9836521977ee93651027768f7e0d5722a33 |
| ETH      | 0xf4a2eff88a408ff4c4550148151c33c93442619e |
| ETH      | 0xab5c66752a9e8167967685f1450532fb96d5d24f |
| ETH      | 0x05f51aab068caa6ab7eeb672f88c180f67f17ec7 |

This table contains 6 BTC and 5 ETH addresses available on demo domain.

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

#### address_label

###### cURL

```bash
curl -XGET 'https://demo.anchainai.com/api/address_label?proto=<PROTO>&address=<ADDR>&apikey=<APIKEY>'
```

###### Python

```python
import requests

url = 'https://demo.anchainai.com/api/address_label'
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
curl -XGET 'https://demo.anchainai.com/api/address_risk_score?proto=<PROTO>&address=<ADDR>&apikey=<APIKEY>'
```

###### Python

```python
import requests

url = 'https://demo.anchainai.com/api/address_risk_score'
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

#### address_risk_activity

###### cURL

```bash
curl -XGET 'https://demo.anchainai.com/api/address_risk_activity?proto=<PROTO>&address=<ADDR>&apikey=<APIKEY>'
```

###### Python

```python
import requests

url = 'https://demo.anchainai.com/api/address_risk_activity'
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


