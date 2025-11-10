# Desafio Técnico – Desenvolvedor Pleno (Java / Spring Boot)

## **Objetivo**
Criar um endpoint RESTful para **listar e filtrar projetos**, com suporte a **paginação, ordenação e busca textual simples**.  
O desafio avalia a capacidade de desenvolver APIs limpas, bem estruturadas e documentadas, utilizando **Spring Boot**, **JPA** e **PostgreSQL**.

---

## **1. Requisitos Técnicos**

**Tecnologias obrigatórias**
- Java **11+**
- Spring Boot **3+**
- Spring Data JPA
- PostgreSQL
- Swagger / OpenAPI 3
- JUnit 5

---

## **2. Setup do Banco de Dados**

Crie um arquivo `docker-compose.yml` simples para subir o banco PostgreSQL localmente:

```yaml
version: '3.9'
services:
  postgres:
    image: postgres:15
    container_name: projeto-db
    restart: always
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: admin
      POSTGRES_DB: projetos
    ports:
      - "5432:5432"
```

---

## **3. Entidade `ProjetoEntity`**

Crie uma entidade `ProjetoEntity` com **pelo menos 8 campos**, misturando tipos numéricos, textuais e de data:

```java
@Entity
@Table(name = "projeto")
public class ProjetoEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nome;
    private String responsavel;
    private String secretaria;
    private String municipio;
    private Integer ano;
    private Double valorGlobal;
    private LocalDate prazoFinal;
    private String status;
}
```

---

## **4. Massa de Dados**

Um arquivo **`projetos.csv`** será fornecido para servir como base de testes.  
Você pode importar os dados manualmente no banco local para realizar os testes.

---

## **5. Endpoint Requerido**

**Rota:**  
`GET /api/projetos/buscar`

### **Requisitos Funcionais**
- Deve permitir:
  - Filtros simples por **campos específicos** via query params (ex: `municipio`, `status`, `ano`).
  - Busca textual no campo `nome` via parâmetro `termo`.
  - Paginação (`page`, `size`) e ordenação (`sort`).
  
### **Exemplos**
1. **Busca por município e status:**
   ```
   /api/projetos/buscar?municipio=Goiânia&status=Concluído
   ```
2. **Busca textual:**
   ```
   /api/projetos/buscar?termo=educação
   ```
3. **Com paginação e ordenação:**
   ```
   /api/projetos/buscar?page=0&size=10&sort=nome,asc
   ```

**Regras:**
- Se nenhum filtro for informado, retornar todos os registros paginados.
- A busca textual deve ser **case-insensitive**.
- O retorno deve estar no formato `Page<ProjetoDTO>`.

---

## **6. Swagger**

A documentação deve estar disponível em:
```
http://localhost:8080/swagger-ui.html
```

Deve conter:
- Descrição do endpoint  
- Parâmetros de entrada  
- Exemplo de resposta  

---

## **7. Testes Unitários (JUnit 5)**

Crie uma classe `ProjetoServiceTest` com **pelo menos 5 cenários básicos**:

| Cenário | Descrição |
|----------|------------|
| **1. Buscar todos os projetos** | Deve retornar todos paginados |
| **2. Buscar por campo único** | Deve filtrar corretamente |
| **3. Buscar por termo** | Deve retornar registros que contenham o termo |
| **4. Paginação e ordenação** | Deve respeitar `page`, `size` e `sort` |
| **5. Nenhum resultado encontrado** | Deve retornar página vazia sem erro |

---

## **8. Critérios de Avaliação**

| Critério | Peso |
|-----------|------|
| Implementação funcional do endpoint | 30% |
| Uso correto de JPA e filtros | 25% |
| Clareza e organização do código | 20% |
| Testes unitários | 15% |
| Documentação (Swagger e README) | 10% |

---

## **9. Estrutura Esperada de Entrega**

```
/src/main/java/.../entity/ProjetoEntity.java
/src/main/java/.../controller/ProjetoController.java
/src/main/java/.../service/ProjetoService.java
/src/main/java/.../repository/ProjetoRepository.java
/src/test/java/.../ProjetoServiceTest.java
/docker-compose.yml
/projetos.csv
/DESAFIO_TECNICO_PLENO.md
```

---

## **10. Dicas**
- Mantenha o código simples e legível.  
- Prefira consultas dinâmicas via `CriteriaBuilder` ou `Query by Example`.  
- Evite o uso de Specification API completa.  
- Utilize DTOs apenas se ajudarem na clareza da resposta.  

---

**Boa sorte!**  
O foco deste desafio é avaliar sua capacidade de construir uma API REST funcional, organizada e fácil de manter.
