# wordpress-migration
# wordpress-migration
WordPress AWS Migration Project with RDS

Overview:

This project details the process of migrating a WordPress site from a local environment to AWS. The setup involves running WordPress in a Docker container on an EC2 instance, with the MySQL database hosted on Amazon RDS. This approach leverages AWS’s scalable infrastructure and managed database services.

Prerequisites

Before you begin, ensure you have the following:

    • AWS account with necessary permissions
    • AWS CLI configured on your local machine
    • Docker installed on your local machine and EC2 instance
    • MySQL database backup file (wordpress_backup.sql)
    • WordPress site files
    • GitHub repository for version control (optional)

x
Steps to Migrate:

Set Up AWS Environment

    1. Launch an EC2 Instance:
    • Choose an Amazon Linux 2 or Ubuntu Instance
    • Configure security groups to allow inbound traffic on ports 22 (SSH) and 80 (HTTP).
    
    2. Install Docker on EC2:
    • SSH into the EC2 instance.
    • Install Docker using the package manager.

    3. Create and RDS MySQL instance:
    • Launch an RDS MySQL instance in the same VPC as your EC2 instance.
    • Set up the database name, username, and password.
    • Configure security group to allow inbound traffic on port 3306 from the EC2 instance's IP address or the VPC CIDR block.

Prepare WordPress Docker Setup

    1. Create the Dockerfile for WordPress:
    • Use the official Wordpress Docker image as the base.
    2. Create a .env file:
    
    WORDPRESS_DB_HOST=your-rds-endpoint.amazonaws.com
    WORDPRESS_DB_USER=admin
    WORDPRESS_DB_PASSWORD=your_password
    WORDPRESS_DB_NAME=wordpress
    WORDPRESS_CONFIG_EXTRA="define('WP_HOME', 'http://your_ec2_public_ip'); define('WP_SITEURL', 'http://your_ec2_public_ip');"
    
    3. Build and Run Docker Containers:

    • Build the Wordpress Container
    • Run the Docker Container( docker run --env-file .env -d -p 80:80 --name wordpress-container wordpress:latest)

    4. Import MySQL Database to RDS:

    • Copy Backup file to EC2 instance - wordpress_backup.sql
    • Import the Database
    mysql -h your-rds-endpoint.amazonaws.com -u admin -p your_password wordpress < wordpress_backup.sql
    
    5. Verify the Setup

    • Access the Wordpress site by http://<public-ip>:80




