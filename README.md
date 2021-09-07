# k3d-istio

```
k3d cluster create hello \
    --servers 1 \
    --agents 3 \
    --port 9080:80@loadbalancer \
    --port 9443:443@loadbalancer \
    --api-port 6443 \
    --k3s-server-arg --no-deploy=traefik

kubectl config use-context k3d-hello

kubectl cluster-info
```

```
# istioctl install --skip-confirmation --set profile=demo
# istioctl manifest generate --set profile=demo > istio.yaml
kubectl create namespace istio-system
kubectl apply --filename istio.yaml

kubectl label namespace default istio-injection=enabled
```

```
kubectl apply --filename bookinfo.yaml

kubectl apply --filename bookinfo-gateway.yaml

istioctl analyze
```

```
JSONPATH_HOST='{.status.loadBalancer.ingress[0].ip}'
JSONPATH_PORT='{.spec.ports[?(@.name=="http2")].port}'

INGRESS_HOST=`kubectl get service istio-ingressgateway --namespace istio-system --output jsonpath="$JSONPATH_HOST"`
INGRESS_PORT=`kubectl get service istio-ingressgateway --namespace istio-system --output jsonpath="$JSONPATH_PORT"`

echo "http://$INGRESS_HOST:$INGRESS_PORT/productpage"
```

```
kubectl apply --filename prometheus.yaml

kubectl apply --filename kiali.yaml

istioctl dashboard kiali &

for i in `seq 100`; do
    curl --silent --output /dev/null "http://$INGRESS_HOST:$INGRESS_PORT/productpage"
done
```

## Notes

```
istioctl profile dump demo
```

## References

* [k3d - Kubernetes & Istio (Service Mesh)](https://brettmostert.medium.com/k3d-kubernetes-istio-service-mesh-286a7ba3a64f)
* [istio - Getting Started](https://istio.io/latest/docs/setup/getting-started/)
