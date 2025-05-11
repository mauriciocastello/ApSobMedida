<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>App Profissional - Terno Sob Medida</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: 'Segoe UI', Tahoma, sans-serif; background: #f4f4f9; color: #333; }
    .container { max-width: 800px; margin: 40px auto; background: #fff; padding: 20px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); }
    .hidden { display: none; }
    header { text-align: center; margin-bottom: 20px; }
    header h1, header h2 { color: #222; }
    .models, .colors, .summary, .payment, .thanks { margin-top: 20px; }
    .model-card { display: inline-block; margin: 0 10px 20px; text-align: center; }
    .models img { width: 150px; cursor: pointer; border-radius: 4px; border: 2px solid transparent; }
    .models img.selected { border-color: #007bff; }
    .price { margin-top: 5px; font-weight: bold; color: #007bff; }
    .colors label { margin-right: 10px; }
    button { margin-top: 20px; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer; }
    button.primary { background: #007bff; color: #fff; }
    button.print { background: #28a745; color: #fff; }
    .summary img { max-width: 200px; display: block; margin: 10px auto; border-radius: 4px; }

    /* Medidas page styling with background image and adjusted input positions */
    .measure-img-container { position: relative; width: 100%; max-width: 800px; height: 450px; margin: auto;
      /* Use URL-encoded name if file has spaces or rename file to medidas_masculinas.png */
      background: url('./imagens/medidas_masculinas.png') no-repeat center / contain;
    }
    .input-box { position: absolute; width: 100px; background: rgba(255,255,255,0.9); border: 1px solid #ccc; border-radius: 4px; padding: 4px; text-align: center; }
    /* Inputs aligned next to each measurement label on the image */
    #input_colarinho     { top:  40px;  left: 180px; }
    #input_peito         { top: 120px;  left: 180px; }
    #input_cintura       { top: 200px;  left: 180px; }
    #input_quadril       { top: 280px;  left: 180px; }
    #input_ombro         { top:  40px;  left: 550px; }
    #input_biceps        { top: 120px;  left: 550px; }
    #input_cintura_calca { top: 200px;  left: 550px; }
    #input_manga         { top: 280px;  left: 550px; }
    #input_altura_calca  { top: 360px;  left: 550px; }

    /* Initial page custom background and crest */
    #page1 { background: #013220; color: #fff; padding: 60px 20px; border-radius: 8px; }
    #page1 header h1 { color: #fff; }
    #crest { display: block; margin: 20px auto; max-width: 200px; }
  </style>
</head>
<body>
  <div class="container">
    <section id="page1">
      <!-- Ensure 'imagens/brasao.jpg' is committed to GitHub in this path -->
      <img id="crest" src="./imagens/brasao.jpg" alt="Brasão Mauricio Atelier">
      <header><h1>Bem-vindo ao Terno Sob Medida</h1></header>
      <button class="primary" onclick="nextPage(2)">Iniciar Pedido</button>
    </section>
    <section id="page2" class="hidden">
      <header><h2>Escolha o Modelo</h2></header>
      <div class="models">
        <div class="model-card"><img src="./imagens/modelo1.jpg" alt="Modelo 1" onclick="selectModel('Modelo 1', this)"><p class="price">R$ 1.400,00</p></div>
        <div class="model-card"><img src="./imagens/modelo2.jpg" alt="Modelo 2" onclick="selectModel('Modelo 2', this)"><p class="price">R$ 1.400,00</p></div>
        <div class="model-card"><img src="./imagens/modelo3.jpg" alt="Modelo 3" onclick="selectModel('Modelo 3', this)"><p class="price">R$ 1.400,00</p></div>
      </div>
      <button class="primary" onclick="nextPage(3)">Próximo</button>
    </section>
    <section id="page3" class="hidden">
      <header><h2>Escolha a Cor</h2></header>
      <div class="colors">
        <label><input type="radio" name="color" value="Preto"> Preto</label>
        <label><input type="radio" name="color" value="Marinho"> Marinho</label>
        <label><input type="radio" name="color" value="Cinza"> Cinza</label>
        <label><input type="radio" name="color" value="Bege"> Bege</label>
      </div>
      <button class="primary" onclick="nextPage(4)">Próximo</button>
    </section>
    <section id="page4" class="hidden">
      <header><h2>Suas Medidas</h2></header>
      <div class="measure-img-container">
        <!-- Inputs overlayed on background image -->
        <input id="input_colarinho" class="input-box" type="number" placeholder="Colarinho">
        <input id="input_peito" class="input-box" type="number" placeholder="Peito">
        <input id="input_cintura" class="input-box" type="number" placeholder="Cintura">
        <input id="input_quadril" class="input-box" type="number" placeholder="Quadril">
        <input id="input_ombro" class="input-box" type="number" placeholder="Ombro">
        <input id="input_biceps" class="input-box" type="number" placeholder="Bíceps">
        <input id="input_cintura_calca" class="input-box" type="number" placeholder="Cint. Calça">
        <input id="input_manga" class="input-box" type="number" placeholder="Manga">
        <input id="input_altura_calca" class="input-box" type="number" placeholder="Alt. Calça">
      </div>
      <button class="primary" onclick="collectMeasures()">Resumo</button>
    </section>
    <section id="page5" class="hidden">
      <header><h2>Resumo do Pedido</h2></header>
      <div class="summary"></div>
      <button class="print" onclick="window.print()">Imprimir Pedido</button>
      <button class="primary" onclick="nextPage(6)">Pagamento</button>
    </section>
    <section id="page6" class="hidden">
      <header><h2>Pagamento</h2></header>
      <p>PIX: chave <strong>46991081445</strong></p>
      <button class="primary" onclick="alert('PIX selecionado')">Pagar com PIX</button>
      <a href="https://www.mercadopago.com.br/checkout" target="_blank"><button class="primary">Pagar com Mercado Pago</button></a>
      <button class="primary" onclick="nextPage(7)">Finalizar</button>
    </section>
    <section id="page7" class="hidden thanks">
      <header><h2>Obrigado pelo seu pedido!</h2></header>
      <p>Em breve entraremos em contato via WhatsApp ou e-mail.</p>
    </section>
  </div>
  <script>
    let order = {};
    function nextPage(n) {
      document.querySelectorAll('section').forEach(s => s.classList.add('hidden'));
      document.getElementById('page'+n).classList.remove('hidden');
      if(n===5) showSummary();
    }
    function selectModel(model, el) {
      order.model = model;
      order.modelSrc = el.getAttribute('src');
      document.querySelectorAll('.models img').forEach(i => i.classList.remove('selected'));
      el.classList.add('selected');
    }
    function collectMeasures() {
      const colorInput = document.querySelector('input[name=color]:checked');
      if(!colorInput) { alert('Por favor, escolha uma cor antes de prosseguir.'); return; }
      order.color = colorInput.value;
      order.measures = {
        colarinho: document.getElementById('input_colarinho').value,
        peito: document.getElementById('input_peito').value,
        cintura: document.getElementById('input_cintura').value,
        quadril: document.getElementById('input_quadril').value,
        ombro: document.getElementById('input_ombro').value,
        biceps: document.getElementById('input_biceps').value,
        cintura_calca: document.getElementById('input_cintura_calca').value,
        manga: document.getElementById('input_manga').value,
        altura_calca: document.getElementById('input_altura_calca').value
      };
      nextPage(5);
    }
    function showSummary() {
      const sumDiv = document.querySelector('.summary');
      let html = `<img src="${order.modelSrc}" alt="${order.model}" />`;
      html += `<p><strong>Modelo:</strong> ${order.model}</p><p><strong>Cor:</strong> ${order.color}</p>`;
      for(const [k,v] of Object.entries(order.measures)) {
        html += `<p><strong>${k.replace(/_/g,' ')}:</strong> ${v} cm</p>`;
      }
      sumDiv.innerHTML = html;
    }
  </script>
</body>
</html>
