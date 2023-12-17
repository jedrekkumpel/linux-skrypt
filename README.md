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
    echo -e             "4. Wyświetl klase autora."
    echo -e             "5. Wyjdź."
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
         3)
            echo "autorem jest Gabriel Jędrzejczyk 2TiM"
            ;;
         4)
            echo "klasa autora: 2TIM"
            ;;
         5)
            break
            ;;
       
        *)
            echo "Nieprawidłowa opcja"
            ;;
    esac
done

```

Poniżej postaram się wyjaśnić jak działają mniej zrozumiałe linie kodu:

```python
#!/bin/bash
```
Skrypt rozpoczyna się od deklaracji powłoki, w tym przypadku jest to powłoka Bash.
```python
RED='\033[0;31m'
NC='\033[0m'
echo -e "${RED}"
```
Definiuje zmienne RED i NC, które zawierają kody kolorów w formacie ANSI. Następnie wyświetla kolor czerwony za pomocą zmiennej RED.
Te 3 linie kodu nie są potrzebne, dodałem je tylko ze względów estetycznych, oraz by po wywołaniu skryptu wyróżniał się od reszty tekstu.
```python
remove_last_line() {
    file="$1"
    sed -i '$ d' "$file"
    echo "Usunięto ostatnią linię z pliku $file"
    echo "oto reszta pliku, która pozostała w pliku tekstowym"
    cat  /home/student/Dokumenty/plik1.txt
}
```
Definiuje funkcję remove_last_line, która usuwa ostatnią linię z określonego pliku, wyświetla komunikat o usunięciu linii oraz wyświetla pozostałą zawartość pliku tekstowego. 
Jeśli jest więcej niż jeden plik, a nie zdefiniujemy konkretnego pliku tekstowego, to usunie ostatnią linię z każdego pliku znajdującego się w tym folderze oraz wypisze pozostałe
zawartości wszystkich plików.
```python
search_files() {
    folder="$1"
    keyword="$2"
    files=$(find "$folder" -type f -name "*.txt" -exec grep -l "$keyword" {} +)
    for file in $files; do
        remove_last_line "$file"
    done
}
```
Definiuje funkcję search_files, która wyszukuje pliki tekstowe zawierające określone słowo kluczowe w określonym folderze, a następnie wywołuje funkcję remove_last_line dla każdego znalezionego pliku.
```python
while true; do
    echo -e                                             "====Menu===="
    echo -e             "1. Wyszukaj pliki tekstowe w określonym folderze i usuń ostatnią linię."
    echo -e             "2. Zabij 5 losowych procesów działających w tle, i je wyświetl."
    echo -e             "3. Wyświetl autora skryptu."
    echo -e             "4. Wyświetl klase autora."
    echo -e             "5. Wyjdź.
    read -p "Wybierz opcję: " option
```
Rozpoczyna nieskończoną pętlę, wyświetla menu proste menu tekstowe, prosi użytkownika o wybór opcji i oczekuje na wprowadzenie opcji przez użytkownika.
na początku może być niezrozumiałe to, dlaczego echo jest dodane z opcją "-e". Jak wiemy opcja "-e" interpretuje znaki specjalne, więc menu tekstowe 
które jest odpowiednio wycentrowane za pomocą tabulatora bądź spacji, będzie wyglądać lepiej i przejrzściej niż menu które by było wypisane od lewej strony.
```python
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
         3)
            echo "autorem jest Gabriel Jędrzejczyk 2TiM"
            ;;
         4)
            echo "klasa autora: 2TIM"
            ;;
         5)
            break
            ;;
       
        *)
            echo "Nieprawidłowa opcja"
            ;;
    esac
done
```
W zależności od wybranej opcji, wykonuje określone akcje, takie jak wyszukiwanie plików tekstowych, zabijanie procesów, wyświetlanie autora skryptu lub zakończenie działania skryptu. W przypadku wyboru nieprawidłowej opcji, wyświetla komunikat o błędzie.

<em>🐧WYGLĄD SKRYPTU🐧</em>

![alt text](https://cdn.discordapp.com/attachments/1026955720253505607/1186005358645358662/image.png?ex=6591ac90&is=657f3790&hm=330a03b62811787ef9fc722df22beb2e118bc122fef6ef60d20c8f7aaac9c2a3&)

Po wypisaniu "./skrypy.sh" wyświetla nam się menu tekstowe, z możliwością wyboru liczby od 1 do 5. 

1. Wyszukuje pliki tekstowe w określonym folderze i usuwa ostatnią linię.
2. Zabija 5 losowych procesów działających w tle, i je wyświetla.
3. Wyświetla autora skryptu.
4. Wyświetla klase autora.
5. Wychodzi.

Po wypisaniu nieprawidłowej liczby, czyli większej niż 5 bądź mniejszej niż 0, wyświetla się komunikat "Nieprawidłowa opcja", a menu tekstowe wyświetla się raz jeszcze znów prosząc nas o wybranie opcji. Na dole jest przykład: 

![alt text](https://cdn.discordapp.com/attachments/1026955720253505607/1186015662640992287/image.png?ex=6591b629&is=657f4129&hm=66bf562f8a406adb6ba9cf8b8326a86727e6d7e7441f6afb6593c08fe8c738ec&)

