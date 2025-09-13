# RAGFlow - semantic search over PBS documentation

This is a tool which can index documents such as PDFs and Markdown files and then allows to do a semantic search over the indexed documents. The end goal is a centralized search engine over "all" PBS documentation.

This RAGFlow instance is explicitly not intended to directly build LLM pipelines and agents, even though RAGFlow of course supports this. LLM pipelines should be implemented externally and may use this tool via MCP to look up relevant scouting-specific information.

## Features and roadmap
- [ ] MiData login
- [ ] Add more data sources
  - [ ] All cudesch content from cudesch.scout.ch
  - [ ] PDF brochures about education courses (used in Topkurs)
  - [ ] hering.scout.ch
  - [ ] Anker and other pdfs from the PBS download page
  - [ ] J+S documentation relevant to scouting, if legally allowed
  - [ ] MiData documentation on docu.scout.ch and hitobito.readthedocs.io and github.com/hitobito/hitobito_pbs
  - [ ] thilo and/or "Pfaditechnik in Wort und Bild"
  - [ ] Regional or cantonal documentation
  - [ ] (optional) Cudesch PDFs from issuu that aren't yet on cudesch.scout.ch
- [ ] Automate document re-indexing when some documentation changes
- [ ] Set up multiple datasets / knowledge bases or another way to filter the documents by relevance to common use cases (e.g. only documents for J+S Basis courses)
- [ ] MCP server so that LLMs can use this tool
- [ ] Set up multilinguality / separate datasets for the French and Italian versions of the documentation
- [ ] Set up some automated end-to-end testing
- [ ] Set up renovate bot to auto-update all third-party software

See also the RAGFlow docs here: https://github.com/infiniflow/ragflow

For now, this tool is maintained by [Cosinus](https://github.com/carlobeltrame) and hosted on the PBS Betriebsplattform (thanks!). It may also be integrated into the [pfadi.ai](https://github.com/carlobeltrame/pfadi.ai) tools in the future.

## Preparation for deployment

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

[Here](./pbs-rag-comparison.ods) is an evaluation of several RAG tools listed on the internet as of September 2025. Given the chosen criteria, the solution came down to either RAGFlow or LangChain. LangChain is more flexible and has a bigger ecosystem, but does not come with a finished RAG pipeline, authentication/authorization and application UI out of the box. So for getting a system up quickly to start evaluating approaches, and so that not too much effort has to be invested in developing yet another solution, for now a RAGFlow instance is deployed. In case more advanced and custom use cases come up, a switch to LangChain is possible in the future.

## Contributing

Contributions are welcome. For now there isn't much code to contribute to, but in the future some additional features may require coding extensions. If you want to help out or have some feedback, feel free to open an issue or a PR.
