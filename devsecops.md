# DevSecOps

## trivy

Tool to check vulnerabilities in docker images, git repo, filesystems, etc.

* <https://trivy.dev/>

Scan a docker image:

``` bash
trivy image alpine:latest
```

Scan a git repository:

``` bash
trivy repo /opt/my_application/
# or using the trivy docker image:
docker run -v /opt/my_application/:/scan aquasec/trivy repo /scan
```

## DepFuzzer

Tool from Synactiv to check for *dependency confusion* vulnerabilities in code repositories.

* <https://github.com/synacktiv/DepFuzzer>

Usage for a python project:

``` bash
git clone https://github.com/synacktiv/DepFuzzer
cd DepFuzzer
docker build -t depfuzzer:latest .
docker run --rm -it -v /opt/my_application/:/host depfuzzer --provider pypi --path /host
```
