##

# Best practices for monitoring

When setting up monitoring for a service, keep the following best
practices in mind.

-   **Use the right metric type**&mdash;Most of the libraries available
     today offer various metric types. Choose the appropriate metric
     type for monitoring your system. Following are the types of
     metrics and their purposes.

    -   **Gauge**&mdash;*Gauge* is a constant type of metric. After the
         metric is initialized, the metric value does not change unless
         you intentionally update it.

    -   **Timer**&mdash;*Timer* measures the time taken to complete a
         task.

    -   **Counter**&mdash;*Counter* counts the number of occurrences of a
         particular event.

 For more information about these metric types, see [Data
 Types](https://statsd.readthedocs.io/en/v0.5.0/types.html).

-   **Avoid over-monitoring**&mdash;Monitoring can be a significant
     engineering endeavor. Therefore, be sure not to spend too
     much time and resources on monitoring services, yet make sure all
     important metrics are captured.

-   **Prevent alert fatigue**&mdash;Set alerts for metrics that are
     important and actionable. If you receive too many non-critical
     alerts, you might start ignoring alert notifications over time. As
     a result, critical alerts might get overlooked.

-   **Have a runbook for alerts**&mdash;For every alert, make sure you have
     a document explaining what actions and checks need to be performed
     when the alert fires. This enables any engineer on the team to
     handle the alert and take necessary actions, without any help from
     others.