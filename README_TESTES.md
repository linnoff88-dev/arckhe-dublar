# 🧪 GUIA COMPLETO DE TESTES

Guia para testar as correções aplicadas ao pipeline de dublagem.

---

## 📚 ARQUIVOS DE TESTE DISPONÍVEIS

| Arquivo | O que testa | Tempo | Quando usar |
|---------|-------------|-------|-------------|
| **test_quick.py** | Pós-processamento de áudio | ~10s | Teste rápido de áudio |
| **test_audio_fix.py** | Comparaçêo filtros OLD vs NEW | ~20s | Análise detalhada de áudio |
| **test_correcoes.py** | Split + sincronizaçêo | ~5s | Validar lógica corrigida |
| **Pipeline completo** | Tudo junto | ~20min | Teste final |

---

## 🚀 RECOMENDAÇêO: COMECE PELO TESTE RÁPIDO

### **1. Teste Rápido (10 segundos)**

```batch
python test_quick.py
```

**O que faz**:
- Junta os `seg_*.wav` existentes
- Aplica pós-processamento CORRIGIDO
- Gera vídeo de teste
- Mostra se áudio ficou audível

**Resultado esperado**:
```
Volume:
  Antes pós-processo: -91.0 dB
  Após pós-processo:  -22.5 dB  ✅
  Diferença:          +68.5 dB

✅ Volume CORRETO (áudio deve estar audível)
```

**Validar**:
```batch
ffplay test_VIDEO_CORRECTED.mp4
```

---

### **2. Se passou, rode pipeline completo**

```batch
# Limpar execuçêo antiga
del video_dublado.mp4
rmdir /s /q dub_work

# Rodar com todas as correções
dublar nei.mp4
```

**Validar resultado**:
```batch
# 1. Arquivo no lugar certo?
dir dublado\nei.mp4

# 2. Duraçêo correta?
ffprobe -v error -show_entries format=duration -of default=nw=1 dublado\nei.mp4
# Esperado: ~114s (nêo 294s)

# 3. Volume audível?
ffmpeg -i dublado\nei.mp4 -af volumedetect -vn -f null - 2>&1 | findstr "mean_volume"
# Esperado: -20 dB ou melhor (nêo -91 dB)

# 4. Bitrate correto?
ffprobe -v error -select_streams a:0 -show_entries stream=bit_rate -of default=nw=1 dublado\nei.mp4
# Esperado: ~192000 (nêo 1194)
```

---

## 🔍 TESTES DETALHADOS (OPCIONAL)

### **Teste A: Comparaçêo de Filtros**

```batch
python test_audio_fix.py
```

**Gera**:
- `dub_work/test_final_OLD.wav` (filtros antigos - quase mudo)
- `dub_work/test_final_NEW.wav` (filtros novos - audível)

**Comparar**:
```batch
ffplay dub_work/test_final_OLD.wav  # Deve estar quase mudo
ffplay dub_work/test_final_NEW.wav  # Deve estar audível
```

---

### **Teste B: Validar Split e Sincronizaçêo**

```batch
python test_correcoes.py
```

**Mostra**:
- Comparaçêo timestamps antigos vs novos
- Alvos de sincronizaçêo antes vs depois
- Análise de volume dos arquivos

**Validar saída**:
```
[SPLIT] Segmento 1: 5.55s->17.55s (12.00s, 215 chars)
[SPLIT]   Dividindo em 2 partes (total: 214 chars):
[SPLIT]     Parte 1/2: 5.55s->15.55s (10.00s, 179 chars, 83.6%)  ✅
[SPLIT]     Parte 2/2: 15.55s->17.55s (2.00s, 35 chars, 16.4%)   ✅
```

---

## 📊 CHECKLIST FINAL

Após rodar `dublar nei.mp4` com as correções:

### ✅ **Correçêo #1: Áudio Mudo**
- [ ] Volume mean > -30 dB (nêo -91 dB)
- [ ] Áudio audível ao reproduzir
- [ ] Bitrate ~192 kbps (nêo 1 kbps)

### ✅ **Correçêo #2: Arquivo no Diretório Errado**
- [ ] Arquivo existe em `dublado/nei.mp4`
- [ ] Nome mantém original (nei.mp4, nêo video_dublado.mp4)
- [ ] Pasta `dublado/` foi criada automaticamente

### ✅ **Correçêo #3: Duraçêo Errada**
- [ ] Duraçêo ~114s (igual ao original)
- [ ] Nêo está 2.5x maior (294s)
- [ ] Áudio sincronizado com vídeo

### ✅ **Logs de Debug**
- [ ] Console mostra `[SPLIT]` com timestamps proporcionais
- [ ] Console mostra `[SYNC]` com ações (FIT/PAD/OK)
- [ ] Console mostra ratios de ajuste

---

## 🐛 TROUBLESHOOTING

### **Problema: Volume ainda muito baixo após test_quick.py**

**Causa**: Arquivos `seg_*.wav` foram gerados com volume baixo pelo Bark

**Diagnóstico**:
```batch
ffmpeg -i dub_work/seg_0001.wav -af volumedetect -f null - 2>&1 | findstr "mean_volume"
```

Se mostrar < -50 dB, o problema está no Bark, nêo no pós-processamento.

**Soluçêo**: Ajustar parâmetros do Bark ou usar Coqui TTS:
```batch
dublar nei.mp4 --tts coqui
```

---

### **Problema: Duraçêo ainda 294s após pipeline completo**

**Causa**: Sincronizaçêo nêo está ajustando

**Diagnóstico**: Verificar logs no console:
```
[SYNC] Segmento: seg_0001_xf.wav | Alvo: 10.00s | Atual: 11.93s
[SYNC] → Açêo: OK (dentro da tolerância)  ❌ ERRADO!
```

Se mostrar "OK" quando deveria "FIT", há problema na sincronizaçêo.

**Soluçêo**: Forçar fit:
```batch
dublar nei.mp4 --sync fit --maxstretch 1.2
```

---

### **Problema: Split nêo está dividindo**

**Diagnóstico**: Verificar logs no console:
```
=== ETAPA 5: Split inteligente ===
Split desativado.  ❌
```

**Causa**: maxdur está 0

**Soluçêo**: Usar maxdur > 0:
```batch
dublar nei.mp4 --maxdur 10.0
```

---

## 📈 COMPARAÇêO ANTES vs DEPOIS

| Aspecto | ANTES | DEPOIS |
|---------|-------|--------|
| **Áudio** | -91 dB (mudo) | -20 dB (audível) ✅ |
| **Duraçêo** | 294s (2.5x maior) | 114s (correto) ✅ |
| **Arquivo** | `video_dublado.mp4` (raiz) | `dublado/nei.mp4` ✅ |
| **Bitrate** | 1 kbps | 192 kbps ✅ |
| **Timestamps** | Baseados em estimativa CPS | Proporcionais ao texto ✅ |
| **Sincronizaçêo** | Sem ajuste (none) | Smart com debug ✅ |

---

## 🎯 FLUXO RECOMENDADO

```
1. python test_quick.py
   └─> Se passou (volume OK): ir para passo 2
   └─> Se falhou (volume baixo): verificar seg_*.wav

2. dublar nei.mp4
   └─> Se passou (114s, audível, dublado/): SUCESSO! ✅
   └─> Se falhou: verificar logs de debug no console

3. Validar vídeo final:
   - ffplay dublado/nei.mp4
   - Verificar duraçêo, volume, bitrate
```

---

## 📞 GUIAS DETALHADOS

- **TESTE_RAPIDO.md**: Guia do test_quick.py (teste em 10s)
- **TESTE_MANUAL.md**: Guia completo de todos os testes
- **COMO_USAR.md**: Guia de uso do dublar

---

**Boa sorte com os testes!** 🚀
