# Consul

Consul is a tool for service discovery and configuration. Consul is distributed, highly available, and extremely scalable. 

- Service Discovery
- Health Checking
- Service Segmentation/Service Mesh
- Key/Value Storage
- Multi-Datacenter

## [Consul Glossary](https://www.consul.io/docs/glossary.html)

- Agent
- Client
- Server
- Datacenter
- Consensus
- Gossip
- LAN Gossip
- MAN Gossip
- RPC

# [Required Ports](https://www.consul.io/docs/install/ports.html)

| Use                                      | Default Ports |
| ---------------------------------------- | ------------- |
| LAN Serf: The Serf LAN port(TCP and UDP) | 8301          |
| WAN Serf: The Serf WAN port(TCP and UDP) | 8302          |
| HTTP: The HTTP API                       | 8500          |
| server: Server RPC address(TCP only)     | 8300          |



# Reference

[hashicorp/consul](https://github.com/hashicorp/consul)