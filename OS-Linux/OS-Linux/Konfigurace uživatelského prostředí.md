```bash
alias ll = 'ls -l'  # definuje nový příkaz

source .zshrc  # nahraje do řádku obsah uvedeného souboru
```

```bash
alias greetings = 'echo -e "Ahoj $USER\nTeď je: $(date)'
# přivítání aktuálního uživatele s aktuálním čase

export STARTTIME="$(date)"  # proměnná času spuštění relace
```

- .zshrc - spouští se při otevření terminálu
- .zprofile - spouští se při loginu
