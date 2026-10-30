<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="utf-8">
<title>TD Mundo 3D</title>
<style>
/* Reset e Estilos Gerais - Minimalista e Neutro */
* { margin: 0; padding: 0; box-sizing: border-box; }
body { background: #f8f9fa; color: #333333; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; font-size: 16px; line-height: 1.5; }
button, input, select { font-family: inherit; font-size: 15px; }
button { cursor: pointer; border: none; background: none; }
button:hover { opacity: 0.85; }
input:focus, select:focus, button:focus-visible, a:focus-visible { outline: 2px solid #2b2b2b; outline-offset: 2px; }

/* Container Principal */
.app-container { min-height: 100vh; display: flex; flex-direction: column; background-color: #f8f9fa; position: relative; overflow-x: hidden; }

/* Cabeçalho Limpo */
header { padding: 32px 20px 20px; text-align: center; border-bottom: 1px solid #e5e5e5; background: #ffffff; }
header .tagline { font-size: 12px; text-transform: uppercase; letter-spacing: 2px; color: #666666; margin-bottom: 6px; font-weight: 600; }
header h1 { font-size: 24px; color: #111111; font-weight: 700; letter-spacing: -0.5px; display: inline-block; cursor: default; user-select: none; }
header p { margin-top: 6px; font-size: 14px; color: #555555; max-width: 500px; margin-left: auto; margin-right: auto; }

/* Conteúdo */
main { max-width: 1100px; width: 100%; margin: 0 auto; padding: 24px 20px 80px; flex: 1; }

/* Abas de Navegação em Carrossel Horizontal */
.tabs-container { 
  display: flex; 
  gap: 16px; 
  justify-content: flex-start; 
  overflow-x: auto; 
  padding-bottom: 12px; 
  margin-bottom: 36px; 
  scroll-snap-type: x mandatory;
  -webkit-overflow-scrolling: touch;
}
.tabs-container::-webkit-scrollbar { height: 4px; }
.tabs-container::-webkit-scrollbar-thumb { background: #dcdcdc; border-radius: 4px; }

.category-bubble-wrapper { 
  display: flex; 
  flex-direction: column; 
  align-items: center; 
  gap: 6px; 
  cursor: pointer; 
  flex: 0 0 auto; 
  scroll-snap-align: start;
}
.category-bubble { width: 65px; height: 65px; border-radius: 50%; overflow: hidden; border: 2px solid #dcdcdc; transition: all 0.2s ease; background: #eee; }
.category-bubble img { width: 100%; height: 100%; object-fit: cover; }
.category-bubble-wrapper:hover .category-bubble { transform: scale(1.05); border-color: #111; }
.category-bubble-wrapper.active .category-bubble { border: 2.5px solid #111; box-shadow: 0 3px 10px rgba(0,0,0,0.12); }
.category-name { font-size: 12px; font-weight: 600; color: #333; text-transform: uppercase; letter-spacing: 0.5px; white-space: nowrap; }

/* Seções de Produtos */
.section-box { margin-bottom: 40px; }
.section-title { font-size: 16px; font-weight: 600; color: #111111; margin-bottom: 16px; padding-bottom: 6px; border-bottom: 2px solid #111111; text-transform: uppercase; letter-spacing: 0.5px; }

/* Grid de Produtos Clean */
.products-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(220px, 1fr)); gap: 16px; }
.product-card { background: #ffffff; border: 1px solid #e5e5e5; border-radius: 8px; overflow: hidden; display: flex; flex-direction: column; transition: transform 0.2s ease, box-shadow 0.2s ease; }
.product-card:hover { transform: translateY(-2px); box-shadow: 0 4px 12px rgba(0,0,0,0.05); }

.product-img-box { height: 160px; background: #f0f0f0; display: flex; align-items: center; justify-content: center; overflow: hidden; border-bottom: 1px solid #e5e5e5; }
.product-img-box img { width: 100%; height: 100%; object-fit: cover; }
.product-placeholder { font-size: 13px; color: #888888; font-weight: 500; letter-spacing: 1px; }

.product-info { padding: 14px; display: flex; flex-direction: column; gap: 8px; flex-grow: 1; }
.product-name { font-size: 14px; font-weight: 600; color: #222222; line-height: 1.4; }
.product-price { font-size: 16px; font-weight: 700; color: #111111; }

/* Botão WhatsApp Minimalista */
.wa-btn { margin-top: auto; display: block; text-align: center; padding: 9px; background: #111111; color: #ffffff; text-decoration: none; font-size: 13px; font-weight: 500; border-radius: 6px; transition: background 0.2s; }
.wa-btn:hover { background: #333333; }

/* Painel Admin Lateral */
aside { position: fixed; left: 0; top: 0; bottom: 0; z-index: 30; width: min(380px, 90vw); background: #ffffff; border-right: 1px solid #e5e5e5; box-shadow: 4px 0 20px rgba(0,0,0,0.08); overflow-y: auto; padding: 24px; transition: transform 0.3s ease; transform: translateX(-105%); }
.admin-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px; padding-bottom: 12px; border-bottom: 1px solid #e5e5e5; }
.admin-header h3 { font-size: 16px; font-weight: 600; color: #111111; }
.close-admin { font-size: 18px; font-weight: bold; color: #666; padding: 4px 8px; }

/* Formulários e Inputs do Painel */
.admin-form-group { display: flex; flex-direction: column; gap: 6px; margin-bottom: 14px; }
.admin-form-group label { font-size: 13px; font-weight: 600; color: #444444; }
.admin-form-group input, .admin-form-group select { padding: 10px; border: 1px solid #dcdcdc; border-radius: 6px; background: #fafafa; color: #333; }
.admin-primary-btn { padding: 12px; background: #111111; color: #ffffff; font-weight: 600; border-radius: 6px; width: 100%; margin-top: 6px; }
.admin-primary-btn:hover { background: #333333; }

.admin-nav-tabs { display: grid; grid-template-columns: repeat(4, 1fr); gap: 4px; margin-bottom: 20px; }
.admin-nav-tab { padding: 8px 4px; font-size: 11px; font-weight: 600; border: 1px solid #dcdcdc; border-radius: 4px; background: #f5f5f5; text-align: center; color: #444; }

.sub-admin-tabs { display: grid; grid-template-columns: repeat(2, 1fr); gap: 6px; margin-bottom: 14px; }
.sub-admin-tab { padding: 8px; font-size: 12px; font-weight: 600; border: 1px solid #dcdcdc; border-radius: 4px; background: #f5f5f5; text-align: center; color: #444; cursor: pointer; }

.item-row { display: flex; justify-content: space-between; align-items: center; padding: 8px 10px; border: 1px solid #eee; border-radius: 6px; margin-bottom: 8px; background: #fafafa; font-size: 14px; }
.item-row button { padding: 4px 8px; font-size: 12px; border-radius: 4px; color: #fff; font-weight: 500; }
.btn-edit { background: #0066cc; }
.btn-del { background: #cc0000; }

footer { text-align: center; padding: 20px; font-size: 12px; color: #aaa; border-top: 1px solid #eee; }
</style>

<!-- SDK do Supabase via CDN -->
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
</head>
<body>

<div class="app-container">

  <header>
    <div class="tagline">Catálogo de Produtos</div>
    <!-- ATALHO SECRETO: Dê um clique duplo no título para abrir o Admin -->
    <h1 id="secretAdminTrigger" title="Painel Admin">TD Mundo 3D</h1>
    <p>Ideias que ganham forma. Escolha seu modelo e feche o pedido pelo WhatsApp.</p>
  </header>

  <main>
    <!-- Círculos de Categorias em Carrossel Horizontal -->
    <div class="tabs-container" id="tabList"></div>

    <!-- Container do Catálogo -->
    <div id="catalogContainer"></div>
  </main>

  <footer>
    <span>TD Mundo 3D © Todos os direitos reservados</span>
  </footer>

  <!-- Painel Admin -->
  <aside id="adminPanel">
    <div class="admin-header">
      <h3>Painel Administrativo</h3>
      <button class="close-admin" onclick="toggleAdmin()">✕</button>
    </div>

    <!-- Login -->
    <div id="loginSection" class="admin-form-group">
      <label>Senha de Acesso</label>
      <input id="pinInput" type="password" placeholder="Digite a senha">
      <button class="admin-primary-btn" onclick="fazerLogin()">Entrar</button>
      <div id="pinMsg" style="color: #cc0000; font-size: 13px; margin-top: 4px;"></div>
      <div style="color: #777; font-size: 12px; margin-top: 4px;">Senha padrão para teste: <b>1234</b></div>
    </div>

    <!-- Dashboard -->
    <div id="dashboardSection" style="display:none;">
      <div class="admin-nav-tabs">
        <button class="admin-nav-tab" id="navAdd" onclick="mudarTabAdmin('add')">Adicionar</button>
        <button class="admin-nav-tab" id="navCreate" onclick="mudarTabAdmin('create')">Seção</button>
        <button class="admin-nav-tab" id="navEdit" onclick="mudarTabAdmin('edit')">Editar</button>
        <button class="admin-nav-tab" id="navRemove" onclick="mudarTabAdmin('remove')">Remover</button>
      </div>

      <!-- Aba Adicionar Produto -->
      <div id="tabAddContent" style="display:none;">
        <div style="font-size:15px; font-weight:600; color:#111; margin-bottom:12px;">Adicionar Novo Produto</div>
        <div class="admin-form-group">
          <label>Foto do Produto</label>
          <input id="prodFotoInput" type="file" accept="image/*">
          <img id="prodPreview" src="" alt="" style="display:none; width:100%; height:120px; object-fit:cover; border-radius:6px; margin-top:6px;">
        </div>
        <div class="admin-form-group">
          <label>Nome do Produto</label>
          <input id="prodNome" type="text" placeholder="Ex: Vaso Minimalista">
        </div>
        <div class="admin-form-group">
          <label>Valor (R$)</label>
          <input id="prodValor" type="text" placeholder="Ex: 39.90">
        </div>
        <div class="admin-form-group">
          <label>Seção de Destino</label>
          <select id="prodSecaoSelect"></select>
        </div>
        <button class="admin-primary-btn" onclick="salvarNovoProduto()">Adicionar Produto</button>
      </div>

      <!-- Aba Criar Seção com Imagem -->
      <div id="tabCreateContent" style="display:none;">
        <div style="font-size:15px; font-weight:600; color:#111; margin-bottom:12px;">Criar Nova Seção</div>
        <div class="admin-form-group">
          <label>Foto da Seção (Categoria)</label>
          <input id="secaoFotoInput" type="file" accept="image/*">
          <img id="secaoPreview" src="" alt="" style="display:none; width:100%; height:120px; object-fit:cover; border-radius:6px; margin-top:6px;">
        </div>
        <div class="admin-form-group">
          <label>Nome da Seção</label>
          <input id="novaSecaoNome" type="text" placeholder="Ex: Casa">
        </div>
        <button class="admin-primary-btn" onclick="criarSecao()">Criar Seção</button>
      </div>

      <!-- Aba Editar -->
      <div id="tabEditContent" style="display:none;">
        <div class="sub-admin-tabs">
          <button class="sub-admin-tab" id="subEditProdBtn" onclick="mudarSubEdit('produtos')">Produtos</button>
          <button class="sub-admin-tab" id="subEditSecBtn" onclick="mudarSubEdit('secoes')">Seções</button>
        </div>
        <div id="listaEditarContainer"></div>
        <div id="listaEditarSecoesContainer" style="display:none;"></div>
      </div>

      <!-- Formulário de Edição Individual de Produto -->
      <div id="tabEditFormContent" style="display:none;">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:12px;">
          <span style="font-size:15px; font-weight:600; color:#111;">Editando Produto</span>
          <button onclick="mudarTabAdmin('edit'); mudarSubEdit('produtos');" style="font-size:12px; color:#0066cc; text-decoration:underline;">← Voltar à lista</button>
        </div>
        <input type="hidden" id="editProdId">
        <div class="admin-form-group">
          <label>Alterar Foto</label>
          <input id="editProdFotoInput" type="file" accept="image/*">
          <img id="editProdPreview" src="" alt="" style="display:none; width:100%; height:120px; object-fit:cover; border-radius:6px; margin-top:6px;">
        </div>
        <div class="admin-form-group">
          <label>Nome do Produto</label>
          <input id="editProdNome" type="text">
        </div>
        <div class="admin-form-group">
          <label>Valor (R$)</label>
          <input id="editProdValor" type="text">
        </div>
        <div class="admin-form-group">
          <label>Seção de Destino</label>
          <select id="editProdSecaoSelect"></select>
        </div>
        <button class="admin-primary-btn" onclick="salvarEdicaoProduto()">Salvar Alterações</button>
      </div>

      <!-- Formulário de Edição Individual de Seção -->
      <div id="tabEditSecaoFormContent" style="display:none;">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:12px;">
          <span style="font-size:15px; font-weight:600; color:#111;">Editando Seção</span>
          <button onclick="mudarTabAdmin('edit'); mudarSubEdit('secoes');" style="font-size:12px; color:#0066cc; text-decoration:underline;">← Voltar à lista</button>
        </div>
        <input type="hidden" id="editSecaoId">
        <div class="admin-form-group">
          <label>Alterar Foto da Seção</label>
          <input id="editSecaoFotoInput" type="file" accept="image/*">
          <img id="editSecaoPreview" src="" alt="" style="display:none; width:100%; height:120px; object-fit:cover; border-radius:6px; margin-top:6px;">
        </div>
        <div class="admin-form-group">
          <label>Nome da Seção</label>
          <input id="editSecaoNome" type="text">
        </div>
        <button class="admin-primary-btn" onclick="salvarEdicaoSecao()">Salvar Alterações</button>
      </div>

      <!-- Aba Remover -->
      <div id="tabRemoveContent" style="display:none;">
        <div style="font-size:15px; font-weight:600; color:#111; margin-bottom:12px;">Remover Produto</div>
        <div id="listaRemoverContainer"></div>
      </div>

      <div id="adminMsg" style="margin-top: 12px; font-size: 13px; font-weight: 500; color: #008800;"></div>
    </div>
  </aside>

</div>

<script>
// Configuração do Supabase
const SUPABASE_URL = "https://kdujdhckasageajfdzvl.supabase.co";
const SUPABASE_ANON_KEY = "Sb_publishable_2HEwPu5G3Ox8jZJXhXdzqA_9q-VZTr1";

const supabaseClient = window.supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

const WHATSAPP_NUMERO = "5511999999999";

let storeData = {
  activeTab: 'all',
  isAdminOpen: false,
  isAuthed: false,
  adminTab: 'add',
  subEditMode: 'produtos',
  editingId: null,
  editingSecaoId: null,
  sections: [
    { id: 'casa', name: 'Casa', img: 'https://images.unsplash.com/photo-1513519245088-0e12902e5a38?auto=format&fit=crop&w=200&q=80' },
    { id: 'decoracao', name: 'Decoração', img: 'https://images.unsplash.com/photo-1583847268964-b28dc8f51f92?auto=format&fit=crop&w=200&q=80' },
    { id: 'geek', name: 'Geek', img: 'https://images.unsplash.com/photo-1607604276583-eef5d076aa5f?auto=format&fit=crop&w=200&q=80' }
  ],
  products: [
    { id: 1, name: 'Vaso Minimalista para Casa', price: 45.00, section: 'casa', photo: '' },
    { id: 2, name: 'Suporte Geométrico', price: 25.00, section: 'decoracao', photo: '' },
    { id: 3, name: 'Chaveiro Colecionável', price: 15.00, section: 'geek', photo: '' }
  ]
};

// Funções de Comunicação com o Supabase
async function carregarDadosDoSupabase() {
  try {
    const { data: produtosData, error: prodError } = await supabaseClient.from('products').select('*');
    const { data: secoesData, error: secError } = await supabaseClient.from('sections').select('*');

    if (!prodError && produtosData && produtosData.length > 0) {
      storeData.products = produtosData;
    }
    if (!secError && secoesData && secoesData.length > 0) {
      storeData.sections = secoesData;
    }
  } catch (err) {
    console.log("Usando dados locais de contingência.", err);
  }
  render();
}

function fmtMoeda(val) {
  return 'R$ ' + Number(val).toFixed(2).replace('.', ',');
}

function render() {
  renderTabs();
  renderCatalog();
  renderAdminSelects();
  renderAdminLists();
}

function renderTabs() {
  const container = document.getElementById('tabList');
  
  let html = `
    <div class="category-bubble-wrapper ${storeData.activeTab === 'all' ? 'active' : ''}" onclick="mudarAba('all')">
      <div class="category-bubble" style="display:flex;align-items:center;justify-content:center;background:#111;color:#fff;font-weight:600;font-size:11px;">TUDO</div>
      <span class="category-name">Todos</span>
    </div>
  `;

  storeData.sections.forEach(sec => {
    const isSelected = storeData.activeTab === sec.id;
    html += `
      <div class="category-bubble-wrapper ${isSelected ? 'active' : ''}" onclick="mudarAba('${sec.id}')">
        <div class="category-bubble">
          <img src="${sec.img || 'https://via.placeholder.com/200'}" alt="${sec.name}">
        </div>
        <span class="category-name">${sec.name}</span>
      </div>
    `;
  });
  container.innerHTML = html;
}

function renderCatalog() {
  const container = document.getElementById('catalogContainer');
  let html = '';
  const secsToDisplay = storeData.activeTab === 'all' 
    ? storeData.sections 
    : storeData.sections.filter(s => s.id === storeData.activeTab);

  secsToDisplay.forEach(sec => {
    const produtosDaSecao = storeData.products.filter(p => p.section === sec.id);

    html += `<div class="section-box"><h2 class="section-title">${sec.name}</h2>`;

    if (produtosDaSecao.length === 0) {
      html += `<p style="color:#777; font-size:13px;">Nenhum produto nesta seção.</p>`;
    } else {
      html += `<div class="products-grid">`;
      produtosDaSecao.forEach(p => {
        const textoWa = encodeURIComponent(`Olá! Gostaria de mais informações sobre o produto: ${p.name} (${fmtMoeda(p.price)})`);
        const linkWa = `https://wa.me/${WHATSAPP_NUMERO}?text=${textoWa}`;
        
        html += `
          <div class="product-card">
            <div class="product-img-box">
              ${p.photo ? `<img src="${p.photo}" alt="${p.name}">` : `<span class="product-placeholder">FOTO 3D</span>`}
            </div>
            <div class="product-info">
              <div class="product-name">${p.name}</div>
              <div class="product-price">${fmtMoeda(p.price)}</div>
              <a href="${linkWa}" target="_blank" class="wa-btn">Pedir no WhatsApp</a>
            </div>
          </div>
        `;
      });
      html += `</div>`;
    }
    html += `</div>`;
  });
  container.innerHTML = html;
}

function mudarAba(tabId) {
  storeData.activeTab = tabId;
  render();
}

function toggleAdmin() {
  storeData.isAdminOpen = !storeData.isAdminOpen;
  const panel = document.getElementById('adminPanel');
  panel.style.transform = storeData.isAdminOpen ? 'translateX(0)' : 'translateX(-105%)';
}

document.getElementById('secretAdminTrigger').addEventListener('dblclick', function() {
  toggleAdmin();
});

function fazerLogin() {
  const pin = document.getElementById('pinInput').value;
  if(pin === '1234') {
    storeData.isAuthed = true;
    document.getElementById('loginSection').style.display = 'none';
    document.getElementById('dashboardSection').style.display = 'block';
    mudarTabAdmin('add');
    render();
  } else {
    document.getElementById('pinMsg').innerText = 'Senha incorreta.';
  }
}

function mudarTabAdmin(tab) {
  storeData.adminTab = tab;
  storeData.editingId = null;
  storeData.editingSecaoId = null;
  
  ['add', 'create', 'edit', 'remove'].forEach(t => {
    const btn = document.getElementById('nav' + t.charAt(0).toUpperCase() + t.slice(1));
    if(btn) btn.style.background = (t === tab) ? '#111' : '#f5f5f5';
    if(btn) btn.style.color = (t === tab) ? '#fff' : '#444';
  });

  document.getElementById('tabAddContent').style.display = (tab === 'add') ? 'block' : 'none';
  document.getElementById('tabCreateContent').style.display = (tab === 'create') ? 'block' : 'none';
  document.getElementById('tabEditContent').style.display = (tab === 'edit') ? 'block' : 'none';
  document.getElementById('tabEditFormContent').style.display = 'none';
  document.getElementById('tabEditSecaoFormContent').style.display = 'none';
  document.getElementById('tabRemoveContent').style.display = (tab === 'remove') ? 'block' : 'none';

  if(tab === 'add') limparFormularioAdicao();
  if(tab === 'create') limparFormularioSecao();
  if(tab === 'edit') mudarSubEdit('produtos');
  document.getElementById('adminMsg').innerText = '';
  renderAdminLists();
}

function mudarSubEdit(sub) {
  storeData.subEditMode = sub;
  document.getElementById('subEditProdBtn').style.background = (sub === 'produtos') ? '#111' : '#f5f5f5';
  document.getElementById('subEditProdBtn').style.color = (sub === 'produtos') ? '#fff' : '#444';
  document.getElementById('subEditSecBtn').style.background = (sub === 'secoes') ? '#111' : '#f5f5f5';
  document.getElementById('subEditSecBtn').style.color = (sub === 'secoes') ? '#fff' : '#444';

  document.getElementById('listaEditarContainer').style.display = (sub === 'produtos') ? 'block' : 'none';
  document.getElementById('listaEditarSecoesContainer').style.display = (sub === 'secoes') ? 'block' : 'none';
  document.getElementById('tabEditFormContent').style.display = 'none';
  document.getElementById('tabEditSecaoFormContent').style.display = 'none';
}

function renderAdminSelects() {
  ['prodSecaoSelect', 'editProdSecaoSelect'].forEach(id => {
    const select = document.getElementById(id);
    if(!select) return;
    let html = '';
    storeData.sections.forEach(s => {
      html += `<option value="${s.id}">${s.name}</option>`;
    });
    select.innerHTML = html;
  });
}

function renderAdminLists() {
  const editContainer = document.getElementById('listaEditarContainer');
  if(editContainer) {
    let html = '';
    storeData.products.forEach(p => {
      html += `<div class="item-row">
        <span>${p.name}</span>
        <button class="btn-edit" onclick="carregarParaEditar('${p.id}')">Editar</button>
      </div>`;
    });
    editContainer.innerHTML = html;
  }

  const editSecaoContainer = document.getElementById('listaEditarSecoesContainer');
  if(editSecaoContainer) {
    let html = '';
    storeData.sections.forEach(s => {
      html += `<div class="item-row">
        <span>${s.name}</span>
        <button class="btn-edit" onclick="carregarSecaoParaEditar('${s.id}')">Editar</button>
      </div>`;
    });
    editSecaoContainer.innerHTML = html;
  }

  const removeContainer = document.getElementById('listaRemoverContainer');
  if(removeContainer) {
    let html = '';
    storeData.products.forEach(p => {
      html += `<div class="item-row">
        <span>${p.name}</span>
        <button class="btn-del" onclick="removerProduto('${p.id}')">Apagar</button>
      </div>`;
    });
    removeContainer.innerHTML = html;
  }
}

let tempFotoBase64 = '';
document.getElementById('prodFotoInput').addEventListener('change', function(e) {
  const file = e.target.files[0];
  if(file) {
    const reader = new FileReader();
    reader.onload = function(evt) {
      tempFotoBase64 = evt.target.result;
      const prev = document.getElementById('prodPreview');
      prev.src = tempFotoBase64;
      prev.style.display = 'block';
    }
    reader.readAsDataURL(file);
  }
});

let tempSecaoFotoBase64 = '';
document.getElementById('secaoFotoInput').addEventListener('change', function(e) {
  const file = e.target.files[0];
  if(file) {
    const reader = new FileReader();
    reader.onload = function(evt) {
      tempSecaoFotoBase64 = evt.target.result;
      const prev = document.getElementById('secaoPreview');
      prev.src = tempSecaoFotoBase64;
      prev.style.display = 'block';
    }
    reader.readAsDataURL(file);
  }
});

let tempEditFotoBase64 = '';
document.getElementById('editProdFotoInput').addEventListener('change', function(e) {
  const file = e.target.files[0];
  if(file) {
    const reader = new FileReader();
    reader.onload = function(evt) {
      tempEditFotoBase64 = evt.target.result;
      const prev = document.getElementById('editProdPreview');
      prev.src = tempEditFotoBase64;
      prev.style.display = 'block';
    }
    reader.readAsDataURL(file);
  }
});

let tempEditSecaoFotoBase64 = '';
document.getElementById('editSecaoFotoInput').addEventListener('change', function(e) {
  const file = e.target.files[0];
  if(file) {
    const reader = new FileReader();
    reader.onload = function(evt) {
      tempEditSecaoFotoBase64 = evt.target.result;
      const prev = document.getElementById('editSecaoPreview');
      prev.src = tempEditSecaoFotoBase64;
      prev.style.display = 'block';
    }
    reader.readAsDataURL(file);
  }
});

function limparFormularioAdicao() {
  document.getElementById('prodNome').value = '';
  document.getElementById('prodValor').value = '';
  document.getElementById('prodFotoInput').value = '';
  document.getElementById('prodPreview').style.display = 'none';
  tempFotoBase64 = '';
}

function limparFormularioSecao() {
  document.getElementById('novaSecaoNome').value = '';
  document.getElementById('secaoFotoInput').value = '';
  document.getElementById('secaoPreview').style.display = 'none';
  tempSecaoFotoBase64 = '';
}

async function criarSecao() {
  const nomeInput = document.getElementById('novaSecaoNome');
  const nome = nomeInput.value.trim();
  if(!nome) return;
  const id = nome.toLowerCase().replace(/[^a-z0-9]+/g, '-') + '-' + Date.now();
  const imgFinal = tempSecaoFotoBase64 || 'https://images.unsplash.com/photo-1581783342156-473bcf56c83d?auto=format&fit=crop&w=200&q=80';

  const novaSecao = { id, name: nome, img: imgFinal };
  
  const { error } = await supabaseClient.from('sections').insert([novaSecao]);
  if(error) {
    document.getElementById('adminMsg').innerText = 'Erro ao salvar seção no Supabase.';
    return;
  }

  storeData.sections.push(novaSecao);
  limparFormularioSecao();
  document.getElementById('adminMsg').innerText = 'Seção criada com sucesso!';
  render();
}

async function salvarNovoProduto() {
  const nome = document.getElementById('prodNome').value.trim();
  const valorStr = document.getElementById('prodValor').value.trim().replace(',', '.');
  const valor = parseFloat(valorStr);
  const secao = document.getElementById('prodSecaoSelect').value;

  if(!nome || isNaN(valor)) {
    document.getElementById('adminMsg').innerText = 'Preencha nome e valor válidos.';
    return;
  }

  const novoProd = { id: String(Date.now()), name: nome, price: valor, section: secao, photo: tempFotoBase64 };

  const { error } = await supabaseClient.from('products').insert([novoProd]);
  if(error) {
    document.getElementById('adminMsg').innerText = 'Erro ao salvar produto no Supabase.';
    return;
  }

  storeData.products.push(novoProd);
  limparFormularioAdicao();
  render();
  document.getElementById('adminMsg').innerText = 'Produto adicionado!';
}

function carregarParaEditar(id) {
  const p = storeData.products.find(item => String(item.id) === String(id));
  if(!p) return;
  storeData.editingId = p.id;
  
  document.getElementById('listaEditarContainer').style.display = 'none';
  document.getElementById('tabEditFormContent').style.display = 'block';

  document.getElementById('editProdNome').value = p.name;
  document.getElementById('editProdValor').value = p.price;
  document.getElementById('editProdSecaoSelect').value = p.section;
  
  if(p.photo) {
    tempEditFotoBase64 = p.photo;
    const prev = document.getElementById('editProdPreview');
    prev.src = p.photo;
    prev.style.display = 'block';
  } else {
    tempEditFotoBase64 = '';
    document.getElementById('editProdPreview').style.display = 'none';
  }
}

function carregarSecaoParaEditar(id) {
  const s = storeData.sections.find(item => String(item.id) === String(id));
  if(!s) return;
  storeData.editingSecaoId = s.id;
  
  document.getElementById('listaEditarSecoesContainer').style.display = 'none';
  document.getElementById('tabEditSecaoFormContent').style.display = 'block';

  document.getElementById('editSecaoNome').value = s.name;
  
  if(s.img) {
    tempEditSecaoFotoBase64 = s.img;
    const prev = document.getElementById('editSecaoPreview');
    prev.src = s.img;
    prev.style.display = 'block';
  } else {
    tempEditSecaoFotoBase64 = '';
    document.getElementById('editSecaoPreview').style.display = 'none';
  }
}

async function salvarEdicaoProduto() {
  const nome = document.getElementById('editProdNome').value.trim();
  const valorStr = document.getElementById('editProdValor').value.trim().replace(',', '.');
  const valor = parseFloat(valorStr);
  const secao = document.getElementById('editProdSecaoSelect').value;

  if(!nome || isNaN(valor)) {
    document.getElementById('adminMsg').innerText = 'Preencha nome e valor válidos.';
    return;
  }

  const pAtual = storeData.products.find(item => String(item.id) === String(storeData.editingId));
  const fotoFinal = tempEditFotoBase64 || pAtual.photo;

  const { error } = await supabaseClient.from('products')
    .update({ name: nome, price: valor, section: secao, photo: fotoFinal })
    .eq('id', storeData.editingId);

  if(error) {
    document.getElementById('adminMsg').innerText = 'Erro ao atualizar produto.';
    return;
  }

  storeData.products = storeData.products.map(p => {
    if(String(p.id) === String(storeData.editingId)) {
      return { ...p, name: nome, price: valor, section: secao, photo: fotoFinal };
    }
    return p;
  });

  mudarTabAdmin('edit');
  mudarSubEdit('produtos');
  document.getElementById('adminMsg').innerText = 'Produto atualizado com sucesso!';
}

async function salvarEdicaoSecao() {
  const nome = document.getElementById('editSecaoNome').value.trim();
  if(!nome) {
    document.getElementById('adminMsg').innerText = 'Preencha um nome válido para a seção.';
    return;
  }

  const sAtual = storeData.sections.find(item => String(item.id) === String(storeData.editingSecaoId));
  const imgFinal = tempEditSecaoFotoBase64 || sAtual.img;

  const { error } = await supabaseClient.from('sections')
    .update({ name: nome, img: imgFinal })
    .eq('id', storeData.editingSecaoId);

  if(error) {
    document.getElementById('adminMsg').innerText = 'Erro ao atualizar seção.';
    return;
  }

  storeData.sections = storeData.sections.map(s => {
    if(String(s.id) === String(storeData.editingSecaoId)) {
      return { ...s, name: nome, img: imgFinal };
    }
    return s;
  });

  mudarTabAdmin('edit');
  mudarSubEdit('secoes');
  document.getElementById('adminMsg').innerText = 'Seção atualizada com sucesso!';
}

async function removerProduto(id) {
  if(confirm('Deseja excluir este produto?')) {
    const { error } = await supabaseClient.from('products').delete().eq('id', id);
    if(error) {
      alert('Erro ao excluir produto no Supabase.');
      return;
    }
    storeData.products = storeData.products.filter(p => String(p.id) !== String(id));
    render();
    document.getElementById('adminMsg').innerText = 'Produto removido.';
  }
}

// Inicializa buscando os dados do Supabase ao carregar
carregarDadosDoSupabase();
</script>

</body>
</html>