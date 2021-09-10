# locality-load-balancing

```
CTX_R1_Z1="region1-zone1"
CTX_R1_Z2="region1-zone2"
CTX_R2_Z3="region2-zone3"
CTX_R3_Z4="region3-zone4"

for CTX in "$CTX_R1_Z1" "$CTX_R1_Z2" "$CTX_R2_Z3" "$CTX_R3_Z4"; do
    k3d cluster create "$CTX" \
        --servers 1 \
        --agents 1 \
        --k3s-server-arg --no-deploy=traefik
done

for CTX in "$CTX_R1_Z1" "$CTX_R1_Z2" "$CTX_R2_Z3" "$CTX_R3_Z4"; do
    kubectl config use-context "k3d-$CTX"
    istioctl install --skip-confirmation --set profile=demo
done

for CTX in "$CTX_R1_Z1" "$CTX_R1_Z2" "$CTX_R2_Z3" "$CTX_R3_Z4"; do
    kubectl config use-context "k3d-$CTX"
    kubectl apply --filename sample.yaml
    kubectl apply --namespace sample --filename "helloworld-$CTX.yaml"
done

kubectl config use-context "k3d-$CTX_R1_Z1"
kubectl apply --namespace sample --filename sleep.yaml

CTX_PRIMARY="$CTX_R1_Z1"
kubectl config use-context "k3d-$CTX_PRIMARY"
kubectl apply --namespace sample --filename destinationrule.yaml

kubectl exec deployments/sleep \
    --namespace sample \
    -- curl -sSL http://helloworld.sample:5000/hello

kubectl exec deployments/helloworld-region1-zone1 \
    --namespace sample \
    --container istio-proxy \
    -- curl -sSL -X POST http://127.0.0.1:15000/drain_listeners

for CTX in "$CTX_R1_Z1" "$CTX_R1_Z2" "$CTX_R2_Z3" "$CTX_R3_Z4"; do
    k3d cluster delete "$CTX"
done
```
