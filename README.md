# Install cloudfoundry on windows using virtualbox and bosh-cli2

This is for windows users who want to use bosh-cli2 to install cloudfoundry on virtualbox. 


The setup consists of 2 VMs
- cf: initially a pre-defined vagrantbox, running bosh-director, configured to simulate VMs with warden-containers. After installation has been completed, this VM will also contain cloudfoundry.  
- bosh-cli2: a linux box with bosh-cli v2 [bosh-cli2-v2](https://bosh.io/docs/cli-v2)


Both VMs will be created, running:
```
vagrant up
```

*if you haven't installed vagrant and virtualbox yet, you should do it now*

For the rest of the installation, you have to enter the bosh-cli2 VM. I use git bash, and run the following command: 

```
vagrant ssh bosh-cli2
```
Once you're in the client machine, type the following commands:
```
cd ~/workspace/cf-deployment
export STEMCELL_VERSION=$(bosh int ~/workspace/cf-deployment/cf-deployment.yml --path /stemcells/alias=default/version)
bosh upload-stemcell https://bosh.io/d/stemcells/bosh-warden-boshlite-ubuntu-trusty-go_agent?v=$STEMCELL_VERSION
bosh update-cloud-config ~/workspace/cf-deployment/bosh-lite/cloud-config.yml
bosh -d cf deploy ~/workspace/cf-deployment/cf-deployment.yml -o ~/workspace/cf-deployment/operations/bosh-lite.yml --vars-store ~/deployments/vbox/deployment-vars.yml -v system_domain=bosh-lite.com
```
to retrieve the admin password, type
```
bosh interpolate ~/deployments/vbox/deployment-vars.yml --path /cf_admin_password
