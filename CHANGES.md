# Modifiche rispetto a COSMIC / Changes against upstream

Galactic non è un fork completo: sono **patch** applicate sopra le release di COSMIC (System76).
Questo file tiene traccia di cosa cambia rispetto alla base di partenza.

## Base di partenza (versione 0.0.1)

| Componente | Repository originale | Release | Commit di base | Data del commit |
|---|---|---|---|---|
| COSMIC (meta-repo) | [pop-os/cosmic-epoch](https://github.com/pop-os/cosmic-epoch) | `epoch-1.11.0` | `e98f828` | — |
| `cosmic-files` | [pop-os/cosmic-files](https://github.com/pop-os/cosmic-files) | 1.11.0 | `3cc4b80` | 2026-10-20 |
| `cosmic-applets` | [pop-os/cosmic-applets](https://github.com/pop-os/cosmic-applets) | 1.11.0 | `328d142f` | 2026-10-13 |

Compilato con Rust 1.97 (pacchetto Ubuntu `rustc-1.97`), Ubuntu 26.04 LTS.
Tutto il resto di COSMIC (compositor, pannello, impostazioni, ecc.) è **identico all'originale**.

## File toccati

### `cosmic-files` — 3 file, +256 / −10 righe

| File | Cosa cambia |
|---|---|
| `src/desktop_layout.rs` (nuovo, 85 righe) | Legge/scrive `~/.config/cosmic/com.system76.CosmicFiles/desktop-layout.txt` (una riga per icona: `colonna riga nome`), con cache che si rilegge solo se il file cambia. Legge anche la chiave `desktop_locked` dalla config di COSMIC Files. |
| `src/lib.rs` (+1) | Dichiara il modulo `desktop_layout`. |
| `src/tab.rs` (+180 / −10) | `grid_view`: nel desktop le icone vanno nelle celle salvate (le altre nella prima cella libera, riempimento a colonne come prima); celle vuote disegnate come segnaposto. Nuovi: `desktop_metrics`, `desktop_dnd_pos`, `Message::DndMotion`, `try_move_desktop_icons` (al rilascio di un'icona già sul desktop calcola la cella sotto il puntatore e salva il layout; se il desktop è bloccato non fa nulla). |

Comportamento nuovo: se `desktop-layout.txt` non esiste e il desktop è bloccato, il desktop si comporta **come l'originale**.

### `cosmic-applets` — 6 file, +139 righe (nessuna riga originale modificata, solo aggiunte)

| File | Cosa cambia |
|---|---|
| `cosmic-applet-desktop-lock/` (nuovo) | Applet del pannello con il lucchetto: alterna la chiave `desktop_locked` (bool, assente = bloccato) nella config `com.system76.CosmicFiles`. Icone `changes-prevent-symbolic` / `changes-allow-symbolic`. |
| `Cargo.toml` (+1) | Aggiunge l'applet ai membri del workspace. |
| `Cargo.lock` (+10) | Voce del nuovo pacchetto. |

## Storia di Galactic

### 0.0.1 — 2026-11-13
- Icone del desktop in celle fisse, ricordate tra un avvio e l'altro.
- Trascinamento delle icone su una cella libera (celle occupate: spostamento annullato; più icone: mantengono la distanza).
- Applet del pannello "Blocco icone del desktop"; di default il desktop è bloccato.
- Script `galactic-update`: aggiornamento settimanale con riapplicazione delle patch, copia di sicurezza e ripristino.

## Come si aggiorna questo file

1. Dopo ogni modifica ai sorgenti, rigenera le patch: `scripts/aggiorna-patch`.
2. Quando esce una nuova release di COSMIC e le patch si applicano, aggiorna la tabella "Base di partenza".
3. Aggiungi una riga alla "Storia" qui sopra.
