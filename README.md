# cna-demo
Easy to set up cloud-native architecture demo for PCF. Consists of two apps, greeting-ui and fortune-service. Can be used to demo all three services in Spring Cloud Services, container to container networking, and distributed tracing in PCF Metrics.

This demo comprises four repos:
* https://github.com/markjschreiber/cna-demo-setup.git - Set up instructions and deployment scripts
* https://github.com/markjschreiber/fortune-service.git - Backend service, returns fortunes
* https://github.com/markjschreiber/greeting-ui.git - Front-end service, displays fortunes
* https://github.com/markjschreiber/app-config - Config repo

# instructions

1. Clone the setup repo and the two project repos into a single local directory:
```
mkdir cna-demo
cd cna-demo
git clone https://github.com/markjschreiber/cna-demo-setup.git
git clone https://github.com/markjschreiber/fortune-service.git
git clone https://github.com/markjschreiber/greeting-ui.git
```

2. Use the cf CLI to target the space to which you wish to deploy.

3. From the cna-demo-setup directory, run the setup script:
```
cd cna-demo-setup
. ./00_setup.sh
```

Answer the prompts from the script:

* Provide the git address to back the Config Server
    * The default value is https://github.com/markjschreiber/app-config. If you wish to set or change config values, fork this repo and enter the new value.
* Choose whether or not to leverage container-to-container networking
    * If 'Y', make sure you have the cf CLI 'network-policy-plugin' installed (see https://docs.pivotal.io/pivotalcf/1-11/devguide/deploy-apps/cf-networking.html).
    * Note that at this time, you must set C2C=N in order to demo distributed tracing in PCF Metrics.
* Choose whether or not to build the apps
    * If 'Y', the script will run "mvn clean install" for both fortune-service and greeting-ui.

The script will create the necessary services and deploy the two applications to the targeted space. It will also set the TRUST_CERTS environment variable and, optionally, enable access between the apps for C2C networking.

4. [Optional] To delete the apps, routes, and services created by the setup script, run the cleanup script. Answer the prompts to control if just apps and routes are deleted, or services as well.
```
. ./01_cleanup.sh
```
