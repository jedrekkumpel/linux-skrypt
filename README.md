# linux-skrypt
skrypt z linuxa na lekcje ASO
oto skrypt:

#!/bin/bash

RED='\033[0;31m'
NC='\033[0m'
echo -e "${RED}"

remove_last_line() {
    file="$1"
    sed -i '$ d' "$file"
    echo "Usunięto ostatnią linię z pliku $file"
    echo "oto reszta pliku, która pozostała w pliku tekstowym"
    cat  /home/student/Dokumenty/plik1.txt
}


search_files() {
    folder="$1"
    keyword="$2"
    files=$(find "$folder" -type f -name "*.txt" -exec grep -l "$keyword" {} +)
    for file in $files; do
        remove_last_line "$file"
    done
}


while true; do
    echo -e                                             "====Menu===="
    echo -e             "1. Wyszukaj pliki tekstowe w określonym folderze i usuń ostatnią linię."
    echo -e             "2. Zabij 5 losowych procesów działających w tle, i je wyświetl."
    echo -e             "3. Wyświetl autora skryptu."
    echo -e             "4. wyjdź."
    read -p "Wybierz opcję: " option

    case $option in
        1)
            read -p "Podaj ścieżkę do folderu: " folder
           read -p "Podaj słowo kluczowe: " keyword
            search_files "$folder" "$keyword"
            ;;
        2)
            pids=$(ps aux | awk '{print $2}' | shuf -n 5)
            for pid in $pids; do
                kill "$pid"
            done
            echo "Zabito 5 losowych procesów, oto zabite procesy: "
            ps aux | shuf -n 5
            ;;
        4)
            break
            ;;
        3)
            echo "autorem jest bot"
            ;;
        *)
            echo "Nieprawidłowa opcja"
            ;;
    esac
done
