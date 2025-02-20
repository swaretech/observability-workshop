---
title: 1.1 Generate load
weight: 2
---

{{% button icon="clock" %}}5 minutes{{% /button %}}

## 1. Generate traffic

The Online Boutique deployment contains a container running Locust that we can use to generate load traffic against the website to generate metrics, traces and spans.

{{% expand title="{{% badge style=primary icon=user-ninja %}}**Ninja** - Access your Locust instance{{% /badge %}}" %}}
{{% notice style="blue" %}}
Locust is available on port 82 of the EC2 instance's IP address. Open a new tab in your web browser and go to `http://<ec2-ip-address>:82/`, you will then be able to see the Locust running.
{{% /notice %}}
{{% /expand %}}

Your Workshop instructor will provide you with URL to access the Locust load generator. Enter this URL into your browser and you will see the Locust Start new load test landing page.

![Locust](../../images/locust.png)

Set the **Spawn rate** to be 2 and click **Start Swarming**, this will start a gentle continuous load on the application.

![Spawn Rate](../../images/locust-spawn-rate.png)

![Statistics](../../images/locust-statistics.png)

---

Now go to **Dashboards → All Dashboards → APM Services → Service**.

For this we need to know the name of your application environment. In this workshop all the environments use: `<hostname>-apm-env`.

To find the hostname, on the AWS/EC2 instance run the following command:

{{< tabs >}}
{{% tab title="Echo Hostname" %}}

``` bash
echo $(hostname)-apm-env
```

{{% /tab %}}
{{% tab title="Output Example" %}}

``` text
bdzx-apm-env
```

{{% /tab %}}
{{< /tabs >}}

Select your environment you found in the previous step then select the `frontend` service and set time to Past 15 minutes.

![APM Dashboard](../../images/online-boutique-service-dashboard.png)

With this automatically generated dashboard you can keep an eye out for the health of your service(s) using RED (Rate, Error & Duration) metrics. It provides various performance related charts as well as correlated information on the underlying host and Kubernetes pods (if applicable).

Take some time to explore the various charts in this dashboard

---

## 2. Verify Splunk APM metrics

In the left hand menu card click on **APM** this will bring you to the APM Overview dashboard:

![select APM](../../images/online-boutique-apm.png)

Select the **Explore** on the right hand side and select your environment you found before and set the time to 15 minutes. This will show you the automatically generated Dependency/Service Map for the Online Boutique application.

It should look similar to the screenshot below:

![Online Boutique in APM](../../images/online-boutique-map.png)

The legend at the bottom of the page explains the different visualizations in the Dependency/Service Map.

![APM Legend](../../images/apm-legend.png)

* Service requests, error rate and root error rate.
* Request rate, latency and error rate

Also in this view you can see the overall Error and Latency rates over time charts.

## 3. OpenTelemetry Dashboard

Once the Open Telemetery Collector is deployed the platform will automatically provide a built in dashboard display OpenTelemetry Collector metrics.

From the top left hamburger menu **Dashboards → OpenTelemetry Collector**, scroll all the way to the bottom of the page and validate metrics and spans are being sent:

![OpenTelemetry Collector dashboard](../../images/otel-dashboard.png)

## 4. OpenTelemetry zpages

To debug the traces being sent you can use the zpages extension. [zpages][zpages] are part of the OpenTelemetry collector and provide live data for troubleshooting and statistics.

{{% expand title="{{% badge style=primary icon=user-ninja %}}**Ninja** - Access zPages on your EC2 instance{{% /badge %}}" %}}
{{% notice style="blue" %}}
zPages is available on port `55679` of the EC2 instance's IP address. Open a new tab in your web browser and enter in `http://{==EC2-IP==}:55679/debug/tracez`, you will then be able to see the zpages output.

Alternatively, from your shell prompt you can run a text based browser:

``` bash
lynx http://localhost:55679/debug/tracez
```

{{% /notice %}}
{{% /expand %}}

Your Workshop instructor will provide you with a URL to access zPages. Enter this URL into your browser and you will see the zPages output.

[zpages]: https://github.com/open-telemetry/opentelemetry-specification/blob/main/experimental/trace/zpages.md#tracez

![zpages](../../images/zpages.png)
