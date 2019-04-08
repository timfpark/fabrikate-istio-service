# fabrikate-istio-service

A Fabrikate component that encapsulates the reuseable portions of a tiered Istio service deployment, including VirtualService, Service, Deployment, and DestinationRule.

## Getting Started

From your [Fabrikate](https://github.com/Microsoft/fabrikate) definition at the level at which you'd like to add a service subcomponent, use `fab add` to add `fabrikate-istio-service` for your microservice:

```
$ fab add my-service --source https://github.com/timfpark/fabrikate-istio-service
```

then create a deployment config for each of your environments with one or more tiers. For example, a `prod` deployment config with canary and stable tiers would live in `config/prod.yaml` and look something like this:

```
subcomponents:
  my-service:
    namespace: services
    config:
      gateway: my-ingress.istio-system.svc.cluster.local
      service:
        dns: myservice.mycompany.io
        name: my-service
        port: 80
      tiers:
        canary:
          image: "mycompany/mycontainer:434"
          replicas: 1
          weight: 10
          port: 80
          resources:
            requests:
              cpu: "250m"
              memory: "256Mi"
            limits:
              cpu: "1000m"
              memory: "512Mi"
        stable:
          image: "mycompany/mycontainer:433"
          replicas: 3
          weight: 90
          port: 80
          resources:
            requests:
              cpu: "250m"
              memory: "256Mi"
            limits:
              cpu: "1000m"
              memory: "512Mi"
```