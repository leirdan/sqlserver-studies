# SQL SERVER
Repositório criado para estudar SQL Server, com comandos e explicações acerca deste SGBD.
## 1. MODELAGEM DO BANCO DE DADOS
### TABELAS
### 1.1 ALUNOS
    Usada para registrar alunos do curso.
- ID_aluno;
- nome_aluno;
- data_nascimento;
- data_cadastro;
- data_atualização;

### 1.2 CURSOS
    Usada para registrar cursos.
- ID_curso;
- nome_curso;
- data_atualização;

### 1.3 TURMAS
    Usada para registrar alunos que estão matriculados em certos cursos.
- ID_turma;
- valor_turma;
- desconto;
- data_inicio;
- data_termino;
- data_cadastro;
- ID_aluno (fk);
- ID_turma (fk);

### 1.4 PRESENÇAS
    Usada para manter registro das presenças dos alunos nas turmas.
- ID_situacao;
- data_presenca;
- data_cadastro;
- ID_aluno (fk);
- ID_turma (fk);

#### 1.4.1 Tabela auxiliar: Situação
- ID_situacao;
- nome_situacao;
