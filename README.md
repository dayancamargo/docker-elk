# docker-elk

#Prerequisites
This project gets all `.json` files and saves on elasticsearch. All files need to be filtred by logstash pipeline or can be used a java dependency to encode all logs entry as json elements


##JAVA configuration
logback-spring.xml example:
` 
<!--This appender will save all records as LOG_FILE.json-->
<appender name="jsonAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <File>${LOG_PATH}/${LOG_FILE}.json</File>

    <encoder class="net.logstash.logback.encoder.LogstashEncoder" />

    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
        <FileNamePattern>${LOG_PATH}/compressed/${LOG_FILE}.%d{yyyy-MM-dd}.json.gz</FileNamePattern>
    </rollingPolicy>
</appender>
`

Pom.xml example:
`
<!-- logstash logback - encoder to json log-->
<dependency>
	<groupId>net.logstash.logback</groupId>
	<artifactId>logstash-logback-encoder</artifactId>
	<version>4.11</version>
</dependency>
`

#Tree

* docker-compose - file with docker information


* config - folder with all configuration files, this will be moved to logstash docker as a volume;
	
	* logstash.yml - file with logstash configurations (first version has only pipeline's path)
	* pipeline  - folder with all pipelines used by logstash
		* mainPipeline.conf - pipeline default, it will take files from /logs and send to a elasticsearch

* logs - folder where Logstash will monitor all log files. It will be used as a volume inside logstash docker


* .env - file with environment variables
	* TAG - default:6.2.3 			- elk tag
	* LOGS - ./logs 				- path from where mount volume which has all logs
	* PIPELINES - ./config/pipeline - path from where mount volume which has all pipelines
	* CONFIG -./config/logstash.yml - Logstash's configuration file

#Run
	docker-compose up


#Usage
	
##Kibana
	http://localhost:5601 - acess kibana dash

##Logstash
	Enter on logstash docker (docker exec -it <logstashID> bash)
	on docker execute:
		
		* curl -XGET 'localhost:9600/_node/<types>'   
			* pipelines - Gets pipeline-specific information and settings for each pipeline.
			* os - Gets node-level info about the OS.
			* jvm - Gets node-level JVM info, including info about threads.
		* curl -XGET 'localhost:9600/_node/stats/<types>'
			* jvm -  Gets JVM stats, including stats about threads, memory usage, garbage collectors, and uptime.
			* process - Gets process stats, including stats about file descriptors, memory consumption, and CPU usage.
			* events - Gets event-related statistics for the Logstash instance (regardless of how many pipelines were created and destroyed).
			* pipelines - Gets runtime stats about each Logstash pipeline.
			* reloads - Gets runtime stats about config reload successes and failures.
			* os - Gets runtime stats about cgroups when Logstash is running in a container.


	More on https://www.elastic.co/guide/en/logstash/current/monitoring.html

