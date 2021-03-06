# mockrs
mock restful json http server

mockrs use a json file as database, serve as http server.
You can get part of json file by doing http request

## Install & Usage

```bash
cargo install --git https://github.com/PrivateRookie/mockrs.git

mockrs --help

mockrs 0.1.0
PrivateRookie <996514515@qq.com>
a mock restful json http server

USAGE:
    mockrs [OPTIONS] <db-file>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
        --host <host>    listen ip [env: MOCKRS_HOST=]  [default: 127.0.0.1]
        --port <port>    listen port [env: MOCKRS_PORT=9002]  [default: 9000]

ARGS:
    <db-file>    json file as database [env: MOCKRS_DB_FILE=]
```

db.json content:

```json
{
  "posts": [
    {
      "name": "a"
    },
    {
      "name": "b"
    },
    {
      "name": "d"
    }
  ]
}
```

run mockrs

```bash
mockrs db.json
```

### do query

```bash
# get all content
curl http://localhost:9000/ | jq

{
  "posts": [
    {
      "name": "a"
    },
    {
      "name": "b"
    },
    {
      "name": "d"
    }
  ]
}

# get first post
curl http://localhost:9000/posts/2 | jq

{
  "name": "d"
}

# get firs post's name

curl http://localhost:9000/posts/2/name | jq

"d"
```

### update entry 

```bash
# both "post" and "put" are OK
curl http://localhost:9000/posts/2 \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"name": "XD"}'

curl http://localhost:9000/posts/2 | jq

{
  "name":"XD"
}
```

**note** you can insert entry on top field

```bash
curl http://localhost:9000/users \
  -X POST \
  -H "Content-Type: application/json" \
  -d '[]'

curl http://localhost:9000/users | jq

[]
```

### delete entry

```bash
curl http://localhost:9000/posts/2 -X DELETE

curl http://localhost:9000/posts | jq

[
  {
    "name": "a"
  },
  {
    "name": "d"
  }
]
```

### flush data to a file

```bash
curl http://localhost:9000/_actions/flush \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"file": <path_to_file> }'
```
