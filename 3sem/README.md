<h1 align="center">API 3º Semestre - 01/2024</h1>
<h1 align="center"> 
  <a href="https://github.com/wiz-fatec/dom-rock-pipeline-configurator"><img src="https://img.shields.io/badge/GitHub-Repositório Projeto-181717?style=for-the-badge&logo=github"></a>
</h1>

# Parceiro - [Dom Rock](https://www.domrock.net/)
![a0bc80_bcc237714ccb4f7d8fdfbb89fe82b5e1~mv2](https://github.com/user-attachments/assets/4fd76054-9187-4ff9-b23a-afacb5c039b8)

# Resumo do projeto
  Dom Rock possui uma arquitetura de processamento de dados encadeados denominado pipeline que contempla alguns estágios. Esses estágios são orquestrados de forma automatizada mediante características das fontes de dados e soluções de algoritmos de IA ou modelos matemáticos em função do negócio dos clientes. Na metodologia de implantação da solução, existe a 
necessidade de configurar as fontes de dados envolvidas para que a plataforma possa operar. Essa configuração, atualmente, é manual e trata-se de um passo crítico e fundamental que consome muito tempo de técnicos.  

  Criamos uma interface amigável para configuração das fontes de dados em alguns estágios que levarão a dois benefícios tangíveis:  
  
1) Maior agilidade de configurar implantação para clientes Dom Rock;
2) Diminuir a dependência de técnicos especialistas para a configuração.

# Tecnologias Utilizadas
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/java/java-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/spring/spring-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/slack/slack-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/figma/figma-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/postman/postman-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vuejs/vuejs-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/mysql/mysql-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/dbeaver/dbeaver-original.svg" width="100" height="100" />

-	Java e SpringBoot - Essenciais para formar a lógica por trás da interface;
-	Vue.js - Criar a interface de forma interativa, para saber exatamente por qual caminho entrar;
-	Slack - Comunicação com o cliente;
-	Figma - Criação do estilo da interface;
-	Postman - Interação do código backend com o servidor, usando endpoints para isso;
-	MySQL - Banco de Dados utilizado para armazenar as informações de forma segura;
-	DBeaver - Manipulação do Banco de Dados, discutindo como seria guardado os dados em cada tabela;

# Contribuições Pessoais
No terceiro semestre, contribui com o back-end, desenvolvendo também, algumas partes no front-end:

O algoritmo de ler o cabeçalho do Excel

Montei o algoritmo mais importante do projeto, para ler um arquivo Excel de sua escolha, e meu algoritmo ia ler e retornar os nomes dos cabeçalhos. 
Foi reciclado um pedaço de um código do segundo semestre (2023-2) para trazer os cabeçalhos e o conteúdo dentro do CSV, fazendo uma lógica para trazer apenas os cabeçalhos, fazendo o mesmo para ler arquivos Excel (XLSX, XLSM, etc). Não foi tão dificil de desenvolver, mas gostaria que fosse um pouco mais desafiador, até porque foi só incrementar mais algumas linhas e saiu funcionando tranquilamente.

<details>

<img width="416" alt="primeira_contribuicao" src="https://github.com/user-attachments/assets/77218d9f-c399-46a7-9e97-186ffda80aa2">

</details>
  
Algoritmo para selecionar entre Excel e CSV

Consistia em, quando escrevesse o tipo do Excel, ele selecionaria qual tipo de “leitor” iria processar o arquivo, sendo “CSV” para arquivos CSV e “Excel” para arquivos tipo Excel (XLSX, XLS, XLSB, etc...);
Foi um dos códigos mais trabalhosos que tive que fazer, por conta de todas as validações, fazer as rotas do front e back foi um verdadeiro desafio.

<details>

<img width="331" alt="segunda_contribuicao" src="https://github.com/user-attachments/assets/b049332b-4898-4e5f-b620-fd5386436419">

</details>

Correção de bugs advindas de soluções ou que surgiam.

Alguns bugs que apareciam ou notavam no projeto, atuei para inibir e se certificar de que não haveria mais bugs na solução desenvolvida. No backend, atuei consertando problemas de lógicas incorretas ou códigos quebrados.
No backend foi bem mais fácil do que no frontend, já que tive mais contato com a parte do backend. O frontend foi puramente pesquisa e implementação, foi bem complicado de inicio, mas depois acabei notando que era igual ao que já havia implementado, agora em outra linguagem. Mesmo com as semelhanças do Java e JavaScript, foi dificil se adaptar a esse novo "estilo" de codar.
Por mais que tivesse mais focado em backend, ajudei também as pessoas do front para desenvolverem suas soluções, baseado nos exemplos nos códigos já feitos e em fóruns da internet, acabei entendendo o básico que ajudou a completar o projeto. 

<details>

<img width="511" alt="terceira_contribuicao" src="https://github.com/user-attachments/assets/fe2777bb-fd83-494e-9915-01055edcfaf7">

</details>

# Aprendizados Efetivos
Considero o semestre que mais mudou o meu jeito de pensar e fazer os códigos, pois tivemos muitas mudanças no grupo. Mas, consegui desenvolver a linguagem mais ainda e o Spring-Boot. Sobre front-end, peguei algumas coisas leves para consertar, mas não tive um grande desenvolvimento na linguagem, coisa que ia mudar no próximo semestre

# Hard Skills

<details>

| Habilidade | Nota | Classificação |
| :-----: | :-----: | :-----: |
| GitHub |	★★★☆☆ | Entendi |
| Java | ★★★☆☆ | Entendi |
| Spring Boot | ★★★☆☆ | Entendi |
| TypeScript | ★★☆☆☆ | Já ouvi falar |
| JavaScript | ★★☆☆☆ | Já ouvi falar |
| Vue.js | ★★☆☆☆ | Já ouvi falar |
| CSS | ★★☆☆☆ | Já ouvi falar |
| HTML | ★★☆☆☆ | Já ouvi falar |

</details>

# Soft Skills
<details>

| Habilidade |	Classificação |
| :-----: | :-----: |
| Pró-atividade |	Sugeri tarefas e adiantei algumas coisas da API para que não precisasse fazer tudo na última semana |
| Autonomia | Fui muito autonomo e não precisei de ajuda nenhuma para desenvolver minhas demandas |

</details>

<h1 align="center">Outros Projetos</h1>

- [1º Semestre - Avaliação 360º](../1sem/README.md)
- [2º Semestre - TG Manager](../2sem/README.md)
- 3º Semestre - Dom Rock Pipeline Configurator
- [4º Semestre - GEO-IOT](../4sem/README.md)
- [5º Semestre - VISION](../5sem/README.md)
<p align="center"><a href="../README.md">Voltar a página inicial</p>

