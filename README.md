# Trabalho 5: Projeto de jogo feito em Unity utilizando orientação a objetos
#### João Davi Rigo Mazzarolo e Raul Steinmetz

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184784186-67dd09a3-bf03-48da-97d0-d20c7362db3b.PNG">
</p>

## Objetivo

O objetivo do trabalho é a criação de um projeto inicial de um jogo que contenha o básico para uma pequena gameplay, utilizando a engine Unity de forma a aplicar conceitos e técnicas aprendidos relacionados à programação orientada a objetos. Anexaremos a este read.me alguns vídeos e explicações do que foi realizado, além dos arquivos principais do projeto do jogo.

## Breve Gameplay

https://user-images.githubusercontent.com/85958904/184786789-f904fcf2-29fd-4cc7-8407-cea623f852bd.mp4

## Explicação sobre o funcionamento/Criação do Jogo

Na engine Unity, as partes mais mecânicas do jogo, como programação de contatos, gravidade, sons, colisões, animações e sprites são feitas na interface gráfica disponibilizada, para que o programador possa focar em programar os comportamentos dos objetos, e seu jogo seja desenvolvido mais rapidamente.

### A seguir, são mostradas algumas abstrações da engine

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184790046-61c13243-684e-461c-99b7-dc6f21096f48.png">
</p>

Nessa imagem é mostrado o campo de configuração de áudio. Mais especificamente, a música de fundo do jogo. O objeto do tipo "Audio" é acoplado na câmera, para sempre acompanhar a visão do jogador e manter sempre o mesmo volume.

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184790441-0df1d358-2b98-465d-8495-69eb1273dcca.png">
</p>

Aqui temos uma visão dos atributos da classe Player, que tem um objeto instanciado no jogo. Essa classe possui componentes como: Um corpo rígido (para cálculos de gravidade e massa), um colisor em formato de cápsula (formato aproximado do objeto) e um colisor de formato de caixa (simula o pé do personagem, fator importante na lógica de pulos usada). Também possui um script de movimento, que pega inputs do teclado e transforma-os em movimento. O script será explicado.

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184790511-b006bd82-3334-462e-be0b-9c6f90e1a1d7.png">
</p>

Na imagem acima, apresentamos uma classe chamada Enemy Spawner, que tem por objetivo instanciar os objetos quadráticos que dificultam a vida do jogador. É uma classe bem mais simples, que possui apenas um Transform (acopla atributos posicionais em âmbitos 2D e 3D) e um script.

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184790544-c77eb711-ff61-4deb-9318-697d5b079cb1.png">
</p>

Aqui são mostradas as classes do projeto. Na engine, são mais conhecidas como "Prefabs". Em jogos Unity, apenas são transformados em prefabs objetos que serão reutilizados. Ao serem postos no jogo, podem ter seus atributos e métodos alterados, assim como em qualquer outro âmbito que envolva programação orientada a objetos.

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184791010-c513b141-1c89-4cdd-a76b-e94a9484c80a.png">
</p>

Nessa imagem é explorado como são cortados os sprites para que se tornem animações. Tanto os cortes, quanto o redimensionamento e a criação de animações são feitas de dentro da interface da engine.

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184791042-6f9dd3a4-06b7-462b-a0f9-edf8471539d3.png">
</p>

Essa janela é chamada de "Animator", é utilizada para controlar transições entre animações, por meio de booleanos alterados a nível de código, assim como atrasos e outras configurações das transições.

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184791076-55781644-5931-45a6-85c1-2bbf06d8af02.png">
</p>

Um level é mais comumente chamado de "Scene", e objetos do jogo podem transitar entre cenas. No nosso pequeno projeto foi criada apenas uma cena. Mas outras podem ser facilmente criadas, inclusive com os conteúdos de outras cenas já antes existentes, utilizando o copia e cola dentro do gerenciador de arquivos da interface gráfica.

### Programação de Comportamentos e Atributos de Objetos

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184791079-42ae1da2-ea03-4f7f-94e9-8d40a1da8b8d.png">
</p>

Código que controla objetos quadráticos do jogo (os que voam da esquerda para a direita e servem tanto para atrapalhar quanto para ajudar o jogador). Basicamente o importante é que temos um atributo "xVelocity" que pode ser escolhido pelo objeto que instancia esses quadrados ou até mesmo na engine. Além disso, temos a função "render()" que é chamada a cada frame, e move o corpo físico do objeto para a esquerda do mapa, alterando a posição x e mantendo y sempre livre para mudar conforme interferências externas (contato com o player ou com plataformas / objetos voadores.

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184791085-a71412c8-df75-4778-a694-df7c1c72dd6d.png">
</p>

Esse objeto se localiza na extrema esquerda do mapa e tem a exclusiva função de destruir quaisquer objetos (no caso do jogo sempre serão os quadrados voadores) assim que houver colisão com tais (onCollisionEnter2D).

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184791096-652b5f09-a292-4698-8b68-b15affd728da.png">
</p>

Essa classe de objetos destina-se à instanciação de quadrados voadores. Como podemos ver, ela não tem uma render. Logo no construtor é chamada uma co-rotina que é o modo que usamos no Unity para chamar funções dados certos intervalos de tempo, junto com a função yield (retorna objeto que espera um determinado tempo, para continuar). O intervalo em si é determinado pelo atributo spawningInterval, que é setado exclusivamente na engine. É possível observar que essa co-rotina não interfere nas outras funções do jogo, ou seja, é um código executado em paralelo (a nível de threads).

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184791113-eab1f326-0e20-453d-8c4b-53d88f088ed2.png">
</p>

O script de movimento do jogador, logo no seu construtor, captura os componentes do player (setados na engine) para que possa usá-los em suas funções. O player possui também outros atributos como booleanos de controle e parâmetros setados na interface gráfica do programador. Na render são chamadas funções, que tem como objetivo capturar inputs e transformá-los em movimentos, assim como controlar as animações.

<p align="center">
  <img src="https://user-images.githubusercontent.com/85958904/184791121-ae50a26b-2ee2-490c-9323-66efd3b04587.png">
</p>

A função run utiliza uma abstração do unity, que melhora (suaviza) o movimento do player, utilizando sua função "getAxis", já a função jump utiliza a utilidade padrão para detecção de inputs (nesse caso, a tecla espaço pressionada). A função FlipSprite apenas altera a rotação horizontal do sprite para onde o player estiver olhando.

## Ilustração da Criação da Fase

### Criação de Mapa

Aqui é mostrada a facilidade com a qual qualquer pessoa que desejar expandir o projeto pode construir mapa. Os objetos plataformas são instanciados imediatamente com suas físicas calculadas e o player pode ser movido antes do modo "Play" para testes.

https://user-images.githubusercontent.com/85958904/184787378-259ee0b5-f296-4b64-84f6-3dc269fb61e3.mp4

## Links

- [Documentação do Unity](https://docs.unity3d.com/Manual/index.html)
- [Documentação do C#](https://docs.microsoft.com/en-us/dotnet/csharp/)
- [Site de onde os Sprites foram retirados](https://itch.io/game-assets)
- [Site de onde o Background foi retirado](https://craftpix.net/)
