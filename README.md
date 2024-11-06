# Build and Scan Image

[![test workflow](https://github.com/dentsusoken/build-and-scan-image/actions/workflows/test.yaml/badge.svg)](https://github.com/dentsusoken/build-and-scan-image/actions/workflows/test.yaml)

Build and scan Dockerfile and container image by following tools.

- [hadolint](https://github.com/hadolint/hadolint)
- [dockle](https://github.com/goodwithtech/dockle)
- [trivy](https://github.com/aquasecurity/trivy)
- [Snyk](https://snyk.io/) (option)

## Notes

### Experimental
This repository is experimental. Some options or behavior may be changed without announcement.

### Use in Dentsu Soken project
If you use this repository in Dentsu Soken project, please read [dentsusoken/build-and-scan-image-internal](https://github.com/dentsusoken/build-and-scan-image-internal) too. It can be accessed by only Dentsu Soken members.

## Usage

### Basic

```yaml
- name: Build and scan image
  uses: dentsusoken/build-and-scan-image@main
  with:
    tag: "YOUR_IMAGE_NAME:TAG"  # Image name and optionally tag in "name:tag" format
    path: "."  # Path to base directory to run `docker build` command
```

### All options

See [action.yaml](./action.yaml) .

```yaml
- name: Build and scan image
  uses: dentsusoken/build-and-scan-image@main
  with:
    # Image name and optionally tag in "name:tag" format
    tag: "YOUR_IMAGE_NAME:TAG"

    # Path to base directory to run `docker build` command (default ".")
    path: "."

    # Path to Dockerfile, which is relative path from "path" parameter (default "Dockerfile")
    dockerfile: Dockerfile

    # Enable scanning Dockerfile by hadolint (default "true")
    hadolint-enable: "true"

    # Hadolint version.
    hadolint-version: "2.8.0"

    # Fail step if rules with a severity above this level are violated (default "info")
    # Acceptable value is one of (error|warning|info|style|ignore|none)
    hadolint-severity: info

    # Enable scanning image by dockle (default "true")
    dockle-enable: "true"

    # Dockle version.
    dockle-version: "0.4.3"

    # Fail step if checkpoints with a severity above this level are violated (default "WARN")
    # Acceptable value is one of (INFO|WARN|FATAL)
    dockle-severity: WARN

    # Enable scanning image by trivy (default "true")
    trivy-enable: "true"

    # Trivy version.
    trivy-version: "0.23.0"

    # Fail step if image has vulnerabilities with a severity same as this level
    # (default "UNKNOWN,LOW,MEDIUM,HIGH,CRITICAL")
    # Acceptable value is comma-separated list of (UNKNOWN|LOW|MEDIUM|HIGH|CRITICAL)
    trivy-severity: UNKNOWN,LOW,MEDIUM,HIGH,CRITICAL

    # Vulnerability types which trivy detect to (default "os,library")
    # Acceptable value is comma-separated list of (os|library)
    trivy-vuln-type: os,library

    # Ignore unfixed vulnerabilities (default "false")
    trivy-ignore-unfixed: "false"

    # OCI repository(ies) to retrieve trivy-db in order of priority
    trivy-db-repository: "ghcr.io/aquasecurity/trivy-db:2,public.ecr.aws/aquasecurity/trivy-db:2"

    # OCI repository(ies) to retrieve trivy-java-db in order of priority
    trivy-java-db-repository: "ghcr.io/aquasecurity/trivy-java-db:1,public.ecr.aws/aquasecurity/trivy-db:1"

    # Enable scanning image by snyk (default "false")
    # If enabled, "snyk-token" must be also set.
    snyk-enable: "false"

    # Snyk CLI version.
    snyk-version: "1.848.0"

    # Snyk API Token (default "")
    # This is necessary if "snyk-enable" is "true".
    snyk-token: ""

    # Fail step if image has vulnerabilities with a severity above this level (default "low")
    # Acceptable value is one of (low|medium|high|critical)
    snyk-severity: low

    # Exclude base image vulnerabilities (default "false")
    snyk-exclude-base-image-vulns: "false"
```

## FAQ
See [FAQ](./docs/faq.md).
