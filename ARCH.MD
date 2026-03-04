# ARCH — Documentação da Arquitetura

## Estrutura Final (feature-first)

```
lib/
  app.dart              # Configuração raiz do MaterialApp
  main.dart             # Entry point com Provider setup
  core/
    errors/
      app_error.dart    # Classe de erro genérica
  features/
    todos/
      data/
        datasources/
          todo_local_datasource.dart    # SharedPreferences (armazenamento local)
          todo_remote_datasource.dart   # HTTP client (API externa)
        models/
          todo_model.dart               # Model com parsing JSON
        repositories/
          todo_repository_impl.dart     # Implementação do repositório
      domain/
        entities/
          todo.dart                     # Entidade de domínio (puro)
        repositories/
          todo_repository.dart         # Interface abstrata do repositório
      presentation/
        pages/
          todos_page.dart               # Página principal de Todos
        widgets/
          add_todo_dialog.dart          # Dialog para adicionar todo
        viewmodels/
          todo_viewmodel.dart           # Estado e lógica de UI
```

---

## Fluxo de Dependências

```
UI (TodosPage)
   ↓
ViewModel (TodoViewModel)
   ↓
Repository (TodoRepository interface)
   ↓
DataSources (Remote + Local)
```

**Direção das dependências:** UI → ViewModel → Repository → DataSources

---

## Decisões de Responsabilidade

### 1. Validação
- **Onde ficou:** No `TodoViewModel`
- **Justificativa:** O ViewModel é responsável por validar dados antes de enviar ao repositório. Isso mantém a UI "burra" (apenas exibe) e o ViewModel "inteligente" (contém regras de negócio simples como validação de título vazio).

### 2. Parsing JSON
- **Onde ficou:** No `TodoModel` (camada data)
- **Justificativa:** O Model é responsável por converter JSON → objeto e objeto → JSON. Mantém a transformação de dados封装ada e isolada na camada de dados.

### 3. Tratamento de Erros
- **Onde ficou:** 
  - Exceptions nativas de HTTP/DIO no `TodoRemoteDataSource`
  - Try-catch no `TodoViewModel` para capturar e exibir mensagens de erro
  - `AppError` em `core/errors/` para erros genéricos da aplicação
- **Justificativa:** Errors são tratados em camadas:
  - DataSource lança exceptions para falhas de rede
  - ViewModel captura exceptions e define `errorMessage` para UI exibir
  - UI observa `errorMessage` e exibe feedback ao usuário

### 4. Escolha Remote vs Local
- **Onde ficou:** No `TodoRepositoryImpl`
- **Justificativa:** O Repository é o "orquestrador" que decide de onde buscar dados. Neste projeto didático, sempre busca do remoto e salva timestamp localmente.

### 5. ViewModel sem dependência de Widgets
- **Mudança realizada:** ViewModel agora usa `TodoRepository` (interface), não `TodoRepositoryImpl` (implementação)
- **Justificativa:** Segue Dependency Inversion Principle (DIP) - dependências de módulos de alto nível (ViewModel) apontam para abstrações (interfaces), não implementações.

---

## Regras do Checklist Atendidas

Projeto compila e roda (flutter analyze passa)
Estrutura de pastas organizada (feature-first)
UI não chama HTTP nem SharedPreferences diretamente
ViewModel não conhece Widgets/BuildContext (exceto estado)
Repository centraliza escolha entre remoto/local

