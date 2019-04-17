# wordpress_HA
The end goal of the cloud formation scripts here to is to 
1) Wordpress_HA_infra - To build the underlying infratstructure required for the Wordpress application 
2) Wordpress_HA_site - To build a prodcution level HA wordpress or a test wordpress depending on the users choice.

The workflow is split into 2 part

Wordpress_HA_infra.yml
            * This would be the first script to run setup a AWS VPC environment required for the user. This include
            
            a) VPC- a complete isolate environment
            b) 2 AZ's within 2 subnets - for a site failover if required
            c) Internet gateway - access the website outside the environment.
            d) SG' and NACL - to restrict what access to be provided for the users.

Wordpress_HA_site.yml

          * This script would deploy 
                a) An ALB (application load balancer) - to balance the workload and forward HTTP packets coming in for Port 80.
                b) Autoscaling group - To increase of websites instances being server when the workload to the website increases.
                c) Create a Multi AZ's DB through AWS Aurora. As the number of AZ's increase, the RDB would scale in every AZ being created.
                c) Launch config - defintion of the wordpress instance that would be run which included
                            i) Setting up the HTTP / PHP servers.
                            ii) Installing wordpress
                            iii) Setting up connection parameters to the DB to be connected.
                            
      
     
     #Steps
     # Step 1 
     # Run Wordpress_HA_infra
     This is the frst script that needs to be run for the infratstucture. The user needs to specufy whether the user require a 'test' or a 'prod' environment.
     
     EnvType: 
      Description: Environment type.
      Default: test
      Type: String
      AllowedValues: 
        - prod
        - test
      ConstraintDescription: must specify prod or test.
      
      #Step 2
      #Run Wordpress_HA_site.yml
      This would create a scalable wordpress instances and connectin to the DB's through the bootstrap scripts. Following are the parameters that needs to be specified for the same.
      
      1) SSH key pair - the key that needs to used to login into the instances.
      2) Subnet 1 - The ID of subnet created in the previous infra script. This subnet would be used for both PROD & test environemnts.
      3) Subnet 2 - The ID of subnet 2, which only would be created if the 'PROD' environment is selected.
      4) DB params - Alpha numberic characters for a choice of DB name , userame and password.
     
     
