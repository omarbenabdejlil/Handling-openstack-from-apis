# Handling-openstack-from-apis
## Step 1 : 
> Get Token via keystone api :
`get_token.py`
```yml
import requests
import json

payload = {
    "auth": {
        "identity": {
            "methods": [
                "password"
            ],
            "password": {
                "user": {
                    "name": "admin",
                    "domain": {
                        "name": "Default"
                    },
                    "password": "ADMIN_PASS"}
            }
        },
        "scope": {
            "project": {
                "domain": {
                    "id": "default"
                },
                "name": "admin"
            }
        }
    }
}

res = requests.post('http://192.168.56.101:5000/v3/auth/tokens',
                    headers = {'content-type':'application/json'},
                    data=json.dumps(payload))

print(res.headers)
```
> here provide username (eg.admin) and password (eg.ADMIN_PASS) & "192.168.56.101 = controller IP"
> Output exemple : 
```json
{‘Date’: ‘Wed, 31 Jul 2019 05:20:31 GMT’, ‘Server’: ‘Apache/2.4.29 (Ubuntu)’, ‘Content-Length’: ‘4723’, ‘X-Subject-Token’: ‘gAAAAABdQS3WNgoLUEKmiA8w83_VpFPx-o-C-5mSpRHfEsbjIRjahPRBkA6TDmOGbK7jUfFnajbQ4sBya2RhRYpBZRtyDRLcrF2qhlYgJ3rO7zw1QuE2Dq7lfjXFXszBfNhyR-c10Bv-3tybSzD-GiBG4DcoHKUCRwL6wJpnA4o8nILb763Td48’, ‘Vary’: ‘X-Auth-Token’, ‘x-openstack-request-id’: ‘req-8a4837ff-a157–4b12-b431–94da754c3579’, ‘Keep-Alive’: ‘timeout=5, max=100’, ‘Connection’: ‘Keep-Alive’, ‘Content-Type’: ‘application/json’}

Step 2:Get values of imageRef ,flavorRef ,uuid(your network id)

copy value of “X-Subject-Token”and paste it in “X-Auth-Token” key as follows:
```
`getImageRef.py`
```yml
import requests
import json


res = requests.get('http://192.168.56.101:9292/v2/images',
                    headers={'content-type': 'application/json',
                             'X-Auth-Token': 'gAAAAABdQS3WNgoLUEKmiA8w83_VpFPx-o-C-5mSpRHfEsbjIRjahPRBkA6TDmOGbK7jUfFnajbQ4sBya2RhRYpBZRtyDRLcrF2qhlYgJ3rO7zw1QuE2Dq7lfjXFXszBfNhyR-c10Bv-3tybSzD-GiBG4DcoHKUCRwL6wJpnA4o8nILb763Td48'
                             },
                   )

print(res.text)
```
> Output exemple : 
```json
{“images”: [{“name”: “cirros”, “disk_format”: “qcow2”, “container_format”: “bare”, “visibility”: “public”, “size”: 12716032, “virtual_size”: null, “status”: “active”, “checksum”: “443b7623e27ecf03dc9e01ee93f67afe”, “protected”: false, “min_ram”: 0, “min_disk”: 0, “owner”: “237c099062f649f9bb17788b68be7855”, “os_hidden”: false, “os_hash_algo”: “sha512”, “os_hash_value”: “6513f21e44aa3da349f248188a44bc304a3653a04122d8fb4535423c8e1d14cd6a153f735bb0982e2161b5b5186106570c17a9e58b64dd39390617cd5a350f78”, “id”: “80f3d2a3–704f-4c7c-8fde-fe636b6f2465”, “created_at”: “2019–07–25T17:42:23Z”, “updated_at”: “2019–07–25T17:42:23Z”, “tags”: [], “self”: “/v2/images/80f3d2a3–704f-4c7c-8fde-fe636b6f2465”, “file”: “/v2/images/80f3d2a3–704f-4c7c-8fde-fe636b6f2465/file”, “schema”: “/v2/schemas/image”}], “first”: “/v2/images”, “schema”: “/v2/schemas/images”}
```
`getFlavorRef.py`
> copy value of “X-Subject-Token”and paste it in “X-Auth-Token” key as follows:
```yml
import requests
import json


res = requests.get('http://192.168.56.101:8774/v2.1/flavors',
                    headers={'content-type': 'application/json',
                             'X-Auth-Token': 'gAAAAABdQS3WNgoLUEKmiA8w83_VpFPx-o-C-5mSpRHfEsbjIRjahPRBkA6TDmOGbK7jUfFnajbQ4sBya2RhRYpBZRtyDRLcrF2qhlYgJ3rO7zw1QuE2Dq7lfjXFXszBfNhyR-c10Bv-3tybSzD-GiBG4DcoHKUCRwL6wJpnA4o8nILb763Td48'
                             },
                   )

print(res.text)
```
