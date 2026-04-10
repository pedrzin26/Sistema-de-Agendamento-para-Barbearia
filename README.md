Nome: Pedro Henrique de Oliveira Santos 
RA: 202510434

Atividade do dia 20_03_2026

Sistema de Agendamento para Barbearia
Introdução

O Sistema de Agendamento para Barbearia é uma plataforma online desenvolvida para otimizar o processo de marcação de serviços entre clientes e barbeiros. A solução permite que os clientes escolham serviços, horários e profissionais disponíveis de forma rápida, prática e organizada.

Seu principal propósito é modernizar o atendimento da barbearia, reduzindo filas, evitando atrasos e proporcionando uma melhor gestão dos horários dos profissionais.

Objetivos do Sistema

O sistema tem como principais objetivos:

Facilitar o agendamento de horários para os clientes
Organizar a agenda dos barbeiros
Reduzir filas e tempo de espera
Melhorar a experiência do cliente
Automatizar o controle de atendimentos
Público-Alvo

O sistema é destinado a:

Clientes da barbearia
Barbeiros
Administradores do estabelecimento
Funcionalidades do Sistema
Cadastro de Usuário

O cliente poderá criar uma conta informando:

Nome
E-mail
Telefone
Senha
Login no Sistema

Os usuários poderão acessar o sistema utilizando:

E-mail
Senha
Visualização de Serviços

O cliente poderá visualizar os serviços disponíveis, como:

Corte de cabelo
Barba
Corte + barba
Tratamentos capilares
Agendamento de Horário

O cliente poderá:

Escolher o serviço desejado
Selecionar o barbeiro
Escolher um horário disponível
Confirmar o agendamento
Gerenciamento de Agenda

O barbeiro ou administrador poderá:

Visualizar todos os agendamentos
Definir ou alterar horários disponíveis
Cancelar ou reagendar atendimentos
Requisitos Funcionais
Permitir o cadastro de usuários
Permitir login no sistema
Exibir os serviços disponíveis
Permitir o agendamento de horários
Permitir o gerenciamento da agenda
Requisitos Não Funcionais
Interface simples e fácil de usar
Compatibilidade com dispositivos móveis e computadores
Segurança na proteção dos dados dos usuários
Boa performance no carregamento das páginas
Tecnologias Utilizadas (Exemplo)
Frontend: HTML, CSS e JavaScript
Backend: Python ou Node.js
Banco de Dados: MySQL ou PostgreSQL
Hospedagem: Servidor web ou serviços em nuvem
Conclusão

O Sistema de Agendamento para Barbearia é uma solução eficiente que contribui para a organização do atendimento e a melhoria da experiência do cliente. Por meio da automatização dos agendamentos, o sistema proporciona mais praticidade para os clientes e maior controle para os profissionais, tornando o serviço mais ágil, moderno e profissional.

Parte do código:

<?php
$host = 'localhost';
$db   = 'barbearia';
$user = 'root';
$pass = '';

try {
    $pdo = new PDO("mysql:host=$host;dbname=$db;charset=utf8", $user, $pass);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    die("Erro na conexão: " . $e->getMessage());
}

session_start();
if (!isset($_SESSION['usuario_id'])) {
    $_SESSION['usuario_id'] = 1; 
    $_SESSION['usuario_nome'] = "Cliente de Teste";
}


$mensagem = "";
if ($_SERVER['REQUEST_METHOD'] == 'POST' && isset($_POST['acao']) && $_POST['acao'] == 'agendar') {
    $id_cliente = $_SESSION['usuario_id'];
    $id_servico = $_POST['servico'];
    $data_hora = $_POST['data'] . ' ' . $_POST['hora'];

    $sql = "INSERT INTO agendamentos (id_cliente, id_servico, data_hora) VALUES (?, ?, ?)";
    $stmt = $pdo->prepare($sql);
    
    if ($stmt->execute([$id_cliente, $id_servico, $data_hora])) {
        $mensagem = "<p style='color:green;'>Agendamento realizado com sucesso!</p>";
    } else {
        $mensagem = "<p style='color:red;'>Erro ao agendar.</p>";
    }
}

$pagina = isset($_GET['page']) ? $_GET['page'] : 'home';
?>

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema Barbearia</title>
    <style>
        body { font-family: sans-serif; line-height: 1.6; max-width: 800px; margin: auto; padding: 20px; background: #f4f4f4; }
        nav { background: #333; padding: 10px; margin-bottom: 20px; }
        nav a { color: #fff; margin-right: 15px; text-decoration: none; }
        .container { background: #fff; padding: 20px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th, td { border: 1px solid #ddd; padding: 10px; text-align: left; }
        th { background: #f8f8f8; }
        form input, form select, form button { display: block; width: 100%; margin-bottom: 10px; padding: 10px; }
        button { background: #28a745; color: white; border: none; cursor: pointer; }
    </style>
</head>
<body>

<nav>
    <a href="?page=home">Início</a>
    <a href="?page=agendar">Novo Agendamento</a>
    <a href="?page=admin">Painel do Barbeiro</a>
</nav>

<div class="container">
    <?= $mensagem ?>

    <?php if ($pagina == 'home'): ?>
        <h1>Bem-vindo à Barbearia</h1>
        <p>Olá, <strong><?= $_SESSION['usuario_nome'] ?></strong>! O que deseja fazer hoje?</p>
        <a href="?page=agendar">Clique aqui para marcar um horário.</a>

    <?php elseif ($pagina == 'agendar'): ?>
        <h1>Agendar Horário</h1>
        <form method="POST">
            <input type="hidden" name="acao" value="agendar">
            
            <label>Serviço:</label>
            <select name="servico" required>
                <option value="">Selecione...</option>
                <option value="1">Corte de Cabelo (R$ 30,00)</option>
                <option value="2">Barba (R$ 20,00)</option>
                <option value="3">Corte + Barba (R$ 45,00)</option>
            </select>

            <label>Data:</label>
            <input type="date" name="data" required>

            <label>Hora:</label>
            <input type="time" name="hora" required>

            <button type="submit">Confirmar Agendamento</button>
        </form>

    <?php elseif ($pagina == 'admin'): ?>
        <h1>Painel de Gerenciamento</h1>
        <p>Agendamentos marcados no sistema:</p>
        <table>
            <thead>
                <tr>
                    <th>Data/Hora</th>
                    <th>Cliente</th>
                    <th>Serviço</th>
                </tr>
            </thead>
            <tbody>
                <?php
                $sql = "SELECT a.data_hora, u.nome as cliente, s.nome as servico 
                        FROM agendamentos a 
                        JOIN usuarios u ON a.id_cliente = u.id 
                        JOIN servicos s ON a.id_servico = s.id 
                        ORDER BY a.data_hora ASC";
                $stmt = $pdo->query($sql);
                while ($row = $stmt->fetch(PDO::FETCH_ASSOC)): ?>
                    <tr>
                        <td><?= date('d/m/Y H:i', strtotime($row['data_hora'])) ?></td>
                        <td><?= htmlspecialchars($row['cliente']) ?></td>
                        <td><?= htmlspecialchars($row['servico']) ?></td>
                    </tr>
                <?php endwhile; ?>
            </tbody>
        </table>
    <?php endif; ?>
</div>

</body>
</html>
