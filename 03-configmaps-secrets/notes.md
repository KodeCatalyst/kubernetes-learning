# ConfigMaps & Secrets

ConfigMaps for non-sensitive config, Secrets for sensitive data like passwords/tokens/keys.

## Problem
- Credentials and secrets will otherwise have to be hardcoded into the image and new container images created anything a config or secret changes. 

- Also hard coding secrets into image isn't safe and can get compromised.



Configs and Secrets are passed into pods as env variables. The pods then fetches the values from the config or secret

Secrets are not stored as plain text. They are base64 auto encoded. But that is still not secure(By default secrets are not encrypted). You have to enable encryption at rest for etcd. However secrets in pods are still used as plain text even if they are stored as encrypted. 

Therefore the best way to protect a secret is to prevent unauthorised access to the pod.

