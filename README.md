# Anycubic Slicer Next MCP

Two distribution packages are provided:

| Package | File | What it does | Setup required |
|---|---|---|---|
| **Lite** | `anycubic-slicer-next-lite.mcpb` | Local preset reading/editing only | Double-click, pick 2 folders |
| **Full** | `anycubic-slicer-mcp.zip` | Presets + cloud/printer control (ACE, status, experimental light) | Homebrew, Python 3.12, two venvs, a login token |

If you only want Claude to read and safely edit your slicer presets, **use Lite**. If you also want Claude to check your printer's status, control the ACE filament unit, or (experimentally) toggle the chamber light, use **Full**.

---

## 🇬🇧 English

### 1. What each package contains

#### Lite package — `anycubic-slicer-next-lite.mcpb`

A single-click Claude Desktop extension (MCPB format). Runs 100% locally, reads/writes only your Anycubic Slicer Next preset files on disk. No login, no internet access, no cloud account needed.

**17 tools:**

| Tool | What it does |
|---|---|
| `get_slicer_info` | Reports OS and whether the configured preset folders exist |
| `list_printer_profiles` | Lists all machine (printer) presets |
| `list_filament_profiles` | Lists all filament presets |
| `list_process_profiles` | Lists all process (print profile) presets |
| `get_profile` | Reads one preset exactly as stored on disk (raw, with its `inherits` chain unresolved) |
| `resolve_profile` | Resolves the full `inherits` chain and returns the final effective values — what the slicer would actually use |
| `get_setting` | Looks up a single setting's effective value in a preset |
| `get_settings` | Looks up several related settings at once (e.g. all `support_*` keys) |
| `compare_profiles` | Diffs two presets of the same type and shows only what differs |
| `resolve_pt_name` | Translates a Portuguese UI label (e.g. "Número de paredes") into its JSON key (`wall_loops`) |
| `propose_profile_patch` | Calculates a proposed change and shows a before/after diff — writes nothing |
| `validate_profile_patch` | Checks a proposed change against a semantic catalog (correct namespace, known limits) |
| `clone_profile` | Creates a new user preset inheriting from an existing one |
| `apply_profile_patch` | Writes a change to a user preset, with an automatic timestamped backup |
| `rollback_profile` | Restores the previous version of a preset from its last backup |
| `export_anycubic_bundle` | Packages presets into an `.anycubic_printer`/`.orca_printer` bundle (zip) for re-import |
| `import_anycubic_bundle` | Reads and validates an exported bundle without installing it |

Every write action follows a strict **READ → PROPOSE → (your confirmation) → APPLY** flow; nothing is changed on disk without you seeing a diff first, and every change can be rolled back.

#### Full package — `anycubic-slicer-mcp.zip`

Everything in Lite, **plus** two additional MCP servers:

**Cloud control (`anycubic-cloud`, via the third-party `anycubic-cloud-mcp` package) — 19 tools:**

| Tool | What it does |
|---|---|
| `auth_set` / `auth_status` | Configure/check cloud authentication |
| `printer_list` | List printers linked to your Anycubic account |
| `printer_status` | Live status: online/offline, temperatures, firmware, current job |
| `print_pause` / `print_resume` / `print_cancel` | Control an in-progress print |
| `cloud_file_list` / `cloud_file_upload` / `cloud_file_delete` | Manage files stored in Anycubic Cloud |
| `print_upload` / `print_cloud_gcode` / `print_cloud_file` | Start a print from a local file or from the cloud |
| `ace_set_slot` | Set a filament slot's color/material in the ACE unit |
| `ace_feed_filament` / `ace_retract_filament` | Feed or retract filament through the ACE |
| `ace_set_auto_feed` | Toggle ACE auto-feed |
| `ace_dry_start` / `ace_dry_stop` | Start/stop the ACE filament dryer |

⚠️ **Known limitation:** if authenticated in `web` mode (required when the Slicer Next config file is encrypted — see installation notes below), live peripheral status (camera/ACE/USB physically connected) is **not** reflected accurately, because that specific field only updates via a real-time MQTT connection that `web` mode doesn't establish. Printer name, online status, temperatures, and firmware still work correctly.

**Experimental light control (`anycubic-light-experimental`) — 2 tools, UNVERIFIED on real hardware:**

| Tool | What it does |
|---|---|
| `get_light_capabilities` | Checks whether your printer's firmware *announces* support for chamber/video light — read-only, safe |
| `set_chamber_light` | Attempts to turn the chamber light on/off. The underlying command exists in the protocol but was marked "unused/untested" in the source library. This tool always reports whether the command was *sent*, separately from whether it was *confirmed* — confirmation requires a live MQTT connection this build doesn't yet have |

Camera control was investigated and **deliberately excluded**: the protocol's `CAMERA_OPEN` command returns AWS-style temporary credentials, but nothing in the available source code links those credentials to a specific Kinesis Video Streams channel — making it unusable without reverse-engineering traffic from the official Anycubic app.

### 2. Installing the Lite package

1. Download `anycubic-slicer-next-lite.mcpb`.
2. Double-click it (or drag it onto the Claude Desktop window, or go to **Settings → Extensions → Advanced settings → Install Extension…**).
3. Claude Desktop shows an install screen asking for two folders:
   - **System presets folder** — pre-filled with the standard macOS path (`~/Library/Application Support/AnycubicSlicerNext/system/Anycubic`). Leave as-is unless your install is elsewhere.
   - **Your presets folder** — click "Choose folder" and browse to `~/Library/Application Support/AnycubicSlicerNext/user/<your numeric ID>/` (find your ID by opening that `user` folder in Finder — it's the only subfolder there).
4. Click **Install**.
5. Start a **new** conversation and try: *"List my process profiles"* or *"Resolve profile X and show me its support settings."*

No Python, no Homebrew, no terminal commands. Claude Desktop's built-in **UV runtime** downloads whatever it needs automatically the first time it runs (a few seconds).

### 3. Installing the Full package

This requires manual setup because the cloud portion depends on third-party Python packages that need a proper Python 3.10+ environment, plus your own Anycubic account login token.

**Step 1 — Prerequisites**
```bash
# Install Homebrew if you don't have it:
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
# Follow its on-screen instructions to add it to your PATH, then:
brew install python@3.12
```

**Step 2 — Unpack and set up two isolated environments**

Two separate virtual environments are required because the presets server needs a newer `mcp` library (2.x) while `anycubic-cloud-mcp` needs an older one (1.x) — they conflict if installed together.

```bash
unzip anycubic-slicer-mcp.zip -d ~/anycubic-slicer-mcp
cd ~/anycubic-slicer-mcp

PYTHON312="$(brew --prefix python@3.12)/bin/python3.12"

# Env 1: presets + experimental light (needs mcp 2.x)
$PYTHON312 -m venv .venv
.venv/bin/pip install --upgrade pip
.venv/bin/pip install -r requirements.txt
.venv/bin/pip install -r requirements-experimental.txt

# Env 2: cloud control (needs mcp 1.x)
$PYTHON312 -m venv .venv-cloud
.venv-cloud/bin/pip install --upgrade pip
.venv-cloud/bin/pip install "mcp<2" anycubic-cloud-mcp
```

**Step 3 — Get a login token**

Two possible sources, try in this order:

- *Preferred (enables real-time MQTT status):* `.venv/bin/anycubic-slicer-token` — extracts it from your local Anycubic Slicer Next config. **This fails if your Slicer Next version encrypts that file** (as of writing, current versions do) — you'll get an `access_token not found` error.
- *Fallback (works today, REST-only, no real-time MQTT status):*
  1. Go to `https://cloud-universe.anycubic.com/file` and log in.
  2. Open DevTools (⌥⌘I on Chrome/Safari) → Console tab.
  3. Run: `window.localStorage["XX-Token"]`
  4. Copy the returned string — that's your token. Keep it private.

**Step 4 — Configure Claude Desktop**

Open `~/Library/Application Support/Claude/claude_desktop_config.json` and set:

```json
{
  "mcpServers": {
    "anycubic-slicer-next": {
      "command": "/Users/<you>/anycubic-slicer-mcp/.venv/bin/python3",
      "args": ["-m", "asn_mcp.server"],
      "env": {
        "PYTHONPATH": "/Users/<you>/anycubic-slicer-mcp",
        "ASN_SYSTEM_DIR": "/Users/<you>/Library/Application Support/AnycubicSlicerNext/system/Anycubic",
        "ASN_USER_DIR": "/Users/<you>/Library/Application Support/AnycubicSlicerNext/user/<your id>"
      }
    },
    "anycubic-cloud": {
      "command": "/Users/<you>/anycubic-slicer-mcp/.venv-cloud/bin/anycubic-cloud-mcp",
      "env": {
        "ANYCUBIC_AUTH_MODE": "web",
        "ANYCUBIC_TOKEN": "<paste your token here>"
      }
    },
    "anycubic-light-experimental": {
      "command": "/Users/<you>/anycubic-slicer-mcp/.venv/bin/python3",
      "args": ["-m", "asn_mcp.experimental.light_server"],
      "env": {
        "PYTHONPATH": "/Users/<you>/anycubic-slicer-mcp",
        "ANYCUBIC_AUTH_MODE": "web",
        "ANYCUBIC_TOKEN": "<paste your token here>"
      }
    }
  }
}
```

Use `"slicer"` instead of `"web"` for `ANYCUBIC_AUTH_MODE` if Step 3's preferred method worked for you — that mode supports real-time MQTT status.

**Step 5 — Restart and test**

Quit Claude Desktop completely (⌘Q) and reopen it. In a new conversation, try: *"List my printers"*, *"Show my process profiles"*, *"Check my printer's light capabilities."*

---

## 🇧🇷 Português

### 1. O que cada pacote contém

#### Pacote Lite — `anycubic-slicer-next-lite.mcpb`

Uma extensão de instalação em um clique para o Claude Desktop (formato MCPB). Roda 100% localmente, lê/grava só os arquivos de preset do Anycubic Slicer Next no seu disco. Sem login, sem acesso à internet, sem conta na nuvem.

**17 tools:**

| Tool | O que faz |
|---|---|
| `get_slicer_info` | Informa o sistema operacional e se as pastas de preset configuradas existem |
| `list_printer_profiles` | Lista todos os presets de máquina (impressora) |
| `list_filament_profiles` | Lista todos os presets de filamento |
| `list_process_profiles` | Lista todos os presets de processo (perfil de impressão) |
| `get_profile` | Lê um preset exatamente como gravado em disco (bruto, sem resolver `inherits`) |
| `resolve_profile` | Resolve toda a cadeia de `inherits` e retorna os valores efetivos finais — o que o slicer realmente usaria |
| `get_setting` | Consulta o valor efetivo de uma única configuração em um preset |
| `get_settings` | Consulta várias configurações relacionadas de uma vez (ex.: todas as chaves `support_*`) |
| `compare_profiles` | Compara dois presets do mesmo tipo e mostra só o que difere |
| `resolve_pt_name` | Traduz um nome da interface em português (ex.: "Número de paredes") para a chave JSON (`wall_loops`) |
| `propose_profile_patch` | Calcula uma mudança proposta e mostra o antes/depois — não grava nada |
| `validate_profile_patch` | Verifica uma mudança proposta contra um catálogo semântico (namespace correto, limites conhecidos) |
| `clone_profile` | Cria um novo preset de usuário herdando de um existente |
| `apply_profile_patch` | Grava uma mudança em um preset de usuário, com backup automático com timestamp |
| `rollback_profile` | Restaura a versão anterior de um preset a partir do último backup |
| `export_anycubic_bundle` | Empacota presets em um bundle `.anycubic_printer`/`.orca_printer` (zip) para reimportar |
| `import_anycubic_bundle` | Lê e valida um bundle exportado sem instalá-lo |

Toda ação de escrita segue rigorosamente o fluxo **READ → PROPOSE → (sua confirmação) → APPLY**; nada é alterado em disco sem você ver o diff antes, e toda mudança pode ser desfeita.

#### Pacote Full — `anycubic-slicer-mcp.zip`

Tudo do Lite, **mais** dois servidores MCP adicionais:

**Controle via nuvem (`anycubic-cloud`, via o pacote de terceiros `anycubic-cloud-mcp`) — 19 tools:**

| Tool | O que faz |
|---|---|
| `auth_set` / `auth_status` | Configura/verifica a autenticação na nuvem |
| `printer_list` | Lista as impressoras vinculadas à sua conta Anycubic |
| `printer_status` | Status ao vivo: online/offline, temperaturas, firmware, trabalho atual |
| `print_pause` / `print_resume` / `print_cancel` | Controla uma impressão em andamento |
| `cloud_file_list` / `cloud_file_upload` / `cloud_file_delete` | Gerencia arquivos armazenados na Anycubic Cloud |
| `print_upload` / `print_cloud_gcode` / `print_cloud_file` | Inicia uma impressão a partir de um arquivo local ou da nuvem |
| `ace_set_slot` | Define cor/material de um slot de filamento no ACE |
| `ace_feed_filament` / `ace_retract_filament` | Alimenta ou retrai filamento pelo ACE |
| `ace_set_auto_feed` | Liga/desliga a alimentação automática do ACE |
| `ace_dry_start` / `ace_dry_stop` | Inicia/para a secagem de filamento do ACE |

⚠️ **Limitação conhecida:** se autenticado no modo `web` (necessário quando o arquivo de configuração do Slicer Next está criptografado — ver instruções de instalação abaixo), o status de periféricos ao vivo (câmera/ACE/USB fisicamente conectados) **não** aparece corretamente, porque esse campo específico só é atualizado via uma conexão MQTT em tempo real que o modo `web` não estabelece. Nome da impressora, status online, temperaturas e firmware continuam funcionando normalmente.

**Controle experimental de luz (`anycubic-light-experimental`) — 2 tools, NÃO VALIDADO em hardware real:**

| Tool | O que faz |
|---|---|
| `get_light_capabilities` | Verifica se o firmware da sua impressora *anuncia* suporte a luz de câmara/vídeo — somente leitura, seguro |
| `set_chamber_light` | Tenta ligar/desligar a luz da câmara. O comando existe no protocolo, mas está marcado como "não usado/não testado" na biblioteca de origem. Esta tool sempre informa se o comando foi *enviado*, separado de se foi *confirmado* — a confirmação exige uma conexão MQTT ao vivo que esta versão ainda não tem |

O controle de câmera foi investigado e **excluído deliberadamente**: o comando `CAMERA_OPEN` do protocolo retorna credenciais temporárias estilo AWS, mas nada no código-fonte disponível liga essas credenciais a um canal específico do Kinesis Video Streams — tornando-o inutilizável sem engenharia reversa do tráfego do app oficial da Anycubic.

### 2. Instalando o pacote Lite

1. Baixe `anycubic-slicer-next-lite.mcpb`.
2. Dê duplo clique nele (ou arraste para a janela do Claude Desktop, ou vá em **Configurações → Extensões → Configurações avançadas → Instalar extensão…**).
3. O Claude Desktop mostra uma tela de instalação pedindo duas pastas:
   - **Pasta de presets do sistema** — já vem preenchida com o caminho padrão do macOS (`~/Library/Application Support/AnycubicSlicerNext/system/Anycubic`). Deixe como está, a não ser que sua instalação seja diferente.
   - **Pasta dos seus presets** — clique em "Escolher pasta" e navegue até `~/Library/Application Support/AnycubicSlicerNext/user/<seu ID numérico>/` (encontre seu ID abrindo essa pasta `user` no Finder — é a única subpasta lá dentro).
4. Clique em **Instalar**.
5. Comece uma conversa **nova** e teste: *"Liste meus process profiles"* ou *"Resolva o perfil X e me mostre as configurações de suporte."*

Sem Python, sem Homebrew, sem comandos de terminal. O runtime **UV** embutido no Claude Desktop baixa o que for necessário automaticamente na primeira execução (leva alguns segundos).

### 3. Instalando o pacote Full

Isso exige configuração manual porque a parte de nuvem depende de pacotes Python de terceiros que precisam de um ambiente Python 3.10+ de verdade, além do seu próprio token de login da conta Anycubic.

**Passo 1 — Pré-requisitos**
```bash
# Instale o Homebrew se ainda não tiver:
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
# Siga as instruções na tela para adicioná-lo ao PATH, depois:
brew install python@3.12
```

**Passo 2 — Descompactar e configurar dois ambientes isolados**

São necessários dois ambientes virtuais separados porque o servidor de presets precisa de uma versão mais nova da biblioteca `mcp` (2.x), enquanto o `anycubic-cloud-mcp` precisa de uma mais antiga (1.x) — eles conflitam se instalados juntos.

```bash
unzip anycubic-slicer-mcp.zip -d ~/anycubic-slicer-mcp
cd ~/anycubic-slicer-mcp

PYTHON312="$(brew --prefix python@3.12)/bin/python3.12"

# Ambiente 1: presets + luz experimental (precisa do mcp 2.x)
$PYTHON312 -m venv .venv
.venv/bin/pip install --upgrade pip
.venv/bin/pip install -r requirements.txt
.venv/bin/pip install -r requirements-experimental.txt

# Ambiente 2: controle via nuvem (precisa do mcp 1.x)
$PYTHON312 -m venv .venv-cloud
.venv-cloud/bin/pip install --upgrade pip
.venv-cloud/bin/pip install "mcp<2" anycubic-cloud-mcp
```

**Passo 3 — Conseguir um token de login**

Duas fontes possíveis, tente nesta ordem:

- *Preferencial (habilita status em tempo real via MQTT):* `.venv/bin/anycubic-slicer-token` — extrai do config local do seu Anycubic Slicer Next. **Isso falha se sua versão do Slicer Next criptografar esse arquivo** (no momento em que isso foi escrito, versões atuais criptografam) — você vai receber um erro `access_token not found`.
- *Alternativa (funciona hoje, só REST, sem status em tempo real):*
  1. Acesse `https://cloud-universe.anycubic.com/file` e faça login.
  2. Abra o DevTools (⌥⌘I no Chrome/Safari) → aba Console.
  3. Rode: `window.localStorage["XX-Token"]`
  4. Copie a string retornada — esse é o seu token. Mantenha em segredo.

**Passo 4 — Configurar o Claude Desktop**

Abra `~/Library/Application Support/Claude/claude_desktop_config.json` e defina:

```json
{
  "mcpServers": {
    "anycubic-slicer-next": {
      "command": "/Users/<voce>/anycubic-slicer-mcp/.venv/bin/python3",
      "args": ["-m", "asn_mcp.server"],
      "env": {
        "PYTHONPATH": "/Users/<voce>/anycubic-slicer-mcp",
        "ASN_SYSTEM_DIR": "/Users/<voce>/Library/Application Support/AnycubicSlicerNext/system/Anycubic",
        "ASN_USER_DIR": "/Users/<voce>/Library/Application Support/AnycubicSlicerNext/user/<seu id>"
      }
    },
    "anycubic-cloud": {
      "command": "/Users/<voce>/anycubic-slicer-mcp/.venv-cloud/bin/anycubic-cloud-mcp",
      "env": {
        "ANYCUBIC_AUTH_MODE": "web",
        "ANYCUBIC_TOKEN": "<cole seu token aqui>"
      }
    },
    "anycubic-light-experimental": {
      "command": "/Users/<voce>/anycubic-slicer-mcp/.venv/bin/python3",
      "args": ["-m", "asn_mcp.experimental.light_server"],
      "env": {
        "PYTHONPATH": "/Users/<voce>/anycubic-slicer-mcp",
        "ANYCUBIC_AUTH_MODE": "web",
        "ANYCUBIC_TOKEN": "<cole seu token aqui>"
      }
    }
  }
}
```

Use `"slicer"` em vez de `"web"` no `ANYCUBIC_AUTH_MODE` se o método preferencial do Passo 3 funcionou pra você — esse modo suporta status em tempo real via MQTT.

**Passo 5 — Reiniciar e testar**

Feche o Claude Desktop completamente (⌘Q) e abra de novo. Numa conversa nova, teste: *"Liste minhas impressoras"*, *"Mostre meus process profiles"*, *"Verifique as capacidades de luz da minha impressora."*

---

## Uploading these files to your own GitHub repo / Subindo esses arquivos pro seu próprio GitHub

*No command line needed — this is entirely through the browser.*
*Sem necessidade de linha de comando — isso é feito inteiramente pelo navegador.*

1. Go to **github.com** → click the **+** icon (top right) → **New repository**.
2. Give it a name (e.g. `anycubic-slicer-mcp`), choose Public or Private, click **Create repository**.
3. On the new repo's page, click **"uploading an existing file"** (or **Add file → Upload files**).
4. Drag in these three files: `anycubic-slicer-mcp.zip`, `anycubic-slicer-next-lite.mcpb`, and this `README.md`.
5. Scroll down, click **Commit changes**.

Done — anyone with the repo link can now download both packages and read the install instructions.
