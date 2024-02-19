### ELK Stack Configuration Guide for Web Server Logs

This comprehensive guide provides you with the templates and detailed steps necessary to deploy the ELK Stack for collecting, processing, storing, and visualizing web server logs. This solution leverages Elasticsearch, Logstash, Kibana, and Filebeat to create a powerful system for real-time log monitoring and analysis.

---

#### Elasticsearch Index Template Setup

**Template Configuration (`ElasticsearchIndexTemplate.json`):**

```json
PUT _index_template/template_web_logs
{
  "index_patterns": ["web-logs-*"],
  "template": {
    "settings": {
      "number_of_shards": 1,
      "number_of_replicas": 1
    },
    "mappings": {
      "properties": {
        "timestamp": { "type": "date" },
        "log_level": { "type": "keyword" },
        "message": { "type": "text" },
        "ip": { "type": "ip" },
        "response_time": { "type": "float" }
      }
    }
  }
}
```

**How to Deploy:**

- Replace `template_web_logs` with your template name (e.g., `template_myapp_logs`).
- Use the Elasticsearch API or Kibana Dev Tools, replacing `localhost:9200` with your Elasticsearch server address:
  ```bash
  curl -X PUT "localhost:9200/_index_template/template_web_logs" -H 'Content-Type: application/json' -d@ElasticsearchIndexTemplate.json
  ```

---

#### Logstash Configuration

**Configuration File (`LogstashConfig.conf`):**

```ruby
input {
  beats {
    port => 5044
  }
}

filter {
  grok {
    match => { "message" => "%{COMBINEDAPACHELOG}" }
  }
  date {
    match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]
  }
}

output {
  elasticsearch {
    hosts => ["http://localhost:9200"]
    index => "web-logs-%{+YYYY.MM.dd}"
    user => "elastic"
    password => "changeme"
  }
}
```

**How to Deploy:**

- Adjust `hosts`, `user`, and `password` to match your Elasticsearch details.
- Place the `LogstashConfig.conf` in Logstash's configuration directory.
- Start Logstash: `bin/logstash -f LogstashConfig.conf`.

---

#### Kibana Visualization and Dashboards

**Creating Visualizations:**

1. Access Kibana (typically at `http://localhost:5601`).
2. Create an Index Pattern: Go to Management → Index Patterns → Create new. Use `web-logs-*` as the pattern.
3. Navigate to "Visualize" and create new visualizations using the fields from your logs.
4. Assemble visualizations into dashboards for a comprehensive view.

---

#### Filebeat Configuration

**Configuration File (`filebeat.yml`):**

```yaml
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /var/log/apache2/*.log # Adjust the path to your log files
  fields:
    log_type: apache_log

output.logstash:
  hosts: ["localhost:5044"] # Your Logstash server address
```

**How to Deploy:**

- Install Filebeat on the server where your logs are generated.
- Modify `filebeat.yml` with your log paths and output destination.
- Start Filebeat: `sudo service filebeat start`.

---

### Final Steps and Verification

After deploying each component:

1. **Ensure Data Flow:** Confirm that logs are moving from Filebeat to Logstash, then to Elasticsearch, and finally visualizable in Kibana.
2. **Monitor System Health:** Use Kibana's monitoring features to check the health of Elasticsearch, Logstash, and Filebeat.

This guide provides a foundational setup. Tailor each configuration to fit your specific logging needs and infrastructure for an effective log monitoring and analysis solution.
