# elk_dashboard
To install Elasticsearch, Metricbeat, and Kibana on AlmaLinux 8.6, you can follow these steps. These steps will guide you through the installation and basic setup.

### Step 1: Install Java (Elasticsearch requirement)

Elasticsearch requires Java to run. You can install OpenJDK 11 using the following commands:

```bash
sudo dnf install java-11-openjdk-devel -y
```

### Step 2: Add the Elasticsearch GPG Key and Repository

```bash
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
cat <<EOF | sudo tee /etc/yum.repos.d/elasticsearch.repo
[elasticsearch]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
EOF
```

### Step 3: Install Elasticsearch

```bash
sudo dnf install elasticsearch -y
```

### Step 4: Start and Enable Elasticsearch

```bash
sudo systemctl enable elasticsearch --now
### Step 5: Install Kibana

Add the Kibana repository:

```bash
cat <<EOF | sudo tee /etc/yum.repos.d/kibana.repo
[kibana]
name=Kibana repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
EOF
```

Then install Kibana:

```bash
sudo dnf install kibana -y
```

### Step 6: Start and Enable Kibana

```bash
sudo systemctl enable kibana --now
```

### Step 7: Install Metricbeat

Add the Metricbeat repository:

```bash
cat <<EOF | sudo tee /etc/yum.repos.d/metricbeat.repo
[metricbeat]
name=Metricbeat repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
EOF
```

Then install Metricbeat:

```bash
sudo dnf install metricbeat -y
```

### Step 8: Configure Metricbeat

Modify the Metricbeat configuration file `/etc/metricbeat/metricbeat.yml` to set up the connection to Elasticsearch and Kibana. You can open the file using a text editor:

```bash
sudo nano /etc/metricbeat/metricbeat.yml
```

Ensure the following lines are configured properly:

```yaml
output.elasticsearch:
  hosts: ["http://localhost:9200"]

setup.kibana:
  host: "http://localhost:5601"
```

### Step 9: Start and Enable Metricbeat

```bash
sudo systemctl enable metricbeat --now
```

### Step 10: Open Firewall Ports

If you have a firewall enabled, you need to allow traffic to the necessary ports:

```bash
sudo firewall-cmd --permanent --add-port=9200/tcp
sudo firewall-cmd --permanent --add-port=5601/tcp
sudo firewall-cmd --reload
```

### Accessing Kibana

You should now be able to access Kibana by navigating to `http://<your-ip>:5601` in your web browser.

### Verify Installation

To ensure that everything is running correctly, you can check the status of each service:

```bash
sudo systemctl status elasticsearch
sudo systemctl status kibana
sudo systemctl status metricbeat
```

These commands will provide you with the current status of each service and help diagnose any potential issues.

By following these steps, you should have a working installation of Elasticsearch, Kibana, and Metricbeat on your AlmaLinux 8.6 system.
