## Benefits of MWAA

[MWAA Architecture](/assets/architecture.png)

<ol>
<li>1. Managed Service</li>

Besides the autoscaling of worker node capacity, one of the most considerable advantages of MWAA is the fact that it’s a managed service. You don’t need to monitor webserver, worker nodes, and scheduler logs to ensure that all components within your environment are working — AWS is responsible for keeping your environment up and running at all times. The same is true for security patches and upgrades to new Airflow versions.

<li>2. Integration with AWS</li>

The integration with other AWS services makes it easier to manage communication between Airflow and other services running within your VPC. For instance, instead of maintaining and manually rotating credentials, you can now leverage IAM roles for more robust management of permissions.

<li>3. Convenient Configuration</li>

Another useful feature is a centralized storage of configuration options. Airflow is configured to a large extent by setting variables in the airflow.cfg file. With MWAA, you can manage those settings directly from the management console. Normally, after having changed your configuration variables, you would have to restart your scheduler and worker nodes to apply the changes. With MWAA, it’s performed under the hood without bringing down your Airflow environment.

[mwaa_configuration](/assets/mwaa_config.png)

<li>4. Extensibility via Plugins</li> 

The functionality of MWAA environments can be extended by using plugins — you simply need to upload plugins.zip to your S3 bucket to make custom operators, hooks, and sensors available to all your DAGs.

```bash

__init__.py
    |-- airflow_plugin.py
hooks/
    |-- __init__.py
    |-- airflow_hook.py
operators/
    |-- __init__.py
    |-- airflow_operator.py
sensors/
    |-- __init__.py
    |-- airflow_sensor.py

```

<li>5. Convenient Logging</li>

One of my favorite aspects of MWAA is how easy it is to configure logging to CloudWatch. Setting this up yourself would require a lot of work to configure a CloudWatch Agent on all instances to stream the logs to CloudWatch, and ensure proper log groups for all components (scheduler logs, worker nodes logs, web server logs, and the actual logs from your tasks).

<li>6. Governance Out of the Box</li>

My second favorite feature is the integration of MWAA with IAM: only authorized IAM users can log into your Airflow UI. This is enabled by default without having to implement any custom Auth logic. When I open the link to the Airflow UI within a private browser window, we are not able to access the UI until we sign in with a user who has access to the AWS management console. From the security perspective, I would definitely recommend using MWAA rather than deploying Airfow yourself with EC2. AWS ensured that any data processed within MWAA is by default encrypted with KMS, and you get the Single-Sign-On that works out of the box. Similarly, you don’t need to worry about Security Groups, and a domain name is automatically assigned to your environment.

<li>7. CI/CD pipelines can be easily established</li>

From the DevOps perspective, MWAA is attractive since establishing a CI/CD pipeline for your DAGs is as easy as making sure that any push to the master branch triggers upload of the code to <code>s3://airflow-BUCKET/dags</code>.

<li>8. Support for containerized workflows & consolidated pricing</li>

Lastly, AWS also promises support for containerized workloads with AWS Fargate and enables easy consolidated pricing for all your Airflow components by using tags assigned to your MWAA, S3, and CloudWatch resources.
</ol>