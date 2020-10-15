# Removing Chef Infra Server and Workflow from the AIB

## Creating a reduced AIB without Infra Server and Workflow
1. In the Automate git source, remove these rows from `products.meta`:

- chef/automate-cs-bookshelf
- chef/automate-cs-oc-bifrost
- chef/automate-cs-oc-erchef
- chef/automate-cs-nginx
- chef/automate-workflow-server
- chef/automate-workflow-nginx

2. Create a reduced manifest.json by running the `create-manifest.rb` script:
```
.expeditor/create-manifest.rb
```
3. Download the chef-automate installer: 
```
curl https://packages.chef.io/files/current/latest/chef-automate-cli/chef-automate_linux_amd64.zip | gunzip - > chef-automate && chmod +x chef-automate
```
4. Create an Airgap Installation Bundle (AIB) based on the new manifest.json:
```
./chef-automate airgap bundle create -d -m small.manifest.json
```
5. Copy the resulting `automate-*.aib` file to the server

## Installing Automate using the AIB
1. Download the chef-automate installer: 
```
curl https://packages.chef.io/files/current/latest/chef-automate-cli/chef-automate_linux_amd64.zip | gunzip - > chef-automate && chmod +x chef-automate
```
2. Create a default Automate configuration, which by default does not include the Infra Server or Workflow
```
sudo ./chef-automate init-config
```
3. Install Automate using the AIB:
```
sudo ./chef-automate deploy config.toml --airgap-bundle automate-*.aib
```
