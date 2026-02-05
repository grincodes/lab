
# Resoruces
- github repo fro post mortens [https://github.com/danluu/post-mortems](post-mortems)

circuit breaker for db
request partition for http


# Effective alerts should be:

Actionable
Alert only on conditions that require human intervention.

Accurate
Minimize false positives to maintain trust in your alerts.

Clear
Provide sufficient context—including relevant metrics, service names, and links to diagnostics—for rapid diagnosis.

Appropriate
Route alerts to the right team with the correct priority and escalation paths.


# Shell Terminal commands
for i in {1..50}; do curl -s http://localhost:8000/nonexistent; done