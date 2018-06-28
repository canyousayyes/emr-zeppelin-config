# AWS EMR with MySQL, PostgreSQL support and S3 Storage #

## Prerequisites ##

1. AWS account

1. [AWS CLI](https://aws.amazon.com/cli/)

1. Some knowledge about VPC, EC2 and CloudFormation.

---

## Usage Guide ##

1. Create an EMR cluster in AWS.

    A sample template is attached in this repo. It creates a cluster with most options pre-configured.

    1. Visit [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation).

    1. Click **Create Stack**.

    1. Choose **Upload a template to Amazon S3** and select the `cloudformation.yml` in this directory.

    1. Fill in the parameters and create the stack. Wait for it to complete.

1. SSH into master node

    1. Visit [AWS EMR Console](https://console.aws.amazon.com/elasticmapreduce) and click into the cluster just created.

    1. Click **SSH** right after **Master public DNS**.

    1. Use the command in the popup

    ![](./emr_ssh.png)

    If you see the ASCII art for "EMR", then congrats :)

    1. Otherwise, check to see if you have setup Master node's security group to allow access from your local.

    And that you have the correct key file to SSH into the instance.

1. Run the customization on EMR master node

    ```sh
    # Download the drivers for JDBC
    wget http://central.maven.org/maven2/org/postgresql/postgresql/42.2.2/postgresql-42.2.2.jar -O /home/hadoop/postgresql.jar
    wget http://central.maven.org/maven2/mysql/mysql-connector-java/8.0.11/mysql-connector-java-8.0.11.jar -O /home/hadoop/mysql.jar

    # Overwrite the config files in
    # 1. /etc/zeppelin/conf/interpreter.json
    # 2. /etc/zeppelin/conf/zeppelin-env.sh
    #
    # Using the file in this directory.
    # Remember to fill in the ZEPPELIN_NOTEBOOK_S3_BUCKET in zeppelin-env.sh

    # Restart Zeppelin to take effect
    sudo stop zeppelin
    sudo start zeppelin
    ```

1. Visit Zeppelin by http://<EMR_endpoint>:8890

1. Terminate the cluster when done to save cost :)

---

## Some Notes about Deploying to Production ##
