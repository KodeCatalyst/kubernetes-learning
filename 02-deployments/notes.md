# Deployments

## Why not just use Pods directly?
If you create a Pod directly and it dies, it's just gone. Nothing brings it back.
You'd have to manually recreate it every time. That's not sustainable in
production where Pods die all the time.

## What a Deployment gives 
- **Self-healing** — When pod dies, Deployment notices and creates a replacement automatically
- **Scaling** — just change the replicas number and apply.
- **The kube-controller-manager** is what watches the cluster and notices when
  reality doesn't match desired state. It's the thing that triggered the new Pod
  when one was manually deleted.

## How it works
A Deployment has two key things:
- `replicas` — how many Pods should always be running
- `template` — the Pod blueprint it uses when it needs to create a new one

The `selector.matchLabels` is how the Deployment knows which Pods belong to it.
It has to match the labels in the template — if they don't match, the Deployment
can't track its own Pods.

## The naming pattern
Pods created by a Deployment get auto-generated names:

nginx-deployment-7ccccd94f7-9jjnx

└── deployment name  └── replicaset id  └── pod id

## Deployment + Service
The Service doesn't know or care about the Deployment. It just finds Pods matching
its selector (app=nginx). So as long as the Deployment creates Pods with that label,
the Service automatically load balances across all of them.

When I scaled from 3 to 5 replicas, the Service immediately started routing to
all 5 without any changes. When we scaled back to 3, it stopped routing to the
terminated ones. I changed nothing in the Service.
