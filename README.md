
# Documentação do Script PL/SQL - Sistema Acadêmico

Este arquivo contém os pacotes PL/SQL usados para gerenciar informações de alunos, disciplinas e professores em um sistema acadêmico. O sistema permite realizar operações como exclusão de alunos, cadastro de disciplinas, listagem de alunos e turmas, e consultas relacionadas à média de idade e quantidade de turmas.

## Pacotes Criados

### 1. **PKG_ALUNO**
Este pacote contém procedimentos e cursos relacionados aos alunos, incluindo exclusão de aluno, listagem de alunos maiores de 18 anos e listagem de alunos por curso.

#### Procedimentos e Cursors:

- **EXCLUIR_ALUNO**: Exclui um aluno e suas matrículas associadas.
- **LISTAR_ALUNOS_MAIORES_18**: Lista os alunos maiores de 18 anos.
- **LISTAR_ALUNOS_POR_CURSO**: Lista os alunos matriculados em um curso específico.

```sql
CREATE OR REPLACE PACKAGE PKG_ALUNO AS
    PROCEDURE EXCLUIR_ALUNO (p_id_aluno IN NUMBER);
    PROCEDURE LISTAR_ALUNOS_MAIORES_18 (cur_alunos OUT SYS_REFCURSOR);
    PROCEDURE LISTAR_ALUNOS_POR_CURSO (p_id_curso IN NUMBER, cur_curso OUT SYS_REFCURSOR);
END PKG_ALUNO;
```

### 2. **PKG_DISCIPLINA**
Este pacote gerencia as operações relacionadas às disciplinas, como cadastro de disciplinas, cálculo de total de alunos por disciplina, média de idade dos alunos de uma disciplina e listagem de alunos por disciplina.

#### Procedimentos e Cursors:

- **CADASTRAR_DISCIPLINA**: Cadastra uma nova disciplina.
- **TOTAL_ALUNOS_POR_DISCIPLINA**: Lista o total de alunos matriculados por disciplina.
- **MEDIA_IDADE_DISCIPLINA**: Calcula a média de idade dos alunos de uma disciplina.
- **LISTAR_ALUNOS_DISCIPLINA**: Lista os alunos matriculados em uma disciplina.

```sql
CREATE OR REPLACE PACKAGE PKG_DISCIPLINA AS
    PROCEDURE CADASTRAR_DISCIPLINA(p_nome IN VARCHAR2, p_descricao IN CLOB, p_carga_horaria IN NUMBER);
    PROCEDURE TOTAL_ALUNOS_POR_DISCIPLINA(cur_disciplinas OUT SYS_REFCURSOR);
    PROCEDURE MEDIA_IDADE_DISCIPLINA(p_id_disciplina IN NUMBER, cur_media OUT SYS_REFCURSOR);
    PROCEDURE LISTAR_ALUNOS_DISCIPLINA(p_id_disciplina IN NUMBER, cur_alunos OUT SYS_REFCURSOR);
END PKG_DISCIPLINA;
```

### 3. **PKG_PROFESSOR**
Este pacote contém procedimentos e funções para obter informações sobre os professores, como o total de turmas por professor, o número de turmas de um professor específico e o professor de uma disciplina.

#### Procedimentos e Funções:

- **TOTAL_TURMAS_POR_PROFESSOR**: Lista o total de turmas atribuídas a cada professor.
- **TOTAL_TURMAS_PROFESSOR**: Retorna o número de turmas atribuídas a um professor específico.
- **PROFESSOR_DISCIPLINA**: Retorna o nome do professor de uma disciplina.

```sql
CREATE OR REPLACE PACKAGE PKG_PROFESSOR AS
    PROCEDURE TOTAL_TURMAS_POR_PROFESSOR(cur_professores OUT SYS_REFCURSOR);
    FUNCTION TOTAL_TURMAS_PROFESSOR(p_id_professor IN NUMBER) RETURN NUMBER;
    FUNCTION PROFESSOR_DISCIPLINA(p_id_disciplina IN NUMBER) RETURN VARCHAR2;
END PKG_PROFESSOR;
```

## Passos para Execução no Oracle

### 1. Acesse o Banco de Dados Oracle
Use um cliente Oracle como **SQL*Plus**, **Oracle SQL Developer**, ou outro para acessar o banco de dados Oracle.

- **Conecte-se ao banco de dados**:
  ```bash
  sqlplus usuario/senha@hostname:porta/servico
  ```

### 2. Criação do Pacote
Copie o código dos pacotes PL/SQL e cole no cliente Oracle para criá-los no banco de dados. Execute os seguintes passos:

- Para compilar o pacote `PKG_ALUNO`:
  ```sql
  @pkg_aluno.sql
  ```

- Para compilar o pacote `PKG_DISCIPLINA`:
  ```sql
  @pkg_disciplina.sql
  ```

- Para compilar o pacote `PKG_PROFESSOR`:
  ```sql
  @pkg_professor.sql
  ```

### 3. Verificação da Criação dos Pacotes
Após a execução, você pode verificar se os pacotes foram criados corretamente com o comando:

```sql
SELECT object_name, object_type
FROM user_objects
WHERE object_type IN ('PACKAGE', 'PACKAGE BODY');
```

### 4. Utilização dos Procedimentos e Funções
Para utilizar os procedimentos ou funções, você pode chamá-los diretamente em SQL ou PL/SQL. Exemplos de chamadas:

- **Exclusão de aluno**:
  ```sql
  EXEC PKG_ALUNO.EXCLUIR_ALUNO(123);
  ```

- **Listar alunos maiores de 18 anos**:
  ```sql
  DECLARE
      cur SYS_REFCURSOR;
      v_nome aluno.nome%TYPE;
      v_data_nascimento aluno.data_nascimento%TYPE;
  BEGIN
      PKG_ALUNO.LISTAR_ALUNOS_MAIORES_18(cur);
      LOOP
          FETCH cur INTO v_nome, v_data_nascimento;
          EXIT WHEN cur%NOTFOUND;
          DBMS_OUTPUT.PUT_LINE(v_nome || ' - ' || v_data_nascimento);
      END LOOP;
      CLOSE cur;
  END;
  ```

### 5. Verificação de Pacotes e Objetos Criados
Caso queira verificar os pacotes e objetos criados, utilize o comando:

```sql
SELECT object_name, object_type
FROM user_objects
WHERE object_type IN ('PACKAGE', 'PACKAGE BODY');
```

## Observações

### Dependências
- O script foi desenvolvido para ser executado em um banco de dados Oracle.

### Potenciais Melhorias
- **Validação de Dados**: Pode ser necessário adicionar validações extras para garantir a integridade dos dados, especialmente ao cadastrar ou excluir registros.
- **Logs e Auditoria**: Considerar a implementação de logs de execução para garantir a rastreabilidade de exclusões e alterações no banco de dados.
