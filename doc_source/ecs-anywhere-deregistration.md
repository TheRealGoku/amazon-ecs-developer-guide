# Deregistering an external instance<a name="ecs-anywhere-deregistration"></a>

When you are finished using an external instance, you should deregister the instance from both Amazon ECS and AWS Systems Manager\. Following deregistration, the external instance is no longer able to accept new tasks\.

If you have tasks running on the container instance when you deregister it, these tasks remain running until they stop through some other means\. However, these tasks are orphaned which means they are no longer monitored or accounted for by Amazon ECS\. If an orphaned task on your external instance is part of an Amazon ECS service, then the service scheduler starts another copy of that task, on a different instance, if possible\.

To register an external instance to a new cluster, after the external instance has been deregistered from both Amazon ECS and Systems Manager you can clean up the remaining AWS resources on the instance and register it with a new cluster\.

**To deregister an external instance \(AWS Management Console\)**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, choose the Region in which your external instance is registered\.

1. In the navigation pane, choose **Clusters** and select the cluster that hosts the external instance\.

1. On the **Cluster : *name*** page, choose the **ECS Instances** tab\.  
![\[ECS Instances tab\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/ECS_Instances_tab.png)

1. Select the external instance ID to deregister\. This takes you to the container instance detail page\.

1. On the **Container Instance : *id*** page, choose **Deregister**\.

1. Review the deregistration message\. Select **Deregister from AWS Systems Manager** to also deregister the external instance as an Systems Manager managed instance\. Choose **Deregister**\.
**Note**  
To deregister the external instance as an Systems Manager managed instance using the Systems Manager console, see [Deregistering managed instances](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managed-instances-advanced-deregister.html) in the *AWS Systems Manager User Guide*\.

1. If you want to clean up the remaining AWS resources from the External instance, follow the steps below\. The cleanup steps must be completed prior to registering the external instance with a new cluster\.

**To cleanup AWS resources on your on\-premise server or VM**

1. Ensure that the external instance is deregistered from both Amazon ECS and Systems Manager\.

1. Stop the Amazon ECS container agent and the SSM Agent services on the instance\.

   ```
   sudo systemctl stop ecs amazon-ssm-agent
   ```

1. Remove the Amazon ECS and Systems Manager packages\.

   **For CentOS 7, CentOS 8, and RHEL 7**

   ```
   sudo yum remove -y amazon-ecs-init amazon-ssm-agent
   ```

   **For SUSE Enterprise Server 15**

   ```
   sudo zypper remove -y amazon-ecs-init amazon-ssm-agent
   ```

   **For Debian and Ubuntu**

   ```
   sudo apt remove -y amazon-ecs-init amazon-ssm-agent
   ```

1. Remove the Amazon ECS and Systems Manager files\.

   ```
   sudo rm -rf /var/lib/ecs /etc/ecs /var/lib/amazon/ssm /var/log/ecs /var/log/amazon/ssm
   ```