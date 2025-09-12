# RAGFlow - semantic search over PBS documentation

See docs here: https://github.com/infiniflow/ragflow

## Preparation

### Set up environment variables

```shell
cp .env.example .env
vi .env
```

### Creating passwords for the values

```shell
openssl rand -hex 16 | pbcopy
```

## Installation / Upgrade

```shell
. .env && helm upgrade --install ragflow ./helm \
  --set ingress.host="$APP_DOMAIN" \
  --set env.ELASTIC_PASSWORD="$ELASTIC_PASSWORD" \
  --set env.MYSQL_HOST="$MYSQL_HOST" \
  --set env.MYSQL_USER="$MYSQL_USER" \
  --set env.MYSQL_PASSWORD="$MYSQL_PASSWORD" \
  --set env.MYSQL_DBNAME="$MYSQL_DBNAME" \
  --set env.MINIO_PASSWORD="$MINIO_PASSWORD" \
  --set env.REDIS_PASSWORD="$REDIS_PASSWORD" \
  --set ragflow.service_conf.oauth.midata.issuer="$MIDATA_BASE_URL" \
  --set ragflow.service_conf.oauth.midata.client_id="$MIDATA_CLIENT_ID" \
  --set ragflow.service_conf.oauth.midata.client_secret="$MIDATA_CLIENT_SECRET" \
  --set ragflow.service_conf.oauth.midata.redirect_uri="$MIDATA_OIDC_CALLBACK_URL"
```

The helm chart was taken and adapted from [the RAGFlow repository](https://github.com/infiniflow/ragflow/tree/main/helm), and the original is published under the Apache License 2.0. All modifications to the original are, as long as they are not contributed and merged back into the upstream original, available under the MIT license.

## Why RAGFlow?

[Here](./pbs-rag-comparison.ods) is an evaluation of several RAG tools listed on the internet as of September 2025. Given the chosen criteria, the solution came down to either RAGFlow or LangChain. LangChain is more flexible and has a bigger ecosystem, but does not come with a finished pipeline, authentication/authorization and application UI out of the box. So for getting a system up quickly to start evaluating approaches, and so that not too much effort has to be invested in developing yet another solution, for now a RAGFlow instance is deployed. In case more advanced and custom use cases come up, a switch to LangChain is possible in the future.
