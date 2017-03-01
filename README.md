# amazon-mws-elastic
Upload Amazon sellercentral reports into Elastic (ELK stack) - Logstash filters

## Logstash plugins
### Translate plugin
To install translate plugin, stop logstash service and run the following command on your logstash host
/usr/share/logstash/bin/logstash-plugin install logstash-filter-translate


The geo location field type is mapped as geo_point with template inside Elasticsearch. For logstash to be able to upload this template into Elasticsearch, the template path must be defined in the output settings of the logstash config. This template is uploaded into Elasticsearch when Logstash starts up or restarts.

### Manually import template into Elasticsearch
To manually import this template into Elasticsearch, do the following:
curl -XPUT 'http://localhost:9200/_template/mws-collector-reports-fulfillment' -d@/etc/logstash/templates/mws-collector-reports-fulfillment.json

## Upload csv data into logstash
Everything should be ready for the Amazon sellercentral csv data sets.
First generate Fulfilled shipments data report in sellercentral and then download the report.
Copy this file to the path defined in the input settings in the Logstash config file.
input {
  file {
    path => "/tmp/fulfillment/*"

Now, tail the logstash logs and see if the data is flowing correctly through Logstash.
tail -f /var/log/logstash/logstash-plain.log
[2017-03-01T09:38:50,385][DEBUG][logstash.pipeline        ] output received {"event"=>{"bill-country"=>nil, "amazon-order-item-id"=>"45646456", "type"=>"mws-collector-reports-fulfillment", "tracking-number"=>"xxxxxxxxxxxxx", "path"=>"/tmp/fulfillment/AMAZON_FULFILLED_SHIPMENTS_DATA.csv", "amazon-order-id"=>"xxxxxxx", "item-promotion-discount"=>0.0, "estimated-arrival-date"=>"2017-02-07T04:00:00+00:00", "ship-postal-code"=>49404,

## Visualize Amazon reports from Sellercentral in Kibana (Elastic v5.2)
Select Management->Index patterns->Add New
![2017-03-01 09_36_11-kibana](https://cloud.githubusercontent.com/assets/3597603/23454939/d40dc566-fe65-11e6-87ac-c0958201a249.jpg)

![2017-03-01 09_50_16-kibana](https://cloud.githubusercontent.com/assets/3597603/23454940/d40e0d14-fe65-11e6-8301-050106bff1ce.jpg)
![2017-03-01 09_45_58-mws-collector-reports-fulfillment-_ - kibana](https://cloud.githubusercontent.com/assets/3597603/23454937/d40d54dc-fe65-11e6-9b3d-ee2fa0d1e754.jpg)
![2017-03-01 09_44_06-console - kibana](https://cloud.githubusercontent.com/assets/3597603/23454938/d40da3b0-fe65-11e6-9154-6b1efca29d62.jpg)

![2017-03-01 09_51_15-kibana](https://cloud.githubusercontent.com/assets/3597603/23454936/d40d5158-fe65-11e6-9f55-0f62c716e82f.jpg)
