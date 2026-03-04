# Tarefas de Refatoração - Feature-First

## Objetivo
Refatorar o projeto para padrão feature-first mantendo a lógica interna intacta.

---

## Tasklist

### Fase 1: Preparação e Estrutura Base
- [x] 1.1 - Criar pasta `lib/core/errors/` e mover AppError
- [x] 1.2 - Criar pasta `lib/features/todos/domain/entities/` e mover entidade Todo
- [x] 1.3 - Criar pasta `lib/features/todos/domain/repositories/` e mover interface TodoRepository

### Fase 2: Camada Data
- [x] 2.1 - Criar pasta `lib/features/todos/data/models/` e mover TodoModel
- [x] 2.2 - Criar pasta `lib/features/todos/data/datasources/` e mover dataSources
- [x] 2.3 - Criar pasta `lib/features/todos/data/repositories/` e mover implementação

### Fase 3: Camada Presentation
- [x] 3.1 - Criar pasta `lib/features/todos/presentation/viewmodels/` e mover ViewModel (corrigindo dependência)
- [x] 3.2 - Criar pasta `lib/features/todos/presentation/pages/` e mover TodosPage
- [x] 3.3 - Criar pasta `lib/features/todos/presentation/widgets/` e mover AddTodoDialog

### Fase 4: Arquivos Raiz
- [x] 4.1 - Criar `lib/app.dart` (mover AppRoot)
- [x] 4.2 - Atualizar `lib/main.dart` com novos imports

### Fase 5: Documentação
- [x] 5.1 - Criar/Atualizar `ARCH.md` com diagrama e justificativas

---

## Progresso

**Data início:** Concluído
**Status:** ✅ COMPLETO

---

## Resumo das Alterações

### Nova Estrutura (feature-first)
```
lib/
  app.dart
  main.dart
  core/
    errors/
      app_error.dart
  features/
    todos/
      data/
        datasources/
          todo_local_datasource.dart
          todo_remote_datasource.dart
        models/
          todo_model.dart
        repositories/
          todo_repository_impl.dart
      domain/
        entities/
          todo.dart
        repositories/
          todo_repository.dart
      presentation/
        pages/
          todos_page.dart
        widgets/
          add_todo_dialog.dart
        viewmodels/
          todo_viewmodel.dart
```

### Alterações Importantes
1. ViewModel agora usa interface `TodoRepository` (não implementação direta)
2. Estrutura feature-first organizada
3. Imports corrigidos
4. Lógica interna mantida intacta

