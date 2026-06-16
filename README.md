[![Typing SVG](https://readme-typing-svg.herokuapp.com?font=Fira+Code&size=26&duration=4000&pause=1000&color=308ECDFF&width=435&lines=Semáforo+inteligente;Projeto+sprint)](https://git.io/typing-svg)       
 Este projeto consiste em um semáforo inteligente com dupla finalidade: auxiliar a polícia do Rio de Janeiro durante perseguições policiais, podendo fechar o semáforo quando detecta a viatura, e promover a acessibilidade urbana ao substituir a cor verde pela cor azul, facilitando a identificação do sinal por motoristas daltônicos.
  ## $\color{blue}{Tecnologias{}}$ $\color{blue}{Utilizadas{}}$
  * Tinkercad/ C++
  * Arduino IDE
  * Github
  * Excel
  * Flowgorithm
  ## $\color{blue}{Instruções{}}$
  #### Pré-requisito:
  Arduino IDE instalada no seu computador.
  #### Passo 1: Montagem do Circuito (Hardware)  
  Monte os componentes na protoboard seguindo o esquema de pinos abaixo:<img width="644" height="635" alt="Captura de tela 2026-06-08 154309" src="https://github.com/user-attachments/assets/a65b1391-c355-4688-9836-93916212709f" />

####  Passo 2: Code
  ```cpp
// Definição dos Pinos - Sensor HC-SR04
const int trigPin = 12;
const int echoPin = 13;

// Semáforo 1 (Leds de baixo no circuito)
const int ledR = 4;
const int ledY = 3;
const int ledB = 2; // Usando o Azul como Verde

// Semáforo 2 (Leds de cima no circuito)
const int ledR1 = 7;
const int ledY1 = 6;
const int ledB1 = 5; // Usando o Azul como Verde

// Pino do Buzzer Piezo
const int piezoPin = 10;

// Tempos de transição (em milissegundos)
const int tempoAzul = 3000;
const int tempoAmarelo = 1000;
const int tempoVermelho = 2000;

const int distanciaEmergencia = 10; // em centímetros

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  
  // Configuração do Semáforo 1
  pinMode(ledR, OUTPUT);
  pinMode(ledY, OUTPUT);
  pinMode(ledB, OUTPUT);
  
  // Configuração do Semáforo 2
  pinMode(ledR1, OUTPUT);
  pinMode(ledY1, OUTPUT);
  pinMode(ledB1, OUTPUT);
  
  // Configuração do pino do Piezo
  pinMode(piezoPin, OUTPUT);
  
  Serial.begin(9600);
}

void loop() {
  // ESTADO 1: Semáforo 1 ABERTO (Azul) | Semáforo 2 FECHADO (Vermelho)
  controlarSemaforos(LOW, LOW, HIGH, HIGH, LOW, LOW); 
  esperarComLeitura(tempoAzul);

  // ESTADO 2: Semáforo 1 ATENÇÃO (Amarelo) | Semáforo 2 FECHADO (Vermelho)
  controlarSemaforos(LOW, HIGH, LOW, HIGH, LOW, LOW);
  esperarComLeitura(tempoAmarelo);

  // ESTADO 3: Semáforo 1 FECHADO (Vermelho) | Semáforo 2 ABERTO (Azul)
  controlarSemaforos(HIGH, LOW, LOW, LOW, LOW, HIGH);
  esperarComLeitura(tempoVermelho);
  
  // ESTADO 4: Semáforo 1 FECHADO (Vermelho) | Semáforo 2 ATENÇÃO (Amarelo)
  controlarSemaforos(HIGH, LOW, LOW, LOW, HIGH, LOW);
  esperarComLeitura(tempoAmarelo);
}

// Função para ler a distância do sensor ultrassônico
long lerDistancia() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  long duracao = pulseIn(echoPin, HIGH);
  return duracao / 29 / 2;
}

// Função unificada para controlar todos os LEDs dos dois semáforos
void controlarSemaforos(int r1, int y1, int b1, int r2, int y2, int b2) {
  digitalWrite(ledR, r1);
  digitalWrite(ledY, y1);
  digitalWrite(ledB, b1);
  
  digitalWrite(ledR1, r2);
  digitalWrite(ledY1, y2); 
  digitalWrite(ledB1, b2);
}

// Espera inteligente otimizada para resposta imediata
void esperarComLeitura(int tempo) {
  int passados = 0;
  // Diminuímos o delay interno para 10ms para atualizar a leitura do sensor muito mais rápido
  while (passados < tempo) {
    long cm = lerDistancia();

    // MODIFICAÇÃO: Se detectar proximidade, entra em emergência INSTANTANEAMENTE
    if (cm > 0 && cm < distanciaEmergencia) {
      fecharSemaforoImediato();
      return; 
    }

    delay(10); 
    passados += 10;
  }
}

// Rotina de emergência IMEDIATA (Ambos Vermelhos + Alarme Sonoro na hora)
void fecharSemaforoImediato() {
  Serial.println("EMERGÊNCIA DETECTADA! Bloqueio imediato do cruzamento.");

  // Corta qualquer transição e joga os dois semáforos no VERMELHO no mesmo instante
  controlarSemaforos(HIGH, LOW, LOW, HIGH, LOW, LOW); 
  
  // O alarme começa a tocar imediatamente junto com os LEDs vermelhos
  for (int i = 0; i < 12; i++) {
    tone(piezoPin, 1000); 
    delay(250);
    noTone(piezoPin);     
    delay(250);
  }
  
  Serial.println("Cruzamento liberado. Reiniciando ciclo normal.");
}
```
#### Passo 3: Ultilizar o Arduino IDE  
Transferir todo o código feito no tinkercad para o Arduino IDE para conseguir programar o Arduino!

* Conecte o Arduino ao seu PC via USB.  

* Vá em Ferramentas > Placa e selecione o Arduino Uno

* Vá em Ferramentas > Porta e selecione a porta COM ativa.

* Clique no botão de carregar
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/7648ce74-2971-40c9-93b4-ac12e42f7945" />

#### Passo 4: Testar se está funcionando


  
 
  ## $\color{blue}{Fluxograma{}}$ 
  [Fluxograma.pdf.pdf](https://github.com/user-attachments/files/28998611/Fluxograma.pdf.pdf)


  
 ## $\color{blue}{5W2H{}}$
 [5w2h_Sprint2.xlsx](https://github.com/user-attachments/files/28982329/5w2h_Sprint2.xlsx)  
 
<img width="1166" height="457" alt="image" src="https://github.com/user-attachments/assets/621c2db7-a6b7-475c-aa1b-e965e5d40e25" />


  ## $\color{blue}{ABNT{}}$

  ## $\color{blue}{Acesso{}}$ $\color{blue}{ao{}}$ $\color{blue}{Projeto{}}$
 #### tinkercad: https://www.tinkercad.com/things/lVMtRSs3LjH-semaforo-inteligente-sprint-23?sharecode=6ZbcorSg7qSgv5DHwR53V9htrnXI9KsmOL2csctxCMo
<img width="702.5" height="400" alt="Captura de tela 2026-06-09 083857" src="https://github.com/user-attachments/assets/b4cccd63-a897-4a9d-858b-95285bf9dcea" />



<img width="702.5" height="330" alt="Design sem nome (3)" src="https://github.com/user-attachments/assets/7a2cd399-6306-49dd-973e-a9dd7f5fea49" />

 ## $\color{blue}{Cronograma{}}$
<img width="910" height="624" alt="image" src="https://github.com/user-attachments/assets/1f72d4c2-8175-438a-b095-86f971ad7504" />  
<img width="911" height="212" alt="image" src="https://github.com/user-attachments/assets/7a056ab1-be8f-496e-b505-a48c38372da2" />





  ## $\color{blue}{Pessoas{}}$ $\color{blue}{Desenvolvedoras{}}$ $\color{blue}{do{}}$ $\color{blue}{Projeto{}}$
  * Eduardo Farias Moreira DEV <a href="https://github.com/EduFM202">
    <img alt="followers" title="Me siga no Github" src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white"/></a>
     <a href=""> 
     <a href="https://www.linkedin.com/in/eduardo-farias-moreira-7b8432401/" target="_blank">
        <img 
            alt="LinkedIn" 
            title="LinkedIn" 
            src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"
        />
    </a>

    
  * Julia Vitoria Silva Sanches DEV <a href="https://github.com/JuVitori">
    <img alt="followers" title="Me siga no Github" src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white"/></a>
     <a href=""> 
     <a href="https://www.linkedin.com/in/julia-vitoria-silva-sanches-218bba414/" target="_blank">
        <img 
            alt="LinkedIn" 
            title="LinkedIn" 
            src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"
        />
    </a>
  * Lorena Sousa de Oliveira SM <a href="https://github.com/LoreeOliveira">
    <img alt="followers" title="Me siga no Github" src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white"/></a>
     <a href=""> 
     <a href="https://www.linkedin.com/in/lorena-sousa-de-oliveira-404438401/" target="_blank">
        <img 
            alt="LinkedIn" 
            title="LinkedIn" 
            src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"
        />
    </a>
  * Marcelo Barbosa Souza DEV <a href="https://github.com/Tchelo-Barbosa">
    <img alt="followers" title="Me siga no Github" src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white"/></a>
     <a href=""> 
     <a href="https://www.linkedin.com/in/marcelo-barbosa-2b7433401/" target="_blank">
        <img 
            alt="LinkedIn" 
            title="LinkedIn" 
            src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"
        />
    </a>
  * Miguel Porto Coutinho PO <a href="https://github.com/CoutomgPortinho">
    <img alt="followers" title="Me siga no Github" src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white"/></a>
     <a href=""> 
     <a href="https://www.linkedin.com/in/miguel-porto-coutinho-b2b428401/" target="_blank">
        <img 
            alt="LinkedIn" 
            title="LinkedIn" 
            src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"
        />
    </a>

