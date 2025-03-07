### **Guia de Setup do Projeto IoT-Madness**

Este guia vai mostrar como descarregar e configurar o ambiente **IoT-Madness** usando Docker. Abaixo estão os passos para os alunos configurarem a parte do projeto que se encontra na pasta **Docker-IoT-Setup**. Esta pasta contém os ficheiros necessários para correr os containers com Docker Compose, como Mosquitto (Broker MQTT), Node-RED, Grafana e InfluxDB.

---

## **Requisitos**

1. **Docker** (e Docker Compose)
   - Se ainda não tiver o Docker instalado, pode descarregar o Docker Desktop para o seu sistema operativo: [Docker Desktop](https://www.docker.com/products/docker-desktop).
   - Para Linux, siga as instruções de instalação do Docker e Docker Compose:
     - [Instalar Docker no Linux](https://docs.docker.com/engine/install/)
     - [Instalar Docker Compose](https://docs.docker.com/compose/install/)

2. **Git** (opcional)
   - Se preferir usar Git, descarregue-o aqui: [Git](https://git-scm.com/downloads). No entanto, se não quiser usar Git, pode descarregar a pasta diretamente do repositório.

---

## **Passo 1: Descarregar a Pasta Docker-IoT-Setup**

### **Usando Git (Recomendado)**

1. Abra um terminal ou linha de comandos.
2. Execute o seguinte comando para clonar o repositório:

   ```bash
   git clone https://github.com/Jofralso/IoT-Madness.git
   ```

3. Navegue até à pasta **Docker-IoT-Setup** dentro do repositório:

   ```bash
   cd IoT-Madness/Docker-IoT-Setup
   ```

### **Descarregar a Pasta Diretamente (Sem Git)**

1. Aceda ao repositório no GitHub: [IoT-Madness](https://github.com/Jofralso/IoT-Madness).
2. Navegue até à pasta **Docker-IoT-Setup**.
3. Clique no botão **Code** e depois em **Download ZIP**.
4. Extraia o ficheiro ZIP e coloque-o numa pasta adequada no seu computador.

---

## **Passo 2: Examinar o Ficheiro `docker-compose.yml`**

Abra o ficheiro `docker-compose.yml` na pasta **Docker-IoT-Setup**. Este ficheiro contém a configuração necessária para criar os containers do Docker.

O ficheiro configura os seguintes containers:

- **Mosquitto**: Broker MQTT para comunicação com dispositivos IoT (como o ESP32).
- **Node-RED**: Para orquestrar e processar os dados.
- **InfluxDB**: Base de dados para armazenar dados temporais.
- **Grafana**: Para visualizar os dados em tempo real.

A configuração do ficheiro é explicada no passo anterior, e pode rever esta parte para garantir que tudo está correto.

---

## **Passo 3: Iniciar os Containers com Docker Compose**

Agora, vamos iniciar os containers que estão configurados no `docker-compose.yml`:

1. Abra um terminal ou linha de comandos.
2. Navegue até à pasta onde descarregou o projeto e onde está o ficheiro `docker-compose.yml` (por exemplo, a pasta **Docker-IoT-Setup**).
3. Execute o seguinte comando para iniciar os containers:

   ```bash
   docker-compose up -d
   ```

   O `-d` faz com que os containers sejam executados em segundo plano.

4. Aguarde alguns minutos até que todos os containers sejam inicializados.

---

## **Passo 4: Verificar os Containers em Execução**

Pode verificar se os containers estão a ser executados utilizando o comando:

```bash
docker ps
```

Este comando vai listar todos os containers em execução. Deve ver os seguintes containers:

- **mqtt-broker** (Mosquitto)
- **node-red**
- **grafana**
- **influxdb**

---

## **Passo 5: Aceder aos Serviços**

Agora que os containers estão a funcionar, pode aceder às interfaces de cada serviço no navegador:

- **Mosquitto (Broker MQTT)**: Não tem uma interface gráfica, mas pode usar qualquer cliente MQTT para testar a ligação com `localhost:1883`.
- **Node-RED**: Aceda em [http://localhost:1880](http://localhost:1880). O Node-RED será a plataforma onde vai criar fluxos para interagir com os dados.
- **Grafana**: Aceda em [http://localhost:3000](http://localhost:3000). O login padrão é:
  - Utilizador: `admin`
  - Palavra-passe: `admin`
  - Após o primeiro login, será solicitado que altere a palavra-passe.
- **InfluxDB**: Aceda em [http://localhost:8086](http://localhost:8086).

---

## **Passo 6: Integrar o Node-RED com o Mosquitto e InfluxDB**

No **Node-RED**, pode configurar fluxos para capturar dados de sensores, como o ESP32, e enviar esses dados para o **InfluxDB**. Abaixo estão os passos para ligar o Node-RED ao Mosquitto e ao InfluxDB.

1. **Configurar MQTT no Node-RED**:
   - No painel do Node-RED, adicione um nó **MQTT** e configure-o para se ligar ao broker MQTT em `localhost:1883`.

2. **Configurar InfluxDB no Node-RED**:
   - Adicione um nó **InfluxDB** no fluxo do Node-RED.
   - Configure a URL do InfluxDB como `http://influxdb:8086` e selecione a base de dados `metrics`.

3. **Criar Fluxos e Testar**:
   - Envie dados do **MQTT** para o **InfluxDB** e visualize-os no **Grafana**.

---

## **Passo 7: Parar os Containers**

Quando terminar de usar os containers, pode parar todos os containers com o seguinte comando:

```bash
docker-compose down
```

Este comando vai parar e remover os containers, mas vai manter os dados persistentes no volume configurado.

---

## **Conclusão**

Agora configurou o ambiente completo com Docker para monitoramento de IoT, utilizando Mosquitto (MQTT), Node-RED, InfluxDB e Grafana. Pode expandir este projeto adicionando mais sensores, criando fluxos mais complexos no Node-RED, e criando dashboards interativos no Grafana.

---

**Dúvidas ou Problemas?**

Se tiver problemas durante o setup, pode verificar os logs dos containers com o comando:

```bash
docker logs <nome-do-container>
```

Ou consulte a documentação oficial das ferramentas utilizadas para mais detalhes.
