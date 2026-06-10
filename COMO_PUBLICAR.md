# 🚀 Como Publicar o Sistema AD Novo Tempo
**Tempo estimado: 20–30 minutos**

---

## PASSO 1 — Criar o projeto Firebase

1. Acesse https://console.firebase.google.com
2. Clique em **"Adicionar projeto"**
3. Nome: `adnt-membros` (ou qualquer nome)
4. Desative o Google Analytics (não precisa) → **Criar projeto**

---

## PASSO 2 — Ativar Authentication

1. No menu lateral → **Authentication** → **Começar**
2. Aba **"Sign-in method"** → Ativar **E-mail/senha** → Salvar
3. Aba **"Users"** → **"Adicionar usuário"**
   - Adicione o e-mail e senha da secretaria
   - Adicione outros usuários (pastor, líderes) se necessário

---

## PASSO 3 — Ativar Firestore

1. Menu lateral → **Firestore Database** → **Criar banco de dados**
2. Escolha **"Modo de produção"** → próximo
3. Região: **southamerica-east1 (São Paulo)** → Concluir

**Aplicar as regras de segurança:**
1. Aba **"Regras"** no Firestore
2. Apague o conteúdo e cole o conteúdo do arquivo `firestore.rules`
3. Clique em **Publicar**

---

## PASSO 4 — Ativar Firebase Storage

1. Menu lateral → **Storage** → **Começar**
2. Modo de produção → Região southamerica-east1 → Concluir
3. Aba **"Regras"** → cole:
```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```
4. Publicar

---

## PASSO 5 — Pegar as credenciais do Firebase

1. No Firebase Console → ⚙️ **Configurações do projeto** (engrenagem)
2. Role até **"Seus apps"** → clique em **"</> Web"**
3. Nome do app: `adnt-sistema` → Registrar app
4. Copie o objeto `firebaseConfig` — exemplo:
```js
{
  apiKey: "AIzaSy...",
  authDomain: "adnt-membros.firebaseapp.com",
  projectId: "adnt-membros",
  storageBucket: "adnt-membros.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
}
```

---

## PASSO 6 — Colar as credenciais no sistema

Abra o arquivo `index.html` em um editor de texto (Bloco de Notas ou VS Code).

Encontre este trecho (próximo ao início do `<script type="module">`):
```js
const FIREBASE_CONFIG = {
  apiKey:            "COLE_AQUI_apiKey",
  authDomain:        "COLE_AQUI.firebaseapp.com",
  ...
```

Substitua cada `"COLE_AQUI_..."` pelo valor correspondente das suas credenciais.

**Salve o arquivo.**

---

## PASSO 7 — Criar o repositório no GitHub

1. Acesse https://github.com → **New repository**
2. Nome: `adnt-sistema-membros`
3. Privado (recomendado) → **Create repository**
4. Faça upload dos arquivos:
   - `index.html`
   - `assets/credencial.jpg`
   - `assets/logo.png`
   - `_headers`
   - `COMO_PUBLICAR.md`

**Ou via linha de comando:**
```bash
git init
git add .
git commit -m "Sistema de membros AD Novo Tempo"
git remote add origin https://github.com/SEU_USUARIO/adnt-sistema-membros.git
git push -u origin main
```

---

## PASSO 8 — Deploy no Cloudflare Pages

1. Acesse https://pages.cloudflare.com
2. **"Criar um projeto"** → **"Conectar ao Git"**
3. Conecte sua conta GitHub → selecione o repositório `adnt-sistema-membros`
4. Configurações de build:
   - **Framework:** Nenhum
   - **Comando de build:** *(deixar vazio)*
   - **Diretório de saída:** `/` (raiz)
5. Clique em **"Salvar e implantar"**

Aguarde ~2 minutos. O Cloudflare vai fornecer um link:
**`https://adnt-sistema-membros.pages.dev`**

---

## PASSO 9 — Migrar os dados existentes

Os 41 membros já cadastrados no sistema antigo precisam ir para o Firebase.

1. Abra o sistema antigo (`sistema_ad_novo_tempo.html`) no navegador
2. Vá em **Configurações → Exportar dados** → salve o arquivo JSON
3. No novo sistema (online), faça login
4. Vá em **Configurações → Importar dados** → selecione o JSON exportado
5. Os dados serão salvos automaticamente no Firestore ☁️

---

## ✅ Pronto!

O sistema estará acessível em:
**`https://adnt-sistema-membros.pages.dev`**

- Funciona em qualquer dispositivo (PC, celular, tablet)
- Login obrigatório com e-mail e senha
- Dados salvos automaticamente na nuvem
- Cada alteração (membro, ceia, config) sincroniza em tempo real

---

## ❓ Dúvidas frequentes

**Como adicionar um novo usuário (pastor, líder)?**
Firebase Console → Authentication → Users → Adicionar usuário

**E se ficar sem internet?**
O sistema mostra os últimos dados carregados. Ao voltar online, sincroniza automaticamente.

**Os dados são seguros?**
Sim. Apenas usuários autenticados acessam. O Firebase usa criptografia em trânsito e em repouso.

**Quanto custa?**
Zero. O plano gratuito do Firebase (Spark) suporta até 50.000 leituras/dia e 1GB de storage — muito mais que o necessário para uma igreja.
