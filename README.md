# Auto SEI — Automação de Documentos no SEI Funpresp-jud

<img width="1911" height="1025" alt="image" src="https://github.com/user-attachments/assets/548abc99-7b7d-4cae-ac18-8d7f606740a9" />

Ferramenta Web desenvolvida em Python para automatizar a inclusão de documentos externos no sistema SEI da Funpresp-jud.

---

## Funcionalidades

### Aba Único

Trabalha em **um processo** com **vários documentos**. O usuário informa o número do processo, seleciona a pasta com os PDFs, define o tipo de cada documento na lista e executa. Cada linha exibe o status individual (**OK** / **ERRO**) ao final da execução.

**O código preenche automaticamente no SEI:**
- Tipo de Documento
- Nome na Árvore (derivado do nome do arquivo sem extensão, máx. 50 caracteres; substituível via botão **Nomes**; caso deixe vazio o o programa deixa nome na árvore SEI vazio)
- Data de elaboração (data atual)
- Formato: Nato-digital
- Nível de acesso: Padrão do perfil
- Anexo do PDF

### Aba Múltiplos

Trabalha em **vários processos**, um documento por processo. Os PDFs são listados automaticamente com o número de processo derivado do nome do arquivo. Processo, tipo e nome são editáveis antes de executar.

**O código preenche automaticamente no SEI:**
- Tipo de Documento
- Nome na Árvore (derivado do nome do arquivo sem extensão, máx. 50 caracteres; substituível via botão **Nomes**; caso deixe vazio o o programa deixa nome na árvore SEI vazio)
- Data de elaboração (data atual)
- Formato: Nato-digital
- Nível de acesso: Padrão do perfil
- Anexo do PDF
- *(Nome na Árvore não é preenchido — o SEI mantém o valor padrão)*

---

## Botão "Sem PDF" (ícone PFD azul-vermelho)

Disponível em ambas as abas, em cada linha da tabela. Quando ativado (ícone preenchido em vermelho):

- O registro é criado no SEI normalmente (tipo, data, nato-digital, nível de acesso).
- **Nenhum arquivo PDF é anexado.**
- O campo de nome do arquivo pode ficar vazio — o sistema detecta automaticamente que não há documento e prossegue sem ele.

**Caso de uso:** ao carregar, por exemplo, 5 PDFs de uma pasta, é possível adicionar manualmente uma 6ª linha sem documento associado. Clicando no ícone e deixando-o vermelho, o sistema cria o registro no SEI sem anexar ou exigir arquivo.

---

## Botão "Nomes" (Nome na Árvore)

Ao clicar:

1. Abre um seletor de arquivo para escolher uma planilha Excel (`.xlsx`, `.xls`, `.xlsm`).
2. Solicita o nome da aba da planilha.
3. Lê a coluna **`Nome na árvore`** (cabeçalho exato, incluindo acentuação).
4. Preenche o campo de nome na árvore de cada linha na ordem de aparição, limitado ao número de linhas existentes.

---

## Nomenclatura de Arquivos — Aba Múltiplos

O nome de cada PDF deve corresponder ao número do processo, substituindo as barras (`/`) por pontos (`.`):

| Nome do arquivo             | Processo no SEI      |
|-----------------------------|----------------------|
| `00000.2026.pdf`            | `00413/2026`         |
| `00000.2024.pdf`            | `00000.2024`         |

O campo **Processo** em cada linha é editável, podendo ser corrigido manualmente antes de executar.

-A nomenclatura de acordo com o número do processo não é obrigatória, funcionando apenas como facilitadora para o usuário.

---

## Estrutura do Projeto

```
autosei/
├── run.bat           ← Rodar o código no server e registrar logs 
├── app.py            ← servidor Flask (backend)
├── config.py         ← automação Selenium (adaptada para headless no servidor)
├── externos.py       ← lista de tipos externos (original, não muda)
├── internos.py       ← lista de tipos internos (original, não muda)
├── requirements.txt  ← dependências Python
├── templates/
│   └── index.html    ← interface web
└── uploads/          ← pasta temporária (criada automaticamente)
```

---

## Pré-requisitos
- Windows Server conectado a rede empresarial
- Python **3.10** ou superior
- **Google Chrome** instalado
- **ChromeDriver** compatível com a versão do Chrome instalado
  
---

## Rodar no servidor

## PASSO 1 — Copiar os arquivos para o servidor

Copie a pasta `autosei/` completa para o servidor.

## PASSO 2 — Criar ambiente virtual e instalar dependências

```bash
cd /home/usuario/autosei

# Criar ambiente virtual
python3 -m venv venv

# Instalar pacotes
pip install -r requirements.txt
```
---

## PASSO 3 — Rodar o servidor

### Modo simples (teste / uso interno)

```bash
cd /home/usuario/autosei
source venv/bin/activate
python app.py
```
# CMD ou Terminal VSCode
python app.py
     ou
Rode o arquivo run.bat na pasta --melhor forma

O site estará disponível em: **http://IP-DO-SERVIDOR:5000** --Mude o DNS do servidor para que possa alterar a url so site.

---

## PASSO 4 — Acessar pelo navegador

Abra qualquer navegador na rede interna e acesse:

```
http://IP-DO-SERVIDOR:5000

```
---

## Dicas e Observações

### Chrome sem display (headless)
O `config.py` já está configurado com `--headless=new`. Não é necessário
instalar Xvfb ou qualquer servidor gráfico.

### Múltiplos usuários simultâneos
Cada clique em "Executar" inicia uma thread separada no servidor, abrindo
uma instância do Chrome. Desta forma se vários usuários executarem ao mesmo tempo
o fluxo continua sem interferências e falhas.

### Segurança — uso interno
O sistema **não tem autenticação própria**. Se o servidor estiver
acessível pela intranet apenas, está adequado.

### Logs
Os logs do Selenium aparecem no terminal onde o servidor está rodando.

### Atualizar as listas de tipos
Edite `externos.py` ou `internos.py` — são arquivos Python. Reinicie o fluxo após salvar.

```
## Desenvolvido por

| Nome | GitHub | Contato |
|------|--------|---------|
| João Costa | [devJoaoVCosta](https://github.com/devJoaoVCosta) | jvinsef360@gmail.com |
| Samuel Alves | [SamukaAlves](https://github.com/SamukaAlves) | contatosamuel.lima23@gmail.com |
```
