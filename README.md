# k8-client

Simplified [k8 API](http://kubernetes.io/) client for Node.js.

## Installation

Install via npm:

```
npm i kubernetes-client --save
```

## Initializing

kubernetes-client generates a Kubernetes API client at runtime based
on a Swagger / OpenAPI specification. You can generate a client using
the cluster's kubeconfig file and that cluster's API specification.

To create the config required to make a client, you can either:

let kubernetes-client configure automatically by trying the `KUBECONFIG`
environment variable first, then `~/.kube/config`, then an in-cluster
service account, and lastly settling on a default proxy configuration:

```js
const client = new Client({ version: "1.13" })
```

provide your own path to a file:

```js
const { KubeConfig } = require("kubernetes-client")
const kubeconfig = new KubeConfig()
kubeconfig.loadFromFile("~/some/path")
const Request = require("kubernetes-client/backends/request")

const backend = new Request({ kubeconfig })
const client = new Client({ backend, version: "1.13" })
```

provide a configuration object from memory:

```js
// Should match the kubeconfig file format exactly
const config = {
  apiVersion: "v1",
  clusters: [],
  contexts: [],
  "current-context": "",
  kind: "Config",
  users: []
}
const { KubeConfig } = require("kubernetes-client")
const kubeconfig = new KubeConfig()
kubeconfig.loadFromString(JSON.stringify(config))

const Request = require("kubernetes-client/backends/request")
const backend = new Request({ kubeconfig })
const client = new Client({ backend, version: "1.13" })
```

and you can also specify the context by setting it in the `kubeconfig`
object:

```js
kubeconfig.setCurrentContext("dev")
```

## License

[MIT](LICENSE)

[1]: https://swagger.io/specification/#pathItemObject
[2]: https://swagger.io/specification/#pathTemplating
