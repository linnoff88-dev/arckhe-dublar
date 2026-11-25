# Sistema de Checkpoint / Resume

## O Que É?

O sistema de **checkpoint/resume** permite que você continue o processo de dublagem **de onde parou** caso ocorra algum erro ou interrupçêo. Isso economiza horas de reprocessamento!

## Como Funciona?

### Etapas com Checkpoint

Cada etapa do pipeline salva um checkpoint ao ser concluída:

| Etapa | Nome | Checkpoint |
|-------|------|------------|
| 2 | Extraçêo de áudio | `audio_src.wav` criado |
| 3 | Transcriçêo (Whisper) | `asr.json`, `asr.srt` criados |
| 4 | Traduçêo (M2M100) | `asr_trad.json`, `asr_trad.srt` criados |
| 5 | Split de segmentos | Segmentos ajustados |
| 6 | TTS (Bark/Coqui) | `seg_0001.wav`, `seg_0002.wav`, etc. |
| 7 | Sincronizaçêo | Arquivos `seg_*_sync.wav` |
| 8 | Concatenaçêo | `dub_raw.wav` |
| 9 | Pós-processamento | `dub_final.wav` |
| 10 | Mux final | Vídeo dublado em `dublado/` |

### Arquivo de Checkpoint

O arquivo `dub_work/checkpoint.json` contém:

```json
{
  "last_step": "TTS (geraçêo de áudio)",
  "last_step_num": 6,
  "next_step": 7,
  "timestamp": "2025-10-08T10:30:45.123456"
}
```

---

## Como Usar

### Caso 1: Erro Durante Processamento

Se o processo falhar durante **ETAPA 6 (TTS - geraçêo de áudio)**:

```
=== ETAPA 6: TTS (Bark) ===
  [50/146] 34.2% - ETA: 12m 30s
ERRO: Memória insuficiente!
```

**Basta executar:**

```bash
dublar.bat --continue
```

O processo vai **pular as etapas 2-5** (já completas) e **continuar da etapa 6**, mas apenas os segmentos que faltam!

---

### Caso 2: Interrupçêo Manual (Ctrl+C)

Se você interromper o processo com Ctrl+C durante qualquer etapa:

```bash
dublar.bat --continue
```

Ele continuará da **próxima etapa** após a última checkpoint salvo.

---

### Caso 3: Forçar Reinício Total

Se quiser começar do zero (limpar tudo):

```bash
rd /s /q dub_work
dublar.bat ccode-dia15.mp4
```

---

## Exemplos Práticos

### Exemplo 1: Erro no TTS

```
C:\> dublar.bat ccode-dia15.mp4

=== ETAPA 3: Transcriçêo ===
[OK] 146 segmentos transcritos
[CHECKPOINT] Etapa 3 salva: Transcriçêo

=== ETAPA 4: Traduçêo ===
[OK] 146 segmentos traduzidos
[CHECKPOINT] Etapa 4 salva: Traduçêo

=== ETAPA 6: TTS (Bark) ===
  [30/146] 20.5% - ETA: 15m 12s
ERRO: PyTorch weights_only error!

--- PROCESSO INTERROMPIDO ---
```

**Você corrige o erro (atualiza PyTorch, etc.) e executa:**

```
C:\> dublar.bat --continue

========================================
  MODO RESUME: Continuando dublagem
========================================

[SKIP] ETAPA 2 já completa: dub_work\audio_src.wav
[SKIP] ETAPA 3 já completa: dub_work\asr.json
[SKIP] ETAPA 4 já completa: dub_work\asr_trad.json
[SKIP] ETAPA 5 já completa (split)

=== ETAPA 6: TTS (Bark) ===
  [30/146] Já existe, pulando...
  [31/146] 21.2% - ETA: 14m 50s
  ...
```

---

### Exemplo 2: Testar Diferentes Sincronizações

Você quer testar diferentes modos de sincronizaçêo **sem reprocessar TTS** (que demora 20 minutos):

```bash
# Primeira vez: processa tudo até sync=smart
dublar.bat video.mp4 --sync smart

# Agora testa sync=elastic SEM refazer TTS:
# (modifica manualmente o checkpoint para voltar à etapa 7)

python dublar_tech_v2.py --in video.mp4 --src en --tgt pt --tts bark --voice v2/pt_speaker_1 --sync elastic --continue
```

O script vai **pular etapas 2-6** e recomeçar da **etapa 7** com `sync=elastic`.

---

## Detecçêo Automática

O script detecta **automaticamente** quais arquivos existem:

```python
if asr.json existe:
    SKIP ETAPA 3

if asr_trad.json existe:
    SKIP ETAPA 4

if seg_0001.wav existe:
    SKIP ETAPA 6
```

---

## Vantagens

### ✅ Antes (sem checkpoint)

```
ETAPA 3 (Transcriçêo): 7 minutos
ETAPA 4 (Traduçêo): 1 minuto
ETAPA 6 (TTS): 20 minutos ❌ ERRO!

--- Corrige o erro ---

REINICIA TUDO:
ETAPA 3 (Transcriçêo): 7 minutos (DE NOVO!)
ETAPA 4 (Traduçêo): 1 minuto (DE NOVO!)
ETAPA 6 (TTS): 20 minutos (DE NOVO!)

Total perdido: 8 minutos + frustraçêo
```

### ✅ Agora (com checkpoint)

```
ETAPA 3 (Transcriçêo): 7 minutos ✓
ETAPA 4 (Traduçêo): 1 minuto ✓
ETAPA 6 (TTS): 20 minutos ❌ ERRO!

--- Corrige o erro ---

dublar.bat --continue

[SKIP] ETAPA 3 (0 segundos)
[SKIP] ETAPA 4 (0 segundos)
ETAPA 6 (TTS): Continua de onde parou!

Total economizado: 8 minutos 🚀
```

---

## Limitações

1. **Nêo funciona entre vídeos diferentes**: O checkpoint é por pasta `dub_work/`, entêo cada vídeo deve ter sua própria pasta.

2. **Mudança de parâmetros**: Se você mudar parâmetros críticos (ex: `--src en` para `--src pt`), é melhor limpar `dub_work/` e começar do zero.

3. **Arquivos corrompidos**: Se um arquivo intermediário ficar corrompido, delete-o manualmente:
   ```bash
   del dub_work\seg_0050.wav
   dublar.bat --continue
   ```
   O TTS vai recriar apenas o segmento faltante.

---

## Arquivos Importantes

| Arquivo | Funçêo |
|---------|--------|
| `dub_work/checkpoint.json` | Estado do checkpoint (qual etapa continuar) |
| `dub_work/asr.json` | Transcriçêo original (ETAPA 3) |
| `dub_work/asr_trad.json` | Traduçêo (ETAPA 4) |
| `dub_work/seg_*.wav` | Segmentos de áudio gerados (ETAPA 6) |
| `dub_work/dub_raw.wav` | Áudio concatenado (ETAPA 8) |
| `dub_work/dub_final.wav` | Áudio final processado (ETAPA 9) |
| `dub_work/logs.json` | Log completo do processo |

---

## Comandos Úteis

### Ver checkpoint atual
```bash
type dub_work\checkpoint.json
```

### Forçar continuar de uma etapa específica
Edite `checkpoint.json`:
```json
{
  "last_step": "Transcriçêo",
  "last_step_num": 3,
  "next_step": 4
}
```

### Limpar tudo e recomeçar
```bash
rd /s /q dub_work dublado
dublar.bat video.mp4
```

---

## Troubleshooting

### "ERRO: Nenhum checkpoint encontrado!"
- Execute primeiro uma dublagem normal: `dublar.bat video.mp4`

### "SKIP ETAPA 6 mas seg_0001.wav nêo existe!"
- Limpe a pasta: `rd /s /q dub_work` e recomece

### "Processo continua mas refaz tudo!"
- Verifique se `checkpoint.json` existe e está correto
- Use `--continue` na linha de comando

---

## Resumo

```bash
# Primeira execuçêo (processo normal)
dublar.bat video.mp4

# Se der erro, continue de onde parou:
dublar.bat --continue

# Ou com parâmetros adicionais:
dublar.bat --continue --sync elastic
```

**Economia de tempo**: Em um vídeo de 7 minutos, economiza ~8-10 minutos por erro!

🚀 **Agora você pode experimentar, corrigir erros e iterar SEM perder tempo reprocessando tudo!**
