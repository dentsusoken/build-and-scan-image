# FAQ

### How to ignore specific violations and vulnerabilities?
Some tools provide way to ignore specific violations and vulnerabilities.

- Configuration file
    - [`.hadolint.yaml`](https://github.com/hadolint/hadolint#configure)
    - [`.dockleignore`](https://github.com/goodwithtech/dockle#ignore-the-specified-checkpoints)
    - [`.trivyignore`](https://aquasecurity.github.io/trivy/latest/vulnerability/examples/filter/)
- Inline comment
    - [hadolint](https://github.com/hadolint/hadolint#inline-ignores)

See each tools' document for more details.

### Trivy show many vulnerabilities which doesn't have fixed version. How should I address it?
You can concider changing base images for other one which doesn't contain unnecessary packages. [distroless](https://github.com/GoogleContainerTools/distroless) images are strongly recommended. Unnecessary packages reduction help attack surface reduction and improve security. 

If you have already reduced unnecessary packages, you can set `trivy-ignore-unfixed` as `"true"` to ignore all of unfixed vulnerabilities.
