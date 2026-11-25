# ê°Å¸Å½Â¬ Dublar - Sistema de Dublagem AutomêÂ¡tica de VêÂ­deos

Sistema completo de dublagem automêÂ¡tica de vêÂ­deos usando IA para transcriêÂ§êÂ£o, traduêÂ§êÂ£o e sêÂ­ntese de voz.

## ê°Å¸ÅÅ¸ CaracterêÂ­sticas

- **TranscriêÂ§êÂ£o automêÂ¡tica** com Whisper (faster-whisper)
- **TraduêÂ§êÂ£o** com facebook/m2m100_418M (suporta mêÂºltiplos idiomas)
- **SêÂ­ntese de voz** com Bark ou Coqui TTS
- **SincronizaêÂ§êÂ£o inteligente** com 4 modos: none, fit, pad, smart
- **Suporte GPU** NVIDIA CUDA (opcional, funciona em CPU)
- **PreservaêÂ§êÂ£o de termos têÂ©cnicos** em vêÂ­deos de programaêÂ§êÂ£o
- **Processamento em lote** de mêÂºltiplos vêÂ­deos

## ê°Å¸Å¡â¬ InêÂ­cio RêÂ¡pido

### Windows:
```bash
git clone https://github.com/linnoff88-dev/dublar.git
cd dublar
instalar.bat
dublar.bat video.mp4
```

### Linux:
```bash
git clone https://github.com/linnoff88-dev/dublar.git
cd dublar
python3 -m venv venv
source venv/bin/activate
pip install torch --index-url https://download.pytorch.org/whl/cpu
pip install -r requirements.txt
python3 dublar.py video.mp4 --src_lang en --tgt_lang pt --tts bark --sync smart
```

**ê°Å¸ââ [Guia completo de instalaêÂ§êÂ£o Linux](INSTALL_LINUX.md)**

## ê°Å¸ââ¹ Requisitos

- **Python**: 3.10 ou superior
- **FFmpeg**: ObrigatêÂ³rio (processamento de vêÂ­deo/êÂ¡udio)
- **RAM**: 8GB mêÂ­nimo, 16GB+ recomendado
- **Disco**: 10GB+ para modelos de IA
- **GPU** (opcional): NVIDIA com CUDA 11.8+ para processamento mais rêÂ¡pido

## ê°Å¸Å½Â¯ Uso BêÂ¡sico

### Sintaxe:
```bash
python dublar.py VIDEO.mp4 [opêÂ§êÂµes]
```

### Exemplos:

**InglêÂªs ê¢â â PortuguêÂªs (padrêÂ£o):**
```bash
python dublar.py tutorial.mp4
```

**Espanhol ê¢â â InglêÂªs:**
```bash
python dublar.py video.mp4 --src_lang es --tgt_lang en
```

**VêÂ­deo têÂ©cnico com preservaêÂ§êÂ£o de termos:**
```bash
python dublar_tech_v2.py codigo.mp4 --src_lang en --tgt_lang pt
```

**Usar voz especêÂ­fica:**
```bash
python dublar.py video.mp4 --voice v2/pt_speaker_5
```

### ParêÂ¢metros Principais:

| ParêÂ¢metro | PadrêÂ£o | DescriêÂ§êÂ£o |
|-----------|--------|-----------|
| `--src_lang` | `en` | Idioma de origem (en, es, fr, pt, etc.) |
| `--tgt_lang` | `pt` | Idioma de destino |
| `--tts` | `bark` | Engine de voz (`bark` ou `coqui`) |
| `--sync` | `smart` | Modo de sincronizaêÂ§êÂ£o (`none`, `fit`, `pad`, `smart`) |
| `--voice` | - | Voz especêÂ­fica (ex: `v2/pt_speaker_5`) |
| `--rate` | `22050` | Taxa de amostragem do êÂ¡udio final |

### Modos de SincronizaêÂ§êÂ£o:

- **`none`**: Sem ajuste, pode desincronizar
- **`fit`**: Comprime/expande êÂ¡udio para caber no tempo
- **`pad`**: Adiciona silêÂªncio se necessêÂ¡rio
- **`smart`**: AutomêÂ¡tico (pad se curto, fit se longo) - **Recomendado**

## ê°Å¸âÂ Estrutura de Arquivos

```
dublar/
ê¢âÅê¢ââ¬ê¢ââ¬ dublar.py              # Script principal
ê¢âÅê¢ââ¬ê¢ââ¬ dublar2.py             # VersêÂ£o com melhorias de sync
ê¢âÅê¢ââ¬ê¢ââ¬ dublar3.py             # VersêÂ£o com output em pasta separada
ê¢âÅê¢ââ¬ê¢ââ¬ dublar31.py            # VersêÂ£o com preservaêÂ§êÂ£o de gaps
ê¢âÅê¢ââ¬ê¢ââ¬ dublar_tech_v2.py      # Otimizado para vêÂ­deos têÂ©cnicos
ê¢âÅê¢ââ¬ê¢ââ¬ dublar_sync_v2.py      # VersêÂ£o com sync avanêÂ§ado
ê¢ââ
ê¢âÅê¢ââ¬ê¢ââ¬ requirements.txt       # DependêÂªncias Python
ê¢ââ
ê¢âÅê¢ââ¬ê¢ââ¬ instalar.bat           # Instalador Windows
ê¢âÅê¢ââ¬ê¢ââ¬ dublar.bat             # Launcher Windows
ê¢âÅê¢ââ¬ê¢ââ¬ ativar_gpu.bat         # Configurar GPU no Windows
ê¢ââ
ê¢âÅê¢ââ¬ê¢ââ¬ test_*.py              # Scripts de teste
ê¢ââ
ê¢âÅê¢ââ¬ê¢ââ¬ INSTALL_LINUX.md       # ê°Å¸ââ Guia de instalaêÂ§êÂ£o Linux
ê¢âÅê¢ââ¬ê¢ââ¬ README_TECH.md         # DocumentaêÂ§êÂ£o versêÂ£o têÂ©cnica
ê¢âÅê¢ââ¬ê¢ââ¬ README_SYNC_V2.md      # DocumentaêÂ§êÂ£o sync v2
ê¢âÅê¢ââ¬ê¢ââ¬ README_BAT.md          # DocumentaêÂ§êÂ£o scripts Windows
ê¢âÅê¢ââ¬ê¢ââ¬ MAPA_ARQUIVOS.md       # Estrutura detalhada do projeto
ê¢ââ
ê¢âÅê¢ââ¬ê¢ââ¬ venv/                  # Ambiente virtual (vocêÂª cria)
ê¢âÅê¢ââ¬ê¢ââ¬ dub_work/              # TemporêÂ¡rios (criado automaticamente)
ê¢ââê¢ââ¬ê¢ââ¬ dublado/               # VêÂ­deos finais (criado automaticamente)
```

## ê°Å¸âÂ§ VersêÂµes DisponêÂ­veis

### Scripts Principais:

1. **`dublar.py`** - VersêÂ£o base estêÂ¡vel
   - Funcionalidades essenciais
   - Melhor para comeêÂ§ar

2. **`dublar_tech_v2.py`** - Para vêÂ­deos têÂ©cnicos
   - GlossêÂ¡rio de 100+ termos têÂ©cnicos
   - Preserva nomes de tecnologias
   - Otimizado para tutoriais de programaêÂ§êÂ£o
   - [ê°Å¸ââ DocumentaêÂ§êÂ£o](README_TECH.md)

3. **`dublar_sync_v2.py`** - SincronizaêÂ§êÂ£o avanêÂ§ada
   - Melhor alinhamento de êÂ¡udio
   - Controle fino de timing
   - [ê°Å¸ââ DocumentaêÂ§êÂ£o](README_SYNC_V2.md)

4. **`dublar31.py`** - Com preservaêÂ§êÂ£o de pausas
   - MantêÂ©m silêÂªncios entre frases
   - Mais natural para palestras

## ê°Å¸Â§Âª Testar InstalaêÂ§êÂ£o

```bash
# Ativar ambiente (se ainda nêÂ£o ativou)
source venv/bin/activate  # Linux
# venv\Scripts\activate   # Windows

# Teste rêÂ¡pido
python test_quick.py

# Testar GPU
python test_gpu.py

# Testar Whisper
python test_whisper_gpu.py
```

## ê°Å¸ÅÂ Idiomas Suportados

### TranscriêÂ§êÂ£o (Whisper):
Mais de 90 idiomas incluindo: en, pt, es, fr, de, it, ja, ko, zh, ru, ar, hi, etc.

### TraduêÂ§êÂ£o (M2M100):
100 idiomas incluindo todos os principais.

### SêÂ­ntese de Voz:
- **Bark**: MultilêÂ­ngue (en, pt, es, fr, de, it, pl, zh, ja, hi, etc.)
- **Coqui TTS**: Varia por modelo

## ê°Å¸âÅ  Fluxo de Processamento

```
1. ExtraêÂ§êÂ£o de êÂudio (FFmpeg)
   video.mp4 ê¢â â audio_src.wav

2. TranscriêÂ§êÂ£o (Whisper)
   audio_src.wav ê¢â â asr.srt + asr.json

3. TraduêÂ§êÂ£o (M2M100)
   asr.json ê¢â â asr_trad.json + asr_trad.srt

4. SêÂ­ntese de Voz (Bark/Coqui)
   asr_trad.json ê¢â â seg_0001.wav, seg_0002.wav, ...

5. SincronizaêÂ§êÂ£o (smart/fit/pad/none)
   seg_*.wav ê¢â â seg_*_fit.wav

6. ConcatenaêÂ§êÂ£o + PêÂ³s-processamento
   seg_*_fit.wav ê¢â â dub_final.wav

7. Mixagem Final (FFmpeg)
   video.mp4 + dub_final.wav ê¢â â video_dublado.mp4
```

## ê°Å¸Å½â DocumentaêÂ§êÂ£o Adicional

- **[INSTALL_LINUX.md](INSTALL_LINUX.md)** - Guia completo de instalaêÂ§êÂ£o no Linux
- **[README_TECH.md](README_TECH.md)** - VersêÂ£o otimizada para vêÂ­deos têÂ©cnicos
- **[README_SYNC_V2.md](README_SYNC_V2.md)** - SincronizaêÂ§êÂ£o avanêÂ§ada
- **[README_BAT.md](README_BAT.md)** - Scripts Windows (.bat)
- **[MAPA_ARQUIVOS.md](MAPA_ARQUIVOS.md)** - Estrutura completa do projeto
- **[FIX_CUDA.md](FIX_CUDA.md)** - SoluêÂ§êÂ£o de problemas CUDA
- **[INSTALACAO_GPU.md](INSTALACAO_GPU.md)** - Configurar GPU NVIDIA

## ê¢Å¡Â ê¯Â¸Â Notas Importantes

1. **DiretêÂ³rios automêÂ¡ticos**: `dub_work/` e `dublado/` sêÂ£o criados automaticamente - nêÂ£o crie manualmente
2. **FFmpeg obrigatêÂ³rio**: Sem FFmpeg nada funciona
3. **GPU opcional**: Funciona em CPU, apenas mais lento
4. **Primeira execuêÂ§êÂ£o**: Download de modelos pode levar tempo (5-10GB)
5. **MemêÂ³ria**: VêÂ­deos longos (+30min) podem precisar de 16GB+ RAM

## ê°Å¸Ââº SoluêÂ§êÂ£o de Problemas

### FFmpeg nêÂ£o encontrado:
```bash
# Ubuntu/Debian
sudo apt install ffmpeg

# Windows
# Baixe de https://ffmpeg.org e adicione ao PATH
```

### Erro de CUDA/GPU:
```bash
# ForêÂ§ar CPU (mais estêÂ¡vel)
# Edite o script e mude device="cuda" para device="cpu"
```

### Falta de memêÂ³ria:
```bash
# Use vêÂ­deos menores ou processe em partes
# Reduza batch_size no cêÂ³digo
```

### Modelos nêÂ£o baixam:
```bash
# Verifique conexêÂ£o com internet
# Modelos sêÂ£o baixados automaticamente do HuggingFace
```

## ê°Å¸âÂ Logs e Debug

Cada execuêÂ§êÂ£o gera:
- `dub_work/logs.json` - Log completo do processo
- `dub_work/asr.srt` - TranscriêÂ§êÂ£o original
- `dub_work/asr_trad.srt` - TraduêÂ§êÂ£o
- `dub_work/*.wav` - Arquivos de êÂ¡udio intermediêÂ¡rios

## ê°Å¸Â¤Â Contribuindo

Pull requests sêÂ£o bem-vindos! Para mudanêÂ§as maiores, abra uma issue primeiro.

## ê°Å¸ââ LicenêÂ§a

Este projeto êÂ© open source.

## ê°Å¸ââ Links êÅ¡teis

- **FFmpeg**: https://ffmpeg.org
- **Whisper**: https://github.com/openai/whisper
- **Bark**: https://github.com/suno-ai/bark
- **M2M100**: https://huggingface.co/facebook/m2m100_418M

---

**Desenvolvido com ê¢ÂÂ¤ê¯Â¸Â usando IA de ponta para dublagem de vêÂ­deos**

---

Projeto adaptado da comunidade ARCKHê TDS - ARCKHÉe