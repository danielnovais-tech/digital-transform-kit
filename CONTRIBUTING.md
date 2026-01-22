# Guia de Contribuição

Obrigado por seu interesse em contribuir para o Digital Transform Kit! Este documento fornece diretrizes para ajudá-lo a colaborar de forma eficaz.

## 📋 Índice

- [Como Abrir um Pull Request](#como-abrir-um-pull-request)
- [Padrões de Commit](#padrões-de-commit)
- [Fluxo de Revisão e Aprovação](#fluxo-de-revisão-e-aprovação)
- [Regras de Branches](#regras-de-branches)
- [Processo de Desenvolvimento](#processo-de-desenvolvimento)

## 🔄 Como Abrir um Pull Request

1. **Fork o repositório** e clone-o localmente
2. **Crie uma branch** seguindo as [regras de branches](#regras-de-branches)
3. **Faça suas alterações** seguindo os [padrões de commit](#padrões-de-commit)
4. **Execute os testes** e validações (se aplicável)
5. **Push sua branch** para seu fork
6. **Abra um Pull Request** usando o template fornecido
7. **Aguarde a revisão** e responda aos comentários

### Checklist antes de abrir um PR

- [ ] O código segue os padrões do projeto
- [ ] Os testes foram executados e estão passando
- [ ] A documentação foi atualizada (se necessário)
- [ ] O commit segue o padrão Conventional Commits
- [ ] Não há conflitos com a branch base

## 📝 Padrões de Commit

Este projeto utiliza [Conventional Commits](https://www.conventionalcommits.org/pt-br/) para manter um histórico de commits claro e consistente.

### Formato

```
<tipo>[escopo opcional]: <descrição>

[corpo opcional]

[rodapé opcional]
```

### Tipos Principais

- **feat**: Nova funcionalidade
- **fix**: Correção de bug
- **docs**: Alterações na documentação
- **style**: Formatação, espaços em branco, etc (sem mudança de código)
- **refactor**: Refatoração de código (sem adicionar funcionalidade ou corrigir bugs)
- **perf**: Melhorias de performance
- **test**: Adição ou correção de testes
- **chore**: Tarefas de manutenção, configuração, etc
- **ci**: Alterações em arquivos de CI/CD

### Exemplos

```bash
feat: adicionar template de PR
fix: corrigir validação de formulário
docs: atualizar guia de contribuição
chore: configurar GitHub Actions
ci: adicionar workflow de markdown lint
```

### Commits com Breaking Changes

Para mudanças que quebram compatibilidade, adicione `!` após o tipo ou `BREAKING CHANGE:` no rodapé:

```bash
feat!: alterar estrutura de API
```

ou

```bash
feat: alterar estrutura de API

BREAKING CHANGE: o endpoint /api/v1 foi removido
```

## 🔍 Fluxo de Revisão e Aprovação

### Processo de Revisão

1. **Submissão**: Contribuidor abre um PR
2. **Revisão Automática**: GitHub Actions executa verificações
3. **Revisão por Pares**: Mantenedores revisam o código
4. **Correções**: Contribuidor ajusta conforme feedback
5. **Aprovação**: PR é aprovado por pelo menos um mantenedor
6. **Merge**: PR é mesclado na branch apropriada

### Critérios de Aprovação

- ✅ Todas as verificações automáticas passaram
- ✅ Código revisado e aprovado por pelo menos 1 mantenedor
- ✅ Sem conflitos com a branch base
- ✅ Documentação atualizada (se aplicável)
- ✅ Commits seguem o padrão estabelecido

### Tempo de Resposta

- **Primeira resposta**: Até 3 dias úteis
- **Revisão completa**: Até 7 dias úteis
- **Urgências**: Identificadas com label `priority:high`

## 🌿 Regras de Branches

### Estrutura de Branches

- **`main`**: Branch de produção
  - Sempre estável e pronta para deploy
  - Protegida contra push direto
  - Requer aprovação de PR

- **`develop`**: Branch de desenvolvimento
  - Integração de funcionalidades
  - Base para criar feature branches
  - Merge para `main` em releases

- **`feature/*`**: Novas funcionalidades
  - Formato: `feature/nome-da-funcionalidade`
  - Exemplo: `feature/add-automation-script`
  - Base: `develop`

- **`bugfix/*`**: Correções de bugs
  - Formato: `bugfix/descrição-do-bug`
  - Exemplo: `bugfix/fix-validation-error`
  - Base: `develop`

- **`hotfix/*`**: Correções urgentes em produção
  - Formato: `hotfix/descrição-do-problema`
  - Exemplo: `hotfix/critical-security-fix`
  - Base: `main`

- **`docs/*`**: Alterações em documentação
  - Formato: `docs/descrição-da-alteração`
  - Exemplo: `docs/update-contributing-guide`
  - Base: `develop`

- **`refactor/*`**: Refatorações
  - Formato: `refactor/descrição-da-refatoração`
  - Exemplo: `refactor/improve-code-structure`
  - Base: `develop`

### Nomenclatura

- Use kebab-case (palavras separadas por hífen)
- Seja descritivo mas conciso
- Use inglês ou português de forma consistente
- Evite abreviações obscuras

### Exemplos de Nomes de Branches

✅ **Bom:**
- `feature/add-pr-template`
- `bugfix/fix-markdown-linting`
- `docs/update-readme`
- `hotfix/security-vulnerability`

❌ **Evite:**
- `my-branch`
- `test`
- `fix`
- `update`

## 🚀 Processo de Desenvolvimento

### 1. Setup Inicial

```bash
# Clone o repositório
git clone https://github.com/danielnovais-tech/digital-transform-kit.git
cd digital-transform-kit

# Configure o remote upstream (se for um fork)
git remote add upstream https://github.com/danielnovais-tech/digital-transform-kit.git
```

### 2. Criar Nova Branch

```bash
# Atualize sua branch develop
git checkout develop
git pull upstream develop

# Crie uma nova feature branch
git checkout -b feature/minha-funcionalidade
```

### 3. Desenvolvimento

```bash
# Faça suas alterações
# Execute testes localmente (se aplicável)

# Adicione os arquivos alterados
git add .

# Commit com mensagem seguindo o padrão
git commit -m "feat: adicionar nova funcionalidade"
```

### 4. Sincronizar com Upstream

```bash
# Antes de abrir o PR, sincronize com develop
git checkout develop
git pull upstream develop

# Rebase sua branch (ou merge, conforme preferência)
git checkout feature/minha-funcionalidade
git rebase develop
```

### 5. Push e PR

```bash
# Push para seu fork
git push origin feature/minha-funcionalidade

# Abra um PR no GitHub
# Preencha o template de PR completamente
```

## 🏷️ Labels e Categorização

Os PRs serão automaticamente categorizados com labels baseados em palavras-chave:

- **`enhancement`**: Novas funcionalidades ou melhorias
- **`bugfix`**: Correções de bugs
- **`documentation`**: Alterações em documentação
- **`refactor`**: Refatorações de código

### Palavras-chave para Labels Automáticos

- **enhancement**: feat, feature, add, implement, enhance
- **bugfix**: fix, bug, resolve, correct
- **documentation**: docs, documentation, readme
- **refactor**: refactor, cleanup, restructure

## 📞 Dúvidas e Suporte

Se você tiver dúvidas sobre como contribuir:

1. Verifique a [documentação existente](README.md)
2. Procure em [issues abertas](https://github.com/danielnovais-tech/digital-transform-kit/issues)
3. Abra uma [nova issue](https://github.com/danielnovais-tech/digital-transform-kit/issues/new) com sua dúvida

## 🙏 Reconhecimento

Todas as contribuições são valorizadas e reconhecidas. Obrigado por ajudar a melhorar o Digital Transform Kit!

---

*Última atualização: Janeiro 2026*
