# kubernetes-ingress

```
k3d cluster create region-1 \
    --servers 1 \
    --agents 1 \
    --port 9080:80@loadbalancer \
    --k3s-server-arg --no-deploy=traefik

istioctl install --skip-confirmation --set profile=demo

kubectl label namespace default istio-injection=enabled
kubectl apply --filename httpbin.yaml
```

```
JSONPATH_HOST='{.status.loadBalancer.ingress[0].ip}'
JSONPATH_PORT='{.spec.ports[?(@.name=="http2")].port}'

INGRESS_HOST=`kubectl get service istio-ingressgateway --namespace istio-system --output jsonpath="$JSONPATH_HOST"`
INGRESS_PORT=`kubectl get service istio-ingressgateway --namespace istio-system --output jsonpath="$JSONPATH_PORT"`

echo "http://$INGRESS_HOST:$INGRESS_PORT/"
```

```
kubectl apply --filename ingress.yaml

curl "http://$INGRESS_HOST:$INGRESS_PORT/status/200" \
    --silent --head \
    --header 'Host: httpbin.example.com'
```
