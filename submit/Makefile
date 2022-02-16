run-sparkpi:
	docker-compose run app spark-submit \
		--class org.apache.spark.examples.SparkPi \
		--master yarn \
		--deploy-mode client \
		--executor-memory 4G \
		--num-executors 4 \
		/spark/examples/jars/spark-examples_2.12-3.0.1.jar \
		 1000


run-heda-preprocessing:
	docker-compose run app /data/test-preprocessing-data.sh

run-shell:
	docker-compose run app /bin/bash

run-consumer:
	docker-compose run -d app spark-submit --master yarn --packages org.apache.spark:spark-avro_2.12:3.2.0,org.apache.spark:spark-sql-kafka-0-10_2.12:3.2.0 data/Thesis/kafka_adafruit/consumer_no_schema.py

