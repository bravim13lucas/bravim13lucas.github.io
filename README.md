# Cadastro de Contatos via GitHub Pages e Google Apps Script

Este repositório contém um site simples que permite cadastrar contatos (nome e telefone) e armazená‑los automaticamente em uma planilha do **Google Sheets**. A página é hospedada gratuitamente no **GitHub Pages** e exibe sempre a lista atualizada de contatos cadastrados.

## 📜 Arquivos principais

- **`index.html`** – Página HTML com formulário de cadastro, tabela de exibição e código JavaScript para interagir com o Apps Script.
- **`README.md`** – Este documento explicando como configurar o Google Sheets e o Apps Script para que o site funcione.

## 🚀 Como configurar

Siga os passos abaixo para colocar o site em funcionamento:

1. **Criar a planilha no Google Sheets**

   - Acesse [Google Sheets](https://docs.google.com/spreadsheets/) e crie uma nova planilha.
   - Dê um nome à planilha (por exemplo, `Contatos`).
   - Na primeira linha, preencha as colunas com os títulos **`Nome`** e **`Telefone`** (nessa ordem). As entradas de cadastro serão gravadas a partir da segunda linha.

2. **Criar e configurar o Apps Script**

   - Na planilha criada, clique em **Extensões → Apps Script** para abrir o editor de script.
   - Apague qualquer código que esteja lá e substitua pelo script abaixo:

     ```javascript
     // Substitua pelo ID da sua planilha (encontrado na URL da planilha).
     const SHEET_ID = 'INSIRA_O_ID_DA_SUA_PLANILHA_AQUI';

     /**
      * Função executada em requisições GET.
      * Retorna toda a lista de contatos em formato JSON.
      */
     function doGet(e) {
       const sheet = SpreadsheetApp.openById(SHEET_ID).getActiveSheet();
       const data = sheet.getDataRange().getValues();
       // Ignora a primeira linha (cabeçalhos) e mapeia para objetos nome/telefone
       const result = data.slice(1).map(row => ({
         nome: row[0],
         telefone: row[1],
       }));
       return ContentService.createTextOutput(JSON.stringify(result))
         .setMimeType(ContentService.MimeType.JSON)
         .setHeader('Access-Control-Allow-Origin', '*')
         .setHeader('Access-Control-Allow-Methods', 'GET, POST');
     }

     /**
      * Função executada em requisições POST.
      * Adiciona um novo contato à planilha.
      */
     function doPost(e) {
       const sheet = SpreadsheetApp.openById(SHEET_ID).getActiveSheet();
       const data = JSON.parse(e.postData.contents);
       sheet.appendRow([data.nome, data.telefone]);
       const result = { status: 'success', message: 'Contato adicionado.' };
       return ContentService.createTextOutput(JSON.stringify(result))
         .setMimeType(ContentService.MimeType.JSON)
         .setHeader('Access-Control-Allow-Origin', '*')
         .setHeader('Access-Control-Allow-Methods', 'GET, POST');
     }
     ```

   - Substitua `INSIRA_O_ID_DA_SUA_PLANILHA_AQUI` pelo ID da sua planilha (ele aparece na URL da planilha entre `/d/` e `/edit`).
   - Clique em **Implantar → Nova implantação**.
   - Escolha **Tipo de implantação** como **Aplicativo da web**.
   - Em **Executar como**, selecione **Você mesmo** (ou a conta desejada).
   - Em **Quem tem acesso**, selecione **Qualquer pessoa**. Isso permite que o site acesse o script sem exigir login.
   - Clique em **Implantar** e autorize as permissões necessárias. Ao final, um endereço URL será exibido. **Copie esse URL**, pois ele será utilizado no arquivo `index.html`.

3. **Configurar o `index.html`**

   - Abra o arquivo `index.html` deste repositório.
   - Procure pela linha:
     ```html
     const APPS_SCRIPT_URL = 'INSIRA_AQUI_A_URL_DO_APPS_SCRIPT';
     ```
   - Substitua `INSIRA_AQUI_A_URL_DO_APPS_SCRIPT` pelo URL copiado no passo anterior.
   - Salve e faça commit da alteração.

4. **Publicar no GitHub Pages**

   - Este repositório deve ter o nome **`seu_usuario.github.io`** (por exemplo, `harvlucas.github.io`).
   - Nas configurações do repositório no GitHub, acesse **Pages**.
   - Selecione a branch `main` (ou `master`) e o diretório `/` como fonte e salve.
   - Após alguns minutos, seu site estará disponível em `https://seu_usuario.github.io/`.

Pronto! Ao acessar o endereço do GitHub Pages, você verá o formulário para cadastrar novos contatos e, logo abaixo, a lista atualizada de nomes e telefones armazenados na planilha.

## ✅ Testando o site

1. Preencha o formulário com um nome e telefone e clique em **Cadastrar**.
2. A página enviará os dados para o Apps Script, que gravará na planilha.
3. A lista de contatos na página é atualizada automaticamente para incluir o novo registro.
4. Se você recarregar a página, a tabela continuará exibindo todos os contatos já cadastrados.

## 📌 Observações

Este projeto é um exemplo simples criado para fins de teste e demonstração. Ele usa **Google Apps Script** como backend para persistir dados e **GitHub Pages** para hospedar a interface. Você pode estender este conceito para incluir mais campos, validações ou até mesmo autenticação, ajustando o Apps Script e o front‑end conforme necessário.

---

*Gerado automaticamente por um agente para demonstração de integração entre GitHub Pages e Google Apps Script.*
