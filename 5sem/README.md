<h1 align="center">API 5º Semestre - 01/2025</h1>
<h1 align="center"> 
  <a href="https://github.com/new-ge/VISION"><img src="https://img.shields.io/badge/GitHub-Repositório Projeto-181717?style=for-the-badge&logo=github"></a>
</h1>

# Parceiro - [Youtan](https://youtan.com.br/)
<img width="400" height="157" alt="logo-youtan" src="https://github.com/user-attachments/assets/20d1d1bf-d29c-4101-bba0-215e135f425c" />

# Resumo do projeto
O projeto é uma solução sob medida para a Youtan, empresa especializada no desenvolvimento de softwares e aplicativos sob demanda desde 2002. Localizada no Parque Tecnológico de São José dos Campos e membro do TIC Vale — o maior cluster de TI do Brasil — a Youtan atua com metodologias ágeis e tecnologias modernas para transformar ideias em soluções digitais para Web, Desktop e Mobile. Além disso, oferece o SIGI, seu próprio ERP em modelo SaaS, voltado para empresas B2B de pequeno e médio porte.

# Tecnologias
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/figma/figma-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/java/java-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/jira/jira-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/git/git-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/postman/postman-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/html5/html5-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/javascript/javascript-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vuejs/vuejs-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vscode/vscode-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/github/github-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/typescript/typescript-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/spring/spring-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/postgresql/postgresql-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/docker/docker-original.svg" width="100" height="100"/>
          
- Figma: Esquematização do protótipo da aplicação;
- Java e Spring-Boot: Usado no Back-End para testar as APIs e a Java
- Jira: Gerenciamento das tarefas;
- Git e GitHub: Em conjunto ao GitHub, é usado para versionamento de código
- Postman: Ferramenta para testar APIs.
- HTML, JavaScript, TypeScript e VueJS: Ferramentas usadas no front-end para criação de páginas e aplicações web robustas.
- Vscode: Editor de código para fazer os códigos.
- PostgreSQL: Banco de dados relacional usado pro projeto.
- Docker: Plataforma para criar, gerenciar e rodar aplicações em contêineres.

# Contribuições Individuais
No quinto semestre, contribui com o front-end, back-end e o banco de dados.

Atuei no back-end com as lógicas do SQL para puxar os dados do banco para tratar no back-end.
<details>
<summary> Código em Java - SQL Query no Back-End </summary>
  
```java
     @Query(value = """
                    select u.usr_name, p.project_name, m.milestone_name, SUM(ut.quant)
                    from usr_task ut
                    join milestone m on m.milestone_code = ut.milestone_code
                    join usr u ON u.usr_code = ut.usr_code 
                    join project p on p.project_code = ut.project_code
                    WHERE (:milestone IS NULL OR m.milestone_name = :milestone)
                    AND (:project IS NULL OR p.project_name = :project)
                    AND (:user IS NULL OR u.usr_name = :user)
                    group by u.usr_name, m.milestone_name, p.project_name
                    order by m.milestone_name asc
                    """, nativeQuery = true)
     List<MilestoneDto> countCardsPerSprintAdmin(@Param("milestone") String milestone,
                                                   @Param("project") String project,
                                                   @Param("user") String user);
```
</details>

Atuei no front-end para aplicar as lógicas dos filtros que vinha do back-end para o front-end
<details>
<summary> Código em JavaScript - Lógica do filtro front-end </summary>
  
```javascript
    const fetchData = async (url, labelsRef, dataRef, transformFunction = null, groupByKey = null) => {
      try {
        const response = await axios.get(url);
        const data = response.data;

        const updated = ref(false);

        if (Array.isArray(data)) {
          data.forEach(item => {
            if (!sprintSet.value.has(item.milestoneName)) {
              sprintSet.value.add(item.milestoneName);
              updated.value = true;
            }
          });

          if (updated.value) {
            sprintList.value = Array.from(sprintSet.value).sort((a, b) =>
              a.localeCompare(b, undefined, { numeric: true, sensitivity: 'base' })
            );
          }
        }

        if (transformFunction && typeof transformFunction === 'function') {
          const { labels, dataPoints } = transformFunction(data);
          labelsRef.value = labels;
          dataRef.value = dataPoints;
        } else if (Array.isArray(data)) {
          const groupedCounts = {};

          const keyToGroup = groupByKey ?? (
            'statusName' in data[0] ? 'statusName' :
            'milestoneName' in data[0] ? 'milestoneName' :
            'projectName' in data[0] ? 'projectName' :
            'userName' in data[0] ? 'userName' :
            (data.length >= 2 &&
              'rework' in data[data.length - 1] &&
              'finished' in data[data.length - 2]) ? 'rework-finished' :
              null
          );

          if (keyToGroup) {
            if (keyToGroup === 'rework-finished') {
              let reworkTotal = 0;
              let finishedTotal = 0;

              data.forEach(item => {
                reworkTotal += item.rework ?? 0;
                finishedTotal += item.finished ?? 0;
              });

              groupedCounts['Retrabalho'] = reworkTotal;
              groupedCounts['Concluidas'] = finishedTotal;

              labelsRef.value = ['Retrabalho', 'Concluidas'];
              dataRef.value = [groupedCounts['Retrabalho'], groupedCounts['Concluidas']];
            } else {
              data.forEach(item => {
                const key = item[keyToGroup];
                if (key != null) {
                  const quant = item.quant ?? 0;
                  groupedCounts[key] = (groupedCounts[key] || 0) + quant;
                }
              });

              labelsRef.value = Object.keys(groupedCounts);
              dataRef.value = Object.values(groupedCounts);
            }
          } else {
            labelsRef.value = [];
            dataRef.value = [];
          }
        } else if (typeof data === 'object' && data !== null) {
          const key = groupByKey && groupByKey in data ? data[groupByKey] : null;
          const quant = data.quant ?? 0;
          if (key) {
            labelsRef.value = [key];
            dataRef.value = [quant];
          }
        } else if (typeof data === 'number') {
          dataRef.value = [data];
        }
      } catch (error) {
        console.error(`Erro ao buscar dados de ${url}:`, error);
      }
    };
```
</details>

Atuei também para criar o endpoint que pega todas as informações no Dashboard e transforma em Excel
<details>
<summary> Código em Java - Endpoint exportar Excel </summary>
  
```java
    @GetMapping("/request-excel")
    public ResponseEntity<Void> exportToExcel(HttpServletResponse response, 
                              @RequestParam(required = false) String milestone,
                              @RequestParam(required = false) String project,
                              @RequestParam(required = false) String user)  {
        try {
            List<String> accessList = uRepo.accessControl();
            Workbook workbook = new XSSFWorkbook();

            List<StatsDto> statsList;
            List<MilestoneDto> tasksSprint;
            List<TaskStatusHistoryDto> reworkDetails;
            List<TagDto> tagList;
            List<MilestoneDto> tasksSprintClosed;

            if (accessList.contains("Stakeholder")) {
                statsList = sRepo.countTasksByStatusManager(milestone, project, user);
                tasksSprint = mRepo.countCardsPerSprintManager(milestone, project, user);
                reworkDetails = tshRepo.findTaskStatusHistoryWithReworkFlagManager(milestone, project, user);
                tagList = tagRepo.countTasksByTagManager(milestone, project, user);
                tasksSprintClosed = mRepo.countCardsClosedPerSprintManager(milestone, project, user);
            } else if(accessList.contains("UX") ||
                      accessList.contains("Back") ||
                      accessList.contains("Front") ||
                      accessList.contains("Design")) {
                statsList = sRepo.countTasksByStatusOperator(milestone, project, user);
                tasksSprint = mRepo.countCardsPerSprintOperator(milestone, project, user);
                reworkDetails = tshRepo.findTaskStatusHistoryWithReworkFlagOperator(milestone, project, user);
                tagList = tagRepo.countTasksByTagOperator(milestone, project, user);
                tasksSprintClosed = mRepo.countCardsClosedPerSprintOperator(milestone, project, user);
            } else {
                statsList = sRepo.countTasksByStatusAdmin(milestone, project, user);
                tasksSprint = mRepo.countCardsPerSprintAdmin(milestone, project, user);
                reworkDetails = tshRepo.findTaskStatusHistoryWithReworkFlagAdmin(milestone, project, user);
                tagList = tagRepo.countTasksByTagAdmin(milestone, project, user);
                tasksSprintClosed = mRepo.countCardsClosedPerSprintAdmin(milestone, project, user);
            }

            Sheet statusSheet = workbook.createSheet("Tarefas por Status");

            Row headerStatusRow = statusSheet.createRow(0);
            headerStatusRow.createCell(0).setCellValue("Projeto");
            headerStatusRow.createCell(1).setCellValue("Operador");
            headerStatusRow.createCell(2).setCellValue("Sprint");
            headerStatusRow.createCell(3).setCellValue("Status");
            headerStatusRow.createCell(4).setCellValue("Qtd Tarefas");

            int rowIdxStatus = 1;
            for (StatsDto stats : statsList) {
                Row row = statusSheet.createRow(rowIdxStatus++);
                row.createCell(0).setCellValue(stats.getProjectName());
                row.createCell(1).setCellValue(stats.getUserName());
                row.createCell(2).setCellValue(stats.getMilestoneName());
                row.createCell(3).setCellValue(stats.getStatusName());
                row.createCell(4).setCellValue(stats.getQuant());
            }

            Sheet createdCardsSheet = workbook.createSheet("Tarefas Criadas");

            Row headerCreatedCardsRow = createdCardsSheet.createRow(0);
            headerCreatedCardsRow.createCell(0).setCellValue("Projeto");
            headerCreatedCardsRow.createCell(1).setCellValue("Operador");
            headerCreatedCardsRow.createCell(2).setCellValue("Sprint");
            headerCreatedCardsRow.createCell(3).setCellValue("Qtd Tarefas Criadas");

            int rowIdxCreatedCards = 1;
            for (MilestoneDto milestoneDto : tasksSprint) {
                Row row = createdCardsSheet.createRow(rowIdxCreatedCards++);
                row.createCell(0).setCellValue(milestoneDto.getProjectName());
                row.createCell(1).setCellValue(milestoneDto.getUserName());
                row.createCell(2).setCellValue(milestoneDto.getMilestoneName());
                row.createCell(3).setCellValue(milestoneDto.getQuant());
            }

            Sheet reworkSheet = workbook.createSheet("Retrabalhos");

            Row headerReworkSheet = reworkSheet.createRow(0);
            headerReworkSheet.createCell(0).setCellValue("Projeto");
            headerReworkSheet.createCell(1).setCellValue("Operador");
            headerReworkSheet.createCell(2).setCellValue("Sprint");
            headerReworkSheet.createCell(3).setCellValue("Qtd Retrabalhos");

            int rowIdxRework = 1;
            for (TaskStatusHistoryDto rework : reworkDetails) {
                Row row = reworkSheet.createRow(rowIdxRework++);
                row.createCell(0).setCellValue(rework.getProjectName());
                row.createCell(1).setCellValue(rework.getUserName());
                row.createCell(2).setCellValue(rework.getMilestoneName());
                row.createCell(3).setCellValue(rework.getRework());
            }

            Sheet tagSheet = workbook.createSheet("Tarefas por Tag");

            Row headerTagSheet = tagSheet.createRow(0);
            headerTagSheet.createCell(0).setCellValue("Projeto");
            headerTagSheet.createCell(1).setCellValue("Operador");
            headerTagSheet.createCell(2).setCellValue("Sprint");
            headerTagSheet.createCell(3).setCellValue("Tag");
            headerTagSheet.createCell(4).setCellValue("Qtd Tarefas por Tag");

            int rowIdxTag = 1;
            for (TagDto tag : tagList) {
                Row row = tagSheet.createRow(rowIdxTag++);
                row.createCell(0).setCellValue(tag.getProjectName());
                row.createCell(1).setCellValue(tag.getUserName());
                row.createCell(2).setCellValue(tag.getMilestoneName());
                row.createCell(3).setCellValue(tag.getTagName());
                row.createCell(4).setCellValue(tag.getQuant());
            }

            Sheet tasksClosedSheet = workbook.createSheet("Tarefas Finalizadas");

            Row headerTasksClosedSheet = tasksClosedSheet.createRow(0);
            headerTasksClosedSheet.createCell(0).setCellValue("Projeto");
            headerTasksClosedSheet.createCell(1).setCellValue("Operador");
            headerTasksClosedSheet.createCell(2).setCellValue("Sprint");
            headerTasksClosedSheet.createCell(3).setCellValue("Qtd Tarefas Finalizadas");

            int rowIdxTaskClosed = 1;
            for (MilestoneDto sprintClosed : tasksSprintClosed) {
                Row row = tasksClosedSheet.createRow(rowIdxTaskClosed++);
                row.createCell(0).setCellValue(sprintClosed.getProjectName());
                row.createCell(1).setCellValue(sprintClosed.getUserName());
                row.createCell(2).setCellValue(sprintClosed.getMilestoneName());
                row.createCell(3).setCellValue(sprintClosed.getQuant());
            }

            response.setContentType("application/vnd.openxmlformats-officedocument.spreadsheetml.sheet");
            response.setHeader("Content-Disposition", "attachment");

            OutputStream outputStream = response.getOutputStream();
            workbook.write(outputStream);
            workbook.close();
            outputStream.close();

            return ResponseEntity.ok().build();
        } catch (Exception e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).build();
        }
    }
```
</details>



# Aprendizados Efetivos
Consegui desempenhar minhas tarefas com eficiência, cumprindo com todos os prazos. Além disso, colaborei com colegas de equipe, auxiliando-os em suas demandas e contribuindo para a melhoria da dinâmica do grupo.

## Hard Skills
<details>
  
| Habilidade | Nota | Classificação |
| :-----: | :-----: | :-----: | 
| Figma |	★★★☆☆ | Entendi |
| Java | ★★★★★ | Sei fazer com autonomia |
| Jira | ★★★☆☆ | Entendi |
| Git |	★★★★☆ | Sei fazer com ajuda |
| Postman | ★★★★☆ | Sei fazer com autonomia |
| HTML5 | ★★★☆☆ | Entendi |
| JavaScript |	★★★☆☆ | Entendi |
| VueJS | ★★★☆☆ | Entendi |
| VSCode | ★★★★★ | Sei fazer com autonomia |
| GitHub | ★★★★★ | Sei fazer com autonomia |
| TypeScript | ★★★☆☆ | Entendi |
| Spring Boot | ★★★★★ | Sei fazer com autonomia |
| PostgreSQL |	★★★★★ | Sei fazer com autonomia |
| Docker | ★★★☆☆ | Entendi |

</details> 

## Soft Skills
<details>
  
| Habilidade | Classificação |
| :-----: | :-----: |
| Trabalho em equipe | Busquei não ficar parado e ajudar quem estava ao meu alcance |
| Responsabilidade | Tive o cuidado de não deixar as demandas para depois e fazer-las de acordo com o prazo estipulado |
| Colaboração	| Terminava minhas demandas e ajudava quem precisasse de ajuda |

</details> 

<h1 align="center">Outros Projetos</h1>

- [1º Semestre - Avaliação 360º](../1sem/README.md)
- [2º Semestre - TG Manager](../2sem/README.md)
- [3º Semestre - Dom Rock Pipeline Configurator](../3sem/README.md)
- [4º Semestre - GEO-IOT](../4sem/README.md)
- 5º Semestre - VISION

<p align="center"><a href="../README.md">Voltar a página inicial</p>

