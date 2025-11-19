![Static Badge](https://img.shields.io/badge/%E7%BC%96%E7%A8%8B-%E7%AC%94%E8%AE%B0-green)


![Alt](https://repobeats.axiom.co/api/embed/595f9849e48bbe3d474acbf95db4dbb98826d58a.svg "Repobeats analytics image")

## Git

```cmd
git config --global https.proxy "http://127.0.0.1:7890"

git config --global http.proxy "http://127.0.0.1:7890"



git config --global --unset https.proxy 

git config --global --unset http.proxy 
```

## Docker 

### PAT(Personal Access Tokens)

Use this token as a password when you sign in from the Docker CLI client. [Learn more⁠](https://docs.docker.com/docker-hub/access-tokens/)

**Make sure you copy your personal access token now. Your personal access token is only displayed once. It isn't stored and can't be retrieved later.**

**Access token description**

组件使用

**Expires on**

Feb 17, 2026 at 23:59:59

**Access permissions**

Read, Write, Delete

**To use the access token from your Docker CLI client:**

1. Run

```
docker login -u waterplants321
```

2. At the password prompt, enter the personal access token.

```
dckr_pat_In8uOrtm4MTirXD_1-Q9w0KiCso
```

### Docker Engine配置

```java
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false,
  "proxies": {
    "http-proxy": "http://127.0.0.1:7890",
    "https-proxy": "http://127.0.0.1:7890"
  },
  "registry-mirrors": [
    "https://hlgjbozg.mirror.aliyuncs.com",
    "https://ccr.ccs.tencentyun.com",
    "https://docker.rainbond.cc",
    "https://elastic.m.daocloud.io",
    "https://docker.m.daocloud.io",
    "https://gcr.m.daocloud.io",
    "https://ghcr.m.daocloud.io",
    "https://k8s-gcr.m.daocloud.io",
    "https://k8s.m.daocloud.io",
    "https://mcr.m.daocloud.io",
    "https://nvcr.m.daocloud.io",
    "https://quay.m.daocloud.io"
  ]
}
```

