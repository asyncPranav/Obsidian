

---


```sh
#!/bin/bash

# ==========================================
#              OSINT FRAMEWORK
#           Developer: asyncPranav
# ==========================================

# ====== FULL API URLs (API KEY INCLUDED) ======
NUM_API="https://yourapi.com/number?apikey=YOURKEY"
FAM_API="https://yourapi.com/family?apikey=YOURKEY"

# ====== COLORS ======
RED="\033[1;31m"
GREEN="\033[1;32m"
YELLOW="\033[1;33m"
BLUE="\033[1;34m"
CYAN="\033[1;36m"
MAGENTA="\033[1;35m"
RESET="\033[0m"

# ====== LOADING ANIMATION ======
loading() {
    echo -ne "${CYAN}[*] Initializing OSINT Engine"
    for i in {1..6}; do
        echo -ne "."
        sleep 0.3
    done
    echo -e "${RESET}"
}

# ====== BANNER ======
banner() {
clear
echo -e "${MAGENTA}"
echo " ██████╗  ██████╗ ██╗███╗   ██╗████████╗"
echo "██╔═══██╗██╔═══██╗██║████╗  ██║╚══██╔══╝"
echo "██║   ██║██║   ██║██║██╔██╗ ██║   ██║   "
echo "██║   ██║██║   ██║██║██║╚██╗██║   ██║   "
echo "╚██████╔╝╚██████╔╝██║██║ ╚████║   ██║   "
echo " ╚═════╝  ╚═════╝ ╚═╝╚═╝  ╚═══╝   ╚═╝   "
echo -e "${CYAN}"
echo "      Open Source Intelligence Framework"
echo -e "${YELLOW}            Developer: asyncPranav${RESET}"
echo ""
loading
echo ""
echo -e "${GREEN}Type 'help' to view commands.${RESET}"
echo ""
}

# ====== NUMBER LOOKUP ======
num_lookup() {
    NUMBER=$1

    if [ -z "$NUMBER" ]; then
        echo -e "${RED}[-] Please provide a number.${RESET}"
        return
    fi

    echo -e "${BLUE}[*] Searching number: $NUMBER${RESET}"
    RESPONSE=$(curl -s "${NUM_API}&number=${NUMBER}")

    if [ -z "$RESPONSE" ]; then
        echo -e "${RED}[-] No response from API.${RESET}"
    else
        echo -e "${GREEN}[+] Result:${RESET}"
        echo "$RESPONSE" | jq
    fi
}

# ====== FAMILY LOOKUP ======
fam_lookup() {
    ID=$1

    if [ -z "$ID" ]; then
        echo -e "${RED}[-] Please provide family ID.${RESET}"
        return
    fi

    echo -e "${BLUE}[*] Searching family ID: $ID${RESET}"
    RESPONSE=$(curl -s "${FAM_API}&id=${ID}")

    if [ -z "$RESPONSE" ]; then
        echo -e "${RED}[-] No response from API.${RESET}"
    else
        echo -e "${GREEN}[+] Result:${RESET}"
        echo "$RESPONSE" | jq
    fi
}

# ====== START ======
banner

while true; do
    echo -ne "${GREEN}osint > ${RESET}"
    read input

    case $input in

        exit|quit)
            echo -e "${RED}[*] Shutting down OSINT Framework...${RESET}"
            sleep 1
            exit 0
            ;;

        help)
            echo ""
            echo -e "${CYAN}Available Commands:${RESET}"
            echo "  num <number>   → Lookup Number Info"
            echo "  fam <id>       → Lookup Family Info"
            echo "  help           → Show Help"
            echo "  exit           → Exit Framework"
            echo ""
            ;;

        num*)
            NUMBER=$(echo $input | cut -d' ' -f2)
            num_lookup "$NUMBER"
            ;;

        fam*)
            ID=$(echo $input | cut -d' ' -f2)
            fam_lookup "$ID"
            ;;

        *)
            echo -e "${RED}[-] Unknown command. Type help.${RESET}"
            ;;
    esac
done
```