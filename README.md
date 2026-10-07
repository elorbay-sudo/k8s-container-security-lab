# \# Kubernetes Container Security Lab

# 

# Hands-on lab deploying containerized workloads to Kubernetes, scanning them for vulnerabilities and misconfigurations with Trivy, and remediating findings.

# 

# \## Tools

# Docker Desktop, Kubernetes, kubectl, Trivy (Aqua Security)

# 

# \## What I Did

# 1\. Deployed an outdated nginx image (`nginx:1.19`) to a local Kubernetes cluster as a 2-replica Deployment with a Service, isolated in its own namespace.

# 2\. Scanned the image with Trivy for HIGH and CRITICAL vulnerabilities.

# 3\. Remediated by upgrading to `nginx:stable-alpine` and performing a rolling update with no downtime.

# 4\. Rescanned to validate the fix.

# 5\. Scanned the Kubernetes manifest for misconfigurations and hardened it by adding CPU and memory resource limits.

# 

# \## Results

# 

# \### Image Vulnerabilities (HIGH + CRITICAL)

# | | Before (`nginx:1.19`) | After (`nginx:stable-alpine`) |

# |---|---|---|

# | Critical | 42 | 0 |

# | High | 147 | 2 |

# | \*\*Total\*\* | \*\*189\*\* | \*\*2\*\* |

# 

# \*\*99% reduction; all critical vulnerabilities eliminated.\*\*

# 

# The original image also ran on Debian 10, which is end-of-life and no longer receives security patches. Upgrading the base image addressed both the known CVEs and the unsupported-OS risk.

# 

# \### Manifest Misconfigurations

# Scanned the Kubernetes manifest with `trivy config` and hardened it by adding CPU and memory resource limits and requests, which prevents a single container from exhausting node resources (a denial-of-service risk). 13 findings remain after hardening; see Next Steps.

# 

# \## Remaining Findings \& Next Steps

# \- 2 HIGH vulnerabilities remain in the updated image; track them for the next upstream patch.

# \- 13 manifest misconfigurations remain, primarily around privilege and filesystem settings (such as running as root). These could be addressed by switching to a non-root image like `nginxinc/nginx-unprivileged` and adding a `securityContext` (runAsNonRoot, readOnlyRootFilesystem, dropped capabilities).

# 

# \## Reports

# Full scan output is in the `reports/` folder.

