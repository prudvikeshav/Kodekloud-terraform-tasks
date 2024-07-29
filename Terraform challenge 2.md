# **Welcome to the terraform challenge 2**

In this challenge we will implement a simple LAMP stack using terraform and docker.
    - Utilize /root/code/terraform-challenges/challenge2 directory to store your Terraform configuration files.

 1. Install terraform binary version=1.1.5 on iac-server

 2. Docker provider has already been configured using kreuzwerker/docker provider.
     - Check out the provider.tf given at /root/code/terraform-challenges/challenge2

 3. Create a terraform resource named php-httpd-image for building docker image with following specifications:
     - Image name: php-httpd:challenge

     - Build context: lamp_stack/php_httpd

     - Labels: challenge: second

4.Create a terraform resource named mariadb-image for building docker image with following specifications:
     - Image name: mariadb:challenge

     - Build context: lamp_stack/custom_db

     - Labels: challenge: second

5.Create a terraform resource named mariadb_volume creating a docker volume with name=mariadb-volume

6.Create a terraform resource named private_network and configure the following:
     - Create a Docker network with name=my_network

     - Enable manual container attachment to the network.

     - User-defined key/value metadata: challenge: second
