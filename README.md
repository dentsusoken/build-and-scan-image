# Build and Scan Image

[![test workflow](https://github.com/ISID/build-and-scan-image/actions/workflows/test.yaml/badge.svg)](https://github.com/ISID/build-and-scan-image/actions/workflows/test.yaml)

Build and scan Dockerfile and container image by following tools.

- [hadolint](https://github.com/hadolint/hadolint)
- [dockle](https://github.com/goodwithtech/dockle)
- [trivy](https://github.com/aquasecurity/trivy)
- [docker scan](https://github.com/docker/scan-cli-plugin) (option)
    - This use [Snyk](https://snyk.io/) internally.

## Notes

### Experimental
This repository is experimental. Some options or behavior may be changed without announcement.

### Responsibility
This repository is provided by MIT License. Please see [LICENSE](./LICENSE) for more details.

You, including projects in ISID, must take all responsibility caused by using this action.

Especially, this action uses some forementioned tools and service internally. You must understand it and take all responsibility about it too.

### Commercial usage
Some of trivy's data sources, especially for programing language, are only licensed for non-commercial usage. See following sites for more details.

- https://github.com/aquasecurity/trivy/blob/main/docs/vulnerability/detection/data-source.md
- https://github.com/aquasecurity/trivy/issues/491

If you use this action for commercial usage, you MUST set `trivy-vuln-type` option as `os` and use trivy without non-commercial data sources.

### For ISID members
If you are ISID member, please read [ISID/build-and-scan-image-internal](https://github.com/ISID/build-and-scan-image-internal) too. It can be accessed by only ISID members.

## Usage

### Basic (using deploy key)

```yaml
- uses: actions/checkout@v2  # Checkout your repository which contains Dockerfile.
- uses: actions/checkout@v2  # Checkout this repository.
  with:
    repository: ISID/build-and-scan-image
    ssh-key: ${{ secrets.GH_DEPLOY_KEY }}  # GH_DEPLOY_KEY is a secret that contains deploy key for this repository
    path: ./build-and-scan-image
- name: Build and scan image  # Build and scan Dockerfile and container image
  uses: ./build-and-scan-image
  with:
    tag: "YOUT_IMAGE_NAME:TAG"
```

Please ask administrator of this repository to get deploy key.

### Basic (using personal access token)

```yaml
- uses: actions/checkout@v2  # Checkout your repository which contains Dockerfile.
- uses: actions/checkout@v2  # Checkout this repository.
  with:
    repository: ISID/build-and-scan-image
    token: ${{ secrets.GH_TOKEN }}  # GH_TOKEN is a secret that contains your personal access token.
    path: ./build-and-scan-image
- name: Build and scan image  # Build and scan Dockerfile and container image
  uses: ./build-and-scan-image
  with:
    tag: "YOUT_IMAGE_NAME:TAG"
```

### All options

See [action.yaml](./action.yaml) .

```yaml
- name: Build and scan image
  uses: ./build-and-scan-image
  with:
    # Image name and optionally tag in "name:tag" format
    tag: "YOUT_IMAGE_NAME:TAG"

    # Path to base directory to run `docker build` command (default ".")
    path: "."

    # Path to Dockerfile, which is relative path from "path" parameter (default "Dockerfile")
    dockerfile: Dockerfile

    # Enable scanning Dockerfile by hadolint (default "true")
    hadolint-enable: "true"

    # Hadolint version (see action.yaml to know default version)
    hadolint-version: "0.0.1"

    # Fail step if rules with a severity above this level are violated (default "info")
    # Acceptable value is one of (error|warning|info|style|ignore|none)
    hadolint-severity: info

    # Enable scanning image by dockle (default "true")
    dockle-enable: "true"

    # Dockle version (see action.yaml to know default version)
    docke-version: "0.0.1"

    # Fail step if checkpoints with a severity above this level are violated (default "WARN")
    # Acceptable value is one of (INFO|WARN|FATAL)
    dockle-severity: WARN

    # Enable scanning image by trivy (default "true")
    trivy-enable: "true"

    # Trivy version (see action.yaml to know default version)
    trivy-version: "0.0.1"

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

    # Enable scanning image by docker scan (default "false")
    # If enabled, "docker-scan-snyk-token" must be also set. 
    docker-scan-enable: "false"

    # Docker scan version (see action.yaml to know default version)
    docker-scan-version: "0.0.1"

    # Fail step if image has vulnerabilities with a severity above this level (default "low")
    # Acceptable value is one of (low|medium|high)
    docker-scan-severity: low

    # Snyk API Token (default "")
    # This is necessary if "docker-scan-enable" is "true".
    docker-scan-snyk-token: ""
```

### Ignore specific violations and vulnerabilities
Tools ([hadolint](https://github.com/hadolint/hadolint), [dockle](https://github.com/goodwithtech/dockle), [trivy](https://github.com/aquasecurity/trivy)) provide some way to ignore specific violations and vulnerabilities.

- Using configuration file
    - `.hadolint.yaml`
    - `.dockleignore`
    - `.trivyignore`
- Using inline comment (hadolint only)

See document of each tools for more details.
