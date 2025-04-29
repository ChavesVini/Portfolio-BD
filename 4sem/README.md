# API 4º Semestre - 02/2024

<a href="https://github.com/manolito-fatec/geo-iot-2024-1">
  <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white"  width="200" height="65" />
</a>

# Parceiro - ITO1 Technology
![images](https://github.com/user-attachments/assets/c2b2827f-883a-4889-98d8-6e74f3db2456)

# Resumo do projeto
O projeto tem como objetivo desenvolver uma solução robusta para o armazenamento e consulta de dados de geolocalização em tempo real, gerados por dispositivos IoT, como wearables, tags e smartphones. Utilizando um banco de dados SQL, ORACLE CLOUD, a solução será capaz de lidar com grandes volumes de dados, garantindo escalabilidade e alta disponibilidade. A arquitetura proposta envolve o uso de Spring Boot para o backend, Vue.js para o frontend e comunicação eficiente via APIs RESTful e WebSocket. A segurança dos dados será garantida com criptografia e controle de acesso.

# Tecnologias
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/java/java-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/spring/spring-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/git/git-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/oracle/oracle-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/docker/docker-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vuejs/vuejs-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vitejs/vitejs-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/git/git-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/typescript/typescript-original.svg" width="100" height="100" />

- Java: Linguagem usada no back-end;
- Spring Boot: Framework usado para criação das classes e os links para o front-end e o banco de dados;
- Git: Ferramenta para versionamento de controle de versão;
- Oracle: Banco de dados utilizado para esse projeto;
- Docker: Plataforma para containerização de aplicações;
- Vue.js: Framework no front-end para fazer as telas da aplicação;
- Vite.js: Framework utilizado para builds e o desenvolvimento;
- Redis: Ferramenta para cache;
- TypeScript: Linguagem usada no front-end.

# Contribuições Individuais
No quarto semestre, contribui no front-end e no back-end.

Atuei para criar a primeira versão da modelagem do banco de dados, para utilização no nosso projeto.

<details>
  <summary> Modelagem inicial </summary>
  
![MODELAGEM INICIAL](https://github.com/user-attachments/assets/744f3810-a155-415c-91f2-79e6b53ffa4c)

</details> 

Depois de alguns erros, atuei para finalizar a última versão da modelagem do banco de dados, para utilização no nosso projeto.

<details>
  <summary>Modelagem final</summary>
  
![MODELAGEM FINAL](https://github.com/user-attachments/assets/44c38918-d7e2-4b15-ae3c-a351d86b9bac)

</details>

Também atuei para criar a conexão do back-end para o banco de dados.

<details>
  <summary>Código em Java - Conexão Back com Banco de dados</summary>
  
```java
package com.tg.manager.model;

import com.tg.manager.model.connection.ConnectionDataBase;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class ToDoModel {
    private String feedback;
    private Integer notes;
    private Integer idToDo;

    public void addToDo(String feedbacks, Integer notes, Integer idToDo) {

        try {
            ConnectionDataBase connectionDb = new ConnectionDataBase();
            Connection connection = connectionDb.getConexao();
            String insercaoSQL = "INSERT INTO tg (feedbacks, notes, idToDo) VALUES (?, ?, ?)";
            PreparedStatement preparedStatement = connection.prepareStatement(insercaoSQL);
            preparedStatement.setString(1, feedbacks);
            preparedStatement.setInt(2, notes);
            preparedStatement.setInt(3, idToDo);
            preparedStatement.executeUpdate();
            preparedStatement.close();
            connection.close();
            System.out.println("Dados inseridos com sucesso!");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

</details>


# Aprendizados Efetivos
Aprendi Java numa velocidade consideravelmente rápida, não tive muitos problemas com a sintaxe e até melhorei meu modo de desenvolver as tarefas, criei mais autonomia e desenvolvi minha colaboração. 
Gostei desse semestre pois desenvolvi a parte de banco de dados, algo que tinha visto antes da faculdade, e fui desenvolvendo cada vez mais.

## Hard Skills
<details>
  
| Habilidade |	Classificação |
| :-----: | :-----: |
| GitHub |	★★★☆☆ |
| Java | ★★☆☆☆ |
| PostgreSQL | ★★☆☆☆ |

</details> 

## Soft Skills
<details>
  
Colaboração: Busquei ouvir todas as criticas e pensar numa visão unificada que abrangesse todas as ideias dos integrantes, além de ajudar mais.

</details> 

