# Helm chart template

oc adm policy add-scc-to-user privileged -z argocd-redis-ha -n argocd
oc adm policy add-scc-to-user privileged -z argocd-redis-ha-haproxy -n argocd