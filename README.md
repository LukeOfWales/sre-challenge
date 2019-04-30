This is my submission for the SRE proxy tech assessment posted by adrianhj.

I have chosen to deploy my static site displaying the words 'Hello SRE!' with Nginx, in a container, on an EC2 instance.
My configuration tool of choice is Ansible and AWS is go-to cloud platform.

I opted to use Prometheus, Node Exporter & Grafana as a monitoring stack and included 3 dashboards which begin to populate automatically - however; I apprecate that further tuning of metrics might be required to gain better coverage of the system.

### Commit summary:
- ansible.cfg: additional configuration option required is to disable host_key_checking to avoid issues authentication issues via ssh.
- example_env.sh: placeholder for AWS_ACCESS_KEY & AWS_SECRET_ACCESS_KEY environmental variables. 
- example_variables.yaml: placeholder variables required by the playbook to run. 
- run.sh: The preferred way of initialising the playbook.
- variables.yaml: preferred/opinionated variables for the playbook. This playbook has been tested on Ubuntu 18.04.


### Pre-requisites
Ideally this playbook would be run on from a machine running in AWS with either an IAM role attached (with full administrative permissions) or with AWS_ACCESS_KEY & AWS_SECRET_ACCESS_KEY environmental variables in place.

### Kick-off

When you're ready either execute run.sh or enter the following into your terminal:

``` ansible-playbook playbook.yaml -b --extra-vars @example_variables.yaml```

The playbook should take about 5 minutes to run.

In this time, a new VPC and all the goodies inside will be provisioned and configured to enable internet access to a single EC2 node. 

Once the instance is alive and ready for action - Ansible will install Docker & its dependencies before generating self signed certificates and starting the Nginx container - mounting in our static site.

Once the Nginx container is alive, we perform 3 tests to confirm that:
  1. A http request to the site is provided with a 301.
  2. A http request can follow the redirect to the https version of the site.
  3. A https request recieves the correct response.
  
If you are watching the output, debug notes providing curl commands are output during the testing phase which assist with additional manual testing.
  
After that, things become a bit more relaxed. We use the [CloudAlchemy](https://github.com/cloudalchemy) Grafana, Node Exporter & Prometheus roles to quickly deploy our monitoring stack.

Once complete, you should be able to navigate to the Public IP of the recently created webserver (on port 3000) to view the Grafana UI. 
Username is admin and password is admin. You'll be prompted to change this upon login.
  
  
Thank you for taking the time to consider my application.
Luke
