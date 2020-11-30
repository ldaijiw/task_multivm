# Multi Machine Vagrant

## Timings

120 - 180 Minutes

## Summary

The sample application has the ability to connect to a database. We need to provision our development environment with a vm for the database and one for the database.

Vagrant is capable of running two or more virtual machines at once with different configurations.

## Tasks

* Research how to create a multi machine vagrant environment
* Add a second virtual machine called "db" to your Vagrant file
* Configure the db machine with a different IP from the app
* Provision the db machine with a MongoDB database


## Notes

When you have the second machine running further configuration of the app is required to make it use the database. We will cover this in the next lesson.

You can test your database is working correctly by running the test suite in the test folder. There are two sets of tests. One for the app VM and one for the db VM. Make them all pass.

```
cd test
rake spec
```


## Acceptance Criteria

* Uses vagrant file
* Create 2 VM APP and Mongod
* Localhost set to development.local

* App works on Port 3000 (development.local:3000)

* Work with only command "vagrant up" &/or mininum commands
* All tests passing
* works on /posts
* works on /fibonacci/30

* Documentation exists as README.md file
* Documentation includes: Intro (purpose of repo), Pre Requisits  and Intructions
* Instructions have a clear step by step

* repo exists on github


## Instructions

[For more on setting up and running vagrant](https://github.com/ldaijiw/task_dev_env_app)


The original configuration of the app is set to run on port 3000. However, browsers use port 80 by default when loading web applications

Instead of running the app on port 80 (this would involve giving the app more privileges which could be potentially dangerous), set up a reverse proxy using nginx

### Reverse Proxy

**A proxy server is a server application or appliance that acts as an intermediary for requests from clients sekking resources from servers that provide those resources**
- Proxy server functions on behalf of the client when requesting a service, potentially masking the true origin of the request to the server
- A reverse proxy is a type of proxy server that retrieves resources on behalf of a client from one or more servers
	- The resources are then returned to the client, appearing as if they originated from the reverse proxy server itself
- Instead of a _forward proxy_ which is an intermediary for its associated clients to contact any server, a _reverse proxy_ is an intermediary for its associated servers to be contacted by any client

**A proxy is associated with the client, a reverse proxy is associated with the server**

### Nginx Configuration

The nginx configuration to configure the port to 80 is as follows

``proxy_conf.conf``
```
server {
	listen 80;
	location / {
	proxy_pass https://192.168.10.100:3000;
	}
}
```

With the following added instructions in the ``provision.sh`` file to relink the new configuration

```bash
# unlink default config file
sudo unlink /etc/nginx/sites-enabled/default

# move custom config file to correct dir
sudo cp /home/ubuntu/nginx_config/proxy_config.conf /etc/nginx/sites-available/proxy_config.conf

# link the new proxy 
sudo ln -s /etc/nginx/sites-available/proxy_config.conf /etc/nginx/sites-enabled/proxy_config.conf

sudo systemctl restart nginx.service
```
