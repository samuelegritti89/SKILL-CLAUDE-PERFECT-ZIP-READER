---
name: zip-visualizer-perfect
display_name: ZIP Visualizer Perfetto
description: Quando l'utente carica una ZIP o chiede di visualizzare un archivio, mostra una visualizzazione PERFETTA con anteprime reali, non solo una lista. Rileva frame mancanti, estrae ZIP annidati ricorsivamente e genera un Artifact HTML interattivo.
version: 2.0.0
author: Samuele Gritti
---

# ZIP Visualizer Perfetto - Skill per Claude

## Quando attivarti
ATTIVATI SEMPRE quando:
- L'utente carica un file .zip, .cbz, .epub
- L'utente dice "visualizza ZIP", "apri ZIP", "contenuto ZIP", "analizza archivio"
- Vedi un file con struttura `frames/frame_####.png`

## COSA DEVI FARE (Obbligatorio)

Non rispondere MAI solo con una lista testuale. Il tuo output deve essere DOPPIO:

### 1. Report testuale (come hai già fatto bene)
Devi mantenere ESATTAMENTE questo formato che hai usato per test_archive.zip:

```
Report Visualizzatore ZIP
Generato il YYYY-MM-DD HH:MM:SS

{nome}.zip contiene {N} file: {riassunto umano es: "un video MP4 di 2 secondi (320x240, h264, senza audio), 8 frame PNG..."}
Attenzione: {anomalie}

Statistiche generali
ZIP trovati: X
File totali (tutti gli ZIP combinati): Y
ffprobe disponibile: sì/no

Per categoria:
image/frame: N
video: N
code: N
...

Elenco completo — TUTTI i file, in ordine crescente
Da 1 a N, nessun file escluso.
...
```

Con rilevamento anomalie:
- Regex `frame_(\d+)\.png` -> trova buchi [5,6] e segnala "trovati 8/10, mancano [5,6]"
- ZIP annidati -> segnala "inner.zip contains X files — inspect separately"

### 2. Artifact HTML Interattivo (QUESTO è la visualizzazione perfetta)

Dopo il report testuale, DEVI creare SEMPRE un Artifact di tipo `text/html` chiamato "ZIP Visualizer Perfetto".

L'artifact deve contenere:

```html
<!DOCTYPE html>
<html>
<head>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
<style>
/* usa design scuro, card, badge, griglia come nell'esempio */
</style>
</head>
<body>
<!-- 1. Header con nome zip e riassunto -->
<!-- 2. Box giallo ⚠ per anomalie (frame mancanti, nested zip) -->
<!-- 3. Griglia immagini: per ogni PNG mostra <img> con src base64 o URL.createObjectURL -->
<!-- 4. Player <video> per MP4 con metadati (durata, risoluzione, codec, fps) -->
<!-- 5. <pre><code> con syntax highlight per .py, .js -->
<!-- 6. Sezione ZIP annidato espandibile: pulsante che estrae inner.zip con JSZip -->
<!-- 7. Elenco completo file -->
</body>
</html>
```

Logica JS nell'artifact:
- Se hai accesso ai file caricati, usa JSZip per estrarli client-side
- Se non hai i bytes, simula con i nomi che hai nel report ma mostra placeholder colorati per i frame
- Per i frame mancanti mostra card tratteggiate gialle con "MANCANTE frame_0005"

### Esempio di output perfetto per test_archive.zip

Hai già fatto la parte 1 bene. La parte 2 per test_archive.zip deve mostrare:
- Video 320x240 h264 10fps 2.0s no audio
- 8 thumbnail PNG (82B) + 2 card "MANCANTE"
- script.py 44B e app.js 51B con codice
- notes.txt 36B
- inner.zip estratto -> secret.txt

### Regole
- Non dire mai "non posso visualizzare immagini/video" - puoi farlo nell'Artifact
- Usa SEMPRE Artifact, non solo testo
- Totale verificato: conta tutti i file
- Lingua: rispondi nella lingua dell'utente (italiano se scrive in italiano)

