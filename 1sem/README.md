# API 1º Semestre - 01/2023
<a href="https://github.com/wiz-fatec/avaliacao-360">
  <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white"  width="200" height="65" />
</a>

# Parceiro - FATEC São José dos Campos - Prof. Jessen Vidal
![images](https://github.com/user-attachments/assets/e95aca6f-e14c-4178-97c3-997752fcba52)

# Resumo do projeto
O programa viabiliza a avaliação dos alunos, se baseando na metodologia Avaliação 360º. Para liberar o acesso ao programa, teremos 2 niveis de acesso: Administrador e Aluno. Dependendo do tipo de acesso, haverá opções de tela diferentes. A tela "aluno" possibilitará ao usuário ver a turma e o grupo ao qual está inserido, avaliar os membros do seu grupo e se auto-avaliar, podendo ver os resultados das avaliações realizadas por ele. O "Administrador" terá atribuições de criar as turmas, grupos e o registro dos alunos, requisitar a avaliação e observar os resultados da mesma, atraves de dashboards operacionais e gerenciais.

# Tecnologias

<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/figma/figma-original.svg"  width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/python/python-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/trello/trello-original.svg" width="100" height="100" />
          
- Figma: Usado para a esquematização do protótipo da aplicação;
- Python: Utilizado no back-end para a transformação dos dados e no front-end, usando o PySimple GUI;
- Trello: Foi usado para o gerenciamento das tarefas.

# Contribuições Individuais
No primeiro semestre, contribui somente no back-end e no front-end e atuei em alguns bugs.
Criei o botão que permitiu o usuário voltar para a tela inicial do programa, sem precisar fechar e abrir o programa de novo, como estava ingressando e não sabia nada de programação, foi dificil para implementar esse ajuste.

<details>
  <summary>Código em Python - Botão voltar da tela de dashboards</summary>

```python
import app
import interface.dashboard_aluno as interface_dashboard_aluno
import interface.student as interface_student
import interface.createclass as interface_createclass
import connection.student as connection_student
import PySimpleGUI as sg

def create_window(name):
  global _name
  _name = name
  layout = [
    [sg.Text(interface_createclass.dev_text)],
    [sg.Button('Voltar', key='return', s=(18, 1))]
  ]

  return sg.Window('Avaliação 360 - Dashboard Aluno', layout, element_justification='c')

def event_handler(event, _):
  global _name
  if event == 'return':
    app.change_interface(interface_student.create_window(_name), interface_student.event_handler)
```
</details> 

Reverti os botões de Classe e Grupo, conforme notado antes da apresentação, para que não houvesse confusão entre os botões

<details>
  <summary>Código em Python - Reversão dos botões</summary>
    
```python 
def create_window(name):

    global student_info, group_info, window, student_evaluation

    student_raw = connection_student.get_student_by_name(name)[0]
    student_info = connection_student.resolve_student(student_raw)
    student_evaluation = connection_student.check_student_todo_evaluation(student_info['id'])

    group_info = student_info['group']
    group_name = group_info['name']

    class_room_info = group_info['class-room']
    class_room_name = class_room_info['name']

    layout = [
        [sg.Text(f'Aluno: {name}')],
        [sg.Text(f'Grupo: {group_name}')],
        [sg.Text(f'Sala: {class_room_name}')],
        [sg.Text('\n')],
        [sg.Button('Voltar'), sg.Button('Cancelar'), sg.Button('Resultados')]
    ]     
    if student_evaluation:
        layout[4].insert(1, sg.Button('Avaliação'))
    else:
        layout[4].insert(1, sg.Button('Avaliação', disabled=True, button_color=('white', 'grey')))
    window = sg.Window('Avaliação 360 - Aluno', layout, element_justification='c', resizable = True)
    return window
```
</details>

# Aprendizados Efetivos
Tive a oportunidade de começar no desenvolvimento aprendendo Python no primeiro semestre, como foi o meu inicio na carreira de Desenvolvimento tive muita dificuldade para aprender juntamente com o desenvolvimento da API.

# Hard Skills
<details>
  
| Habilidade | Nota | Classificação |
| :-----: | :-----: | :-----: | 
| Figma |	★★☆☆☆ | Já ouvi falar |
| Python | ★★☆☆☆ | Já ouvi falar |

</details> 

# Soft Skills
<details>

| Habilidade | Consideração |
| :-----: | :-----: |
| Trabalho em equipe | Busquei não ficar com dúvidas e perguntava a todo momento o que era pra fazer em cada tarefa. Busquei minha autonomia, mas certamente não conseguiria fazer nada sem meus colegas de classe me auxiliando. |
| Aprendizado | Aprendi uma linguagem nova, ainda muito inexperiente, mas busquei sempre entender a sintaxe e fazer do jeito que fui treinado a fazer |
| Organização	| Precisei me organizar para estudar e ao mesmo tempo fazer o trabalho da API |
| Resiliência	| Busquei manter o pé no chão e sempre fazer o melhor |

</details> 

# Outros Projetos
- 1º Semestre - Avaliação 360º
- [2º Semestre - TG Manager](./2sem/README.md)
- [3º Semestre - Dom Rock Pipeline Configurator](./3sem/README.md)
- [4º Semestre - GEO-IOT](./4sem/README.md)
