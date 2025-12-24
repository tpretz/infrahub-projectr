# Infrahub On-Premises Compute Infrastructure Model

This repository contains a comprehensive Infrahub schema for modeling on-premises compute infrastructure, including physical facilities, servers, network connectivity, and services.

## Overview

The schema models your complete infrastructure stack from the ground up:

- **Physical Infrastructure**: Datacenters, rooms, racks, and servers
- **Network Infrastructure**: Interfaces, VLANs, VRFs, IP networks, and routing
- **Service Infrastructure**: Applications, load balancers, virtual IPs, and service topology

## Repository Structure

```
.
├── .infrahub.yml                    # Infrahub configuration file
├── schemas/                         # Schema definitions
│   ├── physical-infrastructure.yml  # Physical assets and locations
│   ├── network-infrastructure.yml   # Network components and connectivity
│   └── service-infrastructure.yml   # Services and load balancing
├── objects/                         # Sample data objects
│   ├── datacenters.yml             # Sample datacenters, rooms, and racks
│   ├── servers.yml                 # Sample server configurations
│   ├── networks.yml                # Sample network configurations
│   └── services.yml                # Sample service configurations
├── queries/                         # GraphQL queries
│   ├── datacenter-inventory.graphql # Query datacenter infrastructure
│   └── service-topology.graphql     # Query service topology and dependencies
└── README.md                       # This documentation
```

## Schema Architecture

### Physical Infrastructure (`InfraPhysicalAsset`)

The physical layer models your facilities and hardware:

- **Datacenter**: Physical facilities with power, cooling, and space attributes
- **Room**: Rooms within datacenters (server, network, utility, storage)
- **Rack**: Server racks with mounting and power specifications
- **Server**: Physical servers with detailed hardware specifications

**Key Features:**
- Hierarchical relationships (Datacenter → Room → Rack → Server)
- Asset tracking with serial numbers, asset tags, and status
- Power and space capacity management
- Flexible server types (physical, blade, hypervisor)

### Network Infrastructure (`InfraNetworkInterface`)

The network layer models connectivity and IP addressing:

- **Interface**: Network interfaces on servers with speed, duplex, and status
- **VLAN**: Virtual LANs for traffic segmentation
- **VRF**: Virtual routing and forwarding for network isolation
- **IPNetwork**: IP subnets with CIDR notation and network types
- **IPAddress**: Individual IP assignments with address types
- **Route**: Routing entries with next-hop and metrics

**Key Features:**
- Interface connectivity tracking
- VLAN and VRF associations
- IP address management with subnet relationships
- Support for multiple routing protocols (static, OSPF, BGP)
- Management vs production network separation

### Service Infrastructure (`ServiceServiceEndpoint`)

The service layer models applications and load balancing:

- **Service**: Application service definitions with environments
- **ServiceInstance**: Running service instances on specific servers
- **LoadBalancer**: Load balancers with algorithms and SSL termination
- **VirtualIP**: Virtual IPs for high availability
- **LoadBalancerPool**: Backend server pools with health checking
- **PoolMember**: Individual servers in load balancer pools

**Key Features:**
- Service-to-server mapping with port and protocol details
- Load balancer configuration with multiple algorithms
- Health checking configuration
- Virtual IP management with VRRP/HSRP support
- Environment separation (production, staging, development)

## Key Relationships

The schema establishes several important relationship patterns:

1. **Hierarchical Physical Containment**:
   ```
   Datacenter → Room → Rack → Server
   ```

2. **Network Connectivity**:
   ```
   Server → Interface → IPAddress → IPNetwork → VLAN → VRF
   ```

3. **Service Deployment**:
   ```
   Service → ServiceInstance → Server
   LoadBalancer → LoadBalancerPool → PoolMember → ServiceInstance
   ```

4. **Cross-Layer Integration**:
   - Services reference physical servers and IP addresses
   - Load balancers are deployed on physical servers
   - Network interfaces connect to both physical and virtual resources

## Sample Data

The repository includes comprehensive sample data demonstrating:

- A New York datacenter (`DC-NYC-01`) with server and network rooms
- Multiple racks with various server types
- Web servers, database servers, and load balancer servers
- Production VLANs and IP networks with proper segmentation
- A complete web service topology with load balancing

## GraphQL Queries

Two sample queries are provided:

1. **Datacenter Inventory** (`datacenter-inventory.graphql`):
   - Complete infrastructure hierarchy for a datacenter
   - Hardware specifications and network assignments
   - Physical location and capacity utilization

2. **Service Topology** (`service-topology.graphql`):
   - Service instance deployments and dependencies
   - Load balancer configuration and pool membership
   - End-to-end service connectivity mapping

## Usage Instructions

1. **Load Schema into Infrahub**:
   ```bash
   infrahubctl schema load schemas/
   ```

2. **Import Sample Data**:
   ```bash
   infrahub-sync import objects/
   ```

3. **Execute Sample Queries**:
   ```bash
   infrahubctl query run queries/datacenter-inventory.graphql --variables '{"datacenter_name": "DC-NYC-01"}'
   infrahubctl query run queries/service-topology.graphql --variables '{"service_name": "web-frontend"}'
   ```

## Schema Extensions

The schema is designed for extensibility. Consider adding:

### Monitoring and Observability:
- Monitoring agents and collectors
- Metric definitions and thresholds
- Alert rules and notification channels

### Security and Compliance:
- Security zones and policies
- Certificate management
- Vulnerability tracking

### Automation and Orchestration:
- Configuration templates
- Deployment pipelines
- Infrastructure as Code integration

### Capacity Management:
- Resource utilization metrics
- Capacity planning models
- Cost allocation and tracking

## Best Practices

1. **Naming Conventions**: Use consistent naming patterns across all objects
2. **Unique Identifiers**: Leverage `human_friendly_id` for cross-system references
3. **Status Tracking**: Maintain accurate status information for operational visibility
4. **Hierarchical Organization**: Utilize parent-child relationships for logical grouping
5. **Network Segmentation**: Model VLANs and VRFs to reflect actual network design
6. **Service Mapping**: Ensure services are properly mapped to physical infrastructure

## Contributing

When extending the schema:

1. Follow the established naming patterns and namespaces
2. Use appropriate relationship kinds (`Parent`, `Component`, `Attribute`, `Generic`)
3. Include proper uniqueness constraints and human-friendly IDs
4. Add comprehensive descriptions and appropriate icons
5. Test with sample data before committing changes

## Support

For questions about Infrahub schema design:
- [Infrahub Documentation](https://docs.infrahub.app/)
- [Schema Development Guide](https://docs.infrahub.app/topics/schema)
- [Repository Configuration](https://docs.infrahub.app/reference/dotinfrahub)