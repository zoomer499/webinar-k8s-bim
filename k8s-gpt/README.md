## Ollama install
helm repo add ollama-helm https://otwld.github.io/ollama-helm/
helm repo update
helm upgrade ollama ollama-helm/ollama --namespace ollama  --install --create-namespace -f ollama-default-values.yaml
helm delete ollama --namespace ollama
## K8Sgpt install

helm repo add k8sgpt https://charts.k8sgpt.ai/
helm repo update
helm install release k8sgpt/k8sgpt-operator -n k8sgpt-operator-system --create-namespace -f k8sgpt-operator-default-values.yaml
helm delete release --namespace k8sgpt-operator-system 

##  K8sGPT and Configuring it to Use Ollama

```yaml
apiVersion: core.k8sgpt.ai/v1alpha1
kind: K8sGPT
metadata:
  name: k8sgpt-ollama
  namespace: k8sgpt-operator-system
spec:
  ai:
    anonymized: true
    enabled: true
    model: llama3
    backend: localai
    baseUrl: https://ollama.dev.bimeister.io/v1 #internal service
  noCache: false
  # filters: ["Pod"]
  repository: ghcr.io/k8sgpt-ai/k8sgpt
  version: v0.3.41
```

apiVersion: core.k8sgpt.ai/v1alpha1
kind: K8sGPT
metadata:
  name: k8sgpt-sample-localai
  namespace: k8sgpt-operator-system
spec:
  ai:
    model: ggml-gpt4all-j-v1.3-groovy.bin
    backend: localai
    baseUrl: http://local-ai.local-ai.svc.cluster.local:8080/v1
    enabled: true
  version: v0.3.0
  noCache: false