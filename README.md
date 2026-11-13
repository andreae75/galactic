# Galactic

Piccole modifiche al desktop **COSMIC** (System76), compilato a mano e installato in `~/cosmic`,
senza toccare il sistema. Il nome segue la famiglia di casa (Io, Europa, Ganimede, Cosmos…).

> *Galactic non è un progetto di System76. COSMIC è un marchio di System76; qui si usa solo per dire
> a quale desktop si applicano le patch.*

## Cosa aggiunge

**Icone del desktop dove vuoi tu** (EllesmereUI-style "unlock mode")
- Le icone si trascinano su una cella libera del desktop e ci restano (salvate in
  `~/.config/cosmic/com.system76.CosmicFiles/desktop-layout.txt`, una riga: `colonna riga nome`).
- Le icone nuove vanno nella prima cella libera.
- **Interruttore nel pannello**: l'applet *Blocco icone del desktop* (lucchetto) sblocca/blocca le icone.
  Di default il desktop è **bloccato**.

## Modifiche rispetto a COSMIC

Versione di base, file toccati e storia delle modifiche: vedi [`CHANGES.md`](CHANGES.md).

## Struttura

| Cartella | Contenuto |
|---|---|
| `patches/cosmic-files/` | patch per `cosmic-files` (celle fisse, trascinamento, lettura del blocco) |
| `patches/cosmic-applets/` | nuova applet `cosmic-applet-desktop-lock` |
| `scripts/galactic-update` | scarica l'ultima release stabile di COSMIC, riapplica le patch, compila, installa in `~/cosmic` (con copia di sicurezza `~/cosmic.prev` e ripristino automatico) |
| `scripts/start-galactic` | avvio della sessione con i percorsi di `~/cosmic` |
| `data/` | sessione di login (`galactic.desktop`) e timer systemd settimanale |

## Installazione (Ubuntu, Rust ≥ 1.93)

1. Sorgenti: `git clone --recursive https://github.com/pop-os/cosmic-epoch` in `$GALACTIC_BUILD/cosmic-epoch`
   (default `/mnt/garbage/cosmic-build`, servono ~50 GB) e dipendenze di compilazione di COSMIC.
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

## Licenza

GPL-3.0-or-later, come COSMIC. Vedi `LICENSE`.
