# Repositorio para instalación y configuración de un WP existente en instancias EC2 dentro de AWS

# Master (alias control-plane):
1.	Permisos al sh  ``` chmod u+x master.sh ``` 
2.	Ejecutarlo con  ``` sh ./master.sh ``` 
3.	Iniciar el Master con la sentencia  ``` sudo kubeadm init ``` 
4.	Ejecutar las siguientes 3 sentencias
 ``` 
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config	
 ``` 
5.	Te dara una sentencia que se debera copiar e implementar en los nodos Ejem:   ``` kubeadm join xxxx ``` 
6.	Comprobar que se instalo correctamente con
 ``` sudo kubectl -n kube-system get pod --kubeconfig /etc/kubernetes/admin.conf ``` 
7.	Ejecutar sentencia
 ``` kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml ``` 

# Nodo 1 (alias Worker 1)
1.	Permisos al sh  ``` chmod u+x worker1.sh ``` 
2.	Ejecutarlo con  ``` sh ./worker1.sh ``` 
3.	Ejecutar la sentencia del paso 5 del master.
4.	Ejecutar sentencia
 ``` kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml ``` 
5.	Comprobar desde el Master con “kubectl -n kube-system get pod --kubeconfig /etc/kubernetes/admin.conf” si hay conexión del nodo con el master.

# Nodo 2 (alias Worker 2)
1.	Permisos al sh  ``` chmod u+x worker2.sh ``` 
2.	Ejecutarlo con  ``` sh ./worker2.sh ``` 
3.	Ejecutar la sentencia del paso 5 del master.
4.	Ejecutar sentencia
 ``` kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml ``` 
5.	Comprobar desde el Master con  ``` kubectl -n kube-system get pod --kubeconfig /etc/kubernetes/admin.conf ```  si hay conexión del nodo con el master.

# Instalación del controlador de EFS de AWS en un ubuntu 20/22
```sudo apt -y install git binutils
git clone https://github.com/aws/efs-utils
cd efs-utils
./build-deb.sh
sudo apt -y install ./build/amazon-efs-utils*deb
```

# Sentencias utiles de K8S
1.	Eliminar el pod creado 
```kubectl delete -f . --kubeconfig /etc/kubernetes/admin.conf```
2.	Conocer los pod en ejecución
```kubectl get pod -o wide```
```kubectl -n kube-system get pod --kubeconfig /etc/kubernetes/admin.conf```
3.	Conocer los servicios en ejecución
```kubectl get svc```
4.	Entrar en un contenedor dentro del pod
```kubectl exec --stdin --tty (nombre del pod) -- /bin/bash```
5.	Conocer los deployment en ejecución
```kubectl get deployments -o wide```

# Pasos para desplegar el Wordpress (o cualquier otro servicio web)
Modificar secret.yaml con los datos de la BD en BASE64 y en wp.yaml donde se deben indicar los siguientes datos:
  1. Linea 22: La direccion dns del rds:puerto del rds (ejemplo: mysql.aws.com:3306)
  2. Linea 44: La dirección dns del EFS (ejemplo: efs.aws.com)
  3. Linea 59: La dirección IP interna de la instancia (normalmente comienza con 172.xxx.xxx.xxx)





