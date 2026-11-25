# Corrigir Erro de CUDA/cuDNN

## ❌ Erro:
```
Could not locate cudnn_ops64_9.dll. Please make sure it is in your library path!
Invalid handle. Cannot load symbol cudnnCreateTensorDescriptor
```

---

## ✅ Soluçêo Aplicada

Os arquivos `dublar_tech_v2.py` e `dublar_sync_v2.py` **já foram corrigidos** para:

1. **Tentar usar GPU primeiro** (mais rápido)
2. **Se falhar, usar CPU automaticamente** (mais lento mas funciona)

Agora o código detecta automaticamente qual usar.

---

## 🚀 Execute Normalmente

Seu comando funciona normalmente:

```bash
python dublar_tech_v2.py --in ccode-dia10.mp4 --src en --tgt pt --tts bark --voice v2/pt_speaker_1 --sync smart --tolerance 0.0 --maxstretch 2.0
```

Você verá uma dessas mensagens:

### Se GPU funcionar:
```
Tentando usar GPU (CUDA)...
✓ GPU disponível
```

### Se GPU nêo funcionar (vai usar CPU):
```
Tentando usar GPU (CUDA)...
GPU nêo disponível (...)
Usando CPU (pode ser mais lento)...
```

**Em ambos os casos, o processo continuará normalmente!**

---

## ⚡ (Opcional) Acelerar com GPU

Se você quer que a GPU funcione para processar mais rápido:

### Opçêo 1: Instalar CUDA Toolkit + cuDNN (Complexo)

1. Instale CUDA Toolkit 11.8 ou 12.x:
   https://developer.nvidia.com/cuda-downloads

2. Instale cuDNN:
   https://developer.nvidia.com/cudnn

3. Adicione ao PATH do Windows:
   - `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.x\bin`
   - Pasta onde você extraiu cuDNN

### Opçêo 2: Usar CPU (Mais Simples)

O código já usa CPU automaticamente se GPU falhar. Apenas será ~2-3x mais lento na transcriçêo.

**Para vídeos de demonstraçêo técnica (10-20 min), CPU é aceitável.**

---

## 🔍 Verificar qual está usando

Rode seu comando e veja a mensagem:

```bash
python dublar_tech_v2.py --in video.mp4 --src en --tgt pt ...
```

Procure por:
```
=== ETAPA 3: Transcriçêo (Whisper) ===
Tentando usar GPU (CUDA)...
✓ GPU disponível  ← Usando GPU (rápido)
```

ou

```
=== ETAPA 3: Transcriçêo (Whisper) ===
Tentando usar GPU (CUDA)...
GPU nêo disponível (...)
Usando CPU (pode ser mais lento)...  ← Usando CPU (mais lento mas funciona)
```

---

## 📊 Comparaçêo de Velocidade

| Componente | CPU | GPU |
|------------|-----|-----|
| Whisper (transcriçêo) | ~2-3x real-time | ~0.5x real-time |
| M2M100 (traduçêo) | Rápido | Rápido |
| Bark (TTS) | Lento (~10s/seg) | Mais rápido (~3s/seg) |

**Nota:** Mesmo em CPU, o pipeline funciona. Só demora mais.

---

## ✅ Resumo

**Você nêo precisa fazer nada!**

O código já foi corrigido para:
- ✅ Tentar GPU automaticamente
- ✅ Usar CPU se GPU falhar
- ✅ Continuar processamento normalmente

Apenas execute seu comando normalmente:
```bash
python dublar_tech_v2.py --in video.mp4 --src en --tgt pt --tts bark --voice v2/pt_speaker_1 --sync smart --tolerance 0.0 --maxstretch 2.0
```
