# Docker Hub | IDG - Datapower

# Como subir uma imagem Datapower no docker Hub

- Fazer login no docker hub ou criar uma conta no link abaixo:

[https://hub.docker.com/](https://hub.docker.com/)

- No menu acima, entrar no repositores e clicar no botão create repository
    
    ![Captura de Tela 2023-05-06 às 15.47.48.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_15.47.48.png)
    

- Após criar o repositório, você terá um modelo abaixo; No caso foi criado um repositório com nome de datapower;

![Captura de Tela 2023-05-06 às 17.27.23.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_17.27.23.png)

- Entrar no ambiente que deseja subir a imagem, nesse exemplo será usado uma VM Linux Red Hat 8.6:  `ssh root@9.30.122.34`

- Verificar se a máquina contém o docker instalado: `docker -v`
    
    ![Captura de Tela 2023-05-06 às 17.46.35.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_17.46.35.png)
    

Nesse caso não havia o docker instalado;

- Para instalar o docker utilizar `snap install docker`e depois verificar se a instalação está correta.

No caso esse comando "snap install docker" é utilizado para instalar o Docker no sistema operacional Ubuntu utilizando o gerenciador de pacotes Snap. O Snap é uma tecnologia de empacotamento de software que permite instalar aplicativos em diferentes distribuições Linux de forma rápida e fácil. No entanto, é importante lembrar que esse comando não funcionará em outras distribuições que não sejam baseadas no Ubuntu e que o Docker pode ser instalado de outras formas em sistemas operacionais diferentes.

![Captura de Tela 2023-05-06 às 17.48.50.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_17.48.50.png)

- Utilizar o comando `docker ps -a` para verificar se contém algum contêiner;

O comando **`docker ps -a`** é usado para listar todos os contêineres Docker ativos e inativos no host. O parâmetro **`-a`** lista todos os contêineres, incluindo aqueles que estão inativos. A saída do comando inclui detalhes como o ID do contêiner, o status, o nome, a imagem usada e a data em que o contêiner foi criado.

![Captura de Tela 2023-05-06 às 17.50.07.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_17.50.07.png)

- Abra outro terminar e utilize o seguinte comando `scp ibmdatapower.tar [root@9.30.122.34](mailto:root@9.30.122.34):/tmp` para enviar o arquivo .tar para o ambiente onde iremos utilizar-lo, esse arquivo é uma imagem do datapower que a ibm parou de disponibilizar.

É uma versão 10.0.3 de desenvolvedor, ela não deve ir para produção.

![Captura de Tela 2023-05-06 às 17.57.08.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_17.57.08.png)

- Voltando ao terminal da VM usar o comando `docker load < /tmp/ibmdatapower.tar`

esse comando carrega uma imagem Docker que foi previamente salva em um arquivo no formato tar para o Docker da máquina local. O caminho do arquivo tar é especificado após o sinal de menor que (<). É importante notar que esse comando só funciona com arquivos de imagem Docker no formato tar.

![Captura de Tela 2023-05-06 às 18.12.53.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_18.12.53.png)

- Usar o comando `docker images`para listar as imagens, nesse caso a imagem que subimos no ambiente;

![Captura de Tela 2023-05-06 às 18.16.16.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_18.16.16.png)

- Criando uma nova versão da imagem do datapower para subir no hub porque ela não existe mais: `sudo docker tag ibmcom/datapower isabellaf/datapower:2.0` Não há retorno após utilizar o comando acima;

No comando acima, estamos criando uma nova tag com o nome "isabellaf/datapower" e a versão "2.0" para a imagem original "ibmcom/datapower". Isso é útil quando queremos renomear uma imagem ou criar diferentes versões dela. Podemos ter várias tags apontando para a mesma imagem subjacente, o que nos permite gerência-la de maneira mais eficiente.

Sequencia do comando:

![Captura de Tela 2023-05-06 às 18.22.44.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_18.22.44.png)

- Verificar se criou uma nova versão: `docker images`

![Captura de Tela 2023-05-06 às 18.31.38.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_18.31.38.png)

- Fazer login no docker hub: `docker login`

![Captura de Tela 2023-05-06 às 18.41.49.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_18.41.49.png)

- Enviando a imagem para o repositório criado no inicio `docker push isabellaf/datapower:2.0`

![Captura de Tela 2023-05-06 às 18.45.36.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_18.45.36.png)

- Ir no docker hub e verificar os repositórios, nesse a versão que enviamos foi a 2.0. e foi feita com sucesso.

![Captura de Tela 2023-05-06 às 18.47.26.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_18.47.26.png)

- Após ter a imagem no seu docker para fazer ela funcionar deve-se dar um run "start”na imagem com o comando abaixo (tudo junto):

`docker run -it \
-v $PWD/config:/drouter/config \
-v $PWD/local:/drouter/local \
-e DATAPOWER_ACCEPT_LICENSE=true \
-e DATAPOWER_INTERACTIVE=true \
-p 9090:9090 \
-p 9022:22 \
-p 5554:5554 \
-p 7000-7030:7000-7030 \
--name idg \
isabellaf/datapower:2.0`

![Captura de Tela 2023-05-06 às 18.54.06.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_18.54.06.png)

Após algumas instalações você deverá configurar um login e senha, neste caso utilize o `admin` para os dois;

![Captura de Tela 2023-05-06 às 18.54.44.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_18.54.44.png)

Com isso, configuramos 30 portas para uso, da 7000 até a 7030, que pode ser utilizadas para o HTTP Handlers por exemplo, porém o console web ainda não estará disponível, para inicia-lo, após iniciar o datapower via linha de comando você irá colocar com o usuário/senha admin/admin.

- Para ativar o console web será necessário configurá-lo da seguinte forma:

`configure terminal
web-mgmt
admin-state enabled
local-address eth0_ipv4_1 "9090"
exit
write mem`

O comando "configure terminal" é usado para entrar no modo de configuração do dispositivo de rede. O comando "web-mgmt" é usado para acessar o modo de gerenciamento da interface web. O comando "admin-state enabled" é usado para ativar a interface web. O comando "local-address eth0_ipv4_1 "9090"" é usado para definir o endereço IP da interface web para a porta 9090. O comando "exit" é usado para sair do modo de gerenciamento da interface web e voltar para o modo de configuração. Por fim, o comando "write mem" é usado para salvar as configurações no dispositivo.

![Captura de Tela 2023-05-06 às 19.03.46.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_19.03.46.png)

- Acesse a url: **[https://localhost:9090](https://localhost:9090/)**

![Captura de Tela 2023-05-06 às 19.13.38.png](Docker%20Hub%20IDG%20-%20Datapower%20833ff32fa3514a8493715d7f3aa2f7e9/Captura_de_Tela_2023-05-06_as_19.13.38.png)

Link de suporte: [https://www.ibm.com/docs/en/datapower-gateway/2018.4?topic=docker-running-datapower-gateway-in-containers](https://www.ibm.com/docs/en/datapower-gateway/2018.4?topic=docker-running-datapower-gateway-in-containers)

---

Links de suporte:

- [https://www.youtube.com/watch?v=X7NAzoN3hFc](https://www.youtube.com/watch?v=X7NAzoN3hFc)
- [https://github.com/dbatista/datapower/blob/main/RunDatapowerOnDocker/DatapowerOnDockerOnUnix.md](https://github.com/dbatista/datapower/blob/main/RunDatapowerOnDocker/DatapowerOnDockerOnUnix.md)
- Link do box onde contém a imagem do datapower: [https://ibm.ent.box.com/folder/206429571464](https://ibm.ent.box.com/folder/206429571464)