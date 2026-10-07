# vulnerable-demo

## Architecture

```mermaid
flowchart TB
    subgraph Cluster["Kubernetes Cluster"]
        API[Kubernetes API Server]
        Workloads[Pods / Deployments]

        subgraph Operator["Vulnerability Operator"]
            Controller[Controller / Reconciler]
            Scanner[Image Scanner Engine]
        end

        CRD[(VulnerabilityReport CRD)]
    end

    Registry[(Container Registry)]
    CVEDB[(CVE / Vulnerability Database)]
    Alert[Alerting - Slack / Email / Webhook]

    API -- watches --> Workloads
    Controller -- watches --> API
    Controller --> Scanner
    Scanner -- pulls image metadata --> Registry
    Scanner -- checks against --> CVEDB
    Scanner -- writes findings --> CRD
    Controller -- creates/updates --> CRD
    Controller -- notifies --> Alert
```

The operator watches Pods/Deployments via the Kubernetes API, scans their container images against a CVE database, writes results to a `VulnerabilityReport` custom resource, and sends alerts for critical findings.

## Demo

[Vulnerablity-operator-demo.mp4](./Vulnerablity-operator-demo.mp4)