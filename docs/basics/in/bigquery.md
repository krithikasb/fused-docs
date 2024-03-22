# Google BigQuery



import Tag from '@site/src/components/Tag'

Fused interfaces <Tag color="#3399ff">Google BigQuery</Tag> through the Python bigquery library enabling efficient data processing and analysis. Follow these steps to harness the power of BigQuery within your Fused environment.

# 1. Generate Google access credentials json

Before proceeding, ensure you have access credentials for Google Cloud Platform (GCP). Follow the guide [here](https://developers.google.com/workspace/guides/create-credentials) to create the necessary credentials.



# 2. Set the Access Credentials

Run a UDF to set the Google access credentials as a json file in your Fused account's file system. This process resembles [setting an environment variables file](/core_concepts/?h=environ#environment-variables).

Implement the following Python code snippet to set your credentials json string in the environment:

```python
import fused

@fused.udf
def udf():
    credentials_json_string = "..."

    # Path to your credentials json file
    env_file_path = '/mnt/cache/my_gee_creds.json'

    # Write the json string to the .json file
    with open(env_file_path, 'w') as file:
        file.write(env_vars)
```

# 3. Run a query on BigQuery

Now, you can run queries on BigQuery directly from your Fused environment. See the example code snippet below:


```python
import fused

@fused.udf
def udf(bbox=None, geography_column=None):
    import fused
    from google.oauth2 import service_account
    from google.cloud import bigquery

    # This UDF will only work on runtime with mounted EFS
    key_path = "/mnt/cache/my_gee_creds.json"

    # Authenticate BigQuery
    credentials = service_account.Credentials.from_service_account_file(
        key_path,
        scopes=["https://www.googleapis.com/auth/cloud-platform"]
    )

    # Create a BigQuery client
    client = bigquery.Client(credentials=credentials, project=credentials.project_id)

    # Structure spatial query
    query=f"""
        SELECT * FROM `bigquery-public-data.new_york.tlc_yellow_trips_2015`
        LIMIT 10
    """

    if geography_column:
        return client.query(query).to_geodataframe(geography_column=geography_column)
    else:
        return client.query(query).to_dataframe()
```