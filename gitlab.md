# GitLab
## GitLab-CI can't fetch code from a repo
#### issue
```
fatal: git fetch-pack: expected shallow list
fatal: The remote end hung up unexpectedly
```

#### solution
Gitlab WebUI -> "Settings -> CI / CD -> General pipelines":
  * "git clone"
  * Git shallow clone = 0

## Gitlab-runner can't connect to a repo with LE X3 SSL
#### issue 
gitlab-runner under Xen can't connect to a repo after 29.09.2021

Citrix XenServer uses OpenSSL old version

#### solution
Option 1
```
SERVER=gitlab.example.com
PORT=443
CERTIFICATE=/etc/gitlab-runner/certs/${SERVER}.crt

# Create the certificates hierarchy expected by gitlab
sudo mkdir -p $(dirname "$CERTIFICATE")

# Get the certificate in PEM format and store it
openssl s_client -connect ${SERVER}:${PORT} -showcerts </dev/null 2>/dev/null | sed -e '/-----BEGIN/,/-----END/!d' | sudo tee "$CERTIFICATE" >/dev/null

# Register your runner
gitlab-runner register --tls-ca-file="$CERTIFICATE" [your other options]
```
Or can download the certificate and add it with the option `--tls-ca-file`

Option 2
```
sudo -u gitlab-runner git config --global http.sslVerify false

```

## Upgrade
Can't upgrade thru several major version. Shut upgrade release-by_release:
```
# apt-cache madison gitlab-ce | head -n 20
 gitlab-ce | 14.2.3-ce.0 | https://packages.gitlab.com/gitlab/gitlab-ce/debian stretch/main amd64 Packages
 gitlab-ce | 14.1.5-ce.0 | https://packages.gitlab.com/gitlab/gitlab-ce/debian stretch/main amd64 Packages
 gitlab-ce | 14.0.10-ce.0 | https://packages.gitlab.com/gitlab/gitlab-ce/debian stretch/main amd64 Packages
# apt install gitlab-ce=14.0.10-ce.0
```

<https://docs.gitlab.com/ee/update/index.html#linux-packages-omnibus-gitlab>
