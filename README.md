Markdown
# 📚 Scriptorium — Biblioteca Virtual 3D & Rastreador de Leituras

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3_3D-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript_ES6+-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![PWA](https://img.shields.io/badge/PWA-Instalável-5A0FC8?style=for-the-badge&logo=pwa&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google_Sheets_API-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)

O **Scriptorium** é uma aplicação web progressiva (*Single-File Web App*) para catalogação de livros lidos, controle de progresso de leitura e registro de resenhas pessoais. Desenvolvido inteiramente em um único arquivo `index.html` sem dependências de servidores pagos, o projeto combina renderização física 3D em CSS puro com integrações de câmera, nuvem e importação direta do Amazon Kindle.

---

## ✨ Funcionalidades Principais

### 🪵 1. Estante Física 3D Interativa (Dois Modos de Visão)
- **Vista em Lombadas 3D:** Os livros ficam guardados de lado na prateleira com espessura calculada proporcionalmente ao número real de páginas da obra. Ao interagir, o livro desliza para fora da estante (`translateZ`) e gira (`rotateY`), revelando a capa frontal.
- **Vista em Vitrine (Capas):** Exibição frontal das capas com efeito tridimensional de lombada e folhas laterais.
- **Divisão Dinâmica de Prateleiras:** O layout mede a largura da tela em tempo real (`window.innerWidth`) e divide automaticamente os livros em tábuas de madeira individuais para evitar livros flutuando na quebra de linha.

### 📖 2. Controle de Status e Progresso de Leitura
- Separação por abas: **Todos**, **Lendo Agora**, **Já Lidos** e **Quero Ler (Fila)**.
- **Fita Marcadora Vermelha:** Livros com o status *"Lendo Agora"* exibem automaticamente uma fita marcadora de página no topo da lombada 3D.
- **Atualização Rápida:** Botões de incremento rápido (`+5p`, `+15p`) e entrada direta de páginas dentro do modal do livro, com conclusão automática ao atingir 100%.

### 🔍 3. Busca Automática (Open Library API) & Scanner ISBN
- **Preenchimento Inteligente:** Basta digitar o título ou código ISBN para buscar automaticamente a capa em alta resolução, nome do autor e mediana de páginas via API pública da *Open Library*.
- **Leitor de Código de Barras pela Câmera:** Integração com a câmera traseira do smartphone (`html5-qrcode`) para escanear o código EAN-13/ISBN de livros físicos e preencher os metadados instantaneamente.
- **Upload Local com Compressão:** Suporte a imagens da galeria do dispositivo, redimensionadas automaticamente via `<canvas>` e convertidas para Base64 para economizar espaço no armazenamento local.

### 📱 4. Sincronização com Kindle (`My Clippings.txt`)
- Parser nativo para o arquivo `My Clippings.txt` do Amazon Kindle.
- Extrai títulos, autores e agrupa todas as frases grifadas e notas diretamente na seção de opiniões de cada livro, evitando duplicatas.

### ☁️ 5. Nuvem Gratuita via Google Sheets + Backup JSON
- **Persistência Híbrida:** Salva instantaneamente no `localStorage` do navegador e sincroniza em segundo plano com uma planilha do Google Drive através do **Google Apps Script**.
- **Exportação/Restauração:** Geração de arquivo `.json` completo para backups locais.

### 📲 6. Ergonomia Mobile & PWA (Progressive Web App)
- **Máquina de Estados Touch (2 Toques):** Em telas sensíveis ao toque (onde não existe `:hover`), o 1º toque puxa o livro da estante em 3D para visualização da capa, e o 2º toque abre o modal de anotações.
- **Interface Nativa:** Barra de navegação inferior fixa (*Bottom Navigation*), botão de ação flutuante (*FAB*), modais deslizantes (*Bottom Sheets*) e geração dinâmica de `manifest.json` em memória para instalação na tela inicial do Android e iOS.
- **Iluminação de Ambiente:** Alternância entre **Modo Dia** e **Modo Luz Noturna** (tons quentes de madeira escura e âmbar para leitura).

---

## 🛠️ Arquitetura Técnica

O projeto foi desenhado seguindo o conceito de **Zero-Build Single-File Architecture**:

```text
scriptorium/
├── index.html   # Contém toda a marcação, estilos 3D, lógica JS e Manifesto PWA
└── README.md    # Documentação do projeto
Estrutura do Objeto de Dados (JSON Schema)
Cada livro salvo no localStorage ou enviado ao Google Sheets segue a estrutura abaixo:

JSON
{
  "id": "book-1729270000000",
  "title": "Código Limpo",
  "author": "Robert C. Martin",
  "status": "reading",
  "currentPage": 185,
  "totalPages": 425,
  "spineColor": "#1f4e5f",
  "genre": "Tecnologia / Engenharia",
  "rating": 5,
  "dateFinished": "2026-10-18",
  "cover": "[https://covers.openlibrary.org/b/id/14842144-L.jpg](https://covers.openlibrary.org/b/id/14842144-L.jpg)",
  "review": "Análise pessoal e pontos principais sobre arquitetura limpa...",
  "quote": "Bons programadores escrevem código que humanos podem entender."
}
🚀 Como Executar o Projeto
1. Localmente no Computador
Não é necessário instalar Node.js ou pacotes externos.

Baixe ou clone este repositório:

Bash
git clone [https://github.com/SEU_USUARIO/scriptorium.git](https://github.com/SEU_USUARIO/scriptorium.git)
Abra o arquivo index.html diretamente em qualquer navegador moderno.

2. No Celular (GitHub Pages)
Para utilizar o Scanner de Código de Barras pela câmera e a instalação PWA, acesse através do link hospedado com HTTPS no GitHub Pages:

https://SEU_USUARIO.github.io/scriptorium/

☁️ Como Configurar a Sincronização com Google Sheets (Opcional)
Crie uma nova planilha no Google Sheets.

No menu superior, acesse Extensões > Apps Script.

Cole o script receptor (doGet e doPost para leitura e gravação da aba Livros).

Clique em Implantar > Nova implantação, selecione App da Web, configure o acesso para Qualquer pessoa e copie a URL gerada (terminada em /exec).

No Scriptorium, clique no botão ☁️ Nuvem, cole a URL e clique em Salvar URL.

📖 Como Importar Destaques do Kindle
Conecte seu Kindle ao computador via cabo USB (ou ao celular via adaptador OTG).

Acesse a pasta documents dentro do armazenamento do Kindle e localize o arquivo My Clippings.txt.

No cabeçalho do Scriptorium, clique em 📖 Kindle (.txt) e selecione o arquivo. Todos os livros e destaques serão catalogados automaticamente.

📄 Licença
Este projeto é de código aberto e pode ser modificado livremente para uso pessoal e educacional.


---

## Como Enviar o `README.md` para o seu GitHub

Se você estiver usando o terminal no Linux dentro da pasta do projeto, basta salvar o arquivo acima como `README.md` e rodar:

```bash
git add README.md
git commit -m "docs: adiciona README completo do projeto"
git push
