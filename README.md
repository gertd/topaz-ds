# topaz-ds

Topaz Docker Compose setup

The docker-compose setup consists of:

* ds   - topaz directory only instance (ports GRPC: 19292, HTTP:19393)
* az-1 - topaz authorizer with a remote directory (ports GRPC: 29292, HTTP:29393)
* az-2 - topaz authorizer with an edge directory, synchronized from ds (ports GRPC: 39292, HTTP:39393)

Load the manifest into the directory using:

```
topaz directory set manifest ./ds/model/manifest.yaml --host localhost:19292 --insecure
```

```
>>> set manifest to /Users/gertd/workspace/src/github.com/gertd/topaz-ds/ds/model/manifest.yaml%  
```

Load the data into the directory using:

```
topaz directory import --directory ./ds/data --host localhost:19292 --insecure
```

```
>>> importing data from ./ds/data
        objects 31
      relations 46
```

Make identity resolution request to `az1` using `grpcurl`:

```
grpcurl -insecure -d '{
  "query": "x = input",
  "identityContext": {
    "identity": "beth@the-smiths.com",
    "type": "IDENTITY_TYPE_SUB"
  }
}' localhost:29292 aserto.authorizer.v2.Authorizer.Query
```

Result:

```
{
  "response": {
    "result": [
      {
        "bindings": {
          "x": {
            "identity": {
              "identity": "beth@the-smiths.com",
              "type": "IDENTITY_TYPE_SUB"
            },
            "user": {
              "created_at": "2024-05-13T22:21:01.355016595Z",
              "display_name": "Beth Smith",
              "etag": "512654990579550493",
              "id": "beth@the-smiths.com",
              "key": "beth@the-smiths.com",
              "properties": {
                "email": "beth@the-smiths.com",
                "picture": "https://www.topaz.sh/assets/templates/citadel/img/Beth%20Smith.jpg",
                "roles": [
                  "viewer"
                ],
                "status": "USER_STATUS_ACTIVE"
              },
              "type": "user",
              "updated_at": "2024-05-13T22:21:01.355016595Z"
            }
          }
        },
        "expressions": [
          {
            "location": {
              "col": 1,
              "row": 1
            },
            "text": "x = input",
            "value": true
          }
        ]
      }
    ]
  },
  "metrics": {}
}
```

Make identity resolution request to `az2` using `grpcurl`:

```
grpcurl -insecure -d '{
  "query": "x = input",
  "identityContext": {
    "identity": "beth@the-smiths.com",
    "type": "IDENTITY_TYPE_SUB"
  }
}' localhost:39292 aserto.authorizer.v2.Authorizer.Query
```

Result:

```
{
  "response": {
    "result": [
      {
        "bindings": {
          "x": {
            "identity": {
              "identity": "beth@the-smiths.com",
              "type": "IDENTITY_TYPE_SUB"
            },
            "user": {
              "created_at": "2024-05-13T22:21:08.345990709Z",
              "display_name": "Beth Smith",
              "etag": "512654990579550493",
              "id": "beth@the-smiths.com",
              "key": "beth@the-smiths.com",
              "properties": {
                "email": "beth@the-smiths.com",
                "picture": "https://www.topaz.sh/assets/templates/citadel/img/Beth%20Smith.jpg",
                "roles": [
                  "viewer"
                ],
                "status": "USER_STATUS_ACTIVE"
              },
              "type": "user",
              "updated_at": "2024-05-13T22:21:08.345990709Z"
            }
          }
        },
        "expressions": [
          {
            "location": {
              "col": 1,
              "row": 1
            },
            "text": "x = input",
            "value": true
          }
        ]
      }
    ]
  },
  "metrics": {}
}
```

Make identity resolution request to `az1` using `curl`:

```
curl -k -X POST -d '{
  "query": "x = input",
  "identityContext": {
    "identity": "beth@the-smiths.com",
    "type": "IDENTITY_TYPE_SUB"
  }
}' https://localhost:29393/api/v2/authz/query
```

Result:

```
{
  "response":  {
    "result":  [
      {
        "bindings":  {
          "x":  {
            "identity":  {
              "identity":  "beth@the-smiths.com",
              "type":  "IDENTITY_TYPE_SUB"
            },
            "user":  {
              "created_at":  "2024-05-13T22:21:08.345990709Z",
              "display_name":  "Beth Smith",
              "etag":  "512654990579550493",
              "id":  "beth@the-smiths.com",
              "key":  "beth@the-smiths.com",
              "properties":  {
                "email":  "beth@the-smiths.com",
                "picture":  "https://www.topaz.sh/assets/templates/citadel/img/Beth%20Smith.jpg",
                "roles":  [
                  "viewer"
                ],
                "status":  "USER_STATUS_ACTIVE"
              },
              "type":  "user",
              "updated_at":  "2024-05-13T22:21:08.345990709Z"
            }
          }
        },
        "expressions":  [
          {
            "location":  {
              "col":  1,
              "row":  1
            },
            "text":  "x = input",
            "value":  true
          }
        ]
      }
    ]
  },
  "metrics":  {},
  "trace":  [],
  "trace_summary":  []
}
```

Make identity resolution request to `az2` using `curl`:

```
curl -k -X POST -d '{
  "query": "x = input",
  "identityContext": {
    "identity": "beth@the-smiths.com",
    "type": "IDENTITY_TYPE_SUB"
  }
}' https://localhost:39393/api/v2/authz/query
```

Result:

```
{
  "response":  {
    "result":  [
      {
        "bindings":  {
          "x":  {
            "identity":  {
              "identity":  "beth@the-smiths.com",
              "type":  "IDENTITY_TYPE_SUB"
            },
            "user":  {
              "created_at":  "2024-05-13T22:21:08.345990709Z",
              "display_name":  "Beth Smith",
              "etag":  "512654990579550493",
              "id":  "beth@the-smiths.com",
              "key":  "beth@the-smiths.com",
              "properties":  {
                "email":  "beth@the-smiths.com",
                "picture":  "https://www.topaz.sh/assets/templates/citadel/img/Beth%20Smith.jpg",
                "roles":  [
                  "viewer"
                ],
                "status":  "USER_STATUS_ACTIVE"
              },
              "type":  "user",
              "updated_at":  "2024-05-13T22:21:08.345990709Z"
            }
          }
        },
        "expressions":  [
          {
            "location":  {
              "col":  1,
              "row":  1
            },
            "text":  "x = input",
            "value":  true
          }
        ]
      }
    ]
  },
  "metrics":  {},
  "trace":  [],
  "trace_summary":  []
}
```