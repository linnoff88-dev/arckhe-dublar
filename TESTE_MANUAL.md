# 🧪 GUIA DE TESTE MANUAL DAS CORREÇÕES

Este guia mostra como testar manualmente as correções aplicadas **SEM precisar rodar o pipeline completo**.

---

## 📋 PRÉ-REQUISITOS

Você já deve ter executado `dublar nei.mp4` pelo menos uma vez, para ter os arquivos em `dub_work/`:
- ✅ `dub_work/asr_trad.json` (segmentos traduzidos)
- ✅ `dub_work/segments.csv` (segmentos antigos com timestamps errados)
- ✅ `dub_work/seg_*.wav` (áudios TTS gerados pelo Bark)
- ✅ `dub_work/dub_raw.wav` (áudio concatenado)
- ✅ `dub_work/dub_final.wav` (áudio pós-processado com filtros antigos)

---

## 🧪 TESTE 1: Verificar Correçêo do Split

### **O que testa**: Funçêo `split_long_segments()` com timestamps proporcionais

### **Como executar**:
```batch
python test_correcoes.py
```

### **O que você verá**:

#### Parte 1 - Split inteligente:
```
=== TESTE 1: Split com timestamps proporcionais ===

[SPLIT] Segmento 1: 5.55s->17.55s (12.00s, 215 chars)
[SPLIT]   Dividindo em 2 partes (total: 214 chars):
[SPLIT]     Parte 1/2: 5.55s->15.55s (10.00s, 179 chars, 83.6%)
[SPLIT]     Parte 2/2: 15.55s->17.55s (2.00s, 35 chars, 16.4%)
```

**Antes (ERRADO)**:
- Parte 1: 5.55s→15.54s (9.99s) ❌
- Parte 2: 15.54s→17.55s (2.01s) ❌

**Depois (CORRETO)**:
- Parte 1: 5.55s→15.55s (10.00s) ✅
- Parte 2: 15.55s→17.55s (2.00s) ✅

#### Parte 2 - Comparaçêo de timestamps:
```
COMPARAÇêO: Timestamps Antigos vs Novos

Seg   Antigo               Novo                 Diferença
------------------------------------------------------------
1     5.55s->15.54s (9.99s)  5.55s->15.55s (10.00s)  +0.01s
2     15.54s->17.55s (2.01s) 15.55s->17.55s (2.00s)  -0.01s
```

#### Parte 3 - Sincronizaçêo com novos alvos:
```
TESTE 2: Sincronizaçêo com novos alvos

Seg   Alvo Antigo  Alvo Novo    TTS Gerado   Ajuste Novo
-------------------------------------------------------------
1     9.99s        10.00s       11.93s       COMPRIMIR (0.838x)
2     2.01s        2.00s        3.44s        COMPRIMIR (0.581x)
```

### **O que validar**:
- ✅ Timestamps proporcionais ao tamanho do texto
- ✅ Alvos de sincronizaçêo mais realistas
- ✅ Ratios de compressêo/expansêo razoáveis (<1.3x)

---

## 🧪 TESTE 2: Verificar Correçêo do Pós-Processamento de Áudio

### **O que testa**: Filtros de áudio (antigos vs novos)

### **Como executar**:
```batch
python test_audio_fix.py
```

### **O que você verá**:

#### Processamento:
```
=== TESTE 1: Pós-processamento ANTIGO (com afftdn + equalizer) ===
Filtros antigos: loudnorm=I=-16:TP=-1.5:LRA=11,afftdn=nf=-25,equalizer=...
✅ Gerado: dub_work/test_final_OLD.wav

=== TESTE 2: Pós-processamento NOVO (apenas loudnorm) ===
Filtros novos: loudnorm=I=-14:TP=-1.5:LRA=11
✅ Gerado: dub_work/test_final_NEW.wav
```

#### Comparaçêo de volumes:
```
TESTE 3: Comparaçêo de Volumes

Arquivo                                  Mean Volume     Max Volume      Status
----------------------------------------------------------------------------------
dub_raw.wav (ORIGINAL)                   -91.0 dB        -74.7 dB        MUITO BAIXO ❌
test_final_OLD.wav (FILTROS ANTIGOS)     -91.0 dB        -84.3 dB        MUITO BAIXO ❌
test_final_NEW.wav (FILTROS NOVOS)       -20.0 dB        -5.0 dB         Normal ✅
```

#### Resumo:
```
RESUMO DA ANÁLISE

Volume médio (mean_volume):
  Original (raw):        -91.0 dB
  Após filtros ANTIGOS:  -91.0 dB  (diferença: 0.0 dB)
  Após filtros NOVOS:    -20.0 dB  (diferença: +71.0 dB)

❌ Filtros ANTIGOS destruíram o áudio (< -50 dB)
✅ Filtros NOVOS melhoraram volume em 71.0 dB
✅ Volume NOVO está em nível aceitável (> -30 dB)
```

### **O que validar**:
- ✅ `test_final_OLD.wav` deve ter volume muito baixo (-91 dB)
- ✅ `test_final_NEW.wav` deve ter volume normal (-20 dB)
- ✅ Diferença de pelo menos +50 dB entre OLD e NEW

### **Teste auditivo** (opcional):
```batch
# Tocar áudio com filtros antigos (deve estar quase mudo)
ffplay dub_work/test_final_OLD.wav

# Tocar áudio com filtros novos (deve estar audível)
ffplay dub_work/test_final_NEW.wav
```

---

## 🧪 TESTE 3: Validar Todas as Correções Juntas

### **Como executar**:
```batch
# Limpar arquivos antigos
del video_dublado.mp4
rmdir /s /q dub_work

# Rodar pipeline completo com correções
dublar nei.mp4
```

### **O que observar no console**:

#### Etapa 5 - Split:
```
=== ETAPA 5: Split inteligente ===
[SPLIT] Segmento 1: 5.55s->17.55s (12.00s, 215 chars)
[SPLIT]   Dividindo em 2 partes (total: 214 chars):
[SPLIT]     Parte 1/2: 5.55s->15.55s (10.00s, 179 chars, 83.6%)
[SPLIT]     Parte 2/2: 15.55s->17.55s (2.00s, 35 chars, 16.4%)
[SPLIT] Resultado: 19 segmentos (1 divididos, 17 mantidos)
```

#### Etapa 7 - Sincronizaçêo:
```
=== ETAPA 7: Sincronizaçêo ===
[SYNC] Segmento: seg_0001_xf.wav | Alvo: 10.00s | Atual: 11.93s | Range: [9.50s - 10.50s]
[SYNC] → Açêo: FIT (áudio longo, comprimir)
  [FIT] Ajustando: 11.93s → 10.00s (ratio=0.838)
```

#### Etapa 9 - Pós-processo:
```
=== ETAPA 9: Pós-processo ===
>> ffmpeg -y -i dub_raw.wav -af loudnorm=I=-14:TP=-1.5:LRA=11 -ar 24000 -ac 1 dub_final.wav
```

### **Validaçêo final**:

1. **Duraçêo do vídeo**:
```batch
ffprobe -v error -show_entries format=duration -of default=nw=1 dublado/nei.mp4
```
**Esperado**: ~114 segundos (igual ao original)

2. **Volume do áudio**:
```batch
ffmpeg -i dublado/nei.mp4 -af volumedetect -vn -f null - 2>&1 | findstr "volume"
```
**Esperado**: `mean_volume: -20.0 dB` ou melhor (nêo -91 dB)

3. **Bitrate de áudio**:
```batch
ffprobe -v error -select_streams a:0 -show_entries stream=bit_rate -of default=nw=1 dublado/nei.mp4
```
**Esperado**: ~192000 (192 kbps, nêo 1 kbps)

4. **Arquivo no diretório correto**:
```batch
dir dublado\nei.mp4
```
**Esperado**: Arquivo existe em `dublado/nei.mp4` (nêo `video_dublado.mp4` na raiz)

---

## 📊 RESUMO DE VALIDAÇÕES

| Correçêo | Como Testar | Resultado Esperado |
|----------|-------------|-------------------|
| Split proporcionais | `python test_correcoes.py` | Timestamps refletem % do texto |
| Volume audível | `python test_audio_fix.py` | Volume > -30 dB |
| Sincronizaçêo | Logs do dublar | Ratios < 1.3x |
| Diretório correto | `dir dublado\nei.mp4` | Arquivo existe |
| Duraçêo correta | `ffprobe dublado/nei.mp4` | ~114s |

---

## ❓ FAQ

### **Q: Por que dub_raw.wav tem volume baixo (-91 dB)?**
**A**: Provavelmente os arquivos TTS (seg_*.wav) foram gerados com volume baixo pelo Bark. O loudnorm deveria corrigir isso.

### **Q: test_final_NEW.wav ainda está com volume baixo?**
**A**: Se sim, o problema está no áudio gerado pelo Bark, nêo no pós-processamento. Verifique:
```batch
ffmpeg -i dub_work/seg_0001.wav -af volumedetect -f null - 2>&1 | findstr "volume"
```

### **Q: Como desabilitar o split?**
**A**: Edite `dublar.bat` e mude:
```batch
set "MAXDUR=0"
```
Ou rode: `dublar nei.mp4 --maxdur 0`

### **Q: O vídeo ainda saiu maior que o original?**
**A**: Verifique o console na Etapa 7. Se mostrar "EXPANDIR" em vez de "COMPRIMIR", há um problema na sincronizaçêo.

---

## 🎯 PRÓXIMOS PASSOS

1. ✅ Execute `python test_correcoes.py` para validar split
2. ✅ Execute `python test_audio_fix.py` para validar filtros de áudio
3. ✅ Execute `dublar nei.mp4` para testar pipeline completo
4. ✅ Valide os resultados usando as verificações acima

**Boa sorte com os testes!** 🚀
