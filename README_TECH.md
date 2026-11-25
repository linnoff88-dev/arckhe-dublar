# Pipeline de Dublagem TÉCNICA v2.0

## 🎯 Otimizado para Vídeos de Demonstraçêo Técnica

Esta versêo é **especializada para conteúdo técnico** como:
- Tutoriais de programaçêo
- Demonstrações de código
- Aulas de desenvolvimento
- Apresentações técnicas
- Code reviews

---

## ✨ Melhorias Específicas para Conteúdo Técnico

### 1. **Glossário de 100+ Termos Técnicos**
Preserva termos que NêO devem ser traduzidos:
- `string`, `array`, `function`, `class`, `async`, `await`
- `git`, `commit`, `push`, `pull`, `branch`
- `npm`, `docker`, `API`, `JSON`, `HTTP`
- Nomes de linguagens, frameworks, bibliotecas

### 2. **Controle de Comprimento na Traduçêo**
- Analisa duraçêo do segmento original
- Limita palavras na traduçêo para caber no tempo
- Evita traduções muito longas que causam dessincronia

### 3. **Simplificaçêo Inteligente**
Remove palavras de enchimento mantendo clareza:
- "você sabe" → removido
- "basicamente" → removido
- "por exemplo" → "ex:"
- "o que eu quero dizer é" → removido

### 4. **Melhor Geraçêo de Traduções**
- `num_beams=5`: traduçêo mais fluida
- `length_penalty=1.2`: penaliza traduções longas
- `no_repeat_ngram_size=3`: evita repetições

---

## 🚀 Seu Comando Atualizado

### Antes (dublar31.py):
```bash
python dublar31.py --in ccode-dia10.mp4 --src en --tgt pt --tts bark --voice v2/pt_speaker_1 --sync smart --tolerance 0.0 --maxstretch 2.0
```

### Agora (dublar_tech_v2.py):
```bash
python dublar_tech_v2.py --in ccode-dia10.mp4 --src en --tgt pt --tts bark --voice v2/pt_speaker_1 --sync smart --tolerance 0.0 --maxstretch 2.0
```

**É EXATAMENTE O MESMO COMANDO**, apenas muda o nome do arquivo!

---

## 🆕 Novas Opções Disponíveis

### Opçêo 1: Desativar simplificaçêo (traduçêo literal completa)
```bash
python dublar_tech_v2.py --in ccode-dia10.mp4 --src en --tgt pt --tts bark --voice v2/pt_speaker_1 --sync smart --tolerance 0.0 --maxstretch 2.0 --no-simplify
```

### Opçêo 2: Usar modo elastic (melhor redistribuiçêo)
```bash
python dublar_tech_v2.py --in ccode-dia10.mp4 --src en --tgt pt --tts bark --voice v2/pt_speaker_1 --sync elastic --tolerance 0.0 --maxstretch 2.0
```

### Opçêo 3: Ativar VAD (detectar pausas naturais)
```bash
python dublar_tech_v2.py --in ccode-dia10.mp4 --src en --tgt pt --tts bark --voice v2/pt_speaker_1 --sync smart --tolerance 0.0 --maxstretch 2.0 --enable-vad
```

### RECOMENDADO para demonstrações técnicas:
```bash
python dublar_tech_v2.py --in ccode-dia10.mp4 --src en --tgt pt --tts bark --voice v2/pt_speaker_1 --sync elastic --enable-vad --tolerance 0.0 --maxstretch 2.0
```

---

## 📊 O Que Você Ganha

### Antes (M2M100 puro):
```
Original: "Now we'll import the string module and create an array"
Traduçêo: "Agora vamos importar o módulo de cordas e criar um arranjo"
❌ Problema: "string" virou "cordas", "array" virou "arranjo"
```

### Depois (versêo técnica):
```
Original: "Now we'll import the string module and create an array"
Traduçêo: "Agora vamos importar o módulo string e criar um array"
✅ Termos técnicos preservados!
```

---

## 🔧 Exemplos de Traduçêo Técnica

### Exemplo 1: Termos preservados
```
EN: "Let's use async/await with promises in JavaScript"
PT: "Vamos usar async/await com promises em JavaScript"
```

### Exemplo 2: Simplificaçêo automática
```
Original traduçêo: "Bem, basicamente o que eu quero dizer é que você sabe, nós vamos fazer um loop"
Simplificado: "Vamos fazer um loop"
```

### Exemplo 3: Controle de comprimento
```
Segmento: 3.5 segundos
Original: 15 palavras
Traduçêo normal: 22 palavras (muito longo!)
Traduçêo otimizada: 17 palavras (cabe no tempo)
```

---

## 📈 Métricas Adicionais

O CSV agora inclui:
```csv
idx,t_in,t_out,texto_trad,file,estimated_dur,actual_dur,compression_ratio
1,0.0,2.5,"Vamos criar uma funçêo",seg_0001.wav,1.234,1.189,0.85
```

- `compression_ratio`: 0.85 = traduçêo 15% mais curta (bom!)
- `compression_ratio`: 1.25 = traduçêo 25% mais longa (pode ser problema)

---

## 🎓 Glossário Completo

### Termos SEMPRE preservados:
```
string, array, boolean, null, undefined, true, false
const, let, var, function, class, return, async, await
git, commit, push, pull, merge, branch
npm, yarn, docker, API, JSON, HTTP, REST
console, log, import, export, callback, props, state
```

### Termos traduzidos corretamente:
```
function    → funçêo
method      → método
variable    → variável
parameter   → parâmetro
loop        → loop (mantido)
database    → banco de dados
repository  → repositório
test        → teste
```

---

## 🛠️ Troubleshooting Específico

### "Termos ainda sendo traduzidos incorretamente"
- Adicione ao conjunto `PRESERVE_TERMS` no código (linha ~75)
- Ou use `--no-simplify` para traduçêo mais literal

### "Traduçêo muito curta/cortada"
```bash
--no-simplify  # Desativa simplificaçêo automática
```

### "Traduçêo muito longa"
- O sistema já limita automaticamente
- Se ainda longo, aumente `--maxstretch` para 2.5

### "Perdi clareza com simplificaçêo"
```bash
--no-simplify  # Mantém traduçêo completa
```

---

## 📋 Comparaçêo: Normal vs. Técnica

| Aspecto | dublar_sync_v2.py | dublar_tech_v2.py |
|---------|-------------------|-------------------|
| Público | Geral | **Técnico/Programaçêo** |
| Glossário técnico | ❌ | ✅ 100+ termos |
| Preservaçêo de termos | ❌ | ✅ Automática |
| Controle de comprimento | ❌ | ✅ Baseado em duraçêo |
| Simplificaçêo | ❌ | ✅ Opcional |
| Qualidade traduçêo | Boa | **Melhor (beams=5)** |
| Métricas extras | Básicas | **+ compression_ratio** |

---

## 💡 Dicas para Melhores Resultados

### 1. **Use elastic sync para diálogos técnicos rápidos**
```bash
--sync elastic
```

### 2. **Ative VAD se houver muitas pausas**
```bash
--enable-vad
```

### 3. **Ajuste maxstretch baseado no ritmo**
- Narraçêo lenta: `--maxstretch 1.3`
- Ritmo normal: `--maxstretch 2.0` (seu caso)
- Muito rápido: `--maxstretch 2.5`

### 4. **Teste vozes diferentes**
```bash
--voice v2/pt_speaker_0  # Voz feminina
--voice v2/pt_speaker_1  # Voz masculina 1
--voice v2/pt_speaker_2  # Voz masculina 2
--voice v2/pt_speaker_3  # Voz grave
```

---

## 🔍 Analisando Resultados

Após execuçêo, confira:

1. **`dub_work/asr_trad.json`** - Veja se termos técnicos foram preservados
2. **`dub_work/segments.csv`** - Confira compression_ratio de cada segmento
3. **`dub_work/logs.json`** - Métricas completas de sincronizaçêo

### Exemplo de análise:
```json
{
  "compression_ratio": 0.92,  // ✅ Bom! 8% mais curto
  "text_original": "Let's create a new async function",
  "text_trad": "Vamos criar uma nova funçêo async"
}
```

---

## 🎬 Workflow Recomendado

1. **Primeira execuçêo (teste rápido)**
```bash
python dublar_tech_v2.py --in video.mp4 --src en --tgt pt --tts bark --voice v2/pt_speaker_1 --sync smart --tolerance 0.0 --maxstretch 2.0
```

2. **Confira resultados**
- Assista o vídeo dublado
- Verifique `segments.csv` para ver compression_ratio
- Leia `asr_trad.srt` para checar traduçêo

3. **Ajuste fino (se necessário)**
```bash
# Se traduçêo muito simplificada:
--no-simplify

# Se ainda dessincronia:
--sync elastic --enable-vad

# Se voz muito rápida:
--maxstretch 1.5
```

---

## 📝 Customizaçêo do Glossário

Para adicionar seus próprios termos:

1. Abra `dublar_tech_v2.py`
2. Localize `PRESERVE_TERMS` (linha ~75)
3. Adicione termos:
```python
PRESERVE_TERMS = {
    # ... termos existentes ...
    "react", "vue", "angular",  # Seus frameworks
    "redux", "hooks",           # Suas libs
    "typescript",               # Suas linguagens
}
```

---

## ✅ Checklist de Qualidade

Após dublar, confira:

- [ ] Termos técnicos preservados (string, array, etc.)
- [ ] Nomes de funções/variáveis nêo traduzidos
- [ ] Sincronizaçêo aceitável (offset médio < 0.3s)
- [ ] Voz natural (sem acelerações extremas)
- [ ] Áudio limpo (sem pulos ou cortes)
- [ ] Legendas corretas

---

## 🎯 Resumo

Para seus vídeos de demonstraçêo técnica:

**Comando básico (igual ao anterior, só muda o arquivo):**
```bash
python dublar_tech_v2.py --in ccode-dia10.mp4 --src en --tgt pt --tts bark --voice v2/pt_speaker_1 --sync smart --tolerance 0.0 --maxstretch 2.0
```

**Ganhos automáticos:**
✅ Preserva termos técnicos
✅ Traduçêo mais curta (cabe no tempo)
✅ Remove palavras de enchimento
✅ Melhor qualidade de traduçêo
✅ Métricas detalhadas

**Sem precisar mudar NADA no comando!**
