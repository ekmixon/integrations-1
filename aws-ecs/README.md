# ![](./img/integration_awsecs.png) Amazon EC2 Container Service (ECS)

- [Description](#description)
- [Installation](#installation)
- [Usage](#usage)
- [Metrics](#metrics)
- [Recommended Statistics](#recommended-statistics)
- [License](#license)

### DESCRIPTION

Use SignalFx to monitor Amazon EC2 Container Service (ECS) via [Amazon Web Services](https://github.com/signalfx/integrations/tree/master/aws)[](sfx_link:aws).

For greater insight into your ECS environment, SignalFx's Smart Agent can auto-discover services and provide more in-depth metrics about your containers that are running in ECS.  See the <a target="_blank" href="https://github.com/signalfx/signalfx-agent/tree/master/deployments/ecs">Smart Agent ECS Deployment Guide</a> for instructions on how to deploy the Smart Agent in ECS. Also, Smart Agent can be deployed in ECS task to monitor AWS Fargate containers. See the <a target="_blank" href="https://github.com/signalfx/signalfx-agent/tree/master/deployments/fargate">Smart Agent Fargate Deployment Guide</a> for more detailed deployment instructions.

### FEATURES

##### Built-in dashboards

- **ECS (AWS)**: Overview of all data from ECS via CloudWatch.

  [<img src='./img/dashboard_ecs_overview.png' width=200px>](./img/dashboard_ecs_overview.png)

- **ECS (SignalFx)**: Overview of all data from ECS via SignalFx SmartAgent.

  [<img src='./img/dashboard_ecs_agent_overview.png' width=200px>](./img/dashboard_ecs_agent_overview.png)

- **ECS (AWS) Cluster**: Focus on a single ECS cluster via CloudWatch.

  [<img src='./img/dashboard_ecs_cluster.png' width=200px>](./img/dashboard_ecs_cluster.png)

- **ECS (SignalFx) Cluster**: Focus on a single ECS cluster via SignalFx SmartAgent.

  [<img src='./img/dashboard_ecs_agent_cluster.png' width=200px>](./img/dashboard_ecs_agent_cluster.png)

- **ECS (AWS) Service**: Focus on a single ECS service via CloudWatch.

  [<img src='./img/dashboard_ecs_service.png' width=200px>](./img/dashboard_ecs_service.png)

- **ECS (SignalFx) Task Definition**: Focus on a single ECS Task Defiinition via SignalFx Smart Agent.

  [<img src='./img/dashboard_ecs_agent_taskdef.png' width=200px>](./img/dashboard_ecs_agent_taskdef.png)

### INSTALLATION

#### Step 1: Connect to CloudWatch

To access this integration, [connect to CloudWatch](https://github.com/signalfx/integrations/tree/master/aws)[](sfx_link:aws).

By default, SignalFx will import all CloudWatch metrics that are available in your account. To retrieve metrics for a subset of available services or regions, modify the connection on the Integrations page.

#### Step 2: Deploy the SignalFx Smart Agent

Based on the type of container to monitor, there are two ways to deploy the SignalFx Smart Agent to auto-discover services and collect additional detailed metrics.

- To monitor EC2 containers, see <a target="_blank" href="https://github.com/signalfx/signalfx-agent/tree/master/deployments/ecs">Smart Agent ECS Deployment Guide</a>.

- To monitor Fargate containers, see <a target="_blank" href="https://github.com/signalfx/signalfx-agent/tree/master/deployments/fargate">Smart Agent Fargate Deployment Guide</a>.

### USAGE

SignalFx provides built-in dashboards for this service. Examples are shown below.

![](./img/dashboard_ecs_overview.png)

![](./img/dashboard_ecs_agent_overview.png)

![](./img/dashboard_ecs_cluster.png)

![](./img/dashboard_ecs_agent_cluster.png)

![](./img/dashboard_ecs_service.png)

![](./img/dashboard_ecs_agent_taskdef.png)

#### METRICS

For more information about the metrics emitted by Amazon EC2 Container Service, visit the service's homepage at <a target="_blank" href="https://aws.amazon.com/ecs/">https://aws.amazon.com/ecs/</a>.

<!--- METRICS --->
### RECOMMENDED STATISTICS

The following is a subset of available metrics; these are recommended by Amazon for collection.

| Metric            | Recommended Statistics |
| ----------------- | ---------------------- |
| CPUReservation    | Average                |
| CPUUtilization    | Average                |
| MemoryReservation | Average                |
| MemoryUtilization | Average                |


#### LICENSE

This integration is released under the Apache 2.0 license. See [LICENSE](./LICENSE) for more details.
