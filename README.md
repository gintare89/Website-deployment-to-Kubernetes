This project is used to deploy https://github.com/mhmdomer/ecommerce-laravel monolitic Laravel webiste to Kubernetes, add
Prometehus and Grafana monitoring and configure HPA.

Requirements:
1. Have 3 servers: for control plane - server with public IP, for two worker nodes - server with private IP.
2. SSH into servers
3. On main server install ansible and run k8s_install files (before changing vars)
4. Check if kubernetes created namespaces with: 
kubectl get ns
5. Generate app key and put it in helm_files/values.yaml
6. Run first ansible playbook deploy/install_helm.yaml
7. In Helm website folder delete all files, exept Chart.yaml and copy helm_files to webiste dir using: 
cp -r /your/path/helm_files/* /new/path/website/
8. Run deploy_app.yaml
9. Check if website pods working
kubectl get pods -n ecommerce
