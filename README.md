kinda-influxdb-vs-elasticsearch
===============================

Goal
----

Check whether InfluxDB has such significant write throughput difference with Elasticsearch as reported on official Influx Data [website](https://www.influxdata.com/time-series-database/).

Plan
----

1. Pull InfluxDB docker image
    1. Start image
    1. Insert some amount of data using jMeter in 4 
    1. Stop image
1. Pull Elasticsearch docker image 
    1. Start image
    1. Insert some amount of data using jMeter
    1. Stop image
1. Add results to [my presentation file](https://docs.google.com/presentation/d/1rFy5cCkXF7AUrnD7SnCHWXU8N2rzUkZcbHb_335_JR4/edit?usp=sharing)
1. Add _steps to reproduce_ into this README file. 

Insertion details
-----------------

Insert should be the following:
1. Measurement/index/table = kinda
1. Tag = test
1. RandomValue = random number
1. Timestamp = timestamp 

Steps to reproduce
------------------

1. TBD
