<a href="https://lacework.com"><img src="https://techally-content.s3-us-west-1.amazonaws.com/public-content/lacework_logo_full.png" width="600" title="Lacework" alt="Lacework" /></a>

# Lacework Agent Releases

This Lacework agent GitHub site provides the officially supported Lacework agent releases. From this site, you can download and install Lacework agent releases. 

# Lacework Agent Documentation

For the Lacework Agent Documentation such as information about supported operating systems, how to configure the agent, agent features, Workload dossiers, AWS Fargate, and more, see [Lacework for Workload Security](https://support.lacework.com/hc/en-us/categories/360001044834-Lacework-for-Workload-Security).

# Releases

In the right frame, click **Releases** to view all the available agent releases.
Under the **Assets**, all the release download files are listed. These download files contain the agent installers. In addition, each release contains a direct link to the specific agent release notes and the `docker pull` command for pulling down a Lacework docker install image. 
For all the agent release notes, see [Release Updates](https://support.lacework.com/hc/en-us/categories/360000539793-Release-Updates).

# Install Agents

This README.md file provides installation instructions for installing the agent from this Lacework Agent GitHub repository.
You can also install the agent from the Lacework Console. For a listing of the different agent install methods, see [Agent Install Options](https://support.lacework.com/hc/en-us/sections/360002375733).

## Install Prerequisites

Complete the following prerequisites steps before starting any of the agent install procedures below.

1. Verify that the Lacework agent supports the distribution installed your machine. For more information, see [Supported Operating Systems](https://support.lacework.com/hc/en-us/articles/360005230014).
2. Use sed (GNU sed) version 4.2.2 or higher in the procedures below. 
2. Download the release package `release.tgz`  and the `checksum_sha256.txt` files from this Lacework GitHub Agent Release repository, where `release` is replaced by the agent release number.
    1. In the right frame, click **Releases** to view all the available agent releases.  
    2. Find a release and click `release.tgz` to download the file that contains the agent installers, where `release` is replaced by the agent release number.
    3. For the same release, click the `checksum_sha256.txt` file, and the `checksum_sha256.txt.asc` signature file to download them. 
    4. Create a temporary directory such as `~/lacework` and move the `release.tgz`, `checksum_sha256.txt`, and `checksum_sha256.txt.asc` files to that directory. 
3. Verify that the checksum in the `checksum_sha256.txt` file matches the checksum of the  `release.tgz` file.
    1. Open a terminal window go to the `~/lacework` directory.

       ```sh
       $ cd ~/lacework
       ```

    2. Verify that the `release.tgz` matches the checksum.

       ```sh
       $ shasum -c checksum_sha256.txt
       ```

       If the verification is successful, an `OK` is reported.
4. Verify that the checksum is signed correctly.
    1. Download the Lacework Agent GPG key.

       ```sh
       $ gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 360D55D76727556814078E25FF3E1D4DEE0CC692
       ```

    2. Verify the signature.

       ```sh
       $ gpg --verify checksum_sha256.txt.asc
       ```

       If the verification is successful you should see:

       ```
       gpg: assuming signed data in 'checksum_sha256.txt'
       gpg: Signature made <TIMESTAMP>
       gpg:                using RSA key 360D55D76727556814078E25FF3E1D4DEE0CC692
       gpg: Good signature from "Lacework Inc. <support@lacework.net>"
       ```

6. Unzip the `release.tgz` file into a temporary directory. 
7. Using the Lacework Console, create an agent access token  by following the instructions in [Create Agent Access Tokens](https://support.lacework.com/hc/en-us/articles/360036425594).
8. In the Lacework Console, copy the Access Token by clicking the **Copy to clipboard icon** icon.

## Install Using the `install.sh` Script

For single host installations, Lacework recommends using the installation script called `install.sh`
1. Complete the [prerequisites](#install-prerequisites) steps.
2. Set the `token` environment variable equal to the agent access token copied from the Lacework Console.

   ```sh
   $ export token=<copied_agent_access_token>
   ```

3. Use `sed` to replace `$1` with a valid agent access token in the `install.sh` file.

   ```sh
   $ sed -i.bak "s/ARG1=\$1/ARG1=${token}/g" ~/lacework/install.sh
   ```

4. Run the `install.sh` script to install the agent by following the instructions in [Run the Lacework Agent Installation Script](https://support.lacework.com/hc/en-us/articles/360005321273).

## Install using the Lacework Agent Chef Recipe

1. Complete the [prerequisites](#install-prerequisites) steps.
2. Set the `token` environment variable equal to the agent access token copied from the Lacework Console.

   ```sh
   $ export token=<copied_agent_access_token>
   ```

3. Use `sed` to replace `$1` with a valid agent access token in the `install.sh` file.

   ```sh
   $ sed -i.bak "s/ARG1=\$1/ARG1=${token}/g" ~/lacework/install.sh
   ```

4. Unzip `chef.tar.gz` into a temporary directory such as `~/lacework/chef`

   ```sh
   $ tar -xzf chef.tar.gz
   ```

5. Copy the `install.sh` file with the updated token to the appropriate directory.

   ```sh
   $ cp ~/lacework/install.sh ~/lacework/chef/datacollector/files/default
   ```

6. Install the agent by following the instructions in [Install with Chef](https://support.lacework.com/hc/en-us/articles/360005321413).

## Install using Daemonset Deployment files for Kubernetes

1. Complete the [prerequisites](#install-prerequisites) steps.
2. Set the `token` environment variable equal to the agent access token copied from the Lacework Console.

   ```sh
   $ export token=<copied_agent_access_token>
   ```

3. Use `sed` to replace `LaceworkAccessToken` with a valid agent access token in the `lacework-cfg-k8s.yaml` file.

   ```sh
   $ sed -i.bak "s/\${LaceworkAccessToken}/${token}/g" lacework-cfg-k8s.yaml
   ```

4. Install the agent on Kubernetes by following the instructions in [Deploy on Kubernetes](https://support.lacework.com/hc/en-us/articles/360005263034).
   On the **Releases** page of this site, find the appropriate ```docker pull``` command for a specific release under the **Lacework Agent Docker image** heading.

## Install using Docker Swarm Deployment Files

1. Complete the [prerequisites](#install-prerequisites) steps.
2. Set the `token` environment variable equal to the agent token copied from the Lacework Console.

   ```sh
   $ export token=<copied_agent_access_token>
   ```

3. Use `sed` to  replace `LaceworkAccessToken` with the valid agent access token in the `docker-compose.yml` and `docker-compose-v3.yml` files.

   ```sh
   $ sed -i.bak "s/\${LaceworkAccessToken}/${token}/g" docker-compose.yml
   $ sed -i.bak "s/\${LaceworkAccessToken}/${token}/g" docker-compose-v3.yml
   ```

4. Install the agent by following the instructions in [Install using Docker Swarm](https://support.lacework.com/hc/en-us/articles/360005321473).
   On the **Releases** page of this site, find the appropriate `docker pull` command for a specific release under the **Lacework Agent Docker image** heading.

## Install using Helm Chart

1. Complete the [prerequisites](#install-prerequisites) steps.
2. Set the `token` environment variable equal to the agent token copied from the Lacework Console.

   ```sh
   $ export token=<copied_agent_access_token>
   ```

3. Use `sed` to replace `accessToken` with a valid agent access token in the `values.yaml` file.

   ```sh
   $ sed -i "/accessToken:/s/$/${token}/" ~/lacework/helm/lacework-agent/values.yaml
   ```

4. Optional - Add custom tags to the helm chart.

   ```sh
   $ sed -i "/env:/s/$/${custom tag}/" ~/lacework/helm/lacework-agent/values.yaml
   ```

5. Install the agent by following the helm instructions in [Deploy on Kubernetes](https://support.lacework.com/hc/en-us/articles/360005263034-Deploy-on-Kubernetes).

