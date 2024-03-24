# Trivy
Trivy is a simple and comprehensive vulnerability scanner for containers and other artifacts.


# Capabilites of Trivy scanner.



## Installation of trivy on ubuntu server
```bash
wget https://github.com/aquasecurity/trivy/releases/download/v0.18.3/trivy_0.18.3_Linux-64bit.deb
sudo dpkg -i trivy_0.18.3_Linux-64bit.deb
```

## To Scan the image
```bash
trivy image imagename3
trivy image --severity HIGH,CRITICAL nginx
```

## To output the data in Json file
```
trivy image -f json -o results.json  nginx
```

## To scan the YAML files (for k8s)
```bash
trivy fs --security-checks vuln,config  <path>
```

## Trivy scanning the git repos
```bash
trivy repo https://github.com/knqyf263/trivy-ci-test
```

## Trivy scanning local os path.
```bash
trivy fs /usr/bin/
```

 
