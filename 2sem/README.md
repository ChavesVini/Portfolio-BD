# API 2º Semestre - 02/2023

<a href="https://github.com/wiz-fatec/avaliacao-360">
  <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white"  width="200" height="65" />
</a>

# Parceiro - FATEC São José dos Campos - Prof. Jessen Vidal
![images](https://github.com/user-attachments/assets/e95aca6f-e14c-4178-97c3-997752fcba52)

# Resumo do projeto
Desenvolver uma aplicação que leia um arquivo ".csv" com respostas vindas de um formulário respondido pelos alunos a respeito do Trabalho de Graduação. O objetivo é desenvolver uma aplicação que facilite a visualização de dados e auxilie na questão de aplicação de notas e feedbacks gerando relatórios que se encaixem ao cliente.

# Tecnologias

<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/java/java-original.svg"  width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/postgresql/postgresql-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/github/github-original.svg" width="100" height="100" />
          
- PostgreSQL: Banco de dados open-source, usado para ser o banco de dados e a persistência das informações.
- Java: Utilizado no back-end para a transformação dos dados e no front-end, usando o JavaFX;
- GitHub: Foi usado para o gerenciamento das tarefas.

# Contribuições Individuais
No segundo semestre, contribui no banco de dados e no back-end (conexões do banco de dados).

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

