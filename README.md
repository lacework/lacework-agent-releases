<a href="https://www.lacework.com/"><img src="https://www.lacework.com/wp-content/uploads/2019/07/Lacework_Logo_color_2019.svg" width="250px" height="100px" title="Lacework" alt="Lacework"></a>

# Lacework Agent Releases
This Lacework agent GitHub site provides the officially supported Lacework agent releases. This site provides both the Lacework agent install and source files available for downloading.

# Releases
In the right frame, click **Releases** to view all the available agent releases.
Under the **Assets**, all the release download files are listed. These download files contain the agent installers and the source code. In addition, each release contains a direct link to the specific agent release notes.
For all the agent release notes, see [Release Updates](https://support.lacework.com/hc/en-us/categories/360000539793-Release-Updates).

# Install Agents
This README.md file provides installation instructions for installing the agent from this Lacework Agent GitHub repository.
You can also install the agent from the Lacework Console. For a listing of the different agent install methods, see [Agent Install Options](https://support.lacework.com/hc/en-us/sections/360002375733).

## Install Prerequisites
Complete the following prerequisites steps before starting any of the agent install procedures below.
1. Download the release package ```release.tgz``` file from this Lacework GitHub Agent Release repository, where ```release``` is replaced by the agent release number.
    1. In the right frame, click **Releases** to view all the available agent releases.  
    2. Find a release and click ```release.tgz``` to download the file that contains the agent installers, where ```release``` is replaced by the agent release number.
2. Unzip the ```release.tgz``` file into a temporary directory such as ```/tmp```.
3. Using the Lacework Console, create an agent access token  by following the instructions in [Create Agent Access Tokens](https://support.lacework.com/hc/en-us/articles/360036425594).
4. In the Lacework Console, copy the Access Token by clicking the **Copy to clipboard icon** icon.

## Install Using the install.sh Script
For single host installations, Lacework recommends using the installation script called ```install.sh```.
1. Complete the [prerequisites](#install-prerequisites) steps.
2. Set the ```token``` environment variable equal to the agent access token copied from the Lacework Console.
   ```
   $ export token=<copied_agent_access_token>
   ```
3. Use ```sed``` to replace ```$1``` with a valid agent access token in the ```install.sh``` file.
   ```
   $ sed -i.bak "s/ARG1=\$1/ARG1=${token}/g" /tmp/install.sh
   ```
4. Run the install.sh script to install the agent by following the instructions in [Run the Lacework Agent Installation Script](https://support.lacework.com/hc/en-us/articles/360005321273).

## Install using the Lacework Agent Chef Recipe
1. Complete the [prerequisites](#install-prerequisites) steps.
2. Set the ```token``` environment variable equal to the agent access token copied from the Lacework Console.
   ```
   $ export token=<copied_agent_access_token>
   ```
3. Use ```sed``` to replace ```$1``` with a valid agent access token in the ```install.sh``` file.
   ```
   $ sed -i.bak "s/ARG1=\$1/ARG1=${token}/g" /tmp/install.sh
   ```
4. Unzip ```chef.tar.gz``` into a temporary directory such as ```/tmp/chef```.
   ```
   $ tar -xzf chef.tar.gz
   ```
5. Copy the ```install.sh``` file with the updated token to the appropriate directory.
   ```
   $ cp install.sh /tmp/chef/datacollector/files/default
   ```
6. Install the agent by following the instructions in [Install with Chef](https://support.lacework.com/hc/en-us/articles/360005321413).

## Install using Daemonset Deployment files for Kubernetes
1. Complete the [prerequisites](#install-prerequisites) steps.
2. Set the ```token``` environment variable equal to the agent access token copied from the Lacework Console.
   ```
   $ export token=<copied_agent_access_token>
   ```
3. Use ```sed``` to replace ```LaceworkAccessToken``` with a valid agent access token in the ```lacework-cfg-k8s.yaml``` file.
   ```
   $ sed -i.bak "s/\${LaceworkAccessToken}/${token}/g" lacework-cfg-k8s.yaml
   ```
4. Install the agent on Kubernetes by following the instructions in [Deploy on Kubernetes](https://support.lacework.com/hc/en-us/articles/360005263034).

## Install using Docker Swarm Deployment Files
1. Complete the [prerequisites](#install-prerequisites) steps.
2. Set the ```token``` environment variable equal to the agent token copied from the Lacework Console.
   ```
   $ export token=<copied_agent_access_token>
   ```
3. Use ```sed``` to  replace ```LaceworkAccessToken``` with the valid agent access token in the ```docker-compose.yml``` and ```docker-compose-v3.yml``` files.
   ```
   $ sed -i.-i.bak "s/\${LaceworkAccessToken}/${token}/g" docker-compose.yml
   $ sed -i.-i.bak "s/\${LaceworkAccessToken}/${token}/g" docker-compose-v3.yml
   ```
4. Install the agent by following the instructions in [Install using Docker Swarm](https://support.lacework.com/hc/en-us/articles/360005321473).

<!-- The Helm install instructions are not yet available on Zendesk.
## Install using Helm Chart
1. Complete the [prerequisites](#install-prerequisites) steps.
2. Set the ```token``` environment variable equal to the agent token copied from the Lacework Console.
   ```
   $ export token=<copied_agent_access_token>
   ```
3. Use ```sed``` to replace ```accessToken``` with a valid agent access token in the ```values.yaml``` file.
   ```
   $ sed -i "/accessToken:/s/$/${token}/" /tmp/helm/lacework-agent/values.yaml
   ```
4. Optional - Add custom tags to the helm chart.
   ```
   $ sed -i "/env:/s/$/${custom tag}/" /tmp/helm/lacework-agent/values.yaml
   ```
5. Install the agent by following the instructions in [TODO](todo).

-->
# Lacework Agent Documentation
For information about supported operating systems, how to configure the agent, agent features, and Workload dossiers, see [Lacework for Workload Security](https://support.lacework.com/hc/en-us/categories/360001044834-Lacework-for-Workload-Security).
