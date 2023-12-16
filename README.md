# 🐧 Linux Skrypt 🐧
🐧 skrypt z linuxa na lekcje ASO, wykonany przez Gabriela Jędrzejczyka 🐧

Oto kod skryptu:

```python
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
            echo "autorem jest Gabriel Jędrzejczyk 2TiM"
            ;;
        *)
            echo "Nieprawidłowa opcja"
            ;;
    esac
done
```

Poniżej postaram się wyjaśnić jak działają mniej zrozumiałe linie kodu:


RED='\033[0;31m' i NC='\033[0m': Te linijki definiują zmienne, które zawierają kody kolorów. RED zawiera kod koloru czerwonego, a NC resetuje kolor do domyślnego.

echo -e "${RED}": Ta linijka wyświetla czerwony kolor tekstu w terminalu.

remove_last_line() { ... }: Ta linijka definiuje funkcję o nazwie remove_last_line, która usuwa ostatnią linię z pliku tekstowego i wyświetla resztę pliku.

search_files() { ... }: Ta linijka definiuje funkcję o nazwie search_files, która wyszukuje pliki tekstowe zawierające określone słowo kluczowe w określonym folderze i wywołuje funkcję remove_last_line dla każdego znalezionego pliku.

while true; do ... done: Ta linijka rozpoczyna pętlę nieskończoną, która wyświetla menu i wykonuje odpowiednie akcje w zależności od wyboru użytkownika.

case $option in ... esac: Ta linijka rozpoczyna blok instrukcji warunkowych case, który sprawdza wartość zmiennej option i wykonuje odpowiednią sekcję kodu w zależności od wyboru użytkownika.

read -p "Wybierz opcję: " option: Ta linijka prosi użytkownika o wybór opcji i zapisuje wybór do zmiennej option.

read -p "Podaj ścieżkę do folderu: " folder i read -p "Podaj słowo kluczowe: " keyword: Te linijki proszą użytkownika o podanie ścieżki do folderu i słowa kluczowego, które będą używane w funkcji search_files.

pids=$(ps aux | awk '{print $2}' | shuf -n 5): Ta linijka pobiera identyfikatory procesów (PID) dla wszystkich procesów i losowo wybiera 5 z nich.

for pid in $pids; do ... done: Ta linijka iteruje przez wybrane identyfikatory procesów i wykonuje akcję dla każdego z nich.
