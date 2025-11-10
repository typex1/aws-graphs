# AWS Graph-Based Concepts

## 1. Connected VPCs - Bidirectional Graph

```mermaid
graph TB
    VPC1[VPC-A<br/>10.0.0.0/16<br/>us-east-1]
    VPC2[VPC-B<br/>10.1.0.0/16<br/>us-west-2]
    VPC3[VPC-C<br/>10.2.0.0/16<br/>eu-west-1]
    VPC4[VPC-D<br/>10.3.0.0/16<br/>ap-southeast-1]
    
    VPC1 <-.->|Peering Connection| VPC2
    VPC2 <-.->|Peering Connection| VPC4
    VPC3 <-.->|Peering Connection| VPC4
    VPC2 <-.->|Peering Connection| VPC3
```

## 2. CloudFormation Dependency Graph

```mermaid
graph TD
    VPC[VPC<br/>AWS::EC2::VPC]
    IGW[Internet Gateway<br/>AWS::EC2::InternetGateway]
    RT[Route Table<br/>AWS::EC2::RouteTable]
    SN1[Public Subnet<br/>AWS::EC2::Subnet]
    SN2[Private Subnet<br/>AWS::EC2::Subnet]
    SG[Security Group<br/>AWS::EC2::SecurityGroup]
    EC2[EC2 Instance<br/>AWS::EC2::Instance]
    RDS[RDS Database<br/>AWS::RDS::DBInstance]
    ALB[Load Balancer<br/>AWS::ElasticLoadBalancingV2::LoadBalancer]
    
    VPC --> SN1
    VPC --> SN2
    VPC --> SG
    VPC --> IGW
    VPC --> RT
    SN1 --> EC2
    SN2 --> RDS
    SG --> EC2
    SG --> RDS
    SN1 --> ALB
    EC2 --> ALB
    RT --> SN1
    IGW --> RT
    
    style VPC fill:#e1f5fe
    style EC2 fill:#fff3e0
    style RDS fill:#f3e5f5
    style ALB fill:#e8f5e8
```

## 3. Amazon Neptune Graph Database

```mermaid
graph LR
    subgraph "Neptune Cluster"
        NP1[Neptune Primary<br/>Writer Instance]
        NP2[Neptune Replica 1<br/>Reader Instance]
        NP3[Neptune Replica 2<br/>Reader Instance]
    end
    
    subgraph "Graph Data Model"
        U1[User: John]
        U2[User: Alice]
        P1[Product: Laptop]
        P2[Product: Mouse]
        O1[Order: #12345]
        
        U1 -->|PURCHASED| O1
        O1 -->|CONTAINS| P1
        O1 -->|CONTAINS| P2
        U1 -->|FRIEND_OF| U2
        U2 -->|VIEWED| P1
    end
    
    subgraph "Applications"
        APP1[Recommendation Engine]
        APP2[Fraud Detection]
        APP3[Social Network Analysis]
    end
    
    APP1 --> NP1
    APP2 --> NP2
    APP3 --> NP3
    
    NP1 -.->|Replication| NP2
    NP1 -.->|Replication| NP3
    
    style NP1 fill:#ff9800
    style NP2 fill:#ffcc80
    style NP3 fill:#ffcc80
    style U1 fill:#4caf50
    style U2 fill:#4caf50
    style P1 fill:#2196f3
    style P2 fill:#2196f3
    style O1 fill:#9c27b0
```

## 4. Wikipedia Pages Connection Graph

```mermaid
graph LR
    NP[North Pole]
    GNE[Great Northern Expedition]
    GWL[Gottfried Wilhelm Leibniz]
    LP[List of Paradoxes]
    
    NP -->|links to| GNE
    GNE -->|links to| GWL
    GWL -->|links to| LP
    
    style NP fill:#e3f2fd
    style GNE fill:#f3e5f5
    style GWL fill:#fff3e0
    style LP fill:#e8f5e8
```
## 5. AWS Transit Gateway Hub-and-Spoke Architecture

```mermaid
graph TD
    TGW[Transit Gateway<br/>Hub]
    
    VPC1[Production VPC<br/>10.0.0.0/16]
    VPC2[Development VPC<br/>10.1.0.0/16]
    VPC3[Staging VPC<br/>10.2.0.0/16]
    VPC4[Shared Services VPC<br/>10.3.0.0/16]
    
    DX[Direct Connect<br/>On-Premises]
    
    TGW --- VPC1
    TGW --- VPC2
    TGW --- VPC3
    TGW --- VPC4
    TGW --- DX
    
    style TGW fill:#ff9800
    style VPC1 fill:#4caf50
    style VPC2 fill:#2196f3
    style VPC3 fill:#9c27b0
    style VPC4 fill:#ff5722
    style DX fill:#607d8b
```
