# Modifiche rispetto a COSMIC / Changes against upstream

Galactic non è un fork completo: sono **patch** applicate sopra le release di COSMIC (System76).
Questo file tiene traccia di cosa cambia rispetto alla base di partenza.

## Base di partenza (versione 0.0.1)

| Componente | Repository originale | Release | Commit di base | Data del commit |
|---|---|---|---|---|
| `cosmic-player` | [pop-os/cosmic-player](https://github.com/pop-os/cosmic-player) | 1.11.0 | `34a41f2` | — |
| COSMIC (meta-repo) | [pop-os/cosmic-epoch](https://github.com/pop-os/cosmic-epoch) | `epoch-1.11.0` | `e98f828` | — |
| `cosmic-files` | [pop-os/cosmic-files](https://github.com/pop-os/cosmic-files) | 1.11.0 | `3cc4b80` | 2026-10-20 |
| `cosmic-applets` | [pop-os/cosmic-applets](https://github.com/pop-os/cosmic-applets) | 1.11.0 | `328d142f` | 2026-10-13 |

Compilato con Rust 1.97 (pacchetto Ubuntu `rustc-1.97`), Ubuntu 26.04 LTS.
Tutto il resto di COSMIC (compositor, pannello, impostazioni, ecc.) è **identico all'originale**.

## File toccati

### `cosmic-files` — 6 file, +595 / −12 righe

| File | Cosa cambia |
|---|---|
| `src/desktop_layout.rs` (nuovo, 85 righe) | Legge/scrive `~/.config/cosmic/com.system76.CosmicFiles/desktop-layout.txt` (una riga per icona: `colonna riga nome`), con cache che si rilegge solo se il file cambia. Legge anche la chiave `desktop_locked` dalla config di COSMIC Files. |
| `src/lib.rs` (+1) | Dichiara il modulo `desktop_layout`. |
| `src/tab.rs` (+180 / −10) | `grid_view`: nel desktop le icone vanno nelle celle salvate (le altre nella prima cella libera, riempimento a colonne come prima); celle vuote disegnate come segnaposto. Nuovi: `desktop_metrics`, `desktop_dnd_pos`, `Message::DndMotion`, `try_move_desktop_icons` (al rilascio di un'icona già sul desktop calcola la cella sotto il puntatore e salva il layout; se il desktop è bloccato non fa nulla). |

| `src/archive.rs` (+291) | `.7z` in Rust puro con `sevenz-rust2` (`sevenz_extract`: annullamento, avanzamento, password, percorsi sicuri); `.rar` tramite `unar` o `7z`/`7zz` (`rar_extract`). Aggiunge i tipi MIME e le estensioni `.7z` e `.rar` (il menu "Estrai" compare per questi file). Test: `GALACTIC_TEST_DIR` (archivi `.7z`), `GALACTIC_RAR_FILE` e `GALACTIC_RAR_OUT` (un `.rar` vero). |
| `Cargo.toml` (+2), `Cargo.lock` (+48) | Dipendenza `sevenz-rust2 0.25` (Apache-2.0, `aes256`, `bzip2`, `deflate`). |

Comportamento nuovo: se `desktop-layout.txt` non esiste e il desktop è bloccato, il desktop si comporta **come l'originale**.

### `cosmic-applets` — 6 file, +139 righe (nessuna riga originale modificata, solo aggiunte)

| File | Cosa cambia |
|---|---|
| `cosmic-applet-desktop-lock/` (nuovo) | Applet del pannello con il lucchetto: alterna la chiave `desktop_locked` (bool, assente = bloccato) nella config `com.system76.CosmicFiles`. Icone `changes-prevent-symbolic` / `changes-allow-symbolic`. |
| `Cargo.toml` (+1) | Aggiunge l'applet ai membri del workspace. |
| `Cargo.lock` (+10) | Voce del nuovo pacchetto. |

### `cosmic-player` — 6 file, +284 / −20 righe

| File | Cosa cambia |
|---|---|
| `src/sticky.rs` (nuovo, 111 righe) | Piccolo client Wayland (tramite `cosmic::cctk`) che trova la finestra dal titolo e chiede al compositore lo stato *sticky* (`zcosmic_toplevel_manager_v1.set_sticky`), con più scambi di conferma e verifica dello stato. |
| `src/main.rs` (+187 / −20) | Modalità Mini: `Message::MiniPlayer` rilancia il lettore (variabili `GALACTIC_MINI`, `GALACTIC_START`, `GALACTIC_PAUSED`) con una finestra 400×240 senza barre e chiude quella vecchia; trascinamento (`MiniDrag`); layout compatto (icona con titolo, meno bottoni); `update_controls` non rimette più la barra del titolo in modalità Mini; il comando del menu è filtrato contro il doppio invio. |
| `src/key_bind.rs`, `src/menu.rs`, `i18n/en`, `i18n/it` | Scorciatoia `Ctrl+M`, voce *Riproduzione › Mini Player*, testi. |

## Storia di Galactic

### 0.0.1 — 2026-11-13
- Icone del desktop in celle fisse, ricordate tra un avvio e l'altro.
- Trascinamento delle icone su una cella libera (celle occupate: spostamento annullato; più icone: mantengono la distanza).
- Applet del pannello "Blocco icone del desktop"; di default il desktop è bloccato.
- Script `galactic-update`: aggiornamento settimanale con riapplicazione delle patch, copia di sicurezza e ripristino.

### 0.0.2 — 2026-11-13
- Estrazione di archivi `.7z` (Rust nativo, con password) e `.rar` (tramite `unar`/`7z`) da COSMIC Files.
- `galactic-update`: servizio D-Bus del portale (screenshot) con l'ambiente di `~/cosmic`, collegamento dei componenti di `pop-launcher` (Alt+Tab, ricerca), e compilazione corretta di `cosmic-files` (la modalità applet non va persa).

### 0.0.3 — 2026-11-13
- Mini Player in COSMIC Media Player: finestra piccola, sempre in primo piano (sticky), con layout compatto.

## Come si aggiorna questo file

1. Dopo ogni modifica ai sorgenti, rigenera le patch: `scripts/aggiorna-patch`.
2. Quando esce una nuova release di COSMIC e le patch si applicano, aggiorna la tabella "Base di partenza".
3. Aggiungi una riga alla "Storia" qui sopra.
