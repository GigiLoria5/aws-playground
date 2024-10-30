# Setup Web Server with User Data

This example demonstrates how to set up a basic web server on an EC2 instance using a shell script (`user-data-web-server.sh`). The script accesses the instance's metadata properties from within a running instance and displays its availability zone. Check this [user guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html#instancedata-inside-access) for more information.

## Step 1: Launch a New EC2 Instance

- Go to EC2 and launch a new instance.
- Setup everything with the default configurations (free-tier options).
- Under Network settings tick the `Allow HTTP traffic from the internet` option (or manually create a security group with free inbound access to **port 80**).
- In the Advanced details section, locate the User data field. Copy and paste the content of `user-data-web-server.sh` into it. This will automatically run the script on instance launch and set up the web server.
- Complete the instance configuration steps.

## Step 2: Access the Web Server

- After launching, navigate to the Instances page and wait for the new instance to reach the running state.
- Copy the Public IPv4 address of your instance.
- Open a browser and go to `http://<instance-public-ip>`. You should see a default webpage indicating your instance availibility zone.
