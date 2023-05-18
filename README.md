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
- **SELECT [Colunas] FROM [Tabela]**: permite selecionar e consultar dados em uma tabela. O comando *select* admite algumas opções, como:
    * **WHERE**: permite filtrar dados a partir de uma condição. Pode-se utilizar ainda o operador **and** para concatenar condições de filtragem;
    * **ORDER BY**: permite ordenar os dados a partir de uma coluna e de uma ordem específica, como ordenar uma lista em ordem alfabética, por data de cadastro e mais;
    * **GROUP BY**: indica a ordem na qual as linhas serão retornadas, e também é utilizada para agrupar colunas que foram selecionadas e não têm funções de agregação sendo executadas nelas (por ex.: count, sum, etc.);
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

-- 1ª turma do curso 'Desenvolvendo um jogo com Javascript e HTML5'
insert into dbo.Turmas (ID_Turma, data_inicio, data_termino, data_cadastro, ID_Curso) 
values (3, '05/07/2023', '20/10/2023', GETDATE(), 3);

-- 2ª turma do curso 'Desenvolvendo um jogo com Javascript e HTML5'
insert into dbo.Turmas (ID_Turma, data_inicio, data_termino, data_cadastro, ID_Curso) 
values (4, '05/09/2023', '20/12/2023', GETDATE(), 3);

-- 3ª turma do curso 'Desenvolvendo um jogo com Javascript e HTML5'
insert into dbo.Turmas (ID_Turma, data_inicio, data_termino, data_cadastro, ID_Curso) 
values (5, '20/01/2024', '30/04/2024', GETDATE(), 3);

-- 1ª turma do curso 'Webcomponents com Javascript puro'
insert into dbo.Turmas (ID_Turma, data_inicio, data_termino, data_cadastro, ID_Curso) 
values (6, '01/06/2023', '30/06/2023', GETDATE(), 2);

-- 2ª turma do curso 'Webcomponents com Javascript puro'
insert into dbo.Turmas (ID_Turma, data_inicio, data_termino, data_cadastro, ID_Curso) 
values (7, '01/07/2023', '30/07/2023', GETDATE(), 2);

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

-- matriculando 'Zayn Malik' na turma de 'Desenvolvendo um jogo com Javascript e HTML5'
insert into dbo.AlunosTurmas (ID_Turma, ID_Aluno, preco, desconto, data_cadastro, data_atualizacao) 
values (3, 3, 700, 0.2, getdate(), getdate());

-- matriculando 'Ana Caetano' na turma de 'PEOO com Typescript'
insert into dbo.AlunosTurmas (ID_Turma, ID_Aluno, preco, desconto, data_cadastro, data_atualizacao) 
values (2, 5, 600, 0.1, getdate(), getdate());

```

## 3. CONSULTANDO DADOS NAS TABELAS
### 3.1 EXEMPLOS NA TABELA "Alunos"
### 3.1.1 Selecionando todos os dados
```sql
select Alunos.* from Alunos
```
ou
```sql
select * from Alunos
```
### 3.1.2 Selecionando somente nome, id e data de nascimento
```sql
-- renomeando as colunas
select ID_Aluno as ID, nome_aluno as nome, data_nascimento as nascimento from Alunos
```

### 3.1.3 Selecionando nome e id de quem nasceu antes de 08/10/1993
```sql
select a.nome_aluno as nome, a.ID_Aluno as id 
	from Alunos a 
	where data_nascimento < '08/10/1993'
	order by nome
-- retorna, em ordem, George Harrison, Taylor Swift e Zayn Malik
```
### 3.2 EXEMPLOS NA TABELA "Turmas"
### 3.2.1 Consultando todas as turmas
```sql
select Turmas.* from Turmas
```
### 3.2.2 Consultando o id da turma, do curso e data de início
```sql
-- utilizando apelido para a tabela (alias)
select T.ID_Turma, T.data_inicio, T.ID_Curso from Turmas T
```
### 3.2.3 Consultando apenas turmas que cobram mais que 500 reais
```sql
select at.* 
	from AlunosTurmas at
	where at.preco > 500
    order by preco
```

## 4. COLETANDO DADOS RELACIONADOS COM OS JOINS
    Consultar um diagrama de Venn pode ser uma boa opção.
### 4.1 CONCEITOS
Sejam A e B dois conjuntos distintos. Teremos que:
### Inner Join
* *Inner joins* retornam os **elementos de A com intersecção em B**, ou seja, elementos dos dois conjuntos que são comuns;
* Em conjuntos, seria o equivalente à "*A intersecção B*".
```sql
select *
    from TableA a
    inner join TableB b
    on a.pk = b.pk
```
* *pk* = primary key

### Left Outer Join
* *Left outer joins* retornam, normalmente, os **elementos de A e os elementos de A com intersecção em B**;


* Existem dois tipos de resultados para a consulta:
1. Consulta inclusiva:
* Em conjuntos, pode ser denotado por "*(A diferença B) união (A intersecção B)*"
```sql
select *
    from TableA a
    left join TableB b
        on a.pk = b.pk
```
2. Consulta exclusiva
* Em conjuntos, pode ser denotado por "*(A diferença B)*"
```sql
select *
    from TableA a
    left join TableB b
        on a.pk = b.pk
    where b.pk is null
```
### Right Outer Join

* *Right outer joins* retornam, normalmente, os **elementos de B e os elementos de B com intersecção em A**;


* Existem dois tipos de resultados para a consulta:
1. Consulta inclusiva:
* Em conjuntos, pode ser denotado por "*(B diferença A) união (B intersecção A)*"
```sql
select *
    from TableA a
    right join TableB b
        on a.pk = b.pk
```
2. Consulta exclusiva
* Em conjuntos, pode ser denotado por "*(B diferença A)*"
```sql
select *
    from TableA a
    right join TableB b
        on a.pk = b.pk
    where a.pk is null
```
### Full Outer Join
* *Full outer joins* retornam **A união B**, ou seja, todos os elementos das duas tabelas, em comum ou não.

* Existem dois tipos de resultados para a consulta:
1. Consulta inclusiva:
* Em conjuntos, pode ser denotado por "*A união B*".
```sql
select *
    from TableA a
    full outer join TableB b
        on a.pk = b.pk
```
2. Consulta exclusiva
* Em conjuntos, pode ser denotado por "(A união B) diferença (A intersecção B)".
```sql
select *
    from TableA a
    full outer join TableB b
        on a.pk = b.pk
    where a.pk is null or b.pk is null
```

### 4.2 EXEMPLOS
### 4.2.1 Verificando quantas turmas há para cada curso
```sql
select c.ID_Curso, c.nome_curso, count(t.ID_Turma) as total_turmas
	from Turmas t
	inner join Cursos c
		on t.ID_Curso = c.ID_Curso
	group by c.nome_curso, c.ID_curso
-- group by utilizado pois, quando há uma função de agregação, como count, é necessário que as demais colunas que não estão contidas nessa função sejam declaradas com esse comando; senão, não roda.
```
### 4.2.2 Verificando todos os cursos, independente se há ou não turmas
```sql
select c.nome_curso, count (c.ID_Curso) as total_turmas
	from Turmas t
	left join Cursos c
		on c.ID_Curso = t.ID_Curso
	group by c.nome_curso
```

### 4.2.3 Verificando a quantidade de turmas em que cada aluno está matriculado
```sql
select a.nome_aluno, count (at.ID_Turma) as turmas_matriculadas
	from AlunosTurmas at
	right join Alunos a 
		on a.ID_Aluno = at.ID_Aluno
	 group by nome_aluno

```
### 4.2.4 Verificando em quais turmas e cursos cada aluno está matriculado
```sql
select a.nome_aluno, c.nome_curso, at.preco, at.desconto, t.data_inicio, t.data_termino
	from AlunosTurmas at
	inner join Turmas t on t.ID_Turma = at.ID_Turma
	inner join Cursos c on c.ID_Curso = t.ID_Curso
	inner join Alunos a on a.ID_Aluno = at.ID_Aluno
```