## Exercício 2 - Kubernetes
1. Atividade 1 - múltiplos nginx

	- [x] Crie um namespace chamado `labnginx`
		* Namespace criado pelo arquivo `labnginx-namespace.yaml`
		* `$ kubectl apply -f labnginx-namespace.yaml`
	- [x] Faça o apply de dois pods nginx neste namespace, dentro de um único ingress, onde o Load balancer distribuirá a carga entre estes dois pods
		*  Deployment dos pods através de réplica e do serviço pelo arquivo `labnginx-deployment.yaml`
		* `$ kubectl apply -f labnginx-deployment.yaml`
		*  Deployment do ingress habilitando previamente no Minikube com o comando `$ minikube addons enable ingress` e posteriormente pelo arquivo `labnginx-ingress.yaml`
		* `$ kubectl apply -f labnginx-ingress.yaml`
	- [x] Abra o browser e faça o teste no seu navegador se a tela padrão do nginx abrirá para você
		* Êxito na exibição da tela padrão do nginx através do link [http://labnginx.teste](http://labnginx.teste) onde se foi mapeado o endereço do Node no arquivo `/etc/hosts`, onde no exemplo realizado tem-se:
			* `192.168.39.29 labnginx.teste`
	- [x] Faça um describe dos pods Nginx criados por você. Qual o IP que foi estabelecido para cada pod?
		* IP varia conforme a exclusão e criação dos pods. No exemplo realizado têm-se:
		* `$ kubectl describe pods nginx-585449566-67bsx`
			* Resultado: `nginx-585449566-67bsx 172.17.0.3`
		* `$ kubectl describe pods nginx-585449566-fbrt5`
			* Resultado: `nginx-585449566-fbrt5 172.17.0.2`
	- [x] Verifique logs dos dois pods Nginx criados por você, que informações são mostradas?
		* Primeiramente, pode-se verificar os logs com a flag `--follow`para visualizar cada entrada nova de registro no log. Ao fazer isso nos dois pods e realizar uma consulta ao endereço do ingress, obtêm-se registros de log conforme distribuição de carga round robin
			* `$ kubectl logs nginx-585449566-67bsx --follow`
			* `$ kubectl logs nginx-585449566-fbrt5 --follow`

1. Atividade 2 - wordpress no k8s

	- [x] Crie um namespace chamado `labwordpress`, tudo o que for feito deverá estar dentro deste namespace
		* Namespace criado pelo arquivo `labwordpress-namespace.yaml`
		* `$ kubectl apply -f labwordpress-namespace.yaml`
	- [x] Faça o apply do arquivo de service do MySQL mude a porta padrão do banco MySQL para 3308
		* Serviço criado dentro do deployment `labwordpress-mysql-deployment.yaml` tendo o nome de `wordpress-mysql-service` utilizando a porta 3308
	- [x] Crie o arquivo secret que deverá conter o password do banco MySQL, lembre-se de criar uma senha com fortes padrões de segurança
		* Secret criado pelo arquivo `labwordpress-mysql-secret.yaml` contendo senha de grande segurança  `1ReallyHardPassword*` e codificada em Base64
		* `$ kubectl apply -f labwordpress-mysql-secret.yaml`
	- [x] Faça o apply do arquivo de PersistentVolumeClaim do MySQL para um capacity de 3GB
		* PersistentVolumeClaim criado dentro do deployment `labwordpress-mysql-deployment.yaml` tendo o nome de `mysql-pvclaim` utilizando 3 GiB
	- [x] Faça o apply do arquivo de deployment do MySQL, crie também um volume mount no deployment do MySQL chamado `mysql-persistent-storage-lab`, apontando para `/var/lib/mysql`. Lembre-se de criar o volume em si com o mesmo nome do volume mount
		* Deployment realizado com o nome de `wordpress-mysql` junto do mount e do caminho
		* `$ kubectl apply -f labwordpress-mysql-deployment.yaml`
	- [x] Faça o apply do arquivo de service do Wordpress altere para a TCP Port 80
		* Serviço criado dentro do deployment `labwordpress-deployment.yaml` tendo o nome de `wordpress-service` utilizando a porta 80
	- [x] Faça o apply do arquivo de PersistentVolumeClaim do Wordpress, para um capacity de 3GB
		* PersistentVolumeClaim criado dentro do deployment `labwordpress-deployment.yaml` tendo o nome de `wordpress-pvclaim` utilizando 3 GiB
	- [x] No arquivo de deployment do Wordpress, crie um volume mount no deployment do Wordpress chamado `wordpress-persistent-storage-lab`, apontando para `/var/www/html`. Lembre-se de criar o volume em si com o mesmo nome do volume mount
		* Deployment realizado com o nome de `wordpress` junto do mount e do caminho
	- [x] No arquivo de deployment do wordpress, insira o secret contendo o password do MySQL, criado no começo do exercício
		* Inserido através de `spec.containers.env.valueFrom.secretKeyRef`
	- [x] Faça o apply do arquivo de deployment do wordpress
		* `$ kubectl apply -f labwordpress-deployment.yaml`
	- [x] Verifque se os pods, os services e os pvcs foram criados da forma correta dentro namespace criado no início deste exercício
		* `$ kubectl -n labwordpress get all`
		* `$ kubectl -n labwordpress get pvc`
	- [x] Verifique qual foi a URI gerada através do ingress do Kubernetes
		* Utilizado um host específico no endereço [http://labwordpress.teste](http://labwordpress.teste) mapeado com endereço do Node no arquivo `/etc/hosts`, onde no exemplo realizado tem-se:
			* `192.168.39.29 labwordpress.teste`
	- [x] Copie essa URI do Ingress e cole no browser para abrir a tela inicial do wordpress
		* Ẽxito em mostrar tela inicial de instalação do Wordpress
