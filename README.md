

listener.name.internal.ssl.principal.mapping.rules: "RULE:^.*[Cc][Nn]=([a-zA-Z0-9._-]*).*$/$1/L,DEFAULT"
curl -k -X POST -H "Content-Type: application/json" --upload-file <NAME>.json http://localhost:8083/connectors
