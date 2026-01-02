V1

```sh
# --- PS1 -----------------------------------------------------------
PS1="${debian_chroot:+($debian_chroot)}\u@\h \[\e[1;33m\]\w\[\e[m\] \$ "
# -------------------------------------------------------------------
```

V2

```sh
# --- PS1 -----------------------------------------------------------
red='\[\e[0;31m\]'
RED='\[\e[1;31m\]'
blue='\[\e[0;34m\]'
BLUE='\[\e[1;34m\]'
cyan='\[\e[0;36m\]'
CYAN='\[\e[1;36m\]'
green='\[\e[0;32m\]'
GREEN='\[\e[1;32m\]'
yellow='\[\e[0;33m\]'
YELLOW='\[\e[1;33m\]'
PURPLE='\[\e[1;35m\]'
purple='\[\e[0;35m\]'
nc='\[\e[0m\]'

PS1="$PURPLE\u$nc@$CYAN\H$nc: $GREEN\w$nc $YELLOW\W$nc\n$GREEN$ $nc"
# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac
# -------------------------------------------------------------------
```

V3 (colors, git branch display, last command exit status indicator)

```sh
# ~~~~ PS1 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# ──  Kolory ─────────────────────────────────
Color_Reset="\[\e[0m\]"
Color_User="\[\e[38;5;82m\]"        # zielony
Color_Host="\[\e[38;5;33m\]"        # niebieski
Color_Path="\[\e[38;5;208m\]"       # pomarańczowy
Color_GitBranch="\[\e[38;5;135m\]"  # fioletowy
Color_Prompt="\[\e[38;5;226m\]"     # żółty

# Kolory bez \[ \] dla funkcji
RED_PLAIN="\e[31m"
GREEN_PLAIN="\e[32m"
RESET_PLAIN="\e[0m"

RED="\[\e[31m\]"
GREEN="\[\e[32m\]"
YELLOW="\[\e[33m\]"
BLUE="\[\e[34m\]"
MAGENTA="\[\e[35m\]"
CYAN="\[\e[36m\]"

# ──  Funkcja wyciągająca nazwę gałęzi Git (jeśli jest) ──
function __git_ps1() {
    git rev-parse --is-inside-work-tree &>/dev/null || return
    branch=$(git symbolic-ref --short HEAD 2>/dev/null || git describe --tags --exact-match HEAD 2>/dev/null)
    [[ -n $branch ]] && echo " ($branch)"
}

# ──  Status ostatniego polecenia ──
function _exit_status() {
    [[ $LAST_EXIT_CODE -eq 0 ]] && echo -e "${GREEN_PLAIN}✔${RESET_PLAIN}" || echo -e "${RED_PLAIN}✘${RESET_PLAIN}"
}

# ──  Zachowaj exit code przed każdym promptem ──
PROMPT_COMMAND='LAST_EXIT_CODE=$?'

# ──  Definicja prompta ───────────────────────────────────────
export PS1="\$(_exit_status) ${Color_User}\u${Color_Reset}@${Color_Host}\h${Color_Reset}:${Color_Path}\w${Color_Reset}${Color_GitBranch}\$(__git_ps1)${Color_Reset}
${Color_Prompt}\\$ ${Color_Reset}"

# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```
