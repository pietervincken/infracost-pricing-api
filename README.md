> [!NOTE]  
> This is a clone of the original repository and is not connected to Infracost!
> Do not use for production workloads, dependencies and implementation are heavily outdated.

# How to use

The pricing API downloads pricing data directly from cloud providers such as AWS and Azure.
It stores it locally in a Postgres database and exposes it via a REST API.

## First time setup

1. Run `docker-compose up -d api` to start the database, initialise the database and start the API server.
2. Run `docker-compose up update_job` to scrape Azure & AWS pricing data.
3. Run `docker-compose up dump_job` dumps the scrape data into zip files in the `data/` folder for offline use later on.

## Earlier products dump available (offline mode)

1. Run `docker-compose up -d api` to start the database, initialise the database and start the API server.
2. Run `docker-compose up load_job` to load pricing data from the products zip files into the database.

## Run infracost cli against the local API

You can run the infracost CLI against the local pricing API by setting the `PRICING_API_ENDPOINT` and `INFRACOST_API_KEY` environment variables. 

```bash
docker run --rm \
  -e PRICING_API_ENDPOINT=http://host.docker.internal:4000 \
  -e INFRACOST_API_KEY=<token> \
  -v <path-to-terraform-folder>:/tf \
  infracost/infracost:ci-test \
  breakdown --show-skipped --path /tf/
```