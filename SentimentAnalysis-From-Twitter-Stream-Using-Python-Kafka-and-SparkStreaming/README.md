#Sentiment Analysis on the data recieved from Twitter Stream and then pushing the data to Apache Kafa and Apache Spark Streaming APIs for further processing required for analysis 

This sample experiment processes live data streams using Spark’s streaming APIs and Pythonfor sentiment analysis of real-time tweets. 

##Requirements
One of the first requirements is to get access to the streaming data; in this case, real-time tweets. Twitter provides a very 
convenient API to fetch tweets in a streaming manner. Kafka is to buffer the tweets before processing. 

###Project Setup 
 
####Setup Required Python Libraries 
A text file containing the required python packages: `req.txt`

To install all of these, execute it using pip (only missing packages will be installed):    
`$ sudo pip install -r req.txt`
 
####Setting up and Initializing Kafka 
Download and extract the latest binary from https://kafka.apache.org/downloads.html

#####Start zookeeper service:  
`$ bin/zookeeper-server-start.sh config/zookeeper.properties`
 
#####Start kafka service: 
`$ bin/kafka-server-start.sh config/server.properties`
 
#####Create a topic named twitterstm in kafka: 
`$ bin/kafka-topics.sh --create --zookeeper --partitions 1 --topic twitterstm localhost:2181 --replication-factor 1`

 
####Using the Twitter Streaming API 
In order to download the tweets from twitter streaming API and push them to kafka queue,Use the python script
TwitterApp.py. The script will need your twitter authentication tokens (keys).

Once authentication tokens are ready, create `twitter-credentials.txt` with these credential details.

Upon updating the text file with your twitter keys, start downloading tweets from the twitter stream API and push them to the twitterstm topic in Kafka. The steps as below  
`$ python TwitterApp.py`   
Allow the program to keep on running for collecting tweets from the Twitter.
 
#####To check if the data is landing in Kafka: 
`$ bin/kafka-consumer.sh --zookeeper localhost:2181 --topic twitterstm --from-beginning`

#####Running the Stream Analysis Program:
`$ $SPARK_HOME/bin/spark-submit --packages org.apache.spark:spark-streaming-kafka_2.10:1.5.1 twitterStream.py`

####Documentation
https://kafka.apache.org/
https://spark.apache.org/streaming/
https://www.analyticsvidhya.com/blog/2018/07/hands-on-sentiment-analysis-dataset-python/
