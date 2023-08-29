# Installation through docker-compose

Installation instructions for Docker can be found [here](https://docs.docker.com/engine/install/).

Copy below docker file below and run it on the server/local machine

```
version: "3.6"

services:
  es:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.10.1
    container_name: "elastic-search"
    environment:
      - discovery.type=single-node
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - xpack.security.enabled=false
  db:
    image: postgres
    container_name: "postgres_db"
    volumes:
    #  - ./init-multi-postgres-databases.sh:/docker-entrypoint-initdb.d/init-multi-postgres-databases.sh
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "9999:5432"
    environment:
      - POSTGRES_DB=registry
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=J93!s27Xma
    env_file:
      - .env
  registry:
    image: dockerhub/sunbird-rc-core:v0.0.13
    container_name: "registry"
    volumes:
      - ./schemas:/home/sunbirdrc/config/public/_schemas
    environment:
      - connectionInfo_uri=jdbc:postgresql://db:5432/registry
      - connectionInfo_username=postgres
      - connectionInfo_password=J93!s27Xma
      - elastic_search_connection_url=es:9200
      - elastic_search_auth_enabled=false
      - elastic_search_username=elastic
      - elastic_search_password=
      - search_providerName=dev.sunbirdrc.registry.service.ElasticSearchService
      - sunbird_sso_realm=sunbird-rc
      - sunbird_sso_url=http://keycloak:8080/auth
      - sunbird_sso_admin_client_id=admin-api
      - sunbird_sso_client_id=registry-frontend
      - sunbird_sso_admin_client_secret=4a858f16-6023-4f23-bda2-1bc4df1a3e7e
      - claims_url=http://claim-ms:8082
      - sign_url=http://certificate-signer:8079/sign
      - sign_health_check_url=http://certificate-signer:8079/health
      - signature_enabled=true
      - pdf_url=http://certificate-api:8078/api/v1/certificatePDF
      - certificate_health_check_url=http://certificate-api:8078/health
      - template_base_url=http://registry:8081/api/v1/templates/ #Looks for certificate templates for pdf copy of the signed certificate
      - sunbird_keycloak_user_set_password=true
      - filestorage_connection_url=http://file-storage:9000
      - filestorage_access_key=admin
      - filestorage_secret_key=12345678
      - filestorage_bucket_key=issuance
      - registry_base_apis_enable=false
      - sunbird_keycloak_user_password=abcd@123
      - logging.level.root=INFO
      - enable_external_templates=true
      - async_enabled=false
      - authentication_enabled=true
      - kafka_bootstrap_address=kafka:9092
      - webhook_enabled=false
      - webhook_url=http://localhost:5001/api/v1/callback
      - redis_host=redis
      - redis_port=6379
      - manager_type=DefinitionsManager
    ports:
      - "18081:8081"
    depends_on:
      - es
      - db
  
  keycloak:
    image: sunbirdrc/keycloak:latest
    container_name: "keycloak"
    volumes:
      - ./imports:/opt/jboss/keycloak/imports
      - keycloak_data:/opt/jboss/keycloak
    environment:
      - KEYCLOAK_LOGO=https://svgshare.com/i/hCs.svg
      - DB_VENDOR=postgres
      - DB_ADDR=db
      - DB_PORT=5432
      - DB_DATABASE=registry
      - DB_USER=postgres
      - DB_PASSWORD=J93!s27Xma
      - KEYCLOAK_USER=admin
      - KEYCLOAK_PASSWORD=admin
      - KEYCLOAK_IMPORT=/opt/jboss/keycloak/imports/realm-export.json
      - KEYCLOAK_FRONTEND_URL=http://localhost:8080/auth
      - PROXY_ADDRESS_FORWARDING=true
      - VALIDATE_NONCE=false
    ports:
      - "18080:8080"
      - "19990:9990"
    depends_on:
      - db
  telemetry_service_clickhouse:
    image: clickhouse/clickhouse-server
    container_name: telemetry-clickhouse
    volumes:
      - clickhouse_data:/var/lib/clickhouse
    environment:
      CLICKHOUSE_USER: clickhouse
      CLICKHOUSE_PASSWORD: "*!73uK*9xLEsnhIR"
    ports:
      - "18123:8123"
      - "19000:9000"
  telemetry-service:
    image: dubea/telemetry-service1:latest
    ports:
      - 19001:9001
    environment:
      telemetry_local_storage_type: "clickhouse"
      CLICKHOUSE_HOST: "http://telemetry-clickhouse:8123"
      CLICKHOUSE_USER: "clickhouse"
      CLICKHOUSE_PASSWORD: "*!73uK*9xLEsnhIR"
    depends_on:
      - telemetry_service_clickhouse
  bff_service:
    image: dubea/bff:1.0
    container_name: "bff-service"
    ports:
      - 3003:3000
    deploy:
      placement:
        constraints:
          - node.role == manager
      replicas: 1
      update_config:
        parallelism: 1
        delay: 0s
        failure_action: rollback
        order: start-first
      restart_policy:
        condition: on-failure

    environment:
      - name=value #Need to add the environment varibales in that
      - PORT=3000
      - CRED_URL=http://139.59.45.135:3002
      - DID_URL=http://139.59.45.135:3000
      - SCHEMA_URL=http://139.59.45.135:3001
      - MIDDLEWARE_URL=http://139.59.45.135:3003
      - REALM_ID=sunbird-rc
      - KEYCLOAK_CLIENT_ID=admin-api
      - KEYCLOAK_CLIENT_SECRET=4a858f16-6023-4f23-bda2-1bc4df1a3e7e
      - KEYCLOAK_URL=https://ulp-sbrc.uniteframework.io/auth/
      - REGISTRY_URL=https://ulp-sbrc.uniteframework.io/registry/
      - TESTVAR=ulp reference environment by RC
      - PROOF_OF_ASSESSMENT=clf0qfvna0000tj154706406y
      - PROOF_OF_ENROLLMENT=clf0rjgov0002tj15ml0fdest
      - PROOF_OF_BENIFIT=clf0wvyjs0008tj154rc071i1
      - TELEMETRY_URL=https://ulp.uniteframework.io/telemetry
  bulk_issuance:
    image: dubea/bulk-issuance:1.0
    container_name: "bulk-issuance"
    ports:
      - 3007:3007
    deploy:
      placement:
        constraints:
          - node.role == manager
      replicas: 1
      update_config:
        parallelism: 1
        delay: 0s
        failure_action: rollback
        order: start-first
      restart_policy:
        condition: on-failure

    environment:
      - name=value #Need to add the environment variables in that
      - PORT=3007
      - CRED_URL=http://139.59.45.135:3002
      - DID_URL=http://139.59.45.135:3000
      - SCHEMA_URL=http://139.59.45.135:3001
      - MIDDLEWARE_URL=http://139.59.45.135:3003
      - KEYCLOAK_URL=https://ulp-sbrc.uniteframework.io/auth/
      - REGISTRY_URL=https://ulp-sbrc.uniteframework.io/registry/
      #schemaId
      - PROOF_OF_ASSESSMENT=clf0qfvna0000tj154706406y
      - PROOF_OF_ENROLLMENT=clf0rjgov0002tj15ml0fdest
      - PROOF_OF_BENIFIT=clf0wvyjs0008tj154rc071i1
      #keycloak
      - REALM_ID=sunbird-rc
      - KEYCLOAK_CLIENT_ID=admin-api
      - KEYCLOAK_CLIENT_SECRET=4a858f16-6023-4f23-bda2-1bc4df1a3e7e
  rc-wallet:
    image: dubea/rc-ewallet:1.2
    container_name: "ewallet"
    volumes:
      - ./ewallet/config/config.json:/usr/share/nginx/html/assets/config/config.json
  rc-verification:
    image: dubea/verification:1.2
    container_name: "verification-portal"
    volumes:
      - ./varification/config/config.json:/usr/share/nginx/html/assets/config/config.json
  nginx-gateway:
    image: dubea/nginx-gateway:latest
    ports:
      - "182:80"
volumes:
  postgres_data:
    driver: local
  didl3_data:
    driver: local
  cred_schema_data:
    driver: local
  cred_ms_data:
    driver: local
  clickhouse_data:
    driver: local
  keycloak_data:
    driver: local
```

