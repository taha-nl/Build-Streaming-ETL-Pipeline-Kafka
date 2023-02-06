# Build a Streaming ETL Pipeline using Kafka

## Scenario
 As a vehicle passes a toll plaza, the vehicle’s data like vehicle_id,vehicle_type,toll_plaza_id and timestamp are streamed to Kafka.our job is to create a data pipe line that collects the streaming data and loads it into a database.
 
 ## Prerequisites:
 * Docker
 
 ## Environment Preparation
  first ,we are going to work with a docker ubuntu-image  container
  
  ### step1: ubuntu container
  ```
  docker pull ubuntu
  docker run -itd -p 9092:9092 --name kafka --hostname kafka  ubuntu 
  ```
  This command runs a new Docker container with the name "kafka" and hostname "kafka" from the "ubuntu" image. The -itd options run the container in the background and allocate a TTY. The -p 9092:9092 option maps port 9092 of the host to port 9092 in the container, making the Kafka service running in the container accessible on the host machine on port 9092.
  
  ### step2: installing kafka
  * open bash shell in Docker container
  
  ```
  docker exec -it kafka bash
  
  ```
  * upgrade apt & download wget
  
  ```
  apt-get upgrade
  apt-get install wget
  ```
  
  * Download Kafka.
  ```
  wget https://archive.apache.org/dist/kafka/2.8.0/kafka_2.12-2.8.0.tgz
  
  ```
  * Extract Kafka
  ```
  tar -xzf kafka_2.12-2.8.0.tgz
  ```
  * Install the python module kafka-python using the pip command.
  ```
  python3 -m pip install kafka-python
  
  ```
  
  * Install the python module mysql-connector-python using the pip command.
  
  ```
  python3 -m pip install mysql-connector-python==8.0.31
  ```
  
  ### step3: mysql container
  1. Pull the MySQL image from Docker Hub with the following command:
  ```
  docker pull mysql
  ```
  2. Start the container and map a host port to the container's internal port with the following command:
  ```
  docker run --name mysqlcontainer -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -d mysql
  ```
  3. Connect to the MySQL container using the following command:
  ```
  docker exec -it mysqlcontainer mysql -u root -p
  
  ```
  
  ## Start Kafka
 open  the bash shell in Docker container 
 ```
 docker exec -it kafka bash
 ```
 
  ### Start Zookeeper
  Start zookeeper server by running the following command:
  
  ``` 
  cd kafka_2.12-2.8.0
  bin/zookeeper-server-start.sh config/zookeeper.properties
  ```
   ### Start Kafka server
  Start Kafka server by running the following command:
  open a new window in terminal and open bash shell in docker container
  then start the kafka server
  
  ``` 
  cd kafka_2.12-2.8.0
  bin/kafka-server-start.sh config/server.properties
  ```
  
  ### Create a topic
  open a new window and go to bash shell container
 Create a Kakfa topic named toll
  
  ``` 
  cd kafka_2.12-2.8.0
  bin/kafka-topics.sh  --create --topic toll --bootstrap-server localhost:9092
  
  ```
  ## RUN
  put the scripts in kafka container and run each one in a new window
  ```
  python3 name-file.py
  ```
  ![image](https://user-images.githubusercontent.com/89319105/217097703-4cb50392-c53e-4dcb-882b-02350f2c107d.png)
![image](https://user-images.githubusercontent.com/89319105/217097811-c36a6b8c-dc8d-490c-8a69-7013eb54ffa6.png)

  
  ## sheck results
  go to mysql container
  ```
  SELECT * FROM livetolldata;
  
  ```
  ![image](https://user-images.githubusercontent.com/89319105/217098464-fbf7a92e-6fef-4c05-bfad-3d8e4cd754d9.png)

  
  
  
  
 
