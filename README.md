# tinyproxy-docker

Simple forward proxy using [tinyproxy](https://github.com/tinyproxy/tinyproxy)

[![dockerhub](https://img.shields.io/badge/rainist-tinyproxy-blue.svg?logo=docker)](https://hub.docker.com/r/rainist/tinyproxy) [![love](https://img.shields.io/badge/%3C%2F%3E%20with%20%E2%99%A5%20by-Rainist-blue.svg)](https://github.com/Rainist)

## Setup

### on Kubernetes

1. Create configmap

    ```yaml
    apiVersion: v1
    kind: ConfigMap
    data:
      tinyproxy.conf: |
        Port 8000
        Listen 0.0.0.0

        Upstream proxy-host:proxy-port

        MaxClients 100
        MinSpareServers 1
        MaxSpareServers 5
        StartServers 1
        MaxRequestsPerChild 0
    metadata:
      name: proxy-config
      namespace: namespace
    ```

1. Mount configmap to `/etc/tinyproxy`

    ```json
    "spec": {
        "volumes": [
          {
            "name": "proxy-config",
            "configMap": {
              "name": "proxy-config",
              "defaultMode": 420
            }
          }
        ],
        "containers": [
          {
            "name": "proxy",
            "image": "rainist/tinyproxy:1.8.4-r3",
            "volumeMounts": [
              {
                "name": "proxy-config",
                "mountPath": "/etc/tinyproxy"
              }
            ],
            ...
          }
        ],
        ...
    }
    ```
