# Galactic

[English](#english) · [Italiano](#italiano)

---

## English

Small patches for the **COSMIC** desktop (System76), built from source and installed in `~/cosmic`,
without touching the rest of the system. The name follows the planets-and-moons naming of my home servers.

> *Galactic is not a System76 project. COSMIC is a trademark of System76; it is used here only to say
> which desktop the patches apply to.*

### What it adds

**Desktop icons where you want them** (inspired by the "unlock mode" of the EllesmereUI addon for World of Warcraft)
- Drag icons onto any free cell of the desktop and they stay there. Positions are saved in
  `~/.config/cosmic/com.system76.CosmicFiles/desktop-layout.txt` (one line per icon: `column row name`).
- New icons go into the first free cell.
- **Panel toggle**: the *Desktop Icons Lock* applet (a padlock) locks or unlocks the icons.
  The desktop is **locked by default**, and with no layout file it behaves exactly like stock COSMIC.

**Extract `.7z` and `.rar` archives** in COSMIC Files (right-click › *Extract here* / *Extract to…*)
- `.7z`: native, pure Rust ([`sevenz-rust2`](https://github.com/hasenbanck/sevenz-rust), Apache-2.0), including solid and password-protected archives, with progress and cancellation.
- `.rar`: through a system tool (`unar`, or `7z`/`7zz` as a fallback). RAR decoding is RARLAB proprietary code and cannot be embedded in a GPL program. If neither tool is installed, a message explains what to install.

### Layout of this repository

| Folder | Content |
|---|---|
| `patches/cosmic-files/` | patches for `cosmic-files` (fixed cells, drag and drop, lock check, `.7z`/`.rar` extraction) |
| `patches/cosmic-applets/` | the new `cosmic-applet-desktop-lock` applet |
| `scripts/galactic-update` | fetches the latest stable COSMIC release, re-applies the patches, builds and installs into `~/cosmic` (backup in `~/cosmic.prev`, automatic rollback) |
| `scripts/aggiorna-patch` | regenerates the patches from the local commits and prints a summary |
| `scripts/start-galactic` | session start-up script using the paths in `~/cosmic` |
| `data/` | login session (`galactic.desktop`) and a weekly systemd timer |

### Changes against upstream

Base versions, touched files and history: see [`CHANGES.md`](CHANGES.md).

### Installation (Ubuntu, Rust ≥ 1.93)

1. Sources: `git clone --recursive https://github.com/pop-os/cosmic-epoch` into `$GALACTIC_BUILD/cosmic-epoch`
   (default `/mnt/garbage/cosmic-build`, about 50 GB needed) plus COSMIC's build dependencies.
2. `install -m755 scripts/galactic-update ~/.local/bin/` and `scripts/start-galactic ~/cosmic/bin/`.
3. `galactic-update --force` (the first build compiles everything: hours).
4. Login session (needs sudo):
   `sed "s#@HOME@#$HOME#" data/galactic.desktop | sudo tee /usr/share/wayland-sessions/galactic.desktop`
5. Weekly update (optional): copy `data/galactic-update.*` into `~/.config/systemd/user/` and run
   `systemctl --user enable --now galactic-update.timer`.
6. In the panel: Settings › Desktop › Panel › applets, add *Desktop Icons Lock*.
   Suggested spot: on the right, next to the keyboard-layout selector (tested).

If a patch no longer applies to a new COSMIC release, `galactic-update` stops, restores the previous version
and (if `GALACTIC_MAIL` is set) sends an e-mail.

### License

GPL-3.0-or-later, like COSMIC. See `LICENSE`.

---

## Italiano

Piccole patch per il desktop **COSMIC** (System76), compilato a mano e installato in `~/cosmic`,
senza toccare il resto del sistema. Il nome segue la famiglia di pianeti e lune dei miei server di casa.

> *Galactic non è un progetto di System76. COSMIC è un marchio di System76; qui si usa solo per dire
> a quale desktop si applicano le patch.*

### Cosa aggiunge

**Icone del desktop dove vuoi tu** (ispirato all'"unlock mode" dell'addon EllesmereUI per World of Warcraft)
- Le icone si trascinano su una qualsiasi cella libera del desktop e ci restano. Le posizioni sono salvate in
  `~/.config/cosmic/com.system76.CosmicFiles/desktop-layout.txt` (una riga per icona: `colonna riga nome`).
- Le icone nuove vanno nella prima cella libera.
- **Interruttore nel pannello**: l'applet *Blocco icone del desktop* (un lucchetto) blocca o sblocca le icone.
  Di default il desktop è **bloccato**, e senza file di layout si comporta esattamente come COSMIC originale.

**Estrazione di archivi `.7z` e `.rar`** in COSMIC Files (clic destro › *Estrai qui* / *Estrai in…*)
- `.7z`: nativo, in Rust puro ([`sevenz-rust2`](https://github.com/hasenbanck/sevenz-rust), Apache-2.0), anche solidi e con password, con avanzamento e annullamento.
- `.rar`: tramite uno strumento di sistema (`unar`, oppure `7z`/`7zz`). Il decodificatore RAR è codice proprietario di RARLAB e non si può incorporare in un programma GPL. Se non c'è nessuno dei due strumenti, un messaggio spiega cosa installare.

### Struttura del repository

| Cartella | Contenuto |
|---|---|
| `patches/cosmic-files/` | patch per `cosmic-files` (celle fisse, trascinamento, controllo del blocco, estrazione `.7z`/`.rar`) |
| `patches/cosmic-applets/` | la nuova applet `cosmic-applet-desktop-lock` |
| `scripts/galactic-update` | scarica l'ultima release stabile di COSMIC, riapplica le patch, compila e installa in `~/cosmic` (copia di sicurezza `~/cosmic.prev`, ripristino automatico) |
| `scripts/aggiorna-patch` | rigenera le patch dai commit locali e mostra un riepilogo |
| `scripts/start-galactic` | avvio della sessione con i percorsi di `~/cosmic` |
| `data/` | sessione di login (`galactic.desktop`) e timer systemd settimanale |

### Modifiche rispetto a COSMIC

Versioni di base, file toccati e storia: vedi [`CHANGES.md`](CHANGES.md).

### Installazione (Ubuntu, Rust ≥ 1.93)

1. Sorgenti: `git clone --recursive https://github.com/pop-os/cosmic-epoch` in `$GALACTIC_BUILD/cosmic-epoch`
   (default `/mnt/garbage/cosmic-build`, servono circa 50 GB) e le dipendenze di compilazione di COSMIC.
2. `install -m755 scripts/galactic-update ~/.local/bin/` e `scripts/start-galactic ~/cosmic/bin/`.
3. `galactic-update --force` (la prima volta compila tutto: ore).
4. Sessione al login (richiede sudo):
   `sed "s#@HOME@#$HOME#" data/galactic.desktop | sudo tee /usr/share/wayland-sessions/galactic.desktop`
5. Aggiornamento settimanale (facoltativo): copia `data/galactic-update.*` in `~/.config/systemd/user/` e
   `systemctl --user enable --now galactic-update.timer`.
6. Nel pannello: Impostazioni › Desktop › Pannello › applet, aggiungi *Blocco icone del desktop*.
   Posizione consigliata: a destra, accanto alla selezione della lingua (provato e funzionante).

Se una patch non si applica più a una nuova versione di COSMIC, `galactic-update` si ferma, rimette la versione
precedente e (se `GALACTIC_MAIL` è impostata) avvisa per mail.

### Licenza

GPL-3.0-or-later, come COSMIC. Vedi `LICENSE`.
