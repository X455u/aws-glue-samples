## Spark UI

### Launching the Spark History Server and Viewing the Spark UI Using Docker

If you prefer local access (not to have EC2 instance for Apache Spark history server), you can also use a Docker to start the Apache Spark history server and view the Spark UI locally. This Dockerfile is a sample that you should modify to meet your requirements.

#### Pre-requisite
- Install Docker

#### Build docker image
1. Download a Dockerfile and pom file from GitHub
2. Run commands shown below
    ```
        $ docker build -t glue/sparkui:latest .
    ```


#### Start the Spark history server
1.  Configure AWS CLI so that you have access to your AWS account if you have not already done so
2.  Run commands shown below
    - Set **LOG_DIR** by replacing **s3a://path_to_eventlog** with your event log directory
    ```
        $ LOG_DIR="s3a://path_to_eventlog/"
        $ docker run -itd -v ~/.aws:/root/.aws:ro -e AWS_PROFILE -e AWS_DEFAULT_REGION -e SPARK_HISTORY_OPTS="$SPARK_HISTORY_OPTS -Dspark.history.fs.logDirectory=$LOG_DIR -Dspark.hadoop.fs.s3a.aws.credentials.provider=com.amazonaws.auth.DefaultAWSCredentialsProviderChain" -p 18080:18080 glue/sparkui:latest "/opt/spark/bin/spark-class org.apache.spark.deploy.history.HistoryServer"
    ```

#### View the Spark UI using Docker
1. Open http://localhost:18080 in your browser
