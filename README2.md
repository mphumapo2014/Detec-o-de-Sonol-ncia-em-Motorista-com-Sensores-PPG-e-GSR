# Detecção de Sonolência em Motorista com Sensores PPG e GSR

![Status do Projeto](https://img.shields.io/badge/status-concluído-green)
![Licença](https://img.shields.io/badge/licença-MIT-blue)

Repositório do Trabalho de Conclusão de Curso (TCC) para o curso de Engenharia Eletrônica da Universidade de Brasília (UnB) - Faculdade UnB Gama (FGA).

## 📖 Sobre o Projeto

Este projeto apresenta um sistema embarcado vestível (wearable) para a **detecção de sonolência em tempo real em motoristas**. Utilizando sensores de fotopletismografia (PPG) e resposta galvânica da pele (GSR), o sistema monitora sinais fisiológicos que se alteram durante a transição da vigília para o sono, acionando alertas sonoros e visuais para prevenir acidentes.

A solução é de baixo custo, não invasiva e integra hardware e software para aquisição, processamento, visualização e alerta, utilizando um microcontrolador ESP32, um broker MQTT e uma dashboard em Node-RED.

### 🚗 Contextualização

A sonolência ao volante é uma das principais causas de acidentes graves nas rodovias brasileiras, com impacto comparável à condução sob efeito de álcool. Segundo dados da Polícia Rodoviária Federal, a categoria "Condutor Dormindo" foi responsável por milhares de acidentes, mortes e feridos nos últimos anos. Este projeto busca oferecer uma solução tecnológica acessível e eficaz para mitigar esse grave problema de segurança viária.

## 🛠️ Arquitetura do Sistema

O sistema opera em um fluxo contínuo de dados:

1.  **Aquisição de Sinais:** Os sensores PPG (MAX30102) e GSR, posicionados no dedo e na mão do motorista, coletam dados de frequência cardíaca (BPM), saturação de oxigênio (SpO2) e condutância da pele.
2.  **Processamento Embarcado:** O microcontrolador **ESP32** recebe os dados, aplica filtros para reduzir ruídos (Média Móvel e Filtro Exponencial) e calcula os parâmetros fisiológicos.
3.  **Lógica de Detecção:** O firmware no ESP32 compara os valores de BPM e condutância com limiares pré-definidos. Se os critérios de sonolência são atendidos, um alerta é acionado.
4.  **Alerta Local:** Um **display OLED** exibe os dados em tempo real e um **buzzer** emite um alerta sonoro.
5.  **Comunicação IoT:** Os dados processados são publicados em um broker **Mosquitto MQTT** via Wi-Fi.
6.  **Visualização Remota:** Uma dashboard desenvolvida em **Node-RED** se inscreve nos tópicos MQTT e exibe os dados em gráficos e medidores, permitindo o monitoramento remoto.


*Substitua o link acima por uma imagem da arquitetura (como a Figura 20 do seu TCC) para um README mais visual.*

## 🚀 Tecnologias Utilizadas

### Hardware
*   **Microcontrolador:** ESP32-WROOM-32D
*   **Sensor PPG/SpO2:** Módulo MAX30102
*   **Sensor GSR:** Módulo Sensor de Resposta Galvânica da Pele
*   **Display:** OLED 0.96" I2C (SSD1306)
*   **Atuador:** Buzzer Ativo 5V
*   **Fonte de Energia:** Bateria de Li-Ion ou Power Bank

### Software e Plataformas
*   **Firmware:** Arduino IDE (Linguagem C/C++)
*   **Bibliotecas Principais:** `Adafruit_SSD1306`, `MAX30105`, `PubSubClient`
*   **Protocolo de Comunicação:** MQTT (Message Queuing Telemetry Transport)
*   **Broker MQTT:** Mosquitto
*   **Plataforma de Visualização:** Node-RED com o plugin `node-red-dashboard`

## 📂 Estrutura do Repositório


## ⚙️ Começando

Siga os passos abaixo para replicar o ambiente do projeto.

### Pré-requisitos
1.  **Arduino IDE:** [Download aqui](https://www.arduino.cc/en/software)
2.  **Node-RED:** [Instruções de instalação](https://nodered.org/docs/getting-started/local)
3.  **Mosquitto MQTT Broker:** [Instruções de instalação](https://mosquitto.org/download/)

### 1. Configuração do Hardware
Conecte os componentes ao ESP32 conforme o esquemático (Figura 16 do TCC):
*   **MAX30102 e Display OLED (I2C):**
    *   `SCL` -> `GPIO 22`
    *   `SDA` -> `GPIO 21`
    *   `VCC` -> `3.3V`
    *   `GND` -> `GND`
*   **Sensor GSR:**
    *   `SIG` -> `GPIO 34` (pino analógico)
    *   `VCC` -> `3.3V`
    *   `GND` -> `GND`
*   **Buzzer:**
    *   `SIG` -> `GPIO 4`
    *   `VCC` -> `VIN (5V)`
    *   `GND` -> `GND`

### 2. Configuração do Software
1.  **Clone o repositório:**
    ```bash
    git clone https://github.com/[SEU_USUARIO]/[NOME_DO_REPOSITORIO].git
    ```
2.  **Arduino IDE:**
    *   Instale o suporte para a placa ESP32 (via Gerenciador de Placas).
    *   Instale as seguintes bibliotecas via Gerenciador de Bibliotecas: `Adafruit SSD1306`, `Adafruit GFX`, `MAX30105 by SparkFun`, `PubSubClient`.
    *   Abra o arquivo `/codigo_esp32/firmware_sonolencia.ino`.
    *   Altere as credenciais de Wi-Fi e o endereço do servidor MQTT no código:
      ```cpp
      const char* ssid = "NOME_DA_SUA_REDE_WIFI";
      const char* password = "SENHA_DA_SUA_REDE_WIFI";
      const char* mqttServer = "ENDERECO_IP_DO_SEU_BROKER";
      ```
    *   Compile e envie o código para o ESP32.

3.  **Broker MQTT:**
    *   Inicie o serviço do Mosquitto no seu computador ou Raspberry Pi.

4.  **Node-RED:**
    *   Inicie o Node-RED.
    *   Acesse a interface no navegador (normalmente `http://localhost:1880`).
    *   Vá em **Menu > Importar** e selecione o arquivo `/node_red_flow/flow.json` deste repositório.
    *   Faça o deploy do fluxo.

## 📈 Uso
1.  Ligue o dispositivo. Ele se conectará automaticamente ao Wi-Fi e ao broker MQTT.
2.  Posicione os sensores PPG e GSR nos dedos.
3.  Abra a dashboard do Node-RED (normalmente `http://localhost:1880/ui`) para visualizar os dados em tempo real.
4.  O sistema emitirá um alerta sonoro e visual caso detecte um estado de sonolência.

## 📊 Resultados
Os experimentos realizados demonstraram a viabilidade da abordagem. A análise estatística confirmou diferenças significativas nos parâmetros fisiológicos entre os estados de vigília e sono. O sensor **GSR provou ser o indicador mais robusto**, alcançando uma **acurácia de 98.65%**, com alta sensibilidade (98.70%) e especificidade (98.18%) na detecção de sonolência, utilizando um classificador simples baseado em limiar.

## 💡 Trabalhos Futuros
*   **Validação em Larga Escala:** Realizar testes com um número maior e mais diverso de participantes em simuladores de direção.
*   **Machine Learning Embarcado:** Implementar modelos de aprendizado de máquina (ex: Random Forest, LSTMs) diretamente no ESP32 com frameworks como TinyML para uma detecção mais adaptativa e personalizada.
*   **Fusão de Sensores:** Combinar os dados fisiológicos com dados do veículo (via barramento CAN) ou de câmeras para criar um sistema de detecção multimodal mais robusto.
*   **Calibração Automática:** Desenvolver algoritmos que calibrem os limiares de detecção automaticamente para cada usuário.

## 📜 Como Citar
Se você utilizar este trabalho em sua pesquisa, por favor, cite-o da seguinte forma:

MPHILI, Lourenço Jamba. **Detecção de sonolência em motorista usando sensor PPG e GSR**. Trabalho de Conclusão de Curso (Graduação em Engenharia Eletrônica) – Faculdade UnB Gama, Universidade de Brasília, Brasília, 2025.

## 👤 Autor

*   **Autor:** Lourenço Jamba Mphili
*   **Orientador:** Prof. Dr. Gerardo Antonio Idrobo Pizo

## 📄 Licença
Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

---