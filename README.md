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

```yml
{‘Date’: ‘Wed, 31 Jul 2019 05:20:31 GMT’, ‘Server’: ‘Apache/2.4.29 (Ubuntu)’, ‘Content-Length’: ‘4723’, ‘X-Subject-Token’: ‘gAAAAABdQS3WNgoLUEKmiA8w83_VpFPx-o-C-5mSpRHfEsbjIRjahPRBkA6TDmOGbK7jUfFnajbQ4sBya2RhRYpBZRtyDRLcrF2qhlYgJ3rO7zw1QuE2Dq7lfjXFXszBfNhyR-c10Bv-3tybSzD-GiBG4DcoHKUCRwL6wJpnA4o8nILb763Td48’, ‘Vary’: ‘X-Auth-Token’, ‘x-openstack-request-id’: ‘req-8a4837ff-a157–4b12-b431–94da754c3579’, ‘Keep-Alive’: ‘timeout=5, max=100’, ‘Connection’: ‘Keep-Alive’, ‘Content-Type’: ‘application/json’}
```

## Step 2:Get values of imageRef ,flavorRef ,uuid(your network id)

> copy value of “X-Subject-Token”and paste it in “X-Auth-Token” key as follows:
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
```yml
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
> Output exemple : 
```yml
{“flavors”: [{“id”: “69355004–7010–45f9–9d7e-9cec723ee076”, “name”: “m1.extra_tiny”, “links”: [{“rel”: “self”, “href”: “http://192.168.56.101:8774/v2.1/flavors/69355004-7010-45f9-9d7e-9cec723ee076"}, {“rel”: “bookmark”, “href”: “http://192.168.56.101:8774/flavors/69355004-7010-45f9-9d7e-9cec723ee076"}]}]}
```
`getNetworkId.py`
```yml
import requests

res= requests.get("http://192.168.56.101:9696/v2.0/networks",
                     headers={'content-type': 'application/json',
                             'X-Auth-Token': 'gAAAAABdQS3WNgoLUEKmiA8w83_VpFPx-o-C-5mSpRHfEsbjIRjahPRBkA6TDmOGbK7jUfFnajbQ4sBya2RhRYpBZRtyDRLcrF2qhlYgJ3rO7zw1QuE2Dq7lfjXFXszBfNhyR-c10Bv-3tybSzD-GiBG4DcoHKUCRwL6wJpnA4o8nILb763Td48'
                             },
                  )

print(res.text)
```
> Output exemple :
```yml
{“networks”:[{“id”:”66d2d553–1a09–4be9-ad93–5f43c3a0e20d”,”name”:”net1",”tenant_id”:”237c099062f649f9bb17788b68be7855",”admin_state_up”:true,”mtu”:1450,”status”:”ACTIVE”,”subnets”:[“a839d02e-0201–4690-a5dc-8993f4a356ba”],”shared”:false,”availability_zone_hints”:[],”availability_zones”:[“nova”],”ipv4_address_scope”:null,”ipv6_address_scope”:null,”router:external”:false,”description”:””,”port_security_enabled”:true,”tags”:[],”created_at”:”2019–07–30T06:33:07Z”,”updated_at”:”2019–07–30T06:33:07Z”,”revision_number”:2,”project_id”:”237c099062f649f9bb17788b68be7855",”provider:network_type”:”vxlan”,”provider:physical_network”:null,”provider:segmentation_id”:10},{“id”:”88915d02–7975–4157–8771–66051c16757f”,”name”:”provider”,”tenant_id”:”237c099062f649f9bb17788b68be7855",”admin_state_up”:true,”mtu”:1500,”status”:”ACTIVE”,”subnets”:[“4f37759b-de42–4933-a1d8–96f8db1838aa”],”shared”:true,”availability_zone_hints”:[],”availability_zones”:[“nova”],”ipv4_address_scope”:null,”ipv6_address_scope”:null,”router:external”:true,”description”:””,”port_security_enabled”:true,”is_default”:false,”tags”:[],”created_at”:”2019–07–30T12:26:25Z”,”updated_at”:”2019–07–30T15:09:10Z”,”revision_number”:5,”project_id”:”237c099062f649f9bb17788b68be7855",”provider:network_type”:”flat”,”provider:physical_network”:”provider”,”provider:segmentation_id”:null}]}
```
> here net1 is internal network and provider is public network ! 

## step 3 : running instance from an api : 
> As always copy value of “X-Subject-Token”and paste it in “X-Auth-Token” key as follows:
`create_instance.py`
```yml
import requests
import json

payload= {
    "server": {
        "name": "test2",
        "imageRef": "80f3d2a3-704f-4c7c-8fde-fe636b6f2465",
        "flavorRef": "69355004-7010-45f9-9d7e-9cec723ee076",
        "networks": [{
            "uuid": "66d2d553-1a09-4be9-ad93-5f43c3a0e20d"
        }]
    }
}


res = requests.post('http://192.168.56.101:8774/v2.1/servers',
                    headers={'content-type': 'application/json',
                             'X-Auth-Token': 'gAAAAABdQS3WNgoLUEKmiA8w83_VpFPx-o-C-5mSpRHfEsbjIRjahPRBkA6TDmOGbK7jUfFnajbQ4sBya2RhRYpBZRtyDRLcrF2qhlYgJ3rO7zw1QuE2Dq7lfjXFXszBfNhyR-c10Bv-3tybSzD-GiBG4DcoHKUCRwL6wJpnA4o8nILb763Td48',
                             },
                    data=json.dumps(payload)
                   )

print(res.text)
```
- [!] Output should be like this : 
```yml
{“server”: {“id”: “04cb2bb4–5d11–4b71–89b2-e380811c9ddf”, “links”: [{“rel”: “self”, “href”: “http://192.168.56.101:8774/v2.1/servers/04cb2bb4-5d11-4b71-89b2-e380811c9ddf"}, {“rel”: “bookmark”, “href”: “http://192.168.56.101:8774/servers/04cb2bb4-5d11-4b71-89b2-e380811c9ddf"}], “OS-DCF:diskConfig”: “MANUAL”, “security_groups”: [{“name”: “default”}], “adminPass”: “a5Jb7FPLjrh8”}}
```
### Your instance is created successfully.Go and check it on horizon :) .
