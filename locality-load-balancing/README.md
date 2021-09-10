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
```
