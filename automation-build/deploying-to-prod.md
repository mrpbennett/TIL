# Deploying to prod

Things to note:

The `containerPort` and `targetPort` need to be the port that is in your `CMD` within your Docker file so for example:

```Dockerfile
CMD ["uvicorn", "main:app", "--proxy-headers", "--host", "0.0.0.0", "--port", "80"]
```

The port here is `80`. Therefore you would need to change `containerPort` and `targetPort` to `80`

### Deployment

`tam/deployments/<app name>_deployment.yaml`

The below config file is what's needed to deploy the application. If the `CMD` in the Docker file is what you want, you can skip the command and args overrides.

To amend swap out `<app name>` for the name of your application. Replace the `image` with the applications image.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <app name>
  namespace: tam
  annotations:
    fluxcd.io/automated: 'true'
  labels:
    app: <app name>
spec:
  selector:
    matchLabels:
      app: <app name>
  template:
    metadata:
      labels:
        app: <app name>
    spec:
      containers:
      - name: <app name>
        image: registry.pulsepoint.com/tam/exchange-debug-ui:0.0.4

        # the command & args are not needed if CMD in Docker is what you need
        command:
        - "vite"
        args:
        - "preview"
        - "--port"
        - "3000"
        - "--host"
        - "0.0.0.0"
        ports:
        - containerPort: 3000
```

### Services

`tam/services/<app name>_svc.yaml`

Now to add some services so we can expose our app, below is the template and we would need to adjust `app` with the correct naming for your app

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    name: <app name>
  name: <app name>
  namespace: tam
spec:
  type: ClusterIP
  selector:
    app: <app name> # this is the label on the pod
  ports:
    - name: http
      port: 80
      protocol: TCP
      targetPort: 3000
```

### Routing traffic

`tam/ingresses/<app name>_ingress.yaml`

Now to route traffic via ingress controller to pods in the applications service. For this we need to change the `hosts` this can be anything but should come with a prefix of `lga`

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
  name: <app name>
  namespace: tam
spec:
  rules:
    - host: <app name>.data-analytics.pulse.prod
      http:
        paths:
          - backend:
              serviceName: <app name>
              servicePort: 80
    - host: lga-<appname>.pulsepoint.com # eg: lga-sometool.pulsepoint.com
      http:
        paths:
          - backend:
              serviceName: <app name>
              servicePort: 80
  tls:
    - hosts:
        - lga-<appname>.pulsepoint.com # eg: lga-sometool.pulsepoint.com

```