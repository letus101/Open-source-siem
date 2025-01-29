# Open Source SIEM Solution


A comprehensive open-source Security Information and Event Management (SIEM) solution combining best-in-class security tools for enterprise-grade security monitoring.

## 🚀 Features

- **Wazuh**: Host-based intrusion detection and security monitoring
- **Graylog**: Advanced log management and analysis
- **Grafana**: Powerful data visualization and analytics
- **Velociraptor**: Digital forensics and incident response
- **SOCFortress CoPilot**: Security orchestration and automation

## 📋 Prerequisites

- Docker and Docker Compose
- Minimum 16GB RAM
- 4+ CPU cores
- 100GB+ storage space
- Ubuntu/Debian-based system (recommended)

## 🛠️ Quick Setup

1. Clone the repository:
```bash[
git clone https://github.com/letus101/Open-source-siem.git
cd open-source-siem
```

2. Generate SSL certificates:
```bash
cd wazuh
docker compose -f generate-indexer-certs.yml run --rm generator
```

3. Configure Graylog certificates:
```bash
# Copy root-ca.pem to Graylog directory
cp wazuh/root-ca.pem graylog/

# Set proper permissions
sudo chown 1100:1100 graylog/*

# Configure Java truststore
docker exec -it graylog bash
cp /opt/java/openjdk/lib/security/cacerts /usr/share/graylog/data/config/
keytool -importcert -keystore cacerts -storepass changeit -alias wazuh_root_ca -file root-ca.pem/root-ca.pem
```

4. Start the services:
```bash
docker compose up -d
```

5. Configure Velociraptor:
```bash
docker exec -it velociraptor /bin/bash
./velociraptor --config server.config.yaml config api_client --name admin --role administrator,api api.config.yaml
```

## 🔧 Component Configuration

### Wazuh
- Default ports: 1514 (agent), 55000 (API)
- Web interface: https://localhost:5601
- Default credentials: admin/SecretPassword

### Graylog
- Web interface: http://localhost:9000
- Default ports: 12201 (GELF), 514 (Syslog)
- Check logs for initial admin password:
```bash
docker logs "$(docker ps --filter ancestor=ghcr.io/socfortress/copilot-backend:latest --format "{{.ID}}")" 2>&1 | grep "Admin user password"
```

### Grafana
- Web interface: http://localhost:3000
- Default credentials: admin/admin

### Velociraptor
- Web interface: http://localhost:8889
- API: Port 8000
- Frontend: Port 8001

### SOCFortress CoPilot
- Web interface: http://localhost:5000
- API documentation available at /docs endpoint

## 📁 Project Structure

```
open-source-siem/
├── docker-compose.yml    # Main compose file
├── .env                  # Environment variables
├── wazuh/               # Wazuh configuration
├── graylog/             # Graylog configuration
├── grafana/             # Grafana configuration
└── config/              # Additional configurations
```

## 🔐 Security Considerations

- All inter-service communication is encrypted using SSL/TLS
- Default credentials should be changed immediately after installation
- Regular security updates should be applied
- Network segmentation is recommended
- Access control should be properly configured

## 🔗 Resources

- [Wazuh Documentation](https://documentation.wazuh.com/)
- [Graylog Documentation](https://docs.graylog.org/)
- [Grafana Documentation](https://grafana.com/docs/)
- [Velociraptor Documentation](https://docs.velociraptor.app/)
- [SOCFortress Documentation](https://docs.socfortress.co/)

## ⚠️ Troubleshooting

### Common Issues

1. **Connection Issues**
   - Check SSL certificates
   - Verify network connectivity
   - Review firewall rules

2. **Performance Problems**
   - Monitor resource usage
   - Optimize queries
   - Check container limits

3. **Data Collection Issues**
   - Verify agent connectivity
   - Check input configurations
   - Review parsing rules

---

