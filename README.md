# Kuber_setups
To install the operator SDK, I firstly tryed to follow the steps of https://sdk.operatorframework.io/docs/install-operator-sdk/#install-from-github-release, but I run into mistake every time, I think mabe I should try to just install kubernetes first, so I followed kubernetes' official documents, here is what I have done:

### Make sure virtualization is available
use `grep -E --color 'vmx|svm' /proc/cpuinfo` to check if there is not output, shut down the machine, goto VMware's processors setting, there should be a virtualization engine.

### Install kubectl
1. Download the latest released version: `curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl`
2. Make the kubectl binary executable. `chmod +x ./kubectl`
3. Move the binary in to your PATH. `sudo mv ./kubectl /usr/local/bin/kubectl`
4. Test to ensure the version you installed is up-to-date `kubectl version --client`

### Install a Hypervisor
if you want your kubernetes run on host not on VM you can skip to Install Docker part
In my example I used KVM.
run `sudo apt install cpu-checker` and then `sudo kvm-ok` to check if your server can run kvm. If it does, you will see things like: `KVM acceleration can be used`run
```sudo apt update
sudo apt install qemu qemu-kvm libvirt-bin bridge-utils virt-manager
```
to install KVM and its dependency

### Install Minikube via direct download
use `curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube` to download a stand-alone binary

add the Minikube executable to your path
```
sudo mkdir -p /usr/local/bin/
sudo install minikube /usr/local/bin/
```

### Install Docker
Kubernetes also support Minikube run on host, if you want to do that you have to use the `.deb` package for docker.
1. go to `https://download.docker.com/linux/ubuntu/dists/` to choose version then browse to `pool/stable` then download the `.deb` file.
2. Install Docker Engine, changing the path below to the path where you downloaded the Docker package.
`sudo dpkg -i /path/to/package.deb`
3. Verify that Docker Engine is installed correctly by running the hello-world image.
`sudo docker run hello-world`

### Confirm Minikube installation
After you have done things before you can run `minikube start --driver=<driver_name>` to check your installation, if you are using local `--driver=none`.
Run the command below to check the status of the cluster:
`minikube status`
If you want to stop cluster, run `minikube stop`

### Run kubernetes locally
1. start minikube to create a cluster: `minikube start`
2. Now, you can interact with your cluster using kubectl.
Now create a Kubernetes Deployment using an existing image named echoserver, which is a simple HTTP server and expose it on port 8080 using --port.
```
kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.10
```
The output is similar to this: `deployment.apps/hello-minikube created`

3. To access the hello-minikube Deployment, expose it as a Service: `kubectl expose deployment hello-minikube --type=NodePort --port=8080`
4. The hello-minikube Pod is now launched but you have to wait until the Pod is up before accessing it via the exposed Service.  Check if the Pod is up and running: `kubectl get pod`
5. Get the URL of the exposed Service to view the Service details: `minikube service hello-minikube --url`
in my case my webpage looks like this:
