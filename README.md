
# 5GCore-easy-install

The main objective of this project is to automate the installation process of [my5GCore](https://github.com/my5G/my5G-core) or [free5GC](https://github.com/free5gc/free5gc) projects, through Ansible.

**Steps**

Install python-minimal:
```
sudo apt update && apt -y install python-minimal && sudo apt -y install git && sudo apt -y install ansible
```


Clone this repository:
```
git clone https://github.com/ciromacedo/5GCore-easy-install.git
```

Install GOLang:
```

cd 5GCore-easy-install &&  ansible-playbook -K install-golang.yml
source ~/.bashrc
```

Check your kernel version with ```uname -r```, if the result is less then ```5.0.0-23-generic``` run the following:
```
sudo apt-get install -y linux-image-5.0.0-23-generic
```
In the action menu that appears, choose the first option "__install the package maintainer's version__" like as illustrated in the figure below, and after reboot the system.

<p align="center">
    <img src="imagens/kerner-5-0-23.jpeg" height="300"/> 
</p>

Run
```
apt install linux-headers-5.0.0-23-generic
```

Run ```ifconfig``` and get the name of **internet network interface**, like as illustrated in the figure below:
<p align="center">
    <img src="imagens/if_config.png"/> 
</p>


Run the following Ansible playbook (password for sudo is required):
### my5G-Core
```
cd 5GCore-easy-install && ansible-playbook -K my5GInstall.yml -e  "internet_network_interface=<< internet network interface name>>"
```

### free5gc
```
cd 5GCore-easy-install && ansible-playbook -K free5gc-Install.yml -e  "internet_network_interface=<< internet network interface name>>"
```

### Start NFs Functions
```
NRF > UDR > UDM > AUSF > NSSF > AMF > PCF > UPF (sudo -E ./bin/free5gc-upfd) > SMF > SERVER-WEB > SERVER-FRONT-END (REACT_APP_HTTP_API_URL=http://ipaddress:5000/api PORT=3000 yarn start)
```

## Instalação / Configuração do Tester

1 - Instalar GO (Usar 5GEasyInstall)

2 - Clonar o repositório
```
git clone https://github.com/my5G/my5G-RANTester.git
```

3 - Instalar as dependencias
```
cd my5G-RANTester && go mod download
```


3 - Gerar os binários
```
cd cmd && go build app.go
```

4 - Editar o arquivo de configuração ** config/config.yml**
4.1 - Setar o IP para conexão com AMF:
``
amfif:
  ip: "127.0.0.1"
  port: 38412
  name: "free5gc"
``

4.2 COnfigurar os parametros de acesso do UE
``
key: "8baf473f2f8fd09487cccbd7097c6862"
  opc: "8e27b6af0e692e750f32667a3b14605d"
  amf: "8000"
``

5 Após registrar o UE no Core a inicialização de um UE simples pode ser feita dentro da pasta **cmd** com:
``
./app ue
``
6 Se tudo estiver OK será gerada uma interface de rede com nome **uetun1** e um PING pode ser realizado com:
``
 ping google.com -I uetun1
``


### Truques do Tester
1 - remover um aquivo q fica em /root/tem chamad gnb.sock
2 - as vezes tem q remover as interfaces de rede com esses comandos:
    ip link set uetun1 down
    ip link del uetun1
