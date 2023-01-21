# Build and Scan Image

[![test workflow](https://github.com/ISID/build-and-scan-image/actions/workflows/test.yaml/badge.svg)](https://github.com/ISID/build-and-scan-image/actions/workflows/test.yaml)

Build and scan Dockerfile and container image by following tools.

- [hadolint](https://github.com/hadolint/hadolint)
- [dockle](https://github.com/goodwithtech/dockle)
- [trivy](https://github.com/aquasecurity/trivy)
- [Snyk](https://snyk.io/) (option)

## Notes

### Experimental
This repository is experimental. Some options or behavior may be changed without announcement.

### Commercial usage
Some of trivy's data sources, especially for programing language, are only licensed for non-commercial usage. See following sites for more details.

- https://github.com/aquasecurity/trivy/blob/main/docs/vulnerability/detection/data-source.md
- https://github.com/aquasecurity/trivy/issues/491

If you use this action for commercial usage, you MUST set `trivy-vuln-type` option as `os` and use trivy without non-commercial data sources.

### Use in ISID project
If you use this repository in ISID project, please read [ISID/build-and-scan-image-internal](https://github.com/ISID/build-and-scan-image-internal) too. It can be accessed by only ISID members.

## Usage

### Basic

```yaml
- name: Build and scan image
  uses: ISID/build-and-scan-image@main
  with:
    tag: "YOUR_IMAGE_NAME:TAG"  # Image name and optionally tag in "name:tag" format
    path: "."  # Path to base directory to run `docker build` command
```

### All options

See [action.yaml](./action.yaml) .

```yaml
- name: Build and scan image
  uses: ISID/build-and-scan-image@main
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
    docke-version: "0.4.3"

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

    # Vulnerability types which trivy detect to (default "os")
    # Acceptable value is comma-separated list of (os|library)
    # If you use this action for commercial usage, you MUST set this option as "os" and use trivy without non-commercial data sources.
    trivy-vuln-type: os

    # Ignore unfixed vulnerabilities (default "false")
    trivy-ignore-unfixed: "false"

    # Enable scanning image by snyk (default "false")
    # If enabled, "snyk-token" must be also set.
    syn-enable: "false"

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
