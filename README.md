

listener.name.internal.ssl.principal.mapping.rules: "RULE:^.*[Cc][Nn]=([a-zA-Z0-9._-]*).*$/$1/L,DEFAULT"
curl -k -X POST -H "Content-Type: application/json" --upload-file <NAME>.json http://localhost:8083/connectors


wget https://download.oracle.com/otn_software/linux/instantclient/2340000/oracle-instantclient-basic-23.4.0.24.05-1.el8.x86_64.rpm

sudo dnf install -y oracle-instantclient-basic-23.4.0.24.05-1.el8.x86_64.rpm

export LD_LIBRARY_PATH=/usr/lib/oracle/23/client64/lib:$LD_LIBRARY_PATH
