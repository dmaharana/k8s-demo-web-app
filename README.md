# download kind CLI
`curl -Lo ./kind "https://kind.sigs.k8s.io/dl/v0.17.0/kind-$(uname)-amd64"`
`chmod +x kind`

# create kind kubernetes cluster
`kind create cluster --config ./kind-config/kind-2-node-config.yaml`

`kubectl cluster-info --context kind-kind`

# get ip address of nodes
`kubectl get nodes -o yaml | grep address`

# perform build
`docker build . -t k8s-demo-web-app:v1`

# load image into kind kubernetes cluster - "ENSURE the image has a tag other than latest"
`kind load docker-image k8s-demo-web-app:v1`

# update image name in the deployment file
`sed -i 's/k8s-demo-web-app:latest/k8s-demo-web-app:v1/g' k8s-demo-web-app-deployment.yaml`

# apply deployment and service
`kubectl apply -f ./k8s-files`

# access the application
`curl node_public_ip:node_port`

