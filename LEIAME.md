# Sistema de Controle Autônomo para Veículo Virtual com Algoritmo Genético

Bem-vindo(a) ao repositório! Este projeto, desenvolvido em Unity com C#, simula e treina veículos autônomos para navegar em uma pista 2D de forma eficiente, usando redes neurais artificiais (RNAs) otimizadas por um Algoritmo Genético (AG). Projetado para exploração acadêmica, ele serve como uma plataforma robusta para testar métodos de controle autônomo para veículos virtuais.

Este README fornece um guia abrangente, orientado para desenvolvedores, para entender, configurar e estender o sistema. Ele abrange os princípios essenciais, detalhes de implementação técnica e instruções de uso prático.

---

### **Agradecimentos**

Este projeto foi possível graças às contribuições dedicadas de **Lucas de Biace Torres** e **Gabriel Guimarães Moia**. Somos profundamente gratos por seu trabalho e empenho.

---

### **Índice**

* [Visão Geral do Projeto](#visão-geral-do-projeto)
* [Conceitos Principais e Implementação Técnica](#conceitos-principais-e-implementação-técnica)
    * [O Algoritmo Genético (AG) Explicado](#o-algoritmo-genético-ag-explicado)
    * [Rede Neural e Percepção](#rede-neural-e-percepção)
    * [Mecânica Unity 2D](#mecânica-unity-2d)
* [Guia de Configuração e Execução](#guia-de-configuração-e-execução)
    * [Requisitos](#requisitos)
    * [Configuração](#configuração)
    * [Executando a Simulação](#executando-a-simulação)
* [Controle Alternativo: O Controlador PID](#controle-alternativo-o-controlador-pid)
    * [Controle Manual](#controle-manual)
* [Extensibilidade e Contribuição](#extensibilidade-e-contribuição)
* [Licença](#licença)

---

### **Visão Geral do Projeto**

O objetivo principal é treinar um carro virtual para completar uma pista 2D o mais rápido possível e sem bater. O sistema de controle do veículo é totalmente **autônomo**, impulsionado por uma combinação de uma **Rede Neural** e um **Algoritmo Genético**.

A "percepção" do carro se baseia em **11 sensores de raycast** que medem distâncias a obstáculos. Uma rede neural processa essas entradas e gera comandos de direção e aceleração. O Algoritmo Genético, por sua vez, atua como um otimizador sofisticado, evoluindo os pesos internos da rede neural ao longo de gerações sucessivas para melhorar o desempenho.

O projeto também inclui um **controlador PID (Proporcional-Integral-Derivativo)** como um mecanismo de controle alternativo para testes manuais ou ajuste fino. Um mecanismo de eliminação precoce é implementado para acelerar o treinamento, removendo indivíduos com baixo desempenho.

---

### **Conceitos Principais e Implementação Técnica**

#### **O Algoritmo Genético (AG) Explicado**

O script `Paralelgerenciadorevolucao.cs` é o orquestrador central de todo o processo evolutivo.

1.  **Inicialização (`CriarNovaPopulacao()`)**: O processo começa criando uma nova população de carros. A variável `testedozero` determina se os pesos da rede neural serão gerados aleatoriamente (`true`) ou carregados de um arquivo pré-treinado (`false`) usando `CarregarPesosDeArquivo()`.

2.  **Avaliação de Aptidão e Eliminação (`FixedUpdate()`)**: Durante a simulação, o desempenho de cada carro é medido por sua **`pontuacao`** (score de aptidão). Para economizar recursos, uma verificação de desempenho é feita periodicamente, e qualquer carro com uma pontuação baixa (abaixo de `pontuacaoMinima` ou menos de 25% da melhor pontuação) é removido.

3.  **Seleção e Elitismo (`EvoluirPopulacao()`)**: Após a simulação, os carros são classificados por sua pontuação final. Os 20% melhores indivíduos são selecionados como a **"elite"**, e os pesos de suas redes neurais são copiados diretamente para a nova geração.

4.  **Cruzamento e Mutação**: O restante da nova população é criado por recombinação genética. O **Cruzamento** (`CruzarPesos()`) combina os pesos de dois pais de elite. Em seguida, uma **mutação** (`MutarPesos()`) é aplicada, introduzindo pequenas mudanças aleatórias nos pesos para promover inovação.

#### **Rede Neural e Percepção**

A rede neural é o "cérebro" do carro. É uma rede feedforward com uma arquitetura configurável, padrão `{12, 3, 4, 3}`. As 12 entradas vêm dos **11 sensores de raycast** e da velocidade atual do carro. As saídas controlam o movimento horizontal e vertical do carro.

#### **Mecânica Unity 2D**

O projeto usa o sistema de física 2D embutido do Unity. A percepção do carro é feita por raycasts. O script `ParalelControlaIA` processa a saída da rede neural e a traduz em forças físicas ou torques aplicados ao componente `carController` do carro.

---

### **Guia de Configuração e Execução**

#### **Requisitos**

* **Unity**: Versão 6000.0.38f1 (ou compatível).
* **Hardware**: Uma placa de vídeo é necessária.

#### **Configuração**

1.  Abra o projeto no Unity e carregue a cena chamada **"scene done"**.
2.  No painel `Hierarchy`, localize e selecione o objeto **`GENETIC Algorithm MANAGER`**.
3.  No `Inspector`, você pode configurar os parâmetros do AG, como **`testedozero`**, **`populacaoTamanho`** e **`taxaMutacao`**. Lembre-se de revisar o caminho `caminhoArquivoInicial`, que está hardcoded.

#### **Executando a Simulação**

* Clique em **Play** no editor do Unity.
* Os carros surgirão e começarão sua jornada evolutiva.
* Monitore o `Console` para logs em tempo real sobre eliminações, mortes e o salvamento das gerações.

---

### **Controle Alternativo: O Controlador PID** 🛠️

Um **controlador PID** é incluído para um controle de veículo preciso.

#### **Como Acessar e Configurar**

1.  Na `Hierarchy` da cena "scene done", encontre o objeto **"Car PID"**.
2.  Para usar os controles manuais, desabilite a IA desmarcando o componente **`ParalelControlaIA`**.
3.  No mesmo objeto, localize o componente **`PIDcomVelocidade`** para ajustar os Ganhos de Direção (`Kp`, `Ki`, `Kd`) e os Ganhos de Velocidade (`Kp Speed`, `Ki Speed`, `Kd Speed`).

#### **Controle Manual**

Com o controlador da IA desabilitado, você pode dirigir o carro manualmente.

* **WASD ou Setas**: Controle a direção e a aceleração/frenagem.
* **Freio (S / Seta para Baixo)**: Não engata a ré.
* **E / Q**: Troque de marchas para cima e para baixo.

---

### **Extensibilidade e Contribuição**

Este projeto oferece uma base sólida para futuras pesquisas. Contribuições são bem-vindas!

1.  Faça um fork do repositório.
2.  Crie uma branch (`git checkout -b feature/sua-feature`).
3.  Faça o commit das suas mudanças (`git commit -m "Adiciona sua feature"`).
4.  Envie para a sua branch (`git push origin feature/sua-feature`).
5.  Abra um Pull Request.
