# 3D Gaussian Splatting — Qualitätsevaluation mit gsplat

**Dieser Code wurde mit Claude Sonnet 4.6 generiert und funktioniert in diesem Beispiel. Es wird keine Garantie für Fehler übernommen**

Dieses Jupyter Notebook ermöglicht den **quantitativen Vergleich** verschiedener 3DGS-Modelle (`.ply`-Dateien) anhand von gerenderten Ansichten gegenüber Originalfotos. Berechnet werden die Metriken **PSNR** und **SSIM**.

---

## Voraussetzungen

- Google Account (für Google Drive & Colab)
- Postshot-exportierte `.ply`-Datei(en)
- COLMAP-Rekonstruktion der Originalaufnahmen (`cameras.txt`, `images.txt`)
- Originalfotos (z. B. DJI-Drohnenbilder)

---

## Ordnerstruktur in Google Drive

Erstelle folgende Struktur in deinem Google Drive **vor** dem ersten Start:

```
MyDrive/
└── GS_Analyse/
    │── 3mio.ply
    │── 15mio.ply
    │── DJI_0001.jpg
    │── DJI_0002.jpg
    │── ...
    │── cameras.txt
    │── images.txt
    └── vergleich/          ← wird automatisch erstellt (Render-Output)
```

### Wie erhalte ich cameras.txt und images.txt?

https://www.agisoft.com/

Diese Dateien stammen aus der **COLMAP** Ordnerstruktur aus Agisoft Metashape (v.2.3):

1. Agisoft Metashape → (Aktiver Junk) File → `Export` → `Export Cameras` → `Dateityp = COLMAP.txt`
2. Unterordner `colmap/` oder `sparse/0/` suchen
3. `cameras.txt` und `images.txt` kopieren nach `GS_Analyse`

> **Wichtig:** Die Bildnamen in `images.txt` müssen exakt mit den Dateinamen in `GS_Analyse` übereinstimmen.

### Wie rendern in in Postshot?

(https://www.jawset.com/)

1. **Postshot** öffnen
2. Drag and Drop die erstellte **COLMAP** Ordnerstruktur
3. Modell mit den erwarteten 3 Millionen (3'000k) sowie 15 Millionen trainieren
4. Alle anderen Einstellungen nach belieben definieren
5. Model Trainieren
6. `Export` → `Gaussian Splat (.ply)` → Speichern unter `GS_Analyse/ply/`
7. Mehrere Modelle (z. B. 3 Mio. / 15 Mio. Splats) separat exportieren

---

## Notebook starten

1. [colab.research.google.com](https://colab.research.google.com) öffnen
2. `Datei` → `Notebook hochladen` → `gs_evaluation.ipynb` auswählen
3. `Laufzeit` → `Laufzeittyp ändern` → **T4 GPU** aktivieren
4. Zellen der Reihe nach ausführen (`Shift+Enter`)

> **Wichtig:** Beim verbinden des (google)Drives wird eine Anmeldung bei Google erforderlich. Anschliessend werden die Ordner verfügbar sein.

---

## Nach einem Colab-Absturz

Colab-Sessions werden nach ~90 Min. Inaktivität beendet. So weitermachen:

1. **Zellen 1–4** neu ausführen (Pakete + Drive + Funktionen neu laden)
2. **Ab Zelle 5 weitermachen** — bereits gespeicherte Renders werden automatisch übersprungen (`⏭️ Bereits vorhanden`)
3. **Zellen 11–12** am Ende ausführen

---

## Metriken erklärung

| Metrik        | Beschreibung                                                       | Gut     | Sehr gut |
| ------------- | ------------------------------------------------------------------ | ------- | -------- |
| **PSNR** (dB) | Peak Signal-to-Noise Ratio — misst pixelweise Abweichung           | > 25 dB | > 30 dB  |
| **SSIM**      | Structural Similarity Index — misst Helligkeit, Kontrast, Struktur | > 0.80  | > 0.90   |

> **Hinweis:** Bei Drohnenaufnahmen mit automatischer Kamerabelichtung sind niedrigere Werte (~15–20 dB PSNR) normal, da Render und Original unterschiedliche Belichtungskurven haben. Für den **relativen Vergleich** (z. B. 3 Mio. vs. 15 Mio. Splats) sind die Werte dennoch aussagekräftig.

### Quellen

- Kerbl et al. (2023): _3D Gaussian Splatting for Real-Time Radiance Field Rendering_ — NeurIPS 2023. [Paper](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/) | [GitHub](https://github.com/graphdeco-inria/gaussian-splatting)
- Wang et al. (2004): _Image Quality Assessment: From Error Visibility to Structural Similarity_ — IEEE TIP. (SSIM-Definition)
- Ye et al. (2024): _gsplat: An Open-Source Library for Gaussian Splatting_ — [arxiv.org/abs/2409.06765](https://arxiv.org/abs/2409.06765)
- ISPRS (2026): _Metric Assessment of 3D Gaussian Splatting for UAV-Based Urban Mapping_ — [isprs-archives](https://isprs-archives.copernicus.org/articles/XLVIII-2-W12-2026/143/2026/)

---

## Reproduzierbarkeit

Alle Ergebnisse werden in `GS_Analyse/vergleich/` auf Google Drive gespeichert:

- `*_render_*.png` — gerenderte Ansichten
- `gs_metriken.csv` — PSNR/SSIM-Tabelle

Um die Evaluation von Grund auf zu wiederholen: alle `_render_*.png` im Ordner `vergleich/` löschen und Notebook neu ausführen.

---

## Abhängigkeiten

```
gsplat
plyfile
scikit-image
Pillow
torch
numpy
```

Werden in **Zelle 1** automatisch installiert.
