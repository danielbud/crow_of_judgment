
# Crow of Judgment
Restricting OnePasswordItem names to follow naming convention: 1password-namespace-service  


# Steps
```sh
# Create cluster
kind create cluster

# Install kyverno
helm repo add kyverno https://kyverno.github.io/kyverno/
helm repo update

kubectl create ns kyverno-system
kubens kyverno-system
helm upgrade --install kyverno kyverno/kyverno \
    --namespace kyverno-system \
    --wait


# Apply OnePassword fake
kubectl apply -f crd.yaml

# Apply Kyverno policy
kubectl apply -f policy.yaml

# Switch to default namespace
kubens default

# Apply OnePasswordItem with good name
kubectl apply -f 1item-good.yaml

# Apply OnePasswordItem with bad name
kubectl apply -f 1item-bad.yaml

# >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
# Error from server: error when creating "1item-bad.yaml": admission webhook "validate.kyverno.svc-fail" denied the request:

# resource OnePasswordItem/default/1password-badnamespace-something-service was blocked due to the following policies

# enforce-onepassword-names:
#   enforce-onepassword-names: 'validation error: Invalid OnePasswordItem name, use
#     pattern 1password-namespace-servicename. Rule enforce-onepassword-names failed
#     at path /metadata/name/'
```

