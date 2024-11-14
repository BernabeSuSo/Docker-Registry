Intsalar docker Registry

Se debe tener instalado docker-compose
Ejecutar docker compose: docker-compose.yaml

o en su defecto instalar manualmente: 

docker run -d -p 5000:5000 --restart=always --name local-registry registry:2
docker run -d -e ENV_DOCKER_REGISTRY_HOST=ENTER-YOUR-REGISTRY-HOST-HERE -e ENV_DOCKER_REGISTRY_PORT=ENTER-PORT-TO-YOUR-REGISTRY-HOST-HERE -p 8080:80 konradkleine/docker-registry-frontend:v2

Nota: el front end es opcional.



==== Configuracion en equipo remoto ====

En el /etc/hosts
IP'docker-registry dockerhub.local.com

En el /etc/docker/daemon.json (crearlo si no existe)
{
        "insecure-registries": ["dockerhub.local.com:5000"]
}

docker images

REPOSITORY                                             TAG       IMAGE ID       CREATED        SIZE
dockerhub.local.com:5000/bsulvaran/nginx-prueba        latest    fffffc90d343   3 months ago   188MB


probar: docker push dockerhub.local.com:5000/bsulvaran/nginx-prueba

NOTA: Tagear la imagen de esa forma: dockerhub.local.com:5000/bsulvaran/nginx-prueba


==== Configuracion en minikube, entra con minikube ssh (No es persistente el cambio)====
vi /usr/lib/systemd/system/docker.service
cambiar la ultima parte de la linea de abajo: provider=docker --insecure-registry dockerhub.local.com:5000:

ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2376 -H unix:///var/run/docker.sock --default-ulimit=nofile=1048576:1048576 --tlsverify --tlscacert /etc/docker/ca.pem --tlscert /etc/docker/server.pem --tlskey /etc/docker/server-key.pem --label provider=docker --insecure-registry dockerhub.local.com:5000

En el /etc/hosts
IP'docker-registry dockerhub.local.com

sudo systemctl daemon-reload
sudo systemctl restart docker



Listo ahora se puede usar tu imagen en los yaml: dockerhub.local.com:5000/bsulvaran/nginx-prueba
