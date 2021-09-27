## Exercício 1 - Linux
1. Máquina 1

	- [x] Instalar uma máquina Linux (escolham a distribuição que mais agrada)
		* Ubuntu Desktop-current foi instalada em VM do VirtualBox chamada `machine1`
	- [x] Instalar e configurar o apache para rodar na porta 8070
		*  `$ sudo apt-get install apache2`
		* Editar `/etc/apache2/apache2.conf` com o diretório desejado (`/u01/apache/www/`) baseado no `/var/www/`
		* Editar `/etc/apache2/ports.conf` e adicionar a porta 8070 no Listen
		* Editar `/etc/apache2/sites-enabled/000-default.conf` para o VirtualHost na porta 8070 e adicionar uma nova DocumentRoot para o diretório desejado (`/u01/apache/www/`)
	- [x] Instalar e configurar o php
		* `$ sudo apt-get install php libapache2-mod-php`
		* Adicionar o módulo do php no Apache
			* `$ a2enmod php7.4`
	- [x] Adicionar um disco (500MB) a essa máquina e montar a partição no diretório `/u01` – partição em ext4
		* Adicionar um novo disco virtual pelo VirtualBox de 500MB
		* Executar os comandos como root
			* ```sh
			  sudo fdisk -l
			  fdisk /dev/sdb
			  g
			  w
			  fdisk /dev/sdb
			  n
			  default
			  default
			  default
			  y
			  w
			  sudo mkfs .ext4 /dev/sdb1
				```
	- [x] Criar os diretórios `/u01/apache/www/imagens`
		* `$ sudo mkdir -p /u01/apache/www/imagens`
	- [x] Quando a máquina reiniciar, esse diretório precisa continuar aparecendo como montado
		* Para manter a partição montada automaticamente, devemos primeiramente montar a partição
			* `$ sudo mount -t ext4 auto /dev/sdb1 /u01`
		* Depois, editar o arquivo `/etc/fstab` e adicionar o UUID da partição criada (`/dev/sdb1`), para descobrir o UUID, utilizamos o comando `$ blkid`
			* ```sh
			  sudo blkid | grep sdb1
			  echo 'UUID=29d3da92-0680-4212-a2d7-2443c5e87d73 /u01 ext4 defaults 0 0' | sudo tee -a /etc/fstab
				```
	- [x] Criar uma página html `index.html` que tenha um link para outra página chamada `infos_php.php`
		* Adicionar a página `index.html` no diretório `/u01/apache/www`
	- [x] Colocar a imagem do logo da Compasso UOL como conteúdo da página `index.html` junto com o link. A imagem precisa estar localizada no diretório criado anteriormente. Exemplo: `/u01/apache/www/imagens/logo_compasso.jpg`
		* Adicionar link para a imagem no `/u01/apache/www/index.html`
	- [x] Criar a página `infos_php.php` e dentro dela trazer o resultado da função `phpinfo()`
		* Adicionar o arquivo `infos_php.php` no diretório `/u01/apache/www/`

1. Máquina 2

	- [x] Instalar uma máquina Linux (escolham a distribuição que mais agrada)
		* Ubuntu Desktop-current instalada em VM do VirtualBox chamada `machine2`
	- [x] Instalar e configurar o NGiNX para rodar na porta 8090 e criar uma página html
		* `$ sudo apt-get install nginx`
		* Edita o arquivo `/etc/nginx/sites-enabled/`default como root
		* Adiciona a porta, diretório e arquivos index por ordem de prioridade de acordo com o template do próprio arquivo
	- [x] `index.html` que tenha um link para a página criada no exercício anterior (Máquina 1)
		* Cria arquivo index no diretório `/var/www/html/` com o link para o IP da Máquina 1
