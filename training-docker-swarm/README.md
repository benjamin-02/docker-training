# Task

Create a Docker Stack with services consisting of wordpress:latest and mysq1:5.7 official images. To deploy this stack, create a docker-compose.yml file and deploy it. Then verify that wordpress is working properly, i.e. that you deployed it properly. There is only one rule. When deploying wordpress and mysql services, you need to specify a mysql password and this can be assigned to the service as an enviroment variable. But don't specify it directly in the compose file, instead define it as a secret and assign it that way. Also create this secret in the same docker-compose.yml file. 

- Create a stack with services using wordpress:latest and mysq1:5.7 
- Set mysql password (as secret) when creating container from mysql and wordpress images. 
- Create this secret in the same docker-compose.yml