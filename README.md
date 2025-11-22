create database clinica_medica;

create table paciente (
    id_paciente serial primary key,
    nome varchar(100),
    data_nascimento date,
    telefone varchar(20),
    endereco varchar(150)
);

create table especialidade (
    id_especialidade serial primary key,
    nome_especialidade varchar(100)
);

create table medico (
    id_medico serial primary key,
    nome varchar(100),
    crm varchar(20) unique,
    id_especialidade int references especialidade(id_especialidade)
);

create table consulta (
    id_consulta serial primary key,
    id_paciente int references paciente(id_paciente),
    id_medico int references medico(id_medico),
    data_consulta date,
    hora_consulta time,
    diagnostico text
);

create table receita (
    id_receita serial primary key,
    id_consulta int unique references consulta(id_consulta) on delete cascade,
    observacoes text
);

create table medicamento (
    id_medicamento serial primary key,
    nome varchar(100),
    dosagem varchar(50)
);

create table receita_medicamento (
    id_receita int references receita(id_receita) on delete cascade,
    id_medicamento int references medicamento(id_medicamento),
    quantidade int,
    instrucoes text,
    primary key (id_receita, id_medicamento)
);

insert into paciente (nome, data_nascimento, telefone, endereco) values
('maria silva', '1985-04-10', '4399887766', 'rua a, 123'),
('joão pereira', '1990-09-15', '4399776655', 'rua b, 456'),
('ana costa', '2000-01-20', '4399665544', 'rua c, 789');

insert into especialidade (nome_especialidade) values
('cardiologia'), ('dermatologia'), ('pediatria');

insert into medico (nome, crm, id_especialidade) values
('dr. carlos mendes', 'crm123', 1),
('dra. júlia ramos', 'crm456', 2),
('dr. roberto silva', 'crm789', 3);

select*from medico

insert into consulta (id_paciente, id_medico, data_consulta, hora_consulta, diagnostico) values
(1, 1, '2025-01-10', '14:00', 'pressão alta'),
(2, 2, '2025-01-11', '09:00', 'alergia na pele'),
(3, 3, '2025-01-12', '16:00', 'infecção viral');

insert into receita (id_consulta, observacoes) values
(1, 'tomar medicamentos diariamente'),
(2, 'evitar exposição ao sol'),
(3, 'repouso de 3 dias');

insert into medicamento (nome, dosagem) values
('ibuprofeno', '200mg'),
('omeprazol', '20mg'),
('amoxicilina', '500mg');

select*from medicamento

insert into receita_medicamento (id_receita, id_medicamento, quantidade, instrucoes) values
(1, 1, 20, 'tomar 1 comprimido após o almoço'),
(2, 2, 30, 'tomar em jejum'),
(3, 3, 21, 'tomar 1 comprimido a cada 8h');

update paciente
set telefone = '43991112222'
where id_paciente = 1;

delete from receita_medicamento 
where id_medicamento = 2;


delete from medicamento
where id_medicamento = 2;

select nome, telefone from paciente;

select * from medico
where id_especialidade = 1;

select c.id_consulta, p.nome as paciente, m.nome as medico, c.data_consulta
from consulta c
join paciente p on c.id_paciente = p.id_paciente
join medico m on c.id_medico = m.id_medico;

select count(*) as total_pacientes
from paciente;

select * from paciente
where nome like '%a%';

select nome, data_nascimento
from paciente
order by data_nascimento desc;

select distinct id_especialidade
from medico;
