# SQL SERVER
Repositório criado para estudar SQL Server, com comandos e explicações acerca deste SGBD.
## 1. MODELAGEM DO BANCO DE DADOS
* Um banco de dados SQL Server é composto de tabelas, visões (representação visual de tabelas), *procedures* e *functions* (ambas criadas utilizando uma linguagem própria do SQL Server, a **Transact SQL**, mais complexa que a SQL ANSI comum) e *triggers* (comandos a serem executados quando determinada condição for atingida no banco de dados).
* Comumente, cada tabela tem um campo (coluna) que é a **chave primária** daquela tabela. A chave primária (ou *pk*) nada mais é que um valor único que não pode se repetir em outros campos da mesma tabela, e pode ser referenciada na forma de **chave estrangeira** em outra tabela para formar uma relação entre as duas.  
* Neste repositório, será criada uma database de exemplo com algumas tabelas para exemplificar o funcionamento do SQL Server. O banco de dados se refere a uma situação de venda de cursos online com alunos diversos.
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

* Para se criar um banco de dados SQL Server, podemos utilizar do comando **create database** e passar alguns parâmetros para o mesmos. Veja o comando de criação:
    ```sql
    CREATE DATABASE [SQL_TURMAS]
    CONTAINMENT = NONE
    ON  PRIMARY 
    ( NAME = N'SQL_TURMAS', FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL16.SQLANDRIEL\MSSQL\DATA\SQL_TURMAS.mdf' , SIZE = 8192KB , MAXSIZE = UNLIMITED, FILEGROWTH = 65536KB )
    LOG ON 
    ( NAME = N'SQL_TURMAS_log', FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL16.SQLANDRIEL\MSSQL\DATA\SQL_TURMAS_log.ldf' , SIZE = 8192KB , MAXSIZE = 2048GB , FILEGROWTH = 65536KB )
    WITH CATALOG_COLLATION = DATABASE_DEFAULT, LEDGER = OFF
    GO
    ```

### 2.1 PRINCIPAIS COMANDOS

#### Data Definition Language (DDL)
* Comandos que criam os componentes dos bancos de dados e fazem a manutenção.
- **SELECT [Colunas] FROM [Tabela]**: permite selecionar e consultar dados em uma tabela. O comando *select* admite algumas cláusulas, como:
    * **WHERE**: permite filtrar dados a partir de uma condição. Pode-se utilizar ainda o operador **and** para concatenar condições de filtragem;
    * **ORDER BY**: permite ordenar os dados a partir de uma coluna e de uma ordem específica, como ordenar uma lista em ordem alfabética, por data de cadastro e mais;
    * **GROUP BY**: indica a ordem na qual as linhas serão retornadas, e também é utilizada para agrupar colunas que foram selecionadas e não têm funções de agregação sendo executadas nelas (por ex.: count, sum, etc.);
        * **ASC** e **DESC**: cláusulas comumente associadas à esta outra, indica se a coluna será ordenada de forma crescente ou decrescente;
    * **IN**: define possibilidades os quais os valores de um campo pode assumir (por ex.: se sua busca quer um ID 1 ou 3, ele vai selecionar apenas os registros com esses ID's);
    * **NOT IN**: faz o exato oposto do *in* (por ex.: se sua busca não quer um id 1 ou 3, ele vai selecionar todos os outros registros que contenham ID's diferentes);
    * **DISTINCT**: informa à consulta que devem ser trazidos apenas valores sem repetição;
- **TRUNCATE [Nome da Tabela]**: apaga os dados de uma tabela mas não sua modelagem;
- **CREATE TABLE [Nome da Tabela]**: cria uma nova tabela e sua estrutura;
- **DROP TABLE [Nome da Tabela]**: apaga uma tabela existente;
- **ALTER TABLE [Nome da Tabela]**: modifica a estrutura de uma tabela que desejar;

#### Data Manipulation Language (DML)
* Comandos que gerenciam os dados das tabelas.
- **INSERT INTO [Nome da Tabela] (campos da tabela) VALUES (valores)**: insere dados em uma tabela e suas colunas;
- **UPDATE [Nome da Tabela] SET [Campo = 'novo valor'] WHERE [Condição]**: atualiza um registro em uma tabela a partir da cláusula *where* para identificar o registro.
- **DELETE FROM [Nome da Tabela] WHERE [Condição]**: exclui um ou mais registros com base na cláusula *where* utilizada para identificar o(s) registro(s).
- **ADD CONSTRAINT [Nome da Restrição] [Tipo da Restrição]**: cria uma constraint em uma tabela para, por exemplo, definir suas chaves estrangeiras;
- **DROP CONSTRAINT [Nome da Restrição]**: exclui uma restrição previamente criada;

#### Data Control Language (DCL)
* Comandos que gerenciam não a estrutura do banco mas sim informações relacionadas à sua segurança e forma de armazenar dados.
- **SAVEPOINT**: salva o estado do banco de dados de forma temporária;
- **ROLLBACK**: retorna ao estado do banco salvo anteriormente; 
- **COMMIT**: salva o estado do banco de dados de forma definitiva;

### 2.1.1 Tipos de dados numéricos em SQL Server
#### a) Numéricos Exatos
* Os numéricos exatos são números inteiros, ou seja, que não possuem casas decimais. No SQL Server, temos 4 tipos de números exatos inteiros, diferenciados pelo limite do valor mínimo ou máximo que pode ser armazenado dentro da tabela. O limite mínimo e máximo está relacionado ao número de bytes que cada um ocupa dentro do campo.

    * *bigint*: varia de -2^63 a 2^63-1 e ocupa 8 bytes;
    * *int*: varia de -2^31 a 2^31-1 e ocupa 4 bytes;
    * *smallint*: varia de -2^15 a 2^15-1 e ocupa 2 bytes;
    * *tinyint*: varia de 0 a 255 e ocupa 1 byte.

#### b) Numéricos com casas decimais
* Estes numéricos são números com casas decimais previamente definidas. Ao criar um campo desse tipo, temos que definir a *precisão* (P, o número de casas digitais que o número terá, incluindo casas decimais) e a *escala* (S, representa somente a quantidade de números decimais); assim, nota-se que o valor da escala não pode ser nunca maior que o da precisão. São estes os tipos:
    * *numeric* e *decimal*: funcionam da mesma forma, então não há diferença ao criá-los.

#### c) Numéricos Exatos (unidades monetárias)
* Estes são valores que representam o dinheiro, e a diferença entre os tipos é basicamente o seu alcance de valores. São eles:
    * *smallmoney*: -214.748.3648 a 214.748.3647 e ocupa 4 bytes;
    * *money*: -922.337.203.685.477,5808 a 922.337.203.685.477,5807 e ocupa 8 bytes;
#### d) Numéricos lógicos
* Ao contrário das linguagens de programação, no SQL Server não há um tipo *booleano*, mas sim um tipo chamado *BIT*.
    * *BIT*: recebe um valor 0, que representa *false*, ou 1, que representa *true*. Como é um bit, ele ocupa o espaço de 1 bit.
#### e) Numéricos aproximados
* Valores numéricos onde não precisamos definir a quantidade de casas decimais, pois este é um processo executado internamente pelo banco de dados. Temos:
    * *real*: -3,40E+38 a -1.18E-38. 0 e 1.18E-38 a 3,40E+38, e ocupa 4 bytes;
    * *float*: - 1,79E+308 a -2,23E-308,0 e 2,23E-308 a 1.79E+308, e ocupa um valor variável de memória.
        * *float* é o tipo com maior intervalo de números, mas, mesmo assim, ele ainda tem um limite máximo de valores.

### 2.1.2 Tipos de datas em SQL Server
#### a) Date
* O tipo *date* representa uma data simples no formato *YYYY-MM-DD*, podendo ir do dia 01 do mês 01 do ano 0001 até 31 do mês 12 do ano 9999.
#### b) Datetime
* Além de armazenar a data, armazena também a hora corrente no formato *YYYY-MM-DD HH:MM:SS.MMM*. Um detalhe é que o intervalo de anos só vai de 1753 a 9999.
#### c) Datetime2
* Bastante similar ao *Datetime* anterior, a principal diferença está no intervalo dos anos, que vai do ano 01 até o ano 9999. De resto, é exatamente igual ao Datetime.
#### d) Smalldate
* Tipo de *Datetime* que não representa segundos e microssegundos, somente horas e minutos, além da data. O intervalo em anos é de 1900 a 2079, e é escrito no formato *YYYY-MM-DD HH:MM:SS*, onde o campo dos segundos (SS) será sempre 00.
#### e) Time
* Tipo que representa somente horas, portanto, permite uma precisão maior de microssegundos, e é escrito no formato *HH:MM:SS.MMMMM*.

### 2.1.3 Tipos de textos em SQL Server
#### a) Cadeia de caracteres - representa textos: Char e Varchar
* O tipo *char* é um tipo textual que possui um tamanho fixo definido, o que significa que, em um campo de 10 letras, caso apenas 5 sejam preenchidas, o banco de dados irá guardar o registro como essas 5 letras inseridas e mais 5 espaços em branco para ocupar o número de caracteres definidos. Pode ocupar até 8000 bytes.
* O tipo *varchar* é similar ao *char*. No entanto, seu tamanho é variável, pois na mesma situação acima e com um campo do tipo *varchar*, o banco de dados não preencheria o restante do registro com espaços em brancos, mas armazenaria mesmo com 5 letras. Este tipo também ocupa até 8000 bytes, entretanto, é possível defini-lo como *MAX*, o que significa que ele pode armazenar até 2 gigabytes de texto!

#### b) Cadeia de caracteres Unicode - representa os textos usando a tabela UTF-16: NChar e NVarchar
* Os tipos *nchar* e *nvarchar* são usados para representar caracteres Unicode, que utiliza um grupo de símbolos maior que os presentes nos tipos *char* e *varchar*, como letras de outros alfabetos. Desse modo, *nchar* e *nvarchar* são tipos bem parecidos com os seus equivalentes, incluindo o valor *MAX* em *nvarchar*.

#### c) Cadeia de caracteres binários: Binary e Varbinary
* Estes dois tipos funcionam da mesma forma que *char* e *varchar*, porém, são utilizados para representar bytes de arquivos escritos em binário. Ambos variam de 0 a 8000 bytes e *varbinary* tem a mesma possibilidade que *varchar* de utilizar o valor *MAX*.

### 2.1.4 Outros tipos
* **XML**: este tipo guarda o conteúdo de arquivos XML;
* **Geometria Espacial**: representa um sistema de coordenadas euclidianas e relaciona-se com símbolos geométricos;
* **Geográficos espaciais**: representam pontos, linhas e áreas associadas a mapas, podendo, por exemplo, gravar rotas em mapas.

 ### 2.2.1 Tabela "Alunos"

```sql
create table Alunos (
    ID_Aluno int primary key not null,
    nome_aluno varchar(200) not null,
    data_nascimento date not null,
    data_cadastro datetime not null,
    data_atualizacao datetime
);
```
### 2.2.2 Tabela "Cursos"
```sql
create table Cursos (
    ID_Curso int primary key not null,
    nome_curso varchar(200) not null,
    data_cadastro date not null,
    data_atualizacao datetime
);
```

### 2.2.3 Tabela "Turmas"
```sql
create table Turmas (
    ID_Turma int primary key not null,
    data_inicio date not null,
    data_termino date,
    data_cadastro date not null,
    ID_Curso int not null,
);
```
#### 2.2.3.1 Tabela auxiliar do relacionamento many-to-many entre turmas e alunos: Alunos_Turma
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

### 2.2.4 Tabela "Presenças"
```sql
create table Presencas (
	data_presenca date not null,
	data_cadastro date not null,
	ID_Aluno int not null,
	ID_Turma int not null,
	ID_Situacao int not null
);
```
### 2.2.5 Tabela "Situação"
```sql
create table Situacao (
    ID_Situacao int primary key not null,
    nome_situacao varchar(250) not null
);
```

### 2.3. CRIANDO RELACIONAMENTOS COM CONSTRAINTS
* *Constraints* nada mais são que "restrições" as quais sua tabela deve obedecer.

### 2.3.1 Relacionamentos de "Turmas"
```sql
alter table Turmas
    add constraint fk_Cursos FOREIGN KEY (ID_Curso) references Cursos (ID_Curso);
```

### 2.3.2 Relacionamentos de "Presencas"
```sql
alter table Presencas 
	add constraint fk_TurmasPresenca FOREIGN KEY (ID_Turma) references Turmas(ID_Turma);

alter table Presencas 
	add constraint fk_AlunosPresenca FOREIGN KEY (ID_Aluno) references Alunos(ID_Aluno);

alter table Presencas 
	add constraint fk_SituacaoPresenca FOREIGN KEY (ID_Situacao) references Situacao (ID_Situacao);
```
### 2.3.3 Relacionamentos de "AlunosTurmas"
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

### 3.1.4 Selecionando os nomes e data de nascimento de alunos que não nasceram em 1993
```sql
select * 
    from Alunos 
    where DATEPART(YEAR, data_nascimento) 
    not in (1993)
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
### 3.2.4 Consultando os diferentes preços para as turmas
```sql 
select 
	distinct preco	
	from AlunosTurmas 
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
-- group by utilizado pois, quando há uma função de agregação, como count, 
-- é necessário que as demais colunas que não estão contidas nessa função sejam declaradas com esse comando; 
-- se não, não roda.
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
select a.ID_Aluno, a.nome_aluno, c.nome_curso, t.data_inicio, t.data_termino, at.preco, at.desconto
	from AlunosTurmas at
	inner join Turmas t
		on at.ID_Turma = t.ID_Turma
	inner join Cursos c
		on t.ID_Curso = c.ID_Curso
	inner join Alunos a
		on a.ID_Aluno = at.ID_Aluno
```

### 4.2.5 Consultando quantidade de turmas com alunos
```sql
select c.nome_curso, t.ID_Turma, count (at.ID_Turma) as 'Total de turmas', count (a.ID_Aluno) as 'Total de alunos nessa turma' 
	from AlunosTurmas at
	inner join Turmas t
		on at.ID_Turma = t.ID_Turma
	inner join Alunos a
		on a.ID_Aluno = at.ID_Aluno
	inner join Cursos c
		on c.ID_Curso = t.ID_Curso
	group by c.nome_curso, t.ID_Turma
```
## 5. OPERAÇÕES
### 5.1 FUNÇÕES PRINCIPAIS MATEMÁTICAS
* **square(*n*)**: eleva um número ao quadrado;
* **power(*n*, *m*)**: eleva um número n à potência m;
* **abs(*n*)**: retorna o módulo (valor absoluto) de um número;
* **sqrt(*n*)**: tira a raiz quadrada de um número n;
* **pi()**: retorna o número *pi*;
* **getdate()**: retorna a data atual com horário;
* **sign(*n*)**: retorna -1 para um número real negativo e 1 para um positivo;
### 5.2 FUNÇÕES AGREGADAS 
* **sum(*c*)**: soma os valores de uma coluna no SQL;
* **count(*c.c*)**: conta o número de registros de uma coluna;
* **avg(c)**: calcula a média de uma coluna;
* **max(c)**: retorna o valor máximo de uma coluna;
* **min(c)**: retorna o valor mínimo de uma coluna.

### 5.3 FUNÇÕES DE DATA
* **getdate()**: retorna a data corrente no formato norte-americano;
* **convert()**: converte um valor de um tipo para outro. Pode ser utilizado em datas da seguinte forma:
    ```sql
    select convert(char, getdate(), 113)
    -- 19 Mai 2023 11:31:44.100
    ```
    * Os formatos para data (113, 103, 1, etc.) estão disponíveis na tabela *ANSI SQL*.
* **day(*v*)**: retorna o dia de um valor *v* em formato de data:
    ```sql
    select day(getdate())
    -- 19
    ```
* **month(*v*)**: retorna o mês de um valor *v* em formato de data: 
    ```sql
    select month(getdate())
    -- 05
    ```
* **year(*v*)**: retorna o ano de um valor *v* em formato de data: 
    ```sql
    select year(getdate())
    -- 2023
    ```
* **dateadd(*t*, *v*, *d*)**: permite alterar e manipular um valor de data, onde 't' é o tipo do valor a ser adicionado, 'v' é o valor em si e 'd' é onde será adicionado:
    ```sql
    select dateadd(year, -3, getdate())
    -- 2020-05-19 11:47:37.387 
    -- saímos de 2023 para 2020

    select convert(smalldatetime, dateadd(hour, 4, getdate()))
    -- 2023-05-19 15:55:00
    -- adicionamos 4h na hora corrente (11:55:00) e convertemos para smalldatetime
    ```

* **datediff(*t*, *v*, *d*)**: permite calcular a diferença entre duas datas:  
    ```sql
    select datediff(month, getdate(), dateadd(year, 4, getdate()))
    -- 48
    -- usamos o dateadd para adicionar 4 anos na data atual, e calcular essa data do dateadd com a data atual em meses, gerando 48 meses (que é igual a 4 anos).
    ```

