kinda-influxdb-vs-elasticsearch
===============================

Goal
----

Check whether InfluxDB has such significant write throughput difference with Elasticsearch as reported on official Influx Data [website](https://www.influxdata.com/time-series-database/).
Agreement: 
1. Use write commands that are easy to find.
2. Do not search for ways to optimize requests, but use the ones from tutorials.

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
1. Data base name = mydb
1. Measurement/index/table = kinda
1. Tag = test
1. Thread = thread number \[1; 4\]
1. RandomValue = random number \[0; 1000\]
1. Timestamp = timestamp (Only for Elasticsearch)

Steps to reproduce
------------------

1. InfluxDB:
    1. Run the following in terminal:
        ```
        # Pull image
        docker pull influxdb
                    
        # Launch InfluxDB
        docker run -p 8086:8086 \
              -v $PWD:/var/lib/influxdb \
              influxdb
              
        # Create DB
        # curl -G http://localhost:8086/query --data-urlencode "q=CREATE DATABASE mydb"
        # that one was deprecated :(
        # Try this one:
        curl -XPOST http://localhost:8086/query --data-urlencode "q=CREATE DATABASE mydb"
            
        # Check that write functionality is working
        curl -i -XPOST 'http://localhost:8086/write?db=mydb' --data 'kinda,tag=test,thread=1 randomValue=6 946684800'
            
        # Check write command results
        curl -G 'http://localhost:8086/query?db=mydb' --data-urlencode 'q=SELECT * FROM "kinda"'
        # {"results":[{"statement_id":0,"series":[{"name":"kinda","columns":["time","randomValue","tag","thread"],"values":[["1970-01-01T00:00:00.9466848Z",6,"test","1"]]}]}]}
            
        # Clear DB
        curl -i -XPOST 'http://localhost:8086/query' -d db=mydb -d q='DELETE from "kinda"'
        ```
    1. Open ./jmeter/InfluxDB_Test_Plan via jMeter
    1. Run it.
    1. Check results in listeners (e.g. `Response Time Graph`)
    1. Clear DB:
        ```
        # Clear DB
        curl -i -XPOST 'http://localhost:8086/query' -d db=mydb -d q='DELETE from "kinda"'
        ```
    1. Stop Docker by terminating `docker run...`
1. Elasticsearch
            