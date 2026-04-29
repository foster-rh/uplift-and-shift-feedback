# UpLift and Shift Workshop - Feedback & Retrospective

**Workshop:** OpenShift Virtualization on ROSA  
**Lab Guide:** https://rhpds.github.io/uplift-showroom/modules/index.html  
**Facilitator:** Paul Foster  


---

## Feedback Summary

**The Current State: High Engagement, Low Delivery**

The "UpLift and Shift" workshop generates strong initial engagement and customer interest in OpenShift Virtualization on ROSA. However, the current content fails to deliver on the promise. We are attracting enterprise customers who need production-ready solutions for VM migration and hybrid workloads, but we're providing them with basic demonstrations that don't address their real-world requirements.

We need to show them we can deliver on what they are used to in traditional VM environments - networking, DR, storage = CUDN, BGP, FSx, live migrations, NetApp ONTAP.

We need to excite them about containers and VMs running side by side using modern DevOps processes and infrastructure = ambient service mesh, gateway API, connectivity link and dynamic DNS policies, services interconnect and multi-cluster GitOps with ACM.

---


## Existing Hands-On Modules

#### Module 01: Dashboards - Operators, Multi-Tenancy, and Resource Utilization
- **Topics:**
  - OpenShift Operators for VM management
  - Multi-tenant VM isolation
  - Resource monitoring and utilization

- **Feedback:**

This section should really go through what is an operator - then look specifically at the virt operator and how to configure it.

#### Module 02: Declarative IaC for Automating VM Resources
- **Topics:**
  - Infrastructure as Code principles for VMs
  - Automating VM resource provisioning
  - GitOps workflows

- **Feedback:**

This section is relevant - need some basic click ops section covering lots of bits of the UI and metrics and connecting to a VM etc. Don't need to worry about GitOps here, just want customers to get used to using the UI to manage a VM. Probably include creating VMs from OCI images here in some sort of catalog.

#### Module 03: Declarative IaC for Automating Virtual Machine Creation
- **Topics:**
  - VM template creation
  - Automated VM deployment
  - Version control for VM definitions

- **Feedback:**

Repetitive - adds no real value.

#### Module 04: Simplified Configuration for Load Balancing
- **Topics:**
  - Load balancing VMs in OpenShift
  - Service exposure patterns
  - Traffic distribution

- **Feedback:**

We can do so much better - see suggested modules later.

#### Module 05: Network Egress Firewall
- **Topics:**
  - Network security for VMs
  - Egress traffic control
  - Compliance and isolation

- **Feedback:**

Irrelevant - so basic and noddy.

#### Module 06: Istio Ambient Mesh Traffic Management for Virtual Machines
- **Topics:**
  - Istio Ambient Mesh architecture (sidecar-free)
  - VM onboarding to ambient mesh (no proxy injection)
  - ztunnel for L4 connectivity and zero-trust networking
  - Waypoint proxies for L7 policy enforcement
  - **GitOps-driven traffic management** (all policies in Git, deployed via ArgoCD)
  - VirtualServices, DestinationRules, and Gateways as code
  - Traffic routing, canary deployments, circuit breaking
  - Observability with Prometheus/Grafana (**no Kiali**)

- **Feedback:**

Kiali - no one uses this. What are we doing with service mesh 2 and Kiali?


### Proposed New Modules 

The following **7 new modules** are recommended to address enterprise-critical topics and provide production-ready, real-world scenarios:

| Module # | Topic | Key Technologies | Priority |
|----------|-------|------------------|----------|
| **Module 01** | Multi-Cluster GitOps Application Deployment | ACM, ArgoCD, ApplicationSets, Hybrid VMs & Containers, OpenShift Lightspeed | **Critical** |
| **Module 02** | AWS FSx Storage Integration | FSx for ONTAP/Windows, CSI Driver, Multi-AZ HA, GitOps | **High** |
| **Module 03** | Enterprise Storage, Backup & Restore | NetApp, S3, Cross-cluster Migration | **Critical** |
| **Module 04** | Live VM Migration Between Clusters | MTV, Cross-cluster Migration, Zero-downtime | **High** |
| **Module 05** | Connectivity Link DNS Policy & Dynamic Routing | Connectivity Link, Route 53, Network Observability Operator | **Critical** |
| **Module 06** | User Defined Networking (UDN) | BGP, AWS Route Server, Custom Subnetting, Transit Gateway, Network Observability | **High** |
| **Module 07** | Migration Planning & Assessment | Capacity Planning, Cost Analysis, Readiness Checklist | **Medium** |

**Key Themes Across New Modules:**
- ✅ **Production-Ready:** Real enterprise scenarios (not toy examples)
- ✅ **GitOps-Driven:** All configuration as code, no GUI dependencies
- ✅ **Hybrid Workloads:** VMs and containers working together
- ✅ **Multi-Cluster:** HA, DR, and resilience patterns
- ✅ **Cloud-Native Storage:** FSx, S3, backup/restore automation
- ✅ **Advanced Networking:** UDN, BGP, service mesh, DNS failover

---

## New Lab Environment Architecture

The workshop uses a modern multi-cluster architecture demonstrating hybrid VM and container workloads:

- **Application:** Legacy VM and modernized container components deployed as a single application
- **Hub Cluster:** ACM (Advanced Cluster Management) manages GitOps ApplicationSets deploying hybrid workloads
- **Spoke Clusters:** 2 ROSA clusters (dev-spoke-1 and dev-spoke-2) with shared application namespace
- **Cross-Cluster Networking:** Services Interconnect enables transparent VM-to-container communication
- **Traffic Management:** Connectivity Link manages DNS policies for API Gateway and Ambient Service Mesh Envoy Gateway

---

### Content Improvements - Detailed Breakdown

1. **Multi-Cluster GitOps Application Deployment with ACM**
   - **Topic:** Deploy hybrid VM/container application across multiple clusters using ACM and GitOps ApplicationSets
   
   - **Content to Cover:**
     - Advanced Cluster Management (ACM) hub and spoke architecture
     - GitOps ApplicationSets for multi-cluster deployment
     - Hybrid workload orchestration (VMs and containers)
     - OpenShift Lightspeed AI assistance for resource exploration
     - Cross-cluster application visibility and management
   
   - **Hands-On Exercise:**
     - Deploy application on ACM hub cluster integrated with GitOps
     - Create ApplicationSets that deploy to both spoke clusters
     - Deploy hybrid workloads: VMs on dev-spoke-1, containers on both clusters
     - Include a Windows VM in the deployment
     - Verify Ambient Service Mesh components deploy correctly
     - Verify Connectivity Link components deploy correctly
     - Verify Services Interconnect components deploy correctly
     - Use OpenShift Lightspeed prompts to query and understand deployed resources
     - Explore cluster topology and application dependencies
   
   - **Rationale:**
     - Demonstrates enterprise multi-cluster management patterns
     - Shows VMs as first-class citizens in GitOps workflows
     - Proves VMs and containers can be managed with same tooling
     - OpenShift Lightspeed provides AI-assisted learning experience
     - Sets foundation for subsequent modules (storage, migration, networking)

2. **Add Module on AWS FSx Storage Integration for VMs**
   - **Topic:** Using Amazon FSx as persistent storage for VMs running on OpenShift Virtualization in ROSA.

   Move our VMs over to FSx via GitOps and the appsets.

   - **Content to Cover:**
     - Amazon FSx storage types and use cases (FSx for NetApp ONTAP, FSx for Windows File Server, FSx for Lustre)
     - CSI driver integration for FSx with OpenShift Virtualization
     - Performance characteristics and sizing for VM workloads
     - Multi-AZ deployment and high availability
     - Data protection with FSx snapshots and backups
     - Cost optimization strategies for FSx storage
   - **Hands-On Exercise:**
     - Deploy Amazon FSx for NetApp ONTAP in AWS VPC
     - Configure FSx CSI driver in ROSA cluster
     - Create PersistentVolumes using FSx storage
     - Attach FSx volumes to VMs as additional disks
     - Configure VM boot disks on FSx storage
     - Test VM live migration with FSx-backed storage
     - Implement automated snapshots and backup policies
     - Measure storage performance (IOPS, throughput, latency)
     - Update our app to use FSx

   - **Advanced Scenarios:**
     - Multi-tenant storage isolation using FSx SVMs (Storage Virtual Machines)
     - Cross-region replication with FSx SnapMirror?
     - Hybrid cloud data tiering (hot/cold storage)
     - Integration with AWS Backup for centralized backup management
   - **Rationale:**
     - FSx provides enterprise-grade, fully managed storage for VM workloads
     - Native AWS integration reduces operational complexity
     - Critical for Windows VM migrations (FSx for Windows File Server)
     - Demonstrates cloud-native storage alternatives to traditional SAN/NAS


3. **Add Module on Enterprise Storage, Backup & Restore**
   - **Topic:** Real-world VM backup, disaster recovery, and migration from traditional infrastructure to OpenShift Virtualization.

   The VM in our deployed application suffers a critical failure - let's restore it from backup to the other cluster and remove it on cluster spoke 1.

   - **Content to Cover:**
     - Enterprise storage integration (NetApp, Ceph, AWS EBS/EFS)
     - VM snapshot management and lifecycle
     - Cross-platform backup strategies (on-premises to cloud)
     - S3 as backup target for long-term retention
     - Disaster recovery planning and testing
     - VM restoration workflows in OpenShift Virtualization
   - **Hands-On Exercise - Real Migration Scenario:**
     - **Source Environment:** VM running on RHEL box in AWS VPC with NetApp storage
     - **Workflow:**
       1. Create VM snapshots using NetApp on spoke 1 to spoke 2.
       2. Export snapshots and push to AWS S3 bucket
       3. Configure S3 bucket policies and lifecycle management
       4. Import snapshot data from S3 to OpenShift Virtualization on ROSA on spoke 2.
       5. Restore and boot VM on ROSA from S3-backed snapshot
       6. Validate VM functionality and data integrity
       7. Test disaster recovery scenario (simulate failure and restore)
     - **Tools Demonstrated:**
       - NetApp snapshot APIs and CLI
       - AWS S3 CLI and SDK
       - OpenShift Virtualization DataVolume imports

   - **Rationale:** 
     - Storage and DR are top concerns for enterprise VM migrations
     - Participants need realistic, production-ready backup/restore workflows
     - S3 integration is critical for cloud-native storage architectures
     - Demonstrates hybrid cloud data portability and business continuity

4. **Add Module on Live VM Migration Between Clusters**
   - **Topic:** Cross-cluster VM migration for multi-cloud, disaster recovery, and cluster upgrade scenarios.

   Live migrate the VM back to spoke 1.

   Confirm app still works when VM is on other cluster communicating with containers on other cluster.

   - **Content to Cover:**
     - Live migration vs cold migration strategies
     - Multi-cluster OpenShift Virtualization architecture
     - Network connectivity requirements for cross-cluster migration
     - Storage replication and synchronization
     - Migration Toolkit for Virtualization (MTV) overview
     - Zero-downtime migration techniques
     - Rollback and failover strategies
   - **Hands-On Exercise - Multi-Cluster Migration:**
     - **Scenario:** Migrate running VM from dev-spoke-2 to dev-spoke-1 cluster in app namespace.

     - **Workflow:**
       1. Set up migration prerequisites (network connectivity, storage access)
       2. Configure Migration Toolkit for Virtualization (MTV) on hub cluster
       3. Create migration plan for VM workloads
       4. Perform live migration of running VM (zero downtime)
       5. Monitor migration progress and network cutover
       6. Validate VM functionality on destination cluster
       7. Test rollback scenario
     - **Advanced Scenarios:**
       - Bulk migration of multiple VMs
       - Migration across availability zones
       - Migrating VMs with attached persistent storage
       - Network traffic redirection during migration (DNS/load balancer updates)
       - Cluster upgrade via VM migration (evacuate old cluster, migrate to new)
   - **Tools Demonstrated:**
     - Migration Toolkit for Virtualization (MTV)
     - OpenShift Virtualization live migration APIs
     - Multi-cluster networking (Submariner, service mesh)
     - Storage replication (FSx SnapMirror, EBS snapshots)
   - **Rationale:**
     - Critical for DR planning and business continuity
     - Enables cluster upgrades without application downtime
     - Demonstrates multi-cloud portability
     - Real-world scenario for modernizing legacy infrastructure
     - Showcases OpenShift's advanced VM mobility capabilities

5. **Add Module on Connectivity Link DNS Policy and Dynamic Routing**
   - **Topic:** Dynamic DNS-based traffic management with health-aware routing and network flow visualization
   - **Key Technologies:**
     - **Connectivity Link (RHCL/Kuadrant)** - DNS-based traffic management and health-based routing
     - **Network Observability Operator** - Flow visualization and traffic analysis
     - **Route 53 Integration** - Dynamic DNS configuration updates
   - **Content to Cover:**
     - Connectivity Link DNSPolicy custom resources
     - Health-based routing and automatic failover
     - Weighted traffic distribution across clusters
     - Geographic and latency-based DNS routing
     - Network flow visualization with Network Observability Operator
     - Real-time traffic pattern analysis
   - **Hands-On Exercise - Dynamic DNS Routing:**
     - **Workflow:**
       1. **Deploy Connectivity Link DNSPolicy:**
          - Configure DNS domain in Route 53 (e.g., `app.example.com`)
          - Create DNSPolicy resources for multi-cluster application
          - Define health-based routing policies
          - Configure weighted traffic distribution (e.g., 50/50 across dev-spoke-1 and dev-spoke-2)
          - Set up automatic failover rules based on health checks
       2. **Test dynamic DNS routing:**
          - Access application via single DNS name
          - Observe traffic distribution across clusters
          - Simulate cluster failure (scale down workloads on dev-spoke-1)
          - Watch automatic DNS update in Route 53
          - Verify traffic redirected to healthy cluster (dev-spoke-2)
          - Restore dev-spoke-1 and observe automatic rebalancing
       3. **Update DNS policies for dynamic routing:**
          - Modify DNSPolicy weights via GitOps
          - Implement canary deployment patterns (90/10, then 50/50, then 10/90)
          - Configure geographic routing rules
          - Add latency-based routing policies
          - Test traffic shifting in real-time
       4. **Visualize with Network Observability Operator:**
          - Deploy Network Observability Operator on both clusters
          - Configure flow collection for application namespace
          - View traffic flows between clusters in real-time
          - Analyze DNS policy impact on traffic distribution
          - Monitor failover events and traffic patterns
          - Identify network bottlenecks and latency issues
          - Visualize VM-to-container and cross-cluster communication flows

   - **Rationale:**
     - Demonstrates cloud-native DNS-based traffic management
     - Shows real-time impact of routing policy changes
     - Network observability critical for production troubleshooting
     - Proves VMs can participate in modern distributed routing
     - Real-world DR and business continuity pattern

6. **Add Module on User Defined Networking (UDN) for VMs**
   - **Topic:** Custom User Defined Networking (CUDN) for VM workloads
   - **Content to Cover:**
     - User Defined Networking (UDN) concepts in OpenShift Virtualization
     - Custom subnetting for VMs running in CUDN environments
     - BGP route advertisement to AWS Route Server for hybrid connectivity
     - Integration with AWS VPC networking and transit gateway
     - Use cases: Multi-tenancy isolation, custom IP addressing, hybrid cloud networking
   - **Hands-On Exercise:** 
     - Configure UDN for VM workloads
     - Set up BGP peering with AWS Route Server
     - Demonstrate VM-to-VPC connectivity through advertised routes
     - Test traffic flow between VMs in custom subnets and AWS services
     - Observe in Network Observability Operator
   - **Rationale:** Advanced networking is critical for enterprise VM migrations, especially for workloads requiring custom IP schemes or hybrid cloud connectivity

7. **Migration Planning Module**
   - Add a pre-migration assessment section
   - Include capacity planning and cost analysis tools
   - Provide migration readiness checklist
   - Maybe some TCO analysis in here?

