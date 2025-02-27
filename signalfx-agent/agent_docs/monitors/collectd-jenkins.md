
<!--- Generated by to-integrations-repo script in Smart Agent repo, DO NOT MODIFY HERE --->
<!--- GENERATED BY gomplate from scripts/docs/templates/monitor-page.md.tmpl --->

# collectd/jenkins

Monitor Type: `collectd/jenkins` ([Source](https://github.com/signalfx/signalfx-agent/tree/main/pkg/monitors/collectd/jenkins))

**Accepts Endpoints**: **Yes**

**Multiple Instances Allowed**: Yes

## Overview

Monitors jenkins by using the
[jenkins collectd Python
plugin](https://github.com/signalfx/collectd-jenkins), which collects
metrics from Jenkins instances by hitting these endpoints:
[../api/json](https://www.jenkins.io/doc/book/using/remote-access-api/)
(job metrics)  and
[metrics/&lt;MetricsKey&gt;/..](https://plugins.jenkins.io/metrics/)
(default and optional Codahale/Dropwizard JVM metrics).

Requires Jenkins 1.580.3 or later, as well as the Jenkins Metrics Plugin (see Setup).

<!--- SETUP --->
## Install Jenkins Metrics Plugin
This monitor requires the Metrics Plugin in Jenkins. Go to `Manage Jenkins -> Manage Plugins -> Available -> Search "Metrics Plugin"`
to find and install this plugin in your Jenkins UI.


<!--- SETUP --->
## Example Config

Sample YAML configuration:

```yaml
monitors:
- type: collectd/jenkins
  host: 127.0.0.1
  port: 8080
  metricsKey: reallylongmetricskey
```

Sample YAML configuration with specific enhanced metrics included

```yaml
monitors:
- type: collectd/jenkins
  host: 127.0.0.1
  port: 8080
  metricsKey: reallylongmetricskey
  includeMetrics:
  - "vm.daemon.count"
  - "vm.terminated.count"
```

Sample YAML configuration with all enhanced metrics included

```yaml
monitors:
- type: collectd/jenkins
  host: 127.0.0.1
  port: 8080
  metricsKey: reallylongmetricskey
  enhancedMetrics: true
```


## Configuration

To activate this monitor in the Smart Agent, add the following to your
agent config:

```
monitors:  # All monitor config goes under this key
 - type: collectd/jenkins
   ...  # Additional config
```

**For a list of monitor options that are common to all monitors, see [Common
Configuration](../monitor-config.html#common-configuration).**


| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `pythonBinary` | no | `string` | Path to a python binary that should be used to execute the Python code. If not set, a built-in runtime will be used.  Can include arguments to the binary as well. |
| `host` | **yes** | `string` |  |
| `port` | **yes** | `integer` |  |
| `path` | no | `string` |  |
| `metricsKey` | **yes** | `string` | Key required for collecting metrics.  The access key located at `Manage Jenkins > Configure System > Metrics > ADD.` If empty, click `Generate`. |
| `enhancedMetrics` | no | `bool` | Whether to enable enhanced metrics (**default:** `false`) |
| `excludeJobMetrics` | no | `bool` | Set to *true* to to exclude job metrics retrieved from `/api/json` endpoint (**default:** `false`) |
| `includeMetrics` | no | `list of strings` | Used to enable individual enhanced metrics when `enhancedMetrics` is false |
| `username` | no | `string` | User with security access to jenkins |
| `apiToken` | no | `string` | API Token of the user |
| `useHTTPS` | no | `bool` | Whether to enable HTTPS. (**default:** `false`) |
| `sslKeyFile` | no | `string` | Path to the keyfile |
| `sslCertificate` | no | `string` | Path to the certificate |
| `sslCACerts` | no | `string` | Path to the ca file |
| `skipVerify` | no | `bool` | Skip SSL certificate validation (**default:** `false`) |


## Metrics

These are the metrics available for this monitor.
This monitor emits all metrics by default; however, **none are categorized as
[container/host](https://docs.signalfx.com/en/latest/admin-guide/usage.html#about-custom-bundled-and-high-resolution-metrics)
-- they are all custom**.


 - ***`gauge.jenkins.job.duration`*** (*gauge*)<br>    Time taken to complete the job in ms.
 - ***`gauge.jenkins.node.executor.count.value`*** (*gauge*)<br>    Total Number of executors in an instance
 - ***`gauge.jenkins.node.executor.in-use.value`*** (*gauge*)<br>    Total number of executors being used in an instance
 - ***`gauge.jenkins.node.health-check.score`*** (*gauge*)<br>    Mean health score of an instance
 - ***`gauge.jenkins.node.health.disk.space`*** (*gauge*)<br>    Binary value of disk space health
 - ***`gauge.jenkins.node.health.plugins`*** (*gauge*)<br>    Boolean value indicating state of plugins
 - ***`gauge.jenkins.node.health.temporary.space`*** (*gauge*)<br>    Binary value of temporary space health
 - ***`gauge.jenkins.node.health.thread-deadlock`*** (*gauge*)<br>    Boolean value indicating a deadlock
 - ***`gauge.jenkins.node.online.status`*** (*gauge*)<br>    Boolean value of instance is reachable or not
 - ***`gauge.jenkins.node.queue.size.value`*** (*gauge*)<br>    Total number pending jobs in queue
 - ***`gauge.jenkins.node.slave.online.status`*** (*gauge*)<br>    Boolean value for slave is reachable or not
 - ***`gauge.jenkins.node.vm.memory.heap.usage`*** (*gauge*)<br>    Percent utilization of the heap memory
 - ***`gauge.jenkins.node.vm.memory.non-heap.used`*** (*gauge*)<br>    Total amount of non-heap memory used
 - ***`gauge.jenkins.node.vm.memory.total.used`*** (*gauge*)<br>    Total Memory used by instance
The agent does not do any built-in filtering of metrics coming out of this
monitor.


