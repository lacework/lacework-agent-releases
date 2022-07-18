<a href="https://lacework.com"><img src="https://techally-content.s3-us-west-1.amazonaws.com/public-content/lacework_logo_full.png" width="600" title="Lacework" alt="Lacework" /></a>

# Lacework Linux Agent Releases

This Lacework GitHub site provides the officially supported Lacework Linux agent releases. From this site, you can download and install the Lacework Linux agent. 

# Lacework Linux Agent Documentation

For information about supported Linux operating systems, how to configure the agent, agent features, workload dossiers, AWS Fargate, and more, see the [Lacework Linux Agent Documentation](https://docs.lacework.com/onboarding/supported-operating-systems).

# Releases

In the right frame, click **Releases** to view all the available agent releases.
The **Assets** section for each release lists all the release download files. These files contain the agent installers. In addition, each release contains a link to the specific agent release notes and the `docker pull` command for pulling down a Lacework docker install image. 
To view the release notes for all Linux agent releases, see [Linux Agent Release Notes](https://docs.lacework.com/releases/category/linux-agent-releases).

# Install Agents

This README.md file provides instructions for installing the agent from this GitHub repository.
You can also install the agent from the Lacework Console. For information about the different installation methods, see [Linux Agent Installation Methods](https://docs.lacework.com/onboarding/agent-installation-prerequisites).

## Installation Prerequisites

Complete the following steps before you install the agent:

1. Ensure that the Lacework Linux agent supports the distribution installed on your machine. For more information, see [Supported Operating Systems](https://support.lacework.com/hc/en-us/articles/360005230014).
2. Use sed (GNU sed) version 4.2.2 or higher in the procedures below. 
2. Download the release package `release.tgz` (where `release` is the agent release number) and the `checksum_sha256.txt` files from this GitHub repository. 
    1. In the right frame, click **Releases** to view all the available agent releases.  
    2. Find a release and click `release.tgz` to download the file that contains the agent installers, where `release` is the agent release number.
    3. For the same release, click the `checksum_sha256.txt` file, and the `checksum_sha256.txt.asc` signature file to download them. 
    4. Create a temporary directory such as `~/lacework` and move the `release.tgz`, `checksum_sha256.txt`, and `checksum_sha256.txt.asc` files to that directory. 
3. Verify that the checksum in the `checksum_sha256.txt` file matches the checksum of the `release.tgz` file.
    1. In a terminal window, go to the `~/lacework` directory.

       ```sh
       $ cd ~/lacework
       ```

    2. Verify that the `release.tgz` matches the checksum.

       ```sh
       $ shasum -c checksum_sha256.txt
       ```

       If the verification is successful, an `OK` is reported.
4. Verify that the checksum is signed correctly.
    1. Download the Lacework agent GPG key.

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
7. Using the Lacework Console, create an agent access token by following the instructions in [Create Agent Access Tokens](https://docs.lacework.com/onboarding/create-agent-access-tokens-and-download-agent-installers).
8. In the Lacework Console, click the **...** icon in the row for the token and select **Copy** to copy the access token.

## Install Using the `install.sh` Script

For single host installations, Lacework recommends using the `install.sh` installation script.
1. Complete the [prerequisites](#installation-prerequisites) steps.
2. Set the `token` environment variable equal to the agent access token you copied from the Lacework Console.

   ```sh
   $ export token=<copied_agent_access_token>
   ```

3. Use `sed` to replace `$1` with a valid agent access token in the `install.sh` file.

   ```sh
   $ sed -i.bak "s/ARG1=\$1/ARG1=${token}/g" ~/lacework/install.sh
   ```

4. Run the `install.sh` script to install the agent by following the instructions in [Run the Lacework Agent Installation Script](https://docs.lacework.com/onboarding/use-the-lacework-installation-script-installsh).

## Install Using the Lacework Agent Chef Recipe

1. Complete the [prerequisites](#installation-prerequisites) steps.
2. Set the `token` environment variable equal to the agent access token you copied from the Lacework Console.

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

6. Install the agent by following the instructions in [Install with Chef](https://docs.lacework.com/onboarding/install-with-chef).

## Install Using Daemonset Deployment Files for Kubernetes

1. Complete the [prerequisites](#installation-prerequisites) steps.
2. Set the `token` environment variable equal to the agent access token you copied from the Lacework Console.

   ```sh
   $ export token=<copied_agent_access_token>
   ```

3. Use `sed` to replace `LaceworkAccessToken` with a valid agent access token in the `lacework-cfg-k8s.yaml` file.

   ```sh
   $ sed -i.bak "s/\${LaceworkAccessToken}/${token}/g" lacework-cfg-k8s.yaml
   ```

4. Install the agent on Kubernetes by following the instructions in [Deploy on Kubernetes](https://docs.lacework.com/onboarding/deploy-on-kubernetes).
   On the **Releases** page of this site, find the appropriate ```docker pull``` command in the **Lacework Agent Docker Images** section for a specific release.

## Install Using Docker Swarm Deployment Files

1. Complete the [prerequisites](#installation-prerequisites) steps.
2. Set the `token` environment variable equal to the agent token you copied from the Lacework Console.

   ```sh
   $ export token=<copied_agent_access_token>
   ```

3. Use `sed` to  replace `LaceworkAccessToken` with the valid agent access token in the `docker-compose.yml` and `docker-compose-v3.yml` files.

   ```sh
   $ sed -i.bak "s/\${LaceworkAccessToken}/${token}/g" docker-compose.yml
   $ sed -i.bak "s/\${LaceworkAccessToken}/${token}/g" docker-compose-v3.yml
   ```

4. Install the agent by following the instructions in [Install using Docker Swarm](https://docs.lacework.com/onboarding/install-using-docker-compose).
   On the **Releases** page of this site, find the appropriate ```docker pull``` command in the **Lacework Agent Docker Images** section for a specific release.

## Install Using Helm Chart

1. Complete the [prerequisites](#installation-prerequisites) steps.
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

5. Install the agent by following the helm instructions in [Deploy on Kubernetes](https://docs.lacework.com/onboarding/deploy-on-kubernetes).

