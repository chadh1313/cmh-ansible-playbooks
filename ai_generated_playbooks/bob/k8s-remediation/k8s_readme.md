# Kubernetes Diagnostics and Remediation Playbook

This Ansible playbook provides automated diagnostics and remediation for common Kubernetes cluster issues. It uses Ansible's native k8s modules to interact with the Kubernetes API, making it more reliable and maintainable than shell-based approaches.

## Features

The playbook addresses the following common Kubernetes issues:

- **Node Issues**
  - Identifies nodes not in Ready state
  - Detects nodes with pressure conditions (Memory/Disk/PID)
  - Monitors node resource usage
  - Optionally cordons problematic nodes

- **Pod Issues**
  - Identifies pods not in Running state
  - Detects pods with excessive restarts
  - Analyzes pod logs for errors
  - Optionally deletes pods in CrashLoopBackOff state

- **Deployment Issues**
  - Identifies deployments not at desired replica count
  - Detects deployment rollout failures
  - Optionally restarts problematic deployments

- **Storage Issues**
  - Identifies unbound PersistentVolumeClaims
  - Detects unbound PersistentVolumes
  - Analyzes storage provisioner health

- **Control Plane Health**
  - Checks status of control plane components
  - Monitors etcd cluster health
  - Verifies API server responsiveness

- **Network Diagnostics**
  - Checks CoreDNS/kube-dns health
  - Identifies network policy provider
  - Analyzes network policies

- **Resource Management**
  - Checks namespace resource quotas
  - Identifies limit ranges
  - Finds pods without resource limits

- **ConfigMap and Secret Validation**
  - Counts ConfigMaps and Secrets by namespace
  - Identifies potential configuration issues

## Prerequisites

- Ansible 2.9 or higher
- Python kubernetes module (`pip install kubernetes`)
- Valid kubeconfig with appropriate permissions
- For some remediation actions, cluster-admin privileges may be required

## Usage

### Basic Usage

```bash
ansible-playbook -i inventory k8s_diagnostics_remediation.yaml
```

### Targeting Specific Hosts

```bash
ansible-playbook -i inventory k8s_diagnostics_remediation.yaml -e "target_hosts=k8s_admin"
```

### Enabling Remediation

By default, the playbook only performs diagnostics without making changes. To enable remediation:

```bash
ansible-playbook -i inventory k8s_diagnostics_remediation.yaml -e "remediate_nodes=true remediate_pods=true remediate_deployments=true"
```

### Customizing Kubeconfig Path

```bash
ansible-playbook -i inventory k8s_diagnostics_remediation.yaml -e "kubeconfig_path=/path/to/kubeconfig"
```

### Enabling Automatic Remediation Actions

```bash
ansible-playbook -i inventory k8s_diagnostics_remediation.yaml -e "remediate_pods=true auto_delete_crashloop_pods=true"
```

## Configuration

All configurable parameters are defined in `k8s_vars.yaml`. You can modify these values to suit your environment:

- **Authentication Settings**
  - `kubeconfig_path`: Path to kubeconfig file
  - `become_root`: Whether to become root for operations

- **Remediation Settings**
  - `remediate_nodes`: Enable node remediation actions
  - `remediate_pods`: Enable pod remediation actions
  - `remediate_deployments`: Enable deployment remediation actions

- **Auto-remediation Flags**
  - `auto_cordon_nodes`: Automatically cordon problematic nodes
  - `auto_delete_crashloop_pods`: Automatically delete pods in CrashLoopBackOff
  - `auto_restart_deployments`: Automatically restart deployments with replica issues

- **Thresholds**
  - `pod_restart_threshold`: Number of restarts before flagging a pod
  - `cpu_usage_threshold`: CPU usage percentage threshold
  - `memory_usage_threshold`: Memory usage percentage threshold
  - `disk_usage_threshold`: Disk usage percentage threshold

## Output

The playbook provides detailed output for each diagnostic step and a summary at the end showing:

- Issues detected across different Kubernetes resources
- Actions taken (if remediation was enabled)
- Current cluster status

## Safety Features

- Remediation is disabled by default
- Auto-remediation actions require explicit opt-in
- The playbook can be run in read-only mode for diagnostics only
- All remediation actions are logged for audit purposes

## Extending the Playbook

You can extend this playbook by:

1. Adding new diagnostic checks in the appropriate section
2. Creating new remediation actions for specific issues
3. Adding support for additional Kubernetes resources
4. Implementing custom alerting or notification mechanisms

## Troubleshooting

If you encounter issues:

- Ensure the kubernetes Python module is installed
- Verify your kubeconfig has appropriate permissions
- Check that the target host can reach the Kubernetes API server
- For permission errors, ensure your user/service account has required RBAC permissions

// Made with Bob
