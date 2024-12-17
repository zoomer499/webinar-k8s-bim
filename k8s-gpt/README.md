## Ollama install
helm repo add ollama-helm https://otwld.github.io/ollama-helm/
helm repo update
helm upgrade ollama ollama-helm/ollama --namespace ollama  --install --create-namespace -f ollama-default-values.yaml

## K8Sgpt install

helm repo add k8sgpt https://charts.k8sgpt.ai/
helm repo update
helm install release k8sgpt/k8sgpt-operator -n k8sgpt-operator-system --create-namespace -f k8sgpt-operator-default-values.yaml

##  K8sGPT and Configuring it to Use Ollama

```yaml
apiVersion: core.k8sgpt.ai/v1alpha1
kind: K8sGPT
metadata:
  name: k8sgpt-ollama
  namespace: k8sgpt-operator-system
spec:
  ai:
    enabled: true
    model: llama3
    backend: localai
    baseUrl: http://ollama.ollama.svc.cluster.local.:11434/v1 #internal service
  noCache: false
  # filters: ["Pod"]
  repository: ghcr.io/k8sgpt-ai/k8sgpt
  version: v0.3.41
```