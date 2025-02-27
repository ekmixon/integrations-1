
<!--- Generated by to-integrations-repo script in Smart Agent repo, DO NOT MODIFY HERE --->
# Install Using Helm

:warning: **SignalFx Smart Agent is deprecated. For details, see the [Deprecation Notice](./smartagent-deprecation-notice.md)** :warning:

Use the Helm package manager to install the Smart Agent to Kubernetes
environments.

If you don't have Helm installed already, use kubectl
to install the Smart Agent. To learn more, see
[Install the Smart Agent using kubectl](agent-k8s-install-kubectl.md).

## Prerequisites

* Linux kernel version 3.2 or higher
* Terminal or a similar command-line interface application
* Helm:

  - The installation chart for the Smart Agent is compatible with Helm version 3 or higher. Helm version 2 is [deprecated](https://helm.sh/blog/helm-v2-deprecation-timeline/). To learn more, see the [Helm site](https://helm.sh/).

* Tiller installed on all of your Kubernetes hosts. Because of
  the way that the installation chart works, you need Tiller even if you're using
  Helm version 3.
* SignalFx realm for your organization. See [Realms](../../../_sidebars-and-includes/smart-agent-realm-note.html).
* A SignalFx access token. See [Smart Agent Access Token](../../../_sidebars-and-includes/smart-agent-access-token.html).


## Configure Helm for the Smart Agent

The SignalFx Smart Agent chart for Helm comes with a values.yaml configuration
file that contains useful default values. If you want to modify these values,
override them by creating your own values file. Follow these steps:

1. Download the default [values.yaml](https://github.com/signalfx/signalfx-agent/blob/main/deployments/k8s/helm/signalfx-agent/values.yaml) file.
2. Rename the file. For example, rename it to myValues.yaml.
3. Edit the file to update the values with your own choices, then save the file.

When you install the Smart Agent using Helm, add the `-f <values_yaml_file>` parameter to the install command.
For example, add the `-f myValues.yaml` parameter.

## Install with Helm

### Preliminary tasks

1. Remove collector services such as `collectd`.

2. Add the SignalFx Helm chart repository to Helm by entering the following command:

   ```
   helm repo add signalfx https://dl.signalfx.com/helm-repo
   ```

3. Check that the repository is up-to-date by entering the following command:

   ```
   helm repo update
   ```

### Get required values

Get the following required values:

| Name             | Example       | Meaning                                                      |
|:-----------------|:--------------|:-------------------------------------------------------------|
| `<access_token>` | 'abcd1234zzz' | Required. Access token. See [Prerequisites](#prerequisites). |
| `<cluster_name>` | 'myCluster'   | Required.Name of the cluster to monitor                      |
| `<realm>`        | 'us0'         | Required. Your realm. See [Prerequisites](#prerequisites).   |


### Get optional values

Determine if you want to use the following optional values:

| Name                 | Example         | Meaning                                                                                                                                           |
|:---------------------|:----------------|:--------------------------------------------------------------------------------------------------------------------------------------------------|
| `<values_yaml_file>` | `myValues.yaml` | Optional. Custom configuration file. If you don't specify this file, Helm installs the defaults. If you don't use this parameter, don't use `-f`. |
| `<version>`          | `5.1.6`           | Optional. Smart Agent version. See [Smart Agent releases](https://github.com/signalfx/signalfx-agent/releases) for a a list of versions. If you don't use this, Helm installs the latest release. If you don't use this, don't use `--set agentVersion=`. |


### Configure optional OpenShift support

Determine if you want OpenShift support:

   - If you want OpenShift support, substitute the values from the previous steps and run this command:

     ```
     helm install -f <values_yaml_file> --set signalFxAccessToken=<access_token> --set clusterName=<cluster_name> --set agentVersion=<version> --set signalFxRealm=<realm> --generate-name --set kubernetesDistro=openshift signalfx/signalfx-agent
     ```
     
   - If you don't want OpenShift support, substitute the values from the previous steps and run this command:

     ```
     helm install -f <values_yaml_file> --set signalFxAccessToken=<access_token> --set clusterName=<cluster_name> --set agentVersion=<version> --set signalFxRealm=<realm> --generate-name signalfx/signalfx-agent
     ```

**Note:** The ``--generate-name`` option is only supported for Helm version 3 or higher.

### Verify the SignalFx Smart Agent

The SignalFx Smart Agent runs as soon as you install it using Helm.

To verify that your installation and config is working:

* For infrastructure monitoring:
  1. In SignalFx UI, open the **Infrastructure** built-in dashboard.
  2. In the override bar, select **Choose a host**. Select one of your nodes from the dropdown.

  The charts display metrics from the infrastructure for that node.

  To learn more, see [Built-In Dashboards and Charts](https://docs.signalfx.com/en/latest/getting-started/built-in-content/built-in-dashboards.html).

* For Kubernetes monitoring:
  1. In SignalFx UI, from the main menu select **Infrastructure** > **Kubernetes Navigator** > **Cluster map**.
  2. In the cluster display, find the cluster `<cluster_name>` you chose in the previous steps.
  3. Click the magnification icon to view the nodes in the cluster.

  The detail pane on the right hand side of the page displays details of your cluster and nodes.

  To learn more, see [Getting Around the Kubernetes Navigator](https://docs.signalfx.com/en/latest/integrations/kubernetes/get-around-k8s-navigator.html).

* For APM monitoring, learn how to install, configure, and verify the Smart Agent for Microservices APM (**µAPM**). See
  [Get started with SignalFx µAPM](https://docs.signalfx.com/en/latest/apm/apm-getting-started/apm-index.html).

### Uninstall the Smart Agent

- To uninstall the Smart Agent Helm release, see [Helm Uninstall](https://helm.sh/docs/helm/helm_uninstall/).
  See [Helm List](https://helm.sh/docs/helm/helm_list/) to get the release name.

- To remove the SignalFx Helm repo, see [Helm Repo Remove](https://helm.sh/docs/helm/helm_repo_remove/).
