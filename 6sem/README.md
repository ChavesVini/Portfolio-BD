<h1 align="center">API 6º Semestre - 02/2025</h1>
<h1 align="center"> 
  <a href="https://github.com/new-ge/LuminIA"><img src="https://img.shields.io/badge/GitHub-Repositório Projeto-181717?style=for-the-badge&logo=github"></a>
</h1>

# Parceiro - [PRO4TECH](https://www.pro4tech.com.br/)
<img width="230" height="219" alt="images" src="https://github.com/user-attachments/assets/eeaab6f5-e177-4585-8fa5-63257c654ebb" />

# Resumo do projeto
A situação atual mostra que o banco de dados de suporte já não atende bem às necessidades da operação: ele não garante totalmente a conformidade com a lei, não oferece clareza suficiente para auditorias e nem facilita análises históricas ou preditivas. Isso significa que os relatórios acabam sendo engessados e repetitivos, sem dar uma visão completa para quem precisa tomar decisões. A proposta de modernização busca corrigir essas limitações, trazendo mais segurança no tratamento dos dados, transparência no acompanhamento das ações e dashboards que ajudem cada nível da equipe – desde os analistas até gestores e responsáveis estratégicos – a visualizar exatamente as informações de que precisam para trabalhar com mais eficiência.

# Tecnologias
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/figma/figma-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/python/python-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/fastapi/fastapi-original.svg" width="100" height="100"/>  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/jira/jira-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/git/git-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/github/github-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/html5/html5-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/javascript/javascript-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vuejs/vuejs-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vscode/vscode-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/mongodb/mongodb-original.svg" width="100" height="100"/> <img width="100" height="100" alt="59021421" src="https://github.com/user-attachments/assets/adc1dea0-9096-4db4-9779-6794e2f78f04" /> <img width="300" height="100" alt="1_2fD9qRIlRAJay7D5NK3yEA" src="https://github.com/user-attachments/assets/5207b14e-7600-4ce7-855a-2ddc2e53cdc2" /> <img width="100" height="100" alt="Scikit_learn_logo_small svg" src="https://github.com/user-attachments/assets/911787bf-cb3f-4bac-8d92-3977c80c18e8" />

- Figma: Esquematização do protótipo da aplicação;
- Python e FastApi: Usado no Back-End para testar as APIs.
- Jira: Gerenciamento das tarefas;
- Git e GitHub: Em conjunto ao GitHub, é usado para versionamento de código.
- Postman: Ferramenta para testar APIs.
- HTML, JavaScript, TypeScript e VueJS: Ferramentas usadas no front-end para criação de páginas e aplicações web robustas.
- VsCode: Editor de código para fazer os códigos.
- MongoDB: Banco de dados não relacional usado pro projeto.
- Flair, Prophet e Sci-Kit: ML's usados para o projeto para prever sentimento, faq automatizado e previsões de demanda.

# Contribuições Individuais
No sexto semestre, contribui como SM, banco de dados, back-end e front-end.

Atuei no back-end para trazer os dados do SQL Server e tratar no MongoDB.

<details>
<summary> Código em Python - Extraindo dados do SQL Server e convertendo para o MongoDB </summary>
  
```python
import os
from api_6sem_back_end.db.db_configuration import db_data, db_connection_sql_server
import datetime
from flair.models import TextClassifier
from flair.data import Sentence
from pymongo.errors import BulkWriteError
from pymongo import UpdateOne
import json

_model = None

def get_sentiment_model():
    global _model
    if _model is None:
        _model = TextClassifier.load('sentiment-fast')
    return _model

def predict_sentiment(text: str) -> str:
    model = get_sentiment_model()
    sentence = Sentence(text)
    model.predict(sentence)
    label = sentence.labels[0]
    return label.value

def process_data_sql_server(columns_to_keep):
    sql_conn = db_connection_sql_server(
        os.getenv("DB_DRIVER"),
        os.getenv("DB_SERVER"),
        os.getenv("DB_DATABASE")
    )

    cursor = sql_conn.cursor()
    name_tables = cursor.execute(f"SELECT DISTINCT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_TYPE='BASE TABLE'").fetchall()

    all_tables = {}
    columns_by_table = {}

    for i in name_tables:
        table_name = i[0]

        if table_name in columns_to_keep:
            cols = ", ".join(columns_to_keep[table_name])
            query = f"SELECT {cols} FROM {table_name}"

            data_tables = cursor.execute(query)

            columns = [col[0] for col in cursor.description]
            rows = data_tables.fetchall()

            bson = [
                {
                    col: (
                        datetime.datetime(val.year, val.month, val.day)
                        if isinstance(val, datetime.date) and not isinstance(val, datetime.datetime)
                        else val
                    )
                    for col, val in zip(columns, row)
                }
                for row in rows
            ]

            all_tables[table_name] = bson
            columns_by_table[table_name] = columns  
        else:
            pass

    return all_tables

def create_collections_mongo_db(all_tables):
    department_lookup = {d["DepartmentId"]: d["Name"] for d in all_tables["Departments"]}
    status_lookup = {d["StatusId"]: d["Name"] for d in all_tables["Statuses"]}
    priority_lookup = {d["PriorityId"]: d["Name"] for d in all_tables["Priorities"]}
    sla_plains_lookup = {
        d["SLAPlanId"]: {"name": d["Name"], "target_minutes": d["ResolutionMins"]}
        for d in all_tables["SLA_Plans"]
    }
    product_lookup = {d["ProductId"]: d["Name"] for d in all_tables["Products"]}
    category_lookup = {d["CategoryId"]: d["Name"] for d in all_tables["Categories"]}
    sub_category_lookup = {d["SubcategoryId"]: d["Name"] for d in all_tables["Subcategories"]}
    tag_lookup = {t["TagId"]: t["Name"] for t in all_tables["Tags"]}
    access_level_lookup = {t["NivelId"]: t["Acesso"] for t in all_tables["AccessLevel"]}
    agent_access_lookup = {
        agent["AgentId"]: access_level_lookup.get(agent.get("NivelId"))
        for agent in all_tables["Agents"]
    }

    tags_by_ticket = {}
    for tt in all_tables["TicketTags"]:
        ticket_id = tt.get("TicketId")
        tag_id = tt.get("TagId")
        if ticket_id not in tags_by_ticket:
            tags_by_ticket[ticket_id] = []
        tags_by_ticket[ticket_id].append(tag_lookup.get(tag_id))

    history_by_ticket = {}
    for h in all_tables["TicketStatusHistory"]:
        if h.get("TicketId") not in history_by_ticket:
            history_by_ticket[h.get("TicketId")] = []
        history_by_ticket[h.get("TicketId")].append({
            "from": status_lookup.get(h.get("FromStatusId")),
            "to": status_lookup.get(h.get("ToStatusId")),
        })

    users_docs, tickets_docs, history_docs = [], [], []

    for agent in all_tables["Agents"]:
        user_collection = {
            "agent_id": agent["AgentId"],
            "name": agent["FullName"],
            "email": agent["Email"],
            "department": department_lookup.get(agent.get("DepartmentId")),
            "role": access_level_lookup.get(agent.get("NivelId")).strip(),
            "isActive": agent.get("IsActive", True),
            "login": {
                "username": "",
                "password": ""
            },
            "modified_at": datetime.now(datetime.timezone(datetime.timedelta(hours=-3))).isoformat(timespec='seconds'),
            "created_at": datetime.now(datetime.timezone(datetime.timedelta(hours=-3))).isoformat(timespec='seconds')
        }
        users_docs.append(user_collection)

    for ticket in all_tables["Tickets"]:

        sentiment = predict_sentiment(ticket["Description"]) if ticket["Description"] else None

        ticket_collection = {
            "ticket_id": ticket["TicketId"],
            "title": ticket["Title"],
            "description": ticket["Description"],
            "status": status_lookup.get(ticket.get("StatusId") or ticket.get("CurrentStatusId")),
            "priority": priority_lookup.get(ticket.get("PriorityId")),
            "sla": {
                "name": sla_plains_lookup.get(ticket.get("SLAPlanId"), {}).get("name"),
                "target_minutes": sla_plains_lookup.get(ticket.get("SLAPlanId"), {}).get("target_minutes"),
            },
            "product": product_lookup.get(ticket.get("ProductId")),
            "category": category_lookup.get(ticket.get("CategoryId")),
            "sub_category": sub_category_lookup.get(ticket.get("SubcategoryId")),
            "tag": tags_by_ticket.get(ticket["TicketId"], []),
            "history": history_by_ticket.get(ticket["TicketId"], []),
            "created_at": ticket["CreatedAt"],
            "closed_at": ticket["ClosedAt"],
            "access_level": agent_access_lookup.get(ticket.get("AssignedAgentId")).strip(),
            "sentiment": sentiment
        }
        tickets_docs.append(ticket_collection)

    for history in all_tables["AuditLogs"]:
        history_collection = {
            "audit_id": history["AuditId"],
            "name": history["PerformedBy"],
            "operation": history["Operation"],
            "performed_at": history["PerformedAt"],
            "changed_field": list(json.loads(history["DetailsJson"]).values())[0]
        }
        history_docs.append(history_collection)

    return history_docs, tickets_docs, users_docs

def save_on_mongo_db_collections(**collections_docs):
    for collection_name, docs in collections_docs.items():
        if not docs:
            continue

        operations = []
        for doc in docs:
            if doc.get("agent_id"):
                filter_query = {"agent_id": doc["agent_id"]}
            elif doc.get("audit_id"):
                filter_query = {"audit_id": doc["audit_id"]}
            elif doc.get("ticket_id"):
                filter_query = {"ticket_id": doc["ticket_id"]}
            else:
                continue

            operations.append(
                UpdateOne(
                    filter_query,
                    {"$set": doc},
                    upsert=True
                )
            )

        if operations:
            try:
                result = db_data[collection_name].bulk_write(operations, ordered=False)

                print(
                    f"[{collection_name}] "
                    f"Inseridos: {result.upserted_count}, "
                    f"Atualizados: {result.modified_count}, "
                    f"Ignorados: {len(operations) - result.upserted_count - result.modified_count}"
                )
            except BulkWriteError as e:
                print(f"Erro ao salvar: {e.details}")

```
</details>

Atuei no back-end para implementar a lógica do login na aplicação

<details>
<summary> Código em Python - Login </summary>
  
```python
from fastapi import APIRouter
from pydantic import BaseModel
from api_6sem_back_end.db.db_configuration import db_data
from api_6sem_back_end.repositories.repository_login_security import create_jwt_token

router = APIRouter(prefix="/login", tags=["Login"])
collection = db_data["users"]

class LoginRequest(BaseModel):
    username: str
    password: str

@router.post("/validate-login")
def validate_login(login_request: LoginRequest):
    try:
        if login_request.username == "" and login_request.password == "":
            return None
        else:
            pipeline = [
                {
                    "$match": {
                        "login.username": login_request.username,
                        "login.password": login_request.password
                    }
                },
                {
                    "$project": {
                        "_id": 0,
                        "username": {"$getField": {"field": "username", "input": "$login"}},
                        "role": "$role",
                        "name": "$name"
                    }
                }
              ]
            result = list(collection.aggregate(pipeline))
            if result:
                token = create_jwt_token(result[0]["username"], result[0]["role"])
                role = result[0]["role"]
                return {"token": token, "role": role, "name": result[0]["name"], "username": result[0]["username"]}
            else:
                print("Não encontrado!")
                return False
            
    except:
        raise Exception
```
</details>

Implementei a lógica do JWT na aplicação para melhorar a segurança da aplicação

<details>
<summary> Código em Python - lógica do JWT </summary>
  
```python
import glob
import os
from dotenv import load_dotenv
from fastapi import Depends, HTTPException
import datetime
import jwt
from fastapi.security import HTTPAuthorizationCredentials, HTTPBearer

dotenv_path = glob.glob(os.path.join(os.path.dirname(__file__), "*.env"))
load_dotenv(dotenv_path[0])

security = HTTPBearer()
SECRET_KEY = os.getenv("KEY_JWT")

def create_jwt_token(username, role):
    payload = {
        "username": username,
        "role": role,
        "iat": int(datetime.datetime.now().timestamp()),
        "exp": int((datetime.datetime.now() + datetime.timedelta(hours=1)).timestamp())
    }
    token = jwt.encode(payload, SECRET_KEY, algorithm="HS256")

    return token

def verify_token(credentials: HTTPAuthorizationCredentials = Depends(security)):
    try:
        payload = jwt.decode(credentials.credentials, SECRET_KEY, algorithms=["HS256"])
        return payload
    except jwt.ExpiredSignatureError:
        raise HTTPException(status_code=401, detail="Token expirado")
    except jwt.InvalidTokenError:
        raise HTTPException(status_code=401, detail="Token inválido")
```
</details>

# Aprendizados Efetivos

Ser Scrum Master pela primeira vez trouxe muitos desafios. O principal deles foi equilibrar o nível de aprendizado da equipe, adaptando explicações, responsabilidades e expectativas de acordo com o ritmo de cada integrante. No início, isso exigiu bastante esforço, paciência e tentativa e erro para entender a melhor abordagem com cada pessoa.
Por outro lado, a experiência foi muito boa. Aprendi a melhorar minha comunicação, observar mais atentamente a forma como cada membro trabalhava e a ajustar o planejamento para manter o time produtivo sem sobrecarregar ninguém. Também desenvolvi mais sensibilidade para identificar quando alguém precisava de ajuda — mesmo sem pedir — e como motivar a equipe a entregar resultados consistentes.
Ao final, essa vivência me trouxe mais maturidade na liderança e uma visão mais clara de como facilitar o trabalho de outras pessoas dentro de um projeto ágil.

## Hard Skills
<details>
  
| Habilidade | Nota | Classificação |
| :-----: | :-----: | :-----: |
| Figma |	★★★★☆ | Sei fazer com ajuda |
| Python | ★★★★★ | Sei fazer com autonomia |
| FastApi | ★★★★★ | Sei fazer com autonomia |
| Jira | ★★★★☆ | Sei fazer com ajuda |
| Git | ★★★★☆ | Sei fazer com ajuda |
| GitHub | ★★★★★ | Sei fazer com autonomia |
| Postman | ★★★★☆ | Sei fazer com ajuda |
| HTML | ★★★★☆ | Sei fazer com ajuda |
| JavaScript | ★★★★☆ | Sei fazer com ajuda |
| TypeScript | ★★★☆☆ | Entendi |
| Vue.JS | ★★★★☆ | Sei fazer com ajuda |
| VsCode | ★★★★★ | Sei fazer com autonomia |
| MongoDB | ★★★☆☆ | Entendi |
| Flair | ★★☆☆☆ | Já ouvi falar |
| Prophet | ★★★☆☆ | Entendi |
| Sci-Kit | ★★★☆☆ | Entendi |

</details>

## Soft Skills
<details>
  
| Habilidade | Classificação |
| :----: | :----: |
| Liderança | Conduzi o time e apoiei o desenvolvimento de cada membro. |
| Comunicação clara | Para facilitar reuniões e evitar retrabalho. |
| Organização e gestão do tempo | Equilibrando tarefas de SM e desenvolvimento. |
| Escuta ativa | entendimento das necessidades e dificuldades da equipe. |
| Mediação de conflitos | Ajudei o time a manter um ambiente saudável e colaborativo. |
| Empatia | Apoiei membros com mais dificuldade. |

</details>


<h1 align="center">Outros Projetos</h1>

- [1º Semestre - Avaliação 360º](../1sem/README.md)
- [2º Semestre - TG Manager](../2sem/README.md)
- [3º Semestre - Dom Rock Pipeline Configurator](../3sem/README.md)
- [4º Semestre - GEO-IOT](../4sem/README.md)
- [5º Semestre - VISION](../5sem/README.md)
- 6º Semestre - LuminIA

<p align="center"><a href="../README.md">Voltar a página inicial</p>



