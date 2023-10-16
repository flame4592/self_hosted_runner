1) install cert manager https://artifacthub.io/packages/helm/cert-manager/cert-manager

2) get github pat ; scope :- repo , org ( for organization, enterprise for enterprise , also make sure to authorize the token after creation for particular organization )

3) Create ns actions

4) create secret :- 
    Kubectl create secret generic controller-manager -n actions --from-literal=github_token=<GitHub Token>

5) Adding Helm repo for arc
    helm repo add actions-runner-controller https://actions-runner-controller.github.io/actions-runner-controller

6) Update the repo 
    helm repo update

7) search for repo 
    helm search repo actions 

8) install arc 
    helm install runner \
    actions-runner-controller/actions-runner-controller \
    -n actions \
    --version <get version no from step 7 for latest> \
    --set syncPeriod=1m

9) Verify pods 
    kubectl get pods -n actions 

10) Install 01_runner.yaml
    kubectl apply -f 01_runner.yaml -n actions 

11) Install runner deployment 
    kuebctl apply -f 02_runner_deployment.yaml -n actions 

12) auto scaling runner 
    kubectl apply -f hpa.yaml -n actions 