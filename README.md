 Anycubic Slicer Next MCP — Claude conectado à sua impressora 3D

## 🇧🇷 O que é isto?

### O problema que isso resolve

Quem imprime em 3D sabe: quando uma peça sai errada — suporte grudado demais, primeira camada mal aderida, peça frágil, *stringing*, acabamento ruim — o culpado quase sempre está escondido em algum parâmetro do slicer que a maioria das pessoas nunca abriu, nem sabe que existe. E mesmo quando você sabe *qual* configuração mexer, raramente sabe *para qual valor* mudar sem tentativa e erro.

Este projeto conecta o Claude diretamente aos arquivos de configuração do seu Anycubic Slicer Next (ou OrcaSlicer). Na prática, isso transforma o Claude em um assistente técnico de impressão 3D que **realmente enxerga sua configuração de verdade** — não um chute genérico baseado em "geralmente funciona assim", mas o seu perfil, com seus valores, sua impressora, seu material. E agora ele também consegue **fatiar de verdade** e te dar números reais de tempo e consumo — não é mais só sugestão qualitativa.

### Como isso ajuda no dia a dia

**Se você não sabe onde mexer:** em vez de vasculhar dezenas de abas do programa procurando "aquela configuração de suporte", você só descreve o problema em português comum. O Claude traduz isso para a chave técnica certa, olha o valor atual no seu perfil, e explica o que está acontecendo.

**Se você é iniciante em impressão 3D:** não precisa saber o que é "Z-distance" ou "sparse infill pattern" de antemão. Você descreve o sintoma ("a peça quebrou fácil", "ficou uma coisa fiapenta saindo entre as partes", "o suporte não sai"), e o Claude relaciona isso com os parâmetros prováveis, explica o porquê, e sugere o ajuste.

**Fluxo típico (diagnóstico):**
1. Você tira uma foto da impressão com problema (ou simplesmente descreve).
2. Você conta o que aconteceu: "o suporte ficou impossível de tirar", "a peça está mole", "quero mais resistência mas sem gastar muito mais material".
3. O Claude consulta seu perfil real (não um genérico) e identifica os parâmetros relacionados.
4. Ele propõe uma mudança concreta, mostrando o antes/depois — nada é alterado sem você ver e aprovar.
5. Se você aprovar, a mudança é aplicada com backup automático — dá pra desfazer a qualquer momento.

**Fluxo típico (planejamento, com fatiamento real):**
1. Você envia o arquivo 3D (STL/3MF) e descreve o objetivo: "isso é um chaveiro, vai no bolso, quero mais resistência, será em PLA."
2. O Claude analisa a geometria e propõe 2-3 combinações de preset candidatas.
3. Ele **fatia de verdade** cada uma (não estima) e mostra números reais: gramas de filamento, tempo de impressão.
4. Você escolhe com base em dados reais, não em suposição.

**Também serve para o caminho inverso:** se você já manja de impressão 3D e só quer economia de tempo, pode pedir comparações entre dois perfis, exportar/importar pacotes de configuração, ou ajustar parâmetros específicos diretamente, sem precisar navegar pela interface do programa.

### O que isso ainda não faz

Não substitui bom senso: se o problema for mecânico (nivelamento da mesa, calibração do bico), o Claude aponta isso em vez de tentar "consertar" via configuração de software algo que é um problema de hardware. Também não controla a impressora em tempo real além do que a nuvem oficial da Anycubic permite (ver limitações na seção técnica).

---

## 🇬🇧 What is this?

### The problem this solves

Anyone who prints in 3D knows the drill: when a print comes out wrong — support stuck too hard, poor first-layer adhesion, a part that's too fragile, stringing, rough finish — the culprit is almost always hiding in some slicer parameter most people have never opened, or don't even know exists. And even when you know *which* setting to touch, you rarely know *what value* to change it to without trial and error.

This project connects Claude directly to your Anycubic Slicer Next (or OrcaSlicer) configuration files. In practice, that turns Claude into a 3D-printing technical assistant that **actually sees your real configuration** — not a generic guess based on "this usually works," but your profile, your values, your printer, your material. And now it can also **really slice** and give you real time/material numbers — no longer just qualitative suggestions.

### How this helps day to day

**If you don't know where to look:** instead of digging through dozens of tabs hunting for "that one support setting," you just describe the problem in plain language. Claude translates that into the right technical key, checks the current value in your actual profile, and explains what's going on.

**If you're new to 3D printing:** you don't need to know what "Z-distance" or "sparse infill pattern" means beforehand. You describe the symptom ("the part broke too easily," "there's stringy stuff between the parts," "the support won't come off"), and Claude connects that to the likely parameters, explains why, and suggests the fix.

**Typical flow (diagnosis):**
1. You take a photo of the problem print (or just describe it).
2. You explain what happened: "the support was impossible to remove," "the part feels weak," "I want more strength without using much more material."
3. Claude checks your actual profile (not a generic one) and identifies the related parameters.
4. It proposes a concrete change, showing before/after — nothing changes without you seeing and approving it.
5. If you approve, the change is applied with an automatic backup — you can undo it at any time.

**Typical flow (planning, with real slicing):**
1. You send the 3D file (STL/3MF) and describe the goal: "this is a keychain, it goes in a pocket, I want more strength, it'll be PLA."
2. Claude analyzes the geometry and proposes 2–3 candidate preset combinations.
3. It **actually slices** each one (not an estimate) and shows real numbers: grams of filament, print time.
4. You choose based on real data, not guesswork.

**It also works the other way:** if you already know 3D printing well and just want to save time, you can ask for comparisons between two profiles, export/import configuration bundles, or tweak specific parameters directly, without navigating the program's UI at all.

### What this still doesn't do

It doesn't replace good judgment: if the problem is mechanical (bed leveling, nozzle calibration), Claude will point that out instead of trying to "fix" a hardware problem through software settings. It also doesn't control the printer in real time beyond what Anycubic's official cloud allows (see limitations in the technical section).

---

## Perguntas frequentes / FAQ

### 🇧🇷 (1) Isso funciona com OrcaSlicer também?

**Sim, tecnicamente já funciona** — e a razão é simples: o Anycubic Slicer Next é um **fork direto** do OrcaSlicer, usando exatamente o mesmo formato de arquivos de preset (JSON, mesma lógica de herança `inherits`, mesmo formato de bundle `.orca_printer`) e, como confirmamos agora, **a mesma interface de linha de comando para fatiamento real**.

Temos um repositório separado, dedicado só ao OrcaSlicer — veja o link no topo — com um pacote `.mcpb` próprio para instalar em um clique.

### 🇧🇷 (2) Dá pra mandar o arquivo 3D (STL/3MF) e pedir configurações pra um objetivo específico (ex.: "isso é um chaveiro, quero mais resistência, será impresso em PLA")?

**Sim — e isso já está construído e testado, não é mais um plano futuro.** O Claude consegue: analisar a geometria do arquivo (dimensões, se a malha está fechada corretamente), fatiar de verdade com um ou mais presets candidatos usando o próprio executável do Anycubic Slicer Next em modo silencioso (sem abrir a interface), e te dar números reais de tempo de impressão e consumo de filamento — lidos diretamente do G-code gerado, não estimados. Isso permite comparar de verdade duas ou três configurações ("essa gasta 20g e leva 45min, essa outra gasta 24g mas é mais resistente") antes de você decidir. Veja a tool `slice_model` e `compare_slice_configs` na documentação técnica abaixo.

### 🇬🇧 (1) Does this work with OrcaSlicer too?

**Yes, it technically already does** — the reason is simple: Anycubic Slicer Next is a **direct fork** of OrcaSlicer, using the exact same preset file format (JSON, same `inherits` inheritance logic, same `.orca_printer` bundle format) and, as we've now confirmed, **the same command-line interface for real slicing**.

We have a separate repository dedicated just to OrcaSlicer — see the link at the top — with its own one-click `.mcpb` package.

### 🇬🇧 (2) Can I send a 3D file (STL/3MF) and ask for settings tailored to a specific goal (e.g. "this is a keychain, I want more strength, it'll be printed in PLA")?

**Yes — and this is already built and tested, not a future plan anymore.** Claude can: analyze the file's geometry (dimensions, whether the mesh is properly closed), actually slice it with one or more candidate presets using the real Anycubic Slicer Next executable in headless mode (no GUI), and give you real print-time and filament-usage numbers — read straight from the generated G-code, not estimated. This lets you genuinely compare two or three configurations ("this one uses 20g and takes 45min, this other one uses 24g but is stronger") before deciding. See the `slice_model` and `compare_slice_configs` tools in the technical documentation below.

---

# Documentação técnica / Technical documentation

Two distribution packages are provided:

| Package  | File                             | What it does                                                                     | Setup required                                  |
| -------- | -------------------------------- | -------------------------------------------------------------------------------- | ----------------------------------------------- |
| **Lite** | `anycubic-slicer-next-lite.mcpb` | Local preset reading/editing only                                                | Double-click, pick 2 folders                    |
| **Full** | `anycubic-slicer-mcp.zip`        | Presets + real slicing + cloud/printer control (ACE, status, experimental light) | Homebrew, Python 3.12, two venvs, a login token |

If you only want Claude to read and safely edit your slicer presets, **use Lite**. If you also want real slicing, printer status, ACE control, or (experimentally) chamber light control, use **Full**.

## 🇬🇧 English

### 1. What each package contains

#### Lite package — `anycubic-slicer-next-lite.mcpb`

A single-click Claude Desktop extension (MCPB format). Runs 100% locally, reads/writes only your Anycubic Slicer Next preset files on disk. No login, no internet access, no cloud account needed.

**17 tools:**

| Tool                     | What it does                                                                                                   |
| ------------------------ | -------------------------------------------------------------------------------------------------------------- |
| `get_slicer_info`        | Reports OS and whether the configured preset folders exist                                                     |
| `list_printer_profiles`  | Lists all machine (printer) presets                                                                            |
| `list_filament_profiles` | Lists all filament presets                                                                                     |
| `list_process_profiles`  | Lists all process (print profile) presets                                                                      |
| `get_profile`            | Reads one preset exactly as stored on disk (raw, with its `inherits` chain unresolved)                         |
| `resolve_profile`        | Resolves the full `inherits` chain and returns the final effective values — what the slicer would actually use |
| `get_setting`            | Looks up a single setting's effective value in a preset                                                        |
| `get_settings`           | Looks up several related settings at once (e.g. all `support_*` keys)                                          |
| `compare_profiles`       | Diffs two presets of the same type and shows only what differs                                                 |
| `resolve_pt_name`        | Translates a Portuguese UI label (e.g. "Número de paredes") into its JSON key (`wall_loops`)                   |
| `propose_profile_patch`  | Calculates a proposed change and shows a before/after diff — writes nothing                                    |
| `validate_profile_patch` | Checks a proposed change against a semantic catalog (correct namespace, known limits)                          |
| `clone_profile`          | Creates a new user preset inheriting from an existing one                                                      |
| `apply_profile_patch`    | Writes a change to a user preset, with an automatic timestamped backup                                         |
| `rollback_profile`       | Restores the previous version of a preset from its last backup                                                 |
| `export_anycubic_bundle` | Packages presets into an `.anycubic_printer`/`.orca_printer` bundle (zip) for re-import                        |
| `import_anycubic_bundle` | Reads and validates an exported bundle without installing it                                                   |

Every write action follows a strict **READ → PROPOSE → (your confirmation) → APPLY** flow; nothing is changed on disk without you seeing a diff first, and every change can be rolled back.

#### Full package — `anycubic-slicer-mcp.zip`

Everything in Lite, **plus real slicing** and two additional MCP servers:

**Real slicing (same `anycubic-slicer-next` server) — 3 tools, validated end-to-end:**

| Tool                    | What it does                                                                                                                  |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `get_model_info`        | Reads dimensions, triangle count, and whether the mesh is manifold from an STL/3MF — no slicing                               |
| `slice_model`           | Actually slices a file with real presets (by name), returns real print time and filament usage read from the generated G-code |
| `compare_slice_configs` | Slices the same part with several preset combinations and returns the results side by side                                    |

This calls the real Anycubic Slicer Next executable in headless mode (`--slice`, `--load-settings`, `--export-3mf` — confirmed present in the actual app via `--help`), using a disposable temporary folder each time — it never touches your active profile or an open session of the app. Requires one more environment variable: `ASN_BINARY_PATH`, pointing at the executable inside the `.app` (see installation step 4). Print time and filament usage are parsed from text comments in the generated G-code, whose exact format isn't officially documented — validated against OrcaSlicer 2.4.2; real slicing hasn't been re-tested against every Anycubic Slicer Next version yet.

**Cloud control (`anycubic-cloud`, via the third-party `anycubic-cloud-mcp` package) — 19 tools:**

| Tool                                                          | What it does                                                     |
| ------------------------------------------------------------- | ---------------------------------------------------------------- |
| `auth_set` / `auth_status`                                    | Configure/check cloud authentication                             |
| `printer_list`                                                | List printers linked to your Anycubic account                    |
| `printer_status`                                              | Live status: online/offline, temperatures, firmware, current job |
| `print_pause` / `print_resume` / `print_cancel`               | Control an in-progress print                                     |
| `cloud_file_list` / `cloud_file_upload` / `cloud_file_delete` | Manage files stored in Anycubic Cloud                            |
| `print_upload` / `print_cloud_gcode` / `print_cloud_file`     | Start a print from a local file or from the cloud                |
| `ace_set_slot`                                                | Set a filament slot's color/material in the ACE unit             |
| `ace_feed_filament` / `ace_retract_filament`                  | Feed or retract filament through the ACE                         |
| `ace_set_auto_feed`                                           | Toggle ACE auto-feed                                             |
| `ace_dry_start` / `ace_dry_stop`                              | Start/stop the ACE filament dryer                                |

⚠️ **Known limitation:** if authenticated in `web` mode (required when the Slicer Next config file is encrypted — see installation notes below), live peripheral status (camera/ACE/USB physically connected) is **not** reflected accurately, because that specific field only updates via a real-time MQTT connection that `web` mode doesn't establish. Printer name, online status, temperatures, and firmware still work correctly.

**Experimental light control (`anycubic-light-experimental`) — 2 tools, UNVERIFIED on real hardware:**

| Tool                     | What it does                                                                                                                                                                                                                                                                                                                    |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `get_light_capabilities` | Checks whether your printer's firmware *announces* support for chamber/video light — read-only, safe                                                                                                                                                                                                                            |
| `set_chamber_light`      | Attempts to turn the chamber light on/off. The underlying command exists in the protocol but was marked "unused/untested" in the source library. This tool always reports whether the command was *sent*, separately from whether it was *confirmed* — confirmation requires a live MQTT connection this build doesn't yet have |

Camera control was investigated and **deliberately excluded**: the protocol's `CAMERA_OPEN` command returns AWS-style temporary credentials, but nothing in the available source code links those credentials to a specific Kinesis Video Streams channel — making it unusable without reverse-engineering traffic from the official Anycubic app.

### 2. Installing the Lite package

1. Download `anycubic-slicer-next-lite.mcpb`.
2. Double-click it (or drag it onto the Claude Desktop window, or go to **Settings → Extensions → Advanced settings → Install Extension…**).
3. Claude Desktop shows an install screen asking for two folders:
   4. **System presets folder** — pre-filled with the standard macOS path (`~/Library/Application Support/AnycubicSlicerNext/system/Anycubic`). Leave as-is unless your install is elsewhere.
   5. **Your presets folder** — click "Choose folder" and browse to `~/Library/Application Support/AnycubicSlicerNext/user/<your numeric ID>/` (find your ID by opening that `user` folder in Finder — it's the only subfolder there).
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

Two separate virtual environments are required because the presets/slicing server needs a newer `mcp` library (2.x) while `anycubic-cloud-mcp` needs an older one (1.x) — they conflict if installed together.

```bash
unzip anycubic-slicer-mcp.zip -d ~/anycubic-slicer-mcp
cd ~/anycubic-slicer-mcp

PYTHON312="$(brew --prefix python@3.12)/bin/python3.12"

# Env 1: presets + real slicing + experimental light (needs mcp 2.x)
$PYTHON312 -m venv .venv
.venv/bin/pip install --upgrade pip
.venv/bin/pip install -r requirements.txt
.venv/bin/pip install -r requirements-experimental.txt

# Env 2: cloud control (needs mcp 1.x)
$PYTHON312 -m venv .venv-cloud
.venv-cloud/bin/pip install --upgrade pip
.venv-cloud/bin/pip install "mcp<2" anycubic-cloud-mcp
```

**Step 3 — Get a login token** (only needed for cloud control and light — skip if you only want presets + slicing)

Two possible sources, try in this order:

- *Preferred (enables real-time MQTT status):* `.venv/bin/anycubic-slicer-token` — extracts it from your local Anycubic Slicer Next config. **This fails if your Slicer Next version encrypts that file** (as of writing, current versions do) — you'll get an `access_token not found` error.
- *Fallback (works today, REST-only, no real-time MQTT status):*
  - Go to `https://cloud-universe.anycubic.com/file` and log in.
  - Open DevTools (⌥⌘I on Chrome/Safari) → Console tab.
  - Run: `window.localStorage["XX-Token"]`
  - Copy the returned string — that's your token. Keep it private.

**Step 4 — Configure Claude Desktop**

Open `~/Library/Application Support/Claude/claude_desktop_config.json` and set (find your exact executable name with `ls "/Applications/AnycubicSlicerNext.app/Contents/MacOS/"` — it may not match exactly):

```json
{
  "mcpServers": {
    "anycubic-slicer-next": {
      "command": "/Users/<you>/anycubic-slicer-mcp/.venv/bin/python3",
      "args": ["-m", "asn_mcp.server"],
      "env": {
        "PYTHONPATH": "/Users/<you>/anycubic-slicer-mcp",
        "ASN_SYSTEM_DIR": "/Users/<you>/Library/Application Support/AnycubicSlicerNext/system/Anycubic",
        "ASN_USER_DIR": "/Users/<you>/Library/Application Support/AnycubicSlicerNext/user/<your id>",
        "ASN_BINARY_PATH": "/Applications/AnycubicSlicerNext.app/Contents/MacOS/AnycubicSlicerNext"
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

`ASN_BINARY_PATH` is only needed for `get_model_info`/`slice_model`/`compare_slice_configs` — omit it if you only want the preset tools. Use `"slicer"` instead of `"web"` for `ANYCUBIC_AUTH_MODE` if Step 3's preferred method worked for you — that mode supports real-time MQTT status.

**Step 5 — Restart and test**

Quit Claude Desktop completely (⌘Q) and reopen it. In a new conversation, try: *"List my printers"*, *"Show my process profiles"*, *"Get info on this STL file"*, *"Slice this model with my Chaveiro Reforçado profile and tell me the time and filament used"*.

**Known limitations of real slicing:**
- Each call uses a disposable temp folder — shouldn't conflict with an open session of the app, but that combination hasn't been tested.
- Complex parts can take much longer than a simple test cube; there's no configurable timeout on the Claude Desktop side yet, so a very long slice could time out.
- Time/filament parsing relies on undocumented G-code comment formats that could change between slicer versions.

## 🇧🇷 Português

### 1. O que cada pacote contém

#### Pacote Lite — `anycubic-slicer-next-lite.mcpb`

Uma extensão de instalação em um clique para o Claude Desktop (formato MCPB). Roda 100% localmente, lê/grava só os arquivos de preset do Anycubic Slicer Next no seu disco. Sem login, sem acesso à internet, sem conta na nuvem.

**17 tools:**

| Tool                     | O que faz                                                                                                  |
| ------------------------ | ---------------------------------------------------------------------------------------------------------- |
| `get_slicer_info`        | Informa o sistema operacional e se as pastas de preset configuradas existem                                |
| `list_printer_profiles`  | Lista todos os presets de máquina (impressora)                                                             |
| `list_filament_profiles` | Lista todos os presets de filamento                                                                        |
| `list_process_profiles`  | Lista todos os presets de processo (perfil de impressão)                                                   |
| `get_profile`            | Lê um preset exatamente como gravado em disco (bruto, sem resolver `inherits`)                             |
| `resolve_profile`        | Resolve toda a cadeia de `inherits` e retorna os valores efetivos finais — o que o slicer realmente usaria |
| `get_setting`            | Consulta o valor efetivo de uma única configuração em um preset                                            |
| `get_settings`           | Consulta várias configurações relacionadas de uma vez (ex.: todas as chaves `support_*`)                   |
| `compare_profiles`       | Compara dois presets do mesmo tipo e mostra só o que difere                                                |
| `resolve_pt_name`        | Traduz um nome da interface em português (ex.: "Número de paredes") para a chave JSON (`wall_loops`)       |
| `propose_profile_patch`  | Calcula uma mudança proposta e mostra o antes/depois — não grava nada                                      |
| `validate_profile_patch` | Verifica uma mudança proposta contra um catálogo semântico (namespace correto, limites conhecidos)         |
| `clone_profile`          | Cria um novo preset de usuário herdando de um existente                                                    |
| `apply_profile_patch`    | Grava uma mudança em um preset de usuário, com backup automático com timestamp                             |
| `rollback_profile`       | Restaura a versão anterior de um preset a partir do último backup                                          |
| `export_anycubic_bundle` | Empacota presets em um bundle `.anycubic_printer`/`.orca_printer` (zip) para reimportar                    |
| `import_anycubic_bundle` | Lê e valida um bundle exportado sem instalá-lo                                                             |

Toda ação de escrita segue rigorosamente o fluxo **READ → PROPOSE → (sua confirmação) → APPLY**; nada é alterado em disco sem você ver o diff antes, e toda mudança pode ser desfeita.

#### Pacote Full — `anycubic-slicer-mcp.zip`

Tudo do Lite, **mais fatiamento real** e dois servidores MCP adicionais:

**Fatiamento real (mesmo servidor `anycubic-slicer-next`) — 3 tools, validadas de ponta a ponta:**

| Tool                    | O que faz                                                                                                                                  |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `get_model_info`        | Lê dimensões, contagem de triângulos e se a malha é manifold de um STL/3MF — sem fatiar                                                    |
| `slice_model`           | Fatia de verdade um arquivo com presets reais (pelo nome), retorna tempo de impressão e consumo de filamento reais, lidos do G-code gerado |
| `compare_slice_configs` | Fatia a mesma peça com várias combinações de preset e retorna os resultados lado a lado                                                    |

Isso chama o executável real do Anycubic Slicer Next em modo silencioso (`--slice`, `--load-settings`, `--export-3mf` — confirmados presentes no app real via `--help`), usando uma pasta temporária descartável a cada chamada — nunca toca no seu perfil ativo nem numa sessão aberta do programa. Exige mais uma variável de ambiente: `ASN_BINARY_PATH`, apontando pro executável dentro do `.app` (ver passo 4 da instalação). Tempo e consumo de filamento são extraídos de comentários de texto no G-code gerado, cujo formato exato não é documentado oficialmente — validado contra o OrcaSlicer 2.4.2; o fatiamento real ainda não foi retestado em toda versão do Anycubic Slicer Next.

**Controle via nuvem (`anycubic-cloud`, via o pacote de terceiros `anycubic-cloud-mcp`) — 19 tools:**

| Tool                                                          | O que faz                                                              |
| ------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `auth_set` / `auth_status`                                    | Configura/verifica a autenticação na nuvem                             |
| `printer_list`                                                | Lista as impressoras vinculadas à sua conta Anycubic                   |
| `printer_status`                                              | Status ao vivo: online/offline, temperaturas, firmware, trabalho atual |
| `print_pause` / `print_resume` / `print_cancel`               | Controla uma impressão em andamento                                    |
| `cloud_file_list` / `cloud_file_upload` / `cloud_file_delete` | Gerencia arquivos armazenados na Anycubic Cloud                        |
| `print_upload` / `print_cloud_gcode` / `print_cloud_file`     | Inicia uma impressão a partir de um arquivo local ou da nuvem          |
| `ace_set_slot`                                                | Define cor/material de um slot de filamento no ACE                     |
| `ace_feed_filament` / `ace_retract_filament`                  | Alimenta ou retrai filamento pelo ACE                                  |
| `ace_set_auto_feed`                                           | Liga/desliga a alimentação automática do ACE                           |
| `ace_dry_start` / `ace_dry_stop`                              | Inicia/para a secagem de filamento do ACE                              |

⚠️ **Limitação conhecida:** se autenticado no modo `web` (necessário quando o arquivo de configuração do Slicer Next está criptografado — ver instruções de instalação abaixo), o status de periféricos ao vivo (câmera/ACE/USB fisicamente conectados) **não** aparece corretamente, porque esse campo específico só é atualizado via uma conexão MQTT em tempo real que o modo `web` não estabelece. Nome da impressora, status online, temperaturas e firmware continuam funcionando normalmente.

**Controle experimental de luz (`anycubic-light-experimental`) — 2 tools, NÃO VALIDADO em hardware real:**

| Tool                     | O que faz                                                                                                                                                                                                                                                                                                     |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `get_light_capabilities` | Verifica se o firmware da sua impressora *anuncia* suporte a luz de câmara/vídeo — somente leitura, seguro                                                                                                                                                                                                    |
| `set_chamber_light`      | Tenta ligar/desligar a luz da câmara. O comando existe no protocolo, mas está marcado como "não usado/não testado" na biblioteca de origem. Esta tool sempre informa se o comando foi *enviado*, separado de se foi *confirmado* — a confirmação exige uma conexão MQTT ao vivo que esta versão ainda não tem |

O controle de câmera foi investigado e **excluído deliberadamente**: o comando `CAMERA_OPEN` do protocolo retorna credenciais temporárias estilo AWS, mas nada no código-fonte disponível liga essas credenciais a um canal específico do Kinesis Video Streams — tornando-o inutilizável sem engenharia reversa do tráfego do app oficial da Anycubic.

### 2. Instalando o pacote Lite

1. Baixe `anycubic-slicer-next-lite.mcpb`.
2. Dê duplo clique nele (ou arraste para a janela do Claude Desktop, ou vá em **Configurações → Extensões → Configurações avançadas → Instalar extensão…**).
3. O Claude Desktop mostra uma tela de instalação pedindo duas pastas:
   4. **Pasta de presets do sistema** — já vem preenchida com o caminho padrão do macOS (`~/Library/Application Support/AnycubicSlicerNext/system/Anycubic`). Deixe como está, a não ser que sua instalação seja diferente.
   5. **Pasta dos seus presets** — clique em "Escolher pasta" e navegue até `~/Library/Application Support/AnycubicSlicerNext/user/<seu ID numérico>/` (encontre seu ID abrindo essa pasta `user` no Finder — é a única subpasta lá dentro).
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

São necessários dois ambientes virtuais separados porque o servidor de presets/fatiamento precisa de uma versão mais nova da biblioteca `mcp` (2.x), enquanto o `anycubic-cloud-mcp` precisa de uma mais antiga (1.x) — eles conflitam se instalados juntos.

```bash
unzip anycubic-slicer-mcp.zip -d ~/anycubic-slicer-mcp
cd ~/anycubic-slicer-mcp

PYTHON312="$(brew --prefix python@3.12)/bin/python3.12"

# Ambiente 1: presets + fatiamento real + luz experimental (precisa do mcp 2.x)
$PYTHON312 -m venv .venv
.venv/bin/pip install --upgrade pip
.venv/bin/pip install -r requirements.txt
.venv/bin/pip install -r requirements-experimental.txt

# Ambiente 2: controle via nuvem (precisa do mcp 1.x)
$PYTHON312 -m venv .venv-cloud
.venv-cloud/bin/pip install --upgrade pip
.venv-cloud/bin/pip install "mcp<2" anycubic-cloud-mcp
```

**Passo 3 — Conseguir um token de login** (só necessário pra controle via nuvem e luz — pule se só quiser presets + fatiamento)

Duas fontes possíveis, tente nesta ordem:

- *Preferencial (habilita status em tempo real via MQTT):* `.venv/bin/anycubic-slicer-token` — extrai do config local do seu Anycubic Slicer Next. **Isso falha se sua versão do Slicer Next criptografar esse arquivo** (no momento em que isso foi escrito, versões atuais criptografam) — você vai receber um erro `access_token not found`.
- *Alternativa (funciona hoje, só REST, sem status em tempo real):*
  - Acesse `https://cloud-universe.anycubic.com/file` e faça login.
  - Abra o DevTools (⌥⌘I no Chrome/Safari) → aba Console.
  - Rode: `window.localStorage["XX-Token"]`
  - Copie a string retornada — esse é o seu token. Mantenha em segredo.

**Passo 4 — Configurar o Claude Desktop**

Abra `~/Library/Application Support/Claude/claude_desktop_config.json` e defina (confirme o nome exato do executável com `ls "/Applications/AnycubicSlicerNext.app/Contents/MacOS/"` — pode não bater exatamente):

```json
{
  "mcpServers": {
    "anycubic-slicer-next": {
      "command": "/Users/<voce>/anycubic-slicer-mcp/.venv/bin/python3",
      "args": ["-m", "asn_mcp.server"],
      "env": {
        "PYTHONPATH": "/Users/<voce>/anycubic-slicer-mcp",
        "ASN_SYSTEM_DIR": "/Users/<voce>/Library/Application Support/AnycubicSlicerNext/system/Anycubic",
        "ASN_USER_DIR": "/Users/<voce>/Library/Application Support/AnycubicSlicerNext/user/<seu id>",
        "ASN_BINARY_PATH": "/Applications/AnycubicSlicerNext.app/Contents/MacOS/AnycubicSlicerNext"
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

`ASN_BINARY_PATH` só é necessário para `get_model_info`/`slice_model`/`compare_slice_configs` — pode ser omitido se você só quiser as tools de preset. Use `"slicer"` em vez de `"web"` no `ANYCUBIC_AUTH_MODE` se o método preferencial do Passo 3 funcionou pra você — esse modo suporta status em tempo real via MQTT.

**Passo 5 — Reiniciar e testar**

Feche o Claude Desktop completamente (⌘Q) e abra de novo. Numa conversa nova, teste: *"Liste minhas impressoras"*, *"Mostre meus process profiles"*, *"Me mostre as informações desse arquivo STL"*, *"Fatie esse modelo com o perfil Chaveiro Reforçado e me diga tempo e consumo de filamento"*.

**Limitações conhecidas do fatiamento real:**
- Cada chamada usa uma pasta temporária descartável — não deveria conflitar com uma sessão aberta do programa, mas essa combinação específica não foi testada.
- Peças complexas podem demorar bem mais que o cubo de teste; ainda não há timeout configurável do lado do Claude Desktop, então uma fatiada muito longa pode expirar.
- O parsing de tempo/filamento depende de formatos de comentário no G-code não documentados oficialmente, que podem mudar entre versões do slicer.

---

## Uploading these files to your own GitHub repo / Subindo esses arquivos pro seu próprio GitHub

*No command line needed — this is entirely through the browser.*
*Sem necessidade de linha de comando — isso é feito inteiramente pelo navegador.*

1. Go to **github.com** → click the **+** icon (top right) → **New repository**.
2. Give it a name (e.g. `anycubic-slicer-mcp`), choose Public or Private, click **Create repository**.
3. On the new repo's page, click **"uploading an existing file"** (or **Add file → Upload files**).
4. Drag in these files: `anycubic-slicer-mcp.zip`, `anycubic-slicer-next-lite.mcpb`, and this `README.md`.
5. Scroll down, click **Commit changes**.

Done — anyone with the repo link can now download both packages and read the install instructions.
