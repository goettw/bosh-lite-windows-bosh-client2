# Install on windows with bosh-cli V2

```
vagrant up
vagrant ssh bosh-cli2


cd ~/workspace/cf-deployment
export STEMCELL_VERSION=$(bosh int ~/workspace/cf-deployment/cf-deployment.yml --path /stemcells/alias=default/version)
bosh upload-stemcell https://bosh.io/d/stemcells/bosh-warden-boshlite-ubuntu-trusty-go_agent?v=$STEMCELL_VERSION
bosh update-cloud-config ~/workspace/cf-deployment/bosh-lite/cloud-config.yml
bosh -d cf deploy ~/workspace/cf-deployment/cf-deployment.yml -o ~/workspace/cf-deployment/operations/bosh-lite.yml --vars-store ~/deployments/vbox/deployment-vars.yml -v system_domain=bosh-lite.com
```
to retrieve the admin password, type
```
bosh interpolate ~/deployments/vbox/deployment-vars.yml --path /cf_admin_password
