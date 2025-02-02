---
title: jx verify ingress
linktitle: ingress
type: docs
description: "Verifies the ingress configuration defaulting the ingress domain if necessary"
aliases:
  - jx-verify_ingress
---

### Usage

```
jx verify ingress
```

### Synopsis

Verifies the ingress configuration defaulting the ingress domain if necessary

### Examples

  ```bash
  # populate the ingress domain if not using a configured 'ingress.domain' setting
  jx verify ingress%!(EXTRA string=jx-verify)

  ```
### Options

```
  -d, --dir string                 the directory to look for the values.yaml file (default ".")
  -h, --help                       help for ingress
      --ingress-namespace string   The namespace for the Ingress controller (default "nginx")
      --ingress-service string     The name of the Ingress controller Service (default "ingress-nginx-controller")
```



### Source

[jenkins-x-plugins/jx-verify](https://github.com/jenkins-x-plugins/jx-verify)
