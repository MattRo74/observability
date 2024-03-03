**Note:** For the screenshots, you can store all of your answer images in the `answer-img` directory.

## Verify the monitoring installation

*TODO:* run `kubectl` command to show the running pods and services for all components. Take a screenshot of the output and include it here to verify the installation

*kubectl get pods,svc -n monitoring:*

<img src="https://github.com/MattRo74/observability/blob/main/answer-img/kubect_get_pods_svs_monitoring.png">

*kubectl get pods,svc -n observability:*

<img src="https://github.com/MattRo74/observability/blob/main/answer-img/kubect_get_pods_svs_observability.png">

*kubectl get pods -A:*

<img src="https://github.com/MattRo74/observability/blob/main/answer-img/kubectl_get_pods_A.png">

*kubectl get svc -A:*
<img src="https://github.com/MattRo74/observability/blob/main/answer-img/kubectl_get_svc_A.png">


## Setup the Jaeger and Prometheus source
*TODO:* Expose Grafana to the internet and then setup Prometheus as a data source. Provide a screenshot of the home page after logging into Grafana.

<img src="https://github.com/MattRo74/observability/blob/main/answer-img/grafana_homepage.png">
	
## Create a Basic Dashboard
*TODO:* Create a dashboard in Grafana that shows Prometheus as a source. Take a screenshot and include it here.

kubectl --namespace monitoring port-forward svc/prometheus-grafana --address 0.0.0.0 3000:80

**Basic Dashboard - Grafana Mainpage**
<img src="https://github.com/MattRo74/observability/blob/main/answer-img/basic_dashboard.png">

**Basic Dashboard - Prometheus Datasource**
<img src="https://github.com/MattRo74/observability/blob/main/answer-img/prometheus_datasource.png">

**Basic Dashboard - Prometheus Datasource Settings**
<img src="https://github.com/MattRo74/observability/blob/main/answer-img/prometheus_datasource_settings.png">

**Basic Dashboard - Prometheus Datasource Explorer**
<img src="https://github.com/MattRo74/observability/blob/main/answer-img/prometheus_datasource_explore.png">


## Describe SLO/SLI
*TODO:* Describe, in your own words, what the SLIs are, based on an SLO of *monthly uptime* and *request response time*.

**Service Level Objectives (SLOs)** are measureable goals (set by your team). It is important, that this goals are defined from the customers perspective and also, a specific time period needs to be defined:

*monthly uptime*: The User expects an almonst 100 % reachability of the service.
-> The goal is set to a monthly uptime of 99.5 %.
*request response time*: The User expects a really short response time of the service. 
-> The goal is set to a request response time of 50 ms or less each month.

**Service Level Indicators (SLIs)** are metrics to measure the defined goals (SLOs). This measurements shows whether you achieved your set goals or not.

*monthly uptime*: The service has an uptime of 99.7 % last month.
*request response time*: The request response time was on average 10 ms last month.


## Creating SLI metrics.
*TODO:* It is important to know why we want to measure certain metrics for our customer. Describe in detail 5 metrics to measure these SLIs. 


  1. **Request Latency:** The lower the latency for a service, the better a customer will judge it. For example, low latency in streaming services can be a competitive advantage. Therefore, I assume that an SLI is very important in terms of latency.
  2. **Availability:** As a customer, I expect a high level of accessibility from a service. Especially if I have to pay a fee for this service. Let's take the streaming service again. Less Availabilty lead to customer dissatisfaction and ultimately to termination of the service. Therefore, this KPI will be very important.
  3. **Error Rate:** Error rates measures the incorrect API requests to a service. This information is important for the developer team, so that they can specifically fix problems. What is critical is when users accidentally receive data that they are not actually supposed to receive. This can lead to a data protection issue.
  4. **Throughput:** Throughput is a measure of how many units of information a system can process in a given period of time. This metric has a significant role in data transfer and storage throughput. For example the better the throughput of a data driven service is, the better the experience of the user.
  5. **Quality:** If a service is no longer available (because of an overload or a lost connection), it may still be possible to use it based on the previously downloaded data. For example, with a gaming app if the connection to the server has been lost. You can still continue with the app.


## Create a Dashboard to measure our SLIs
*TODO:* Create a dashboard to measure the uptime of the frontend and backend services We will also want to measure to measure 40x and 50x errors. Create a dashboard that show these values over a 24 hour period and take a screenshot.

step to start:
Backend Service:  kubectl port-forward svc/backend-service --address 0.0.0.0 8080:8080 (http://localhost:8080)
Frontend Service: kubectl port-forward svc/frontend-service --address 0.0.0.0 9090:9090 (http://localhost:9090)
Grafana: kubectl -n monitoring port-forward service/prometheus-grafana --address 0.0.0.0 3000:80 (http:/localhost:3000)

<img src="https://github.com/MattRo74/observability/blob/main/answer-img/dashboard_slis.png">

## Tracing our Flask App
*TODO:*  We will create a Jaeger span to measure the processes on the backend. Once you fill in the span, provide a screenshot of it here. Also provide a (screenshot) sample Python file containing a trace and span code used to perform Jaeger traces on the backend service.

> localhost:/home/vagrant # kubectl get -n observability ingresses.v1.networking.k8s.io -o yaml | tail
>         port:
>           number: 16686
>   status:
>     loadBalancer:
>       ingress:
>       - ip: 10.0.2.15
> kind: List
> metadata:
>  resourceVersion: ""
>  selfLink: ""

kubectl port-forward -n observability  service/simplest-query --address 0.0.0.0 16686:16686 (http://localhost:8088)


** Jaeger Tracing

<img src="https://github.com/MattRo74/observability/blob/main/answer-img/jaeger_tracing.png">


** Tracing and Span in Python-Script

<img src="https://github.com/MattRo74/observability/blob/main/answer-img/tracing_init.png">

<img src="https://github.com/MattRo74/observability/blob/main/answer-img/tracing_span.png">


## Jaeger in Dashboards
*TODO:* Now that the trace is running, let's add the metric to our current Grafana dashboard. Once this is completed, provide a screenshot of it here.

(to start see steps above)

<img src="https://github.com/MattRo74/observability/blob/main/answer-img/report_error.png">


## Report Error
*TODO:* Using the template below, write a trouble ticket for the developers, to explain the errors that you are seeing (400, 500, latency) and to let them know the file that is causing the issue also include a screenshot of the tracer span to demonstrate how we can user a tracer to locate errors easily.

<img src="https://github.com/MattRo74/observability/blob/main/answer-img/errortrace.png">

TROUBLE TICKET

Name: HTTP request with error

Date: March 2 2024, 23:26:31.701

Subject: Alert in Grafana: Increasing number of errors regarding HTTP requests

Affected Area: File "/app/app.py", line 92, in errortrace

Severity: high

Description: raise InvalidHandle('Internal Error', status_code=500)


## Creating SLIs and SLOs
*TODO:* We want to create an SLO guaranteeing that our application has a 99.95% uptime per month. Name four SLIs that you would use to measure the success of this SLO.

**Availability:**
- more than 99.95% uptime per month (SLO)
- the application was 98% of the time available last month (SLI)

**Errors:**
- less than 0.03% failed request per month  (SLO)
- the error rate was 0,04% last month (SLI)

**Request Latency:**
- average request time for successful HTTP requests is less than 1.5 seconds per month (SLO)
- the average request time for succesful HTTP request was 1.14 seconds last month (SLI)

**CPU Usage:**
- average monthly CPU Usage is less than 50% (SLO)
- the average CPU Usage was 45% last month (SLI)

## Building KPIs for our plan
*TODO*: Now that we have our SLIs and SLOs, create a list of 2-3 KPIs to accurately measure these metrics as well as a description of why those KPIs were chosen. We will make a dashboard for this, but first write them down here.

**CPU Usage:** Average CPU Usage -> KPI to check the status of our environment (scaling issue)

**Latency:** Sum of HTTP request in seconds -> the time for response should as short as possible for the costumer

**Uptime** Uptime of container frontend and backend -> the app should be almost 100% available for the customer

**Errors:** Sum of HTTP requests with an error code ->  KPI to check the the status of our environment (code problems)

## Final Dashboard
*TODO*: Create a Dashboard containing graphs that capture all the metrics of your KPIs and adequately representing your SLIs and SLOs. Include a screenshot of the dashboard here, and write a text description of what graphs are represented in the dashboard.  


<img src="https://github.com/MattRo74/observability/blob/main/answer-img/final_dashboard.png">

**Avg CPU Usage:** Average CPU Usage in seconds

**Latency:** Sum of HTTP request time (in seconds) with error code 200 and 500

**Uptime Frontend** Uptime of the frontend container

**Uptime Backend** Uptime of the backend container

**Errors 4xx:** Sum of HTTP requests with an 400 error code

**Errors 5xx:** Sum of HTTP requests with an 500 error code
