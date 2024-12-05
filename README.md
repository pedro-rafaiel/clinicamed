# 📋 Clínica Médica - Banco de Dados

Este repositório contém o script SQL completo para a criação e gerenciamento de um sistema de banco de dados para uma clínica médica. O objetivo é gerenciar pacientes, médicos, clínicas, consultas, horários disponíveis e exames de forma eficiente.

---

## 🛠️ Estrutura do Banco de Dados

O banco de dados é composto pelas seguintes tabelas:

1. **Pacientes**: Informações dos pacientes cadastrados.
2. **Médicos**: Dados sobre os médicos e suas especialidades.
3. **Clínicas**: Endereços e nomes das clínicas participantes.
4. **Consultas**: Histórico e status de consultas realizadas, agendadas ou canceladas.
5. **HoráriosDisponiveis**: Horários que os médicos têm disponíveis para agendamento.
6. **Exames**: Exames realizados pelos pacientes.

### **Relacionamentos**
- Um paciente pode realizar várias consultas e exames.
- Médicos possuem horários disponíveis para agendamento.
- Consultas estão vinculadas a pacientes, médicos e clínicas.

---

## 🚀 Tecnologias Utilizadas

- **MySQL**: Banco de dados relacional.
- **SQL**: Linguagem para criação, manipulação e consulta de dados.

---

## 📂 Estrutura das Tabelas

### **Pacientes**
- `id`: Identificador único do paciente.
- `nome`: Nome do paciente.
- `telefone`: Telefone do paciente.
- `endereco`: Endereço do paciente.
- `data_nascimento`: Data de nascimento do paciente.

### **Médicos**
- `id`: Identificador único do médico.
- `nome`: Nome do médico.
- `especialidade`: Especialidade médica (ex.: Cardiologia, Pediatria).

### **Consultas**
- `id`: Identificador único da consulta.
- `id_paciente`: Referência ao paciente (chave estrangeira).
- `id_medico`: Referência ao médico (chave estrangeira).
- `id_clinica`: Referência à clínica (chave estrangeira).
- `data_consulta`: Data e hora da consulta.
- `status`: Status da consulta (`Agendada`, `Realizada`, `Cancelada`).
- `motivo_cancelamento`: Motivo do cancelamento (caso aplicável).

### **Outras Tabelas**
- **Clinicas**: Armazena as clínicas disponíveis.
- **HorariosDisponiveis**: Lista de horários para agendamento com os médicos.
- **Exames**: Registros dos exames realizados pelos pacientes.

---

## 🔍 Consultas Principais

### 1. **Consultas agendadas para o dia atual**
```sql
SELECT 
    Pacientes.nome AS paciente_nome, 
    Medicos.nome AS medico_nome, 
    Clinicas.nome AS clinica_nome, 
    Consultas.data_consulta
FROM Consultas
JOIN Pacientes ON Consultas.id_paciente = Pacientes.id
JOIN Medicos ON Consultas.id_medico = Medicos.id
JOIN Clinicas ON Consultas.id_clinica = Clinicas.id
WHERE DATE(Consultas.data_consulta) = CURDATE()
  AND Consultas.status = 'Agendada';
```

### 2. **Total de consultas realizadas por clínica**
```sql
SELECT 
    Clinicas.nome AS clinica_nome, 
    COUNT(Consultas.id) AS total_consultas,
    SUM(CASE WHEN Consultas.status = 'Realizada' THEN 1 ELSE 0 END) AS total_realizadas
FROM Consultas
JOIN Clinicas ON Consultas.id_clinica = Clinicas.id
GROUP BY Clinicas.id;
```

### 3. **Pacientes com mais de dois médicos diferentes no último ano**
```sql
SELECT id_paciente, COUNT(DISTINCT id_medico) AS medicos_diferentes
FROM Consultas
WHERE data_consulta >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
GROUP BY id_paciente
HAVING medicos_diferentes > 2;
```

---

## 📦 Como Utilizar

1. **Clone o Repositório**
   ```bash
   git clone https://github.com/seu-usuario/nome-repositorio.git
   cd nome-repositorio
   ```

2. **Configure o Banco de Dados**
   - Crie o banco de dados e execute o script SQL completo.

3. **Execute as Consultas**
   - Utilize as consultas SQL fornecidas para obter os dados necessários.

---

Feito por Pedro Rafael para a cadeira de Banco de Dados.
