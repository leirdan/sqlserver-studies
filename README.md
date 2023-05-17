# SQL SERVER
Repositório criado para estudar SQL Server, com comandos e explicações acerca deste SGBD.
## 1. MODELAGEM DO BANCO DE DADOS
### TABELAS
### 1.1 ALUNOS
    Usada para registrar alunos do curso.
- ID_aluno (**pk**);
- nome_aluno;
- data_nascimento;
- data_cadastro;
- data_atualização;

### 1.2 CURSOS
    Usada para registrar cursos.
- ID_curso (**pk**);
- nome_curso;
- data_cadastro;
- data_atualização;

### 1.3 TURMAS
    Usada para registrar alunos que estão matriculados em certos cursos.
- ID_turma (**pk**);
- valor_turma;
- desconto;
- data_inicio;
- data_termino;
- data_cadastro;
- ID_aluno (*fk*);
- ID_turma (*fk*);

### 1.4 PRESENÇAS
    Usada para manter registro das presenças dos alunos nas turmas.
- data_presenca;
- data_cadastro;
- ID_aluno (*fk*);
- ID_situacao (*fk*);
- ID_turma (*fk*);

### 1.5 SITUAÇÃO
    Usado para definir a situação do aluno no curso.
- ID_situacao;
- nome_situacao;
## 2. CRIAÇÃO DAS TABELAS COM SCRIPTS
*faltando os relacionamentos.*

### 2.1 PRINCIPAIS COMANDOS
- **CREATE TABLE [Nome da Tabela]**: cria uma nova tabela e sua estrutura;
- **DROP TABLE [Nome da Tabela]**: apaga uma tabela existente;

### 2.1.1 Tabela "Alunos"
    create table Alunos (
        ID_Aluno int primary key not null,
        nome_aluno varchar(200) not null,
        data_nascimento date not null,
        data_cadastro datetime not null,
        data_atualizacao datetime2(7)
    );

### 2.1.2 Tabela "Cursos"
    create table Cursos (
        ID_Curso int primary key not null,
        nome_curso varchar(200) not null,
        data_cadastro date not null,
        data_atualizacao datetime2(7)
    );

### 2.1.3 Tabela "Turmas"
    create table Turmas (
        ID_Turma int primary key not null,
        valor_turma numeric(10,2) not null,
        desconto numeric(3, 2),
        data_inicio date not null,
        data_termino date,
        data_cadastro date not null,
        ID_Aluno int not null,
        ID_Curso int not null,
    );

### 2.1.4 Tabela "Presenças"
    create table Presencas (
	    data_presenca date not null,
	    data_cadastro date not null,
	    ID_Aluno int not null,
	    ID_Turma int not null,
	    ID_Situacao int not null
    );

### 2.1.5 Tabela "Situação"
    create table Situacao (
	    ID_Situacao int primary key not null,
	    nome_situacao varchar(250) not null
    );