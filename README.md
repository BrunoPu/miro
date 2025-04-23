<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Gestão de Certificado Digital</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #f4f4f4;
    }

    header {
      background-color: #f37021;
      padding: 10px 20px;
      color: white;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    header .logo {
      background: url('https://www.itau.com.br/assets/logo.svg') no-repeat left center;
      background-size: contain;
      width: 50px;
      height: 30px;
    }

    .user-info {
      font-size: 12px;
      text-align: right;
    }

    .content {
      padding: 30px 40px;
    }

    h1 {
      font-size: 18px;
      margin-bottom: 10px;
    }

    .subtitle {
      font-size: 14px;
      margin-bottom: 20px;
    }

    .cards {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
      gap: 20px;
      margin-top: 20px;
    }

    .card {
      background: white;
      border-radius: 8px;
      padding: 15px;
      box-shadow: 0 0 6px rgba(0,0,0,0.1);
      cursor: pointer;
      transition: box-shadow 0.3s;
    }

    .card:hover {
      box-shadow: 0 0 12px rgba(0,0,0,0.2);
    }

    .card h2 {
      font-size: 14px;
      margin-bottom: 8px;
    }

    .card p {
      font-size: 12px;
      color: #555;
    }

    .button {
      margin-top: 30px;
      text-align: right;
    }

    .button button {
      background-color: #f37021;
      color: white;
      border: none;
      padding: 10px 20px;
      border-radius: 5px;
      font-size: 14px;
      cursor: pointer;
    }

    .button button:hover {
      background-color: #d35f1b;
    }
  </style>
</head>
<body>

<header>
  <div class="logo"></div>
  <div class="user-info">
    BRUNO HENRIQUE PUGLIESE<br>
    Código Funcional: 987237598
  </div>
</header>

<div class="content">
  <h1>Bem-vindo, <strong>BRUNO HENRIQUE PUGLIESE</strong></h1>
  <p class="subtitle">
    No sistema de Gestão de Certificado Digital você pode cadastrar, gerir e verificar dúvidas frequentes de certificados.<br>
    <a href="#">Critérios de Segurança da Informação para Gerenciamento de Certificados Digitais (Brasil)</a>
  </p>

  <h3>Selecione opção desejada:</h3>
  <div class="cards">
    <div class="card">
      <h2>Solicitações de Certificado</h2>
      <p>Solicite certificados externos e internos de pessoas e AWPS.</p>
    </div>
    <div class="card">
      <h2>Minhas Solicitações</h2>
      <p>Acompanhe suas solicitações de certificado.</p>
    </div>
    <div class="card">
      <h2>Consulta de Certificados</h2>
      <p>Consulte certificados em seu nome ou de terceiros.</p>
    </div>
    <div class="card">
      <h2>Indicar renovação ou revogação</h2>
      <p>Renove ou revogue certificados On Premise.</p>
    </div>
    <div class="card">
      <h2>Lista Status Renovacao</h2>
      <p>Acompanhamento dos status da renovação.</p>
    </div>
    <div class="card">
      <h2>Gestão de Responsavel</h2>
      <p>Indique, aprove, recuse ou cancele solicitações.</p>
    </div>
    <div class="card">
      <h2>Gestão de Auto Assinado</h2>
      <p>Cadastre seu Certificado Auto Assinado.</p>
    </div>
    <div class="card">
      <h2>Saiba mais</h2>
      <p>Encontre as respostas para suas dúvidas sobre certificados e políticas internas.</p>
    </div>
    <div class="card">
      <h2>Gestão de certificados</h2>
      <p>Faça uma gestão detalhada de todos os certificados do banco.</p>
    </div>
    <div class="card">
      <h2>Gestão Admin</h2>
      <p>Acesse opções administrativas do sistema.</p>
    </div>
  </div>

  <div class="button">
    <button>Avaliar experiência</button>
  </div>
</div>

</body>
</html>