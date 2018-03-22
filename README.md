# docker-elk

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
	http://localhost:5601 - acess kibana dash