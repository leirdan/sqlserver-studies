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

### 2.1 PRINCIPAIS COMANDOS
- **CREATE TABLE [Nome da Tabela]**: cria uma nova tabela e sua estrutura;
- **DROP TABLE [Nome da Tabela]**: apaga uma tabela existente;
- **ALTER TABLE [Nome da Tabela]**: modifica a estrutura de uma tabela que desejar;
- **ADD CONSTRAINT [Nome da Restrição] [Tipo da Restrição]**: cria uma constraint em uma tabela para, por exemplo, definir suas chaves estrangeiras;
- **DROP CONSTRAINT [Nome da Restrição]**: exclui uma restrição previamente criada;
- **INSERT INTO [Nome da Tabela] (campos da tabela) VALUES (valores)**: insere dados em uma tabela e suas colunas;

### 2.1.1 Tabela "Alunos"

```sql
create table Alunos (
    ID_Aluno int primary key not null,
    nome_aluno varchar(200) not null,
    data_nascimento date not null,
    data_cadastro datetime not null,
    data_atualizacao datetime
);
```
### 2.1.2 Tabela "Cursos"
```sql
create table Cursos (
    ID_Curso int primary key not null,
    nome_curso varchar(200) not null,
    data_cadastro date not null,
    data_atualizacao datetime
);
```

### 2.1.3 Tabela "Turmas"
```sql
create table Turmas (
    ID_Turma int primary key not null,
    data_inicio date not null,
    data_termino date,
    data_cadastro date not null,
    ID_Curso int not null,
);
```
#### 2.1.3.1 Tabela auxiliar do relacionamento many-to-many entre turmas e alunos: Alunos_Turma
* A necessidade disto se dá pois haveria uma infinidade de ID_Turma replicados na tabela de Turmas para cada aluno matriculado nela.
```sql
create table AlunosTurmas (
	ID_Turma int not null,
	ID_Aluno int not null,
	preco numeric(10,2) not null,
	desconto numeric (3,2),
	data_cadastro datetime not null,
	data_atualizacao datetime
);
```

### 2.1.4 Tabela "Presenças"
```sql
create table Presencas (
	data_presenca date not null,
	data_cadastro date not null,
	ID_Aluno int not null,
	ID_Turma int not null,
	ID_Situacao int not null
);
```
### 2.1.5 Tabela "Situação"
```sql
create table Situacao (
    ID_Situacao int primary key not null,
    nome_situacao varchar(250) not null
);
```

### 2.2. CRIANDO RELACIONAMENTOS COM CONSTRAINTS
* *Constraints* nada mais são que "restrições" as quais sua tabela deve obedecer.

### 2.2.1 Relacionamentos de "Turmas"
```sql
alter table Turmas
    add constraint fk_Cursos FOREIGN KEY (ID_Curso) references Cursos (ID_Curso);
```

### 2.2.2 Relacionamentos de "Presencas"
```sql
alter table Presencas 
	add constraint fk_TurmasPresenca FOREIGN KEY (ID_Turma) references Turmas(ID_Turma);

alter table Presencas 
	add constraint fk_AlunosPresenca FOREIGN KEY (ID_Aluno) references Alunos(ID_Aluno);

alter table Presencas 
	add constraint fk_SituacaoPresenca FOREIGN KEY (ID_Situacao) references Situacao (ID_Situacao);
```
### 2.2.3 Relacionamentos de "AlunosTurmas"
```sql
alter table AlunosTurmas 
    add constraint fk_Turmas foreign key (ID_Turma) references Turmas (ID_Turma);
alter table AlunosTurmas 
    add constraint fk_Turmas foreign key (ID_Turma) references Turmas (ID_Turma);
```

### 2.3 INSERINDO DADOS A PARTIR DO INSERT
### 2.3.1 Criando alunos
```sql
insert into dbo.Alunos (ID_Aluno, nome_aluno, data_nascimento, data_cadastro, data_atualizacao) 
values (1, 'Taylor Swift', '1989/12/13', '18/05/2023 07:24:00', '18/05/2023 07:24:00');

insert into dbo.Alunos (ID_Aluno, nome_aluno, data_nascimento, data_cadastro, data_atualizacao)
values (2, 'George Harrison', '25/02/1943', '18/05/2023 07:24:00', '18/05/2023 07:24:00')

insert into dbo.Alunos (ID_Aluno, nome_aluno, data_nascimento, data_cadastro, data_atualizacao)
values (3, 'Zayn Malik', '12/01/1993', '18/05/2023 07:24:00', '18/05/2023 07:24:00')

insert into dbo.Alunos (ID_Aluno, nome_aluno, data_nascimento, data_cadastro, data_atualizacao)
values (4, 'Gustavo Bertoni', '08/10/1993', '18/05/2023 07:24:00', '18/05/2023 07:24:00')

insert into dbo.Alunos (ID_Aluno, nome_aluno, data_nascimento, data_cadastro, data_atualizacao)
values (5, 'Ana Caetano', '05/10/1994', '18/05/2023 07:24:00', '18/05/2023 07:24:00')
```

### 2.3.2 Criando cursos
```sql
insert into dbo.Cursos (ID_Curso, nome_curso, data_cadastro, data_atualizacao)
values (1, 'Programação Estruturada e Orientada a Objetos em Typescript', '18/05/2023 07:32:00', '18/05/2023 07:32:00')

insert into dbo.Cursos (ID_Curso, nome_curso, data_cadastro, data_atualizacao)
values (2, 'Webcomponents com Javascript puro', '18/05/2023 07:32:00', '18/05/2023 07:32:00')

insert into dbo.Cursos (ID_Curso, nome_curso, data_cadastro, data_atualizacao)
values (3, 'Desenvolvendo um jogo com Javascript e HTML5', '18/05/2023 07:32:00', '18/05/2023 07:32:00')

insert into dbo.Cursos (ID_Curso, nome_curso, data_cadastro, data_atualizacao)
values (4, 'Fundamentos Matemáticos da Computação I', '18/05/2023 07:32:00', '18/05/2023 07:32:00')
```

### 2.3.3 Criando turmas
```sql
-- turma do curso 'Fundamentos Matemáticos da Computação'
insert into dbo.Turmas (ID_Turma, data_inicio, data_termino, data_cadastro, ID_Curso) 
values (1, '22/05/2023', '22/06/2023', GETDATE(), 4);

-- turma do curso 'PEOO com Typescript'
insert into dbo.Turmas (ID_Turma, data_inicio, data_termino, data_cadastro, ID_Curso) 
values (2, '18/04/2023', '29/05/2023', GETDATE(), 1); 

-- turma do curso 'Desenvolvendo um jogo com Javascript e HTML5'
insert into dbo.Turmas (ID_Turma, data_inicio, data_termino, data_cadastro, ID_Curso) 
values (3, '05/07/2023', '20/10/2023', GETDATE(), 3);
```
### 2.3.4 Adicionando alunos em turmas
```sql
-- matriculando 'Taylor Swift' e 'Zayn Malik' em 'FMC I'
    -- taylor, id 1
insert into dbo.AlunosTurmas (ID_Turma, ID_Aluno, preco, desconto, data_cadastro, data_atualizacao) 
values (1, 1, 300, 0.1, getdate(), getdate());
    -- zayn, id 3
insert into dbo.AlunosTurmas (ID_Turma, ID_Aluno, preco, desconto, data_cadastro, data_atualizacao) 
values (1, 3, 300, 0.1, getdate(), getdate());
```
