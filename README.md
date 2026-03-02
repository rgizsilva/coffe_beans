

listener.name.internal.ssl.principal.mapping.rules: "RULE:^.*[Cc][Nn]=([a-zA-Z0-9._-]*).*$/$1/L,DEFAULT"
curl -k -X POST -H "Content-Type: application/json" --upload-file <NAME>.json http://localhost:8083/connectors


#!/usr/bin/env python3

import cx_Oracle
import sys

ORACLE_HOST = ''
ORACLE_PORT = 
ORACLE_SERVICE = ''
ORACLE_USER = ''
ORACLE_PASSWORD = ''

def get_latest_scn():
    try:
        dsn = cx_Oracle.makedsn(ORACLE_HOST, ORACLE_PORT, service_name=ORACLE_SERVICE)
        connection = cx_Oracle.connect(user=ORACLE_USER, password=ORACLE_PASSWORD, dsn=dsn)
        cursor = connection.cursor()
        
        query = """
        SELECT 
            TO_CHAR(FIRST_TIME, 'YYYY-MM-DD HH24:MI:SS') as FIRST_TIME,
            TO_CHAR(NEXT_TIME, 'YYYY-MM-DD HH24:MI:SS') as NEXT_TIME,
            FIRST_CHANGE# as FIRST_SCN,
            NEXT_CHANGE# as NEXT_SCN
        FROM V$ARCHIVED_LOG
        ORDER BY FIRST_TIME DESC
        FETCH FIRST 1 ROW ONLY
        """
        
        cursor.execute(query)
        result = cursor.fetchone()
        
        if result:
            first_time = result[0]
            next_time = result[1]
            first_scn = result[2]
            next_scn = result[3]
            
            print(f"FIRST_TIME: {first_time}")
            print(f"NEXT_TIME: {next_time}")
            print(f"FIRST_SCN: {first_scn}")
            print(f"NEXT_SCN: {next_scn}")
            
            return first_scn
        else:
            print("Nenhum registro encontrado")
            return None
            
        cursor.close()
        connection.close()
        
    except cx_Oracle.DatabaseError as e:
        error, = e.args
        print(f"Erro Database: {error.code} - {error.message}")
        sys.exit(1)
    except Exception as e:
        print(f"Erro: {e}")
        sys.exit(1)

if __name__ == "__main__":
    scn = get_latest_scn()
    if scn:
        print(f"\nSCN : {scn}")
