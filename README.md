# Santander Sensor Data

![indexpatterns](https://github.com/mcascallares/santander-sensor-data/blob/master/screenshots/dashboard.png)

Quick demo using the Elastic stack to visualize sensor information provided by Santander Town Hall and available through [datos.gob.es](http://datos.gob.es). [This dataset](http://datos.gob.es/es/catalogo/l01390759-sensores-ambientales) contains sensor measurements for light, noise, temperature and battery gathered from different locations across the city of Santander.

## Requirements

- docker engine (v1.12.0+)
- docker-compose
- Elastic stack v5.4 (installed and managed by Docker()

## Usage

### Retrieving new data

Data can be accessed [here](http://datos.gob.es/es/catalogo/l01390759-sensores-ambientales). For this example we will use the [JSON format](http://datos.santander.es/api/rest/datasets/sensores_smart_env_monitoring.json) to load data into Elasticsearch using Logstash.


## Sanitize the data

In order to parse the data with Logstash, we will remove the EOLs in the file. For this I will use [jq JSON processor](https://stedolan.github.io/jq/) but you can use anything that you like, even sed ;)

```
wget http://datos.santander.es/api/rest/datasets/sensores_smart_env_monitoring.json
cat sensores_smart_env_monitoring.json | jq -c . > data_no_eol.json
```

## Running the Elastic stack

To parse, load and visualize the data we only need to execute the containers defined in docker-compose.yml:

```
docker-compose up
```

Note that Elastic containers will also install [X-Pack](https://www.elastic.co/products/x-pack), commercial extensions provided by Elastic, to get access control and better mapping capabilities.


## Configuring Kibana

Once data is loaded into Elasticsearch we will use Kibana to visualize it. To access Kibana the default username and password is **elastic:changeme**

![login](https://github.com/mcascallares/santander-sensor-data/blob/master/screenshots/login.png)

The first step is to configure a Kibana index pattern for the index 'sensors'. In the Management > Kibana > Index Patterns section we should have:

![indexpatterns](https://github.com/mcascallares/santander-sensor-data/blob/master/screenshots/indexpatterns.png)

After this step, we can import the pre-built visualizations and dashboard. For that we should go to Management > Kibana > Saved Objects and import kibana/objects.json file.


## Dashboard

Finally to open the pre-built dashboard we go to Dashboard and we open/select the dashboard called 'Santander'. You can also create your own visualization and dashboards depending on your needs. Enjoy!
