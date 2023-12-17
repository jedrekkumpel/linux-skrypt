<br/>
<p align="center">
  <a href="https://github.com/jedrekkumpel/linux-skrypt">
    <img src="https://media.discordapp.net/attachments/1026955720253505607/1186014700966781020/image.jpg?ex=6591b543&is=657f4043&hm=a398c2629d47343cff77b1c79428eec1ef20eaaf31376a96ae038307df4f9a64&=&format=webp&width=234&height=273" alt="Logo" width="80" height="80">
  </a>
 
  <h3 align="center">Linux Menu</h3>
 
  <p align="center">
    Multitool wykonany w celu rozwijania swoich umiejętności w języku bash.
    <br/>
    <br/>
  </p>
</p>
 
![Downloads](https://img.shields.io/github/downloads/jedrekkumpel/linux-skrypt/total) ![Contributors](https://img.shields.io/github/contributors/jedrekkumpel/linux-skrypt?color=dark-green) ![Issues](https://img.shields.io/github/issues/jedrekkumpel/linux-skrypt)
 
## 🐧O skrypcie 🐧
 
![Screen Shot](https://media.discordapp.net/attachments/1026955720253505607/1186005358645358662/image.png?ex=6591ac90&is=657f3790&hm=330a03b62811787ef9fc722df22beb2e118bc122fef6ef60d20c8f7aaac9c2a3&=&format=webp&quality=lossless&width=527&height=123)
 
Jest to prosty projekt napsiany w języku bash, ma za zadanie:
 
<ol type ="1">
<li><u>Wyszukać pliki tekstowe w określonym folderze i usunąć ostatnią linię.</u></li>
<li><u>Zabić 5 losowych procesów działających w tle, i je wyświetl. </u> </li>
<li><u>Wyświetlić autora skryptu.</u> </li>
<li><u>Wyświetlić klasę autora.</u></li>
<li><u>Dać opcje wyjścia ze skryptu.</u></li>
</ol>
 
 
## 🐧Kod Skryptu:🐧
 
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
 
## 🐧Poniżej postaram się wyjaśnić jak działają mniej zrozumiałe linie kodu🐧
 
```python
#!/bin/bash
```
Skrypt rozpoczyna się od deklaracji powłoki, w tym przypadku jest to powłoka Bash.

## Kolor skryptu 

```python
RED='\033[0;31m'
NC='\033[0m'
echo -e "${RED}"
```
Definiuje zmienne RED i NC, które zawierają kody kolorów w formacie ANSI. Następnie wyświetla kolor czerwony za pomocą zmiennej RED.
Te 3 linie kodu nie są potrzebne, dodałem je tylko ze względów estetycznych, oraz by po wywołaniu skryptu wyróżniał się od reszty tekstu.

## Usuwanie ostatniej linijki w pliku tekstowym

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

## Wyszukiwanie plików i katalogów

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

## Proste menu tekstowe 

```python
while true; do
    echo -e                                             "====Menu===="
    echo -e             "1. Wyszukaj pliki tekstowe w określonym folderze i usuń ostatnią linię."
    echo -e             "2. Zabij 5 losowych procesów działających w tle, i je wyświetl."
    echo -e             "3. Wyświetl autora skryptu."
    echo -e             "4. Wyświetl klase autora."
    echo -e             "5. Wyjdź."
    read -p "Wybierz opcję: " option
```
Rozpoczyna nieskończoną pętlę, wyświetla menu proste menu tekstowe, prosi użytkownika o wybór opcji i oczekuje na wprowadzenie opcji przez użytkownika.
na początku może być niezrozumiałe to, dlaczego echo jest dodane z opcją "-e". Jak wiemy opcja "-e" interpretuje znaki specjalne, więc menu tekstowe 
które jest odpowiednio wycentrowane za pomocą tabulatora bądź spacji, będzie wyglądać lepiej i przejrzściej niż menu które by było wypisane od lewej strony.

## Opcje wyboru menu tekstowego

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

1) Opcja pierwsza
   
🐧 Na początku wczytuje ścieżkę do folderu za pomocą polecenia "read -p "Podaj ścieżkę do folderu: " folder"
🐧 następnie wczytuje słowo kluczowe za pomocą polecenia "read -p "Podaj słowo kluczowe: " keyword"
🐧 Na końcu Następnie wywołuje funkcję "search_files" przekazując wczytaną ścieżkę do folderu i słowo kluczowe jako argumenty.

3) Opcja druga

🐧pids=$(ps aux | awk '{print $2}' | shuf -n 5) Pobiera listę wszystkich procesów uruchomionych w systemie za pomocą polecenia ps aux. Następnie używa polecenia awk '{print $2}' do wyodrębnienia tylko drugiej kolumny, która zawiera identyfikatory procesów (PID). Na końcu, za pomocą polecenia shuf -n 
 5, losowo wybiera 5 identyfikatorów procesów spośród wszystkich znalezionych.

🐧 for pid in $pids; do kill "$pid" Uruchamia pętlę for, która iteruje przez każdy identyfikator procesu (PID) z listy pids. Wewnątrz pętli używa polecenia kill "$pid" do zabicie każdego procesu o wybranym identyfikatorze.

🐧 echo "Zabito 5 losowych procesów, oto zabite procesy: " Wyświetla komunikat informujący o zabicu 5 losowych procesów.

🐧 ps aux | shuf -n 5 Wyświetla losowo wybrane 5 linii z listy wszystkich procesów uruchomionych w systemie. Ten fragment kodu wykonuje zabijanie 5 losowo wybranych procesów i wyświetla informację o zabitych procesach oraz losowo wybrane 5 linii z listy wszystkich procesów.

3)

🐧 echo "autorem jest Gabriel Jędrzejczyk 2TiM" wypisuję nam na ekran imię i nazwisko autora za pomocą komendy echo

4) 

🐧 echo "klasa autora: 2TIM" tak samo jak opcja 3, wypisuję na ekran za pomoca komendy echo klasę autora

5)

break służy do przerwania działania pętli, czyli wyłącza nam skrypt

*)

 echo "Nieprawidłowa opcja" jak wspominałem już wyżej na początku repozytorium, jeśli wypiszemy inną opcję niż 1,2,3,4,5, wyskakuje błąd pod nazwą "Nieprawidłowa opcja" i skrypt znów się uruchamia.

## 🐧Działanie skryptu:🐧
 
![alt text](https://cdn.discordapp.com/attachments/1026955720253505607/1186005358645358662/image.png?ex=6591ac90&is=657f3790&hm=330a03b62811787ef9fc722df22beb2e118bc122fef6ef60d20c8f7aaac9c2a3&)
 
Po wypisaniu "./skrypy.sh" wyświetla nam się menu tekstowe, z możliwością wyboru liczby od 1 do 5. 
 
1. Wyszukuje pliki tekstowe w określonym folderze i usuwa ostatnią linię.
2. Zabija 5 losowych procesów działających w tle, i je wyświetla.
3. Wyświetla autora skryptu.
4. Wyświetla klase autora.
5. Wychodzi.
 
Po wypisaniu nieprawidłowej liczby, czyli większej niż 5 bądź mniejszej niż 0, wyświetla się komunikat "Nieprawidłowa opcja", a menu tekstowe wyświetla się raz jeszcze znów prosząc nas o wybranie opcji. Na dole jest przykład: 
 
![alt text](https://cdn.discordapp.com/attachments/1026955720253505607/1186015662640992287/image.png?ex=6591b629&is=657f4129&hm=66bf562f8a406adb6ba9cf8b8326a86727e6d7e7441f6afb6593c08fe8c738ec&)
 
## <em>🐧OPCJA PIERWSZA🐧</em>
 
Po wybraniu opcji numer 1, wyświetla się komunikat proszący nas o podanie ścieżki do folderu, z którego skrypt ma znaleźć pliki tekstowe i usunąć z nich ostatnią linijkę. Po podaniu ścieżki ukazuje się nam komunikat by podać słowo kluczowe, nie trzeba go podawać, jednak gdy w folderze jest więcej niż jeden plik, a chcemy by skrypt usunął ostatnią linię z konkretnego pliku tekstowego w którym znajduję sie to konkretne słowo to trzeba je wpisać. Przykład z zastosowaniem słowa kluczowego: 
 
![alt text](https://cdn.discordapp.com/attachments/1026955720253505607/1186020121882411170/image.png?ex=6591ba50&is=657f4550&hm=3f8d28226cda17366eb9288d76a90bef7d57fd09560017bbf0c8e1ba72472807&)
 
Oraz przykład bez podania słowa kluczowego: 
 
![alt text](https://cdn.discordapp.com/attachments/1026955720253505607/1186022983509213304/image.png?ex=6591bcfa&is=657f47fa&hm=f3dddc83da16f40ad356f775822d938d0e09345b4c5ff6ec10e8d266db7a2b1a&)
 
Jak można zauważyć, skrypt usunął ostatnie linijki z wszystkich plików tekstowych znajdujących się w folderze dokumenty, w tym przypadku z plików: plik1.txt oraz plik2.txt
 
## <em>🐧OPCJA DRUGA🐧</em>
 
Podczas gdy wybierzemy opcję numer 2, skrypt zabije losowo 5 procesów oraz je wypisze. 
 
![alt text](https://cdn.discordapp.com/attachments/1026955720253505607/1186024237899730998/image.png?ex=6591be25&is=657f4925&hm=5ea55ab323b3b55f1789b5e349aa2d9da34c05c74f99bdfdf9217204f49409a2&)
 
## <em>🐧OPCJA TRZECIA🐧</em>
 
Opcja trzecia wypisuję imię oraz nazwisko twórcy skryptu.
 
![alt text](https://cdn.discordapp.com/attachments/1026955720253505607/1186025355329425468/image.png?ex=6591bf2f&is=657f4a2f&hm=11ce73286eb8c1e316fe5bea52161cdd00dc25512201ab9183c18c5f697ef1f4&) 
 
## <em>🐧OPCJA CZWARTA🐧</em>
 
Opcja czwarta wypisuję klasę twórcy skryptu. 
 
![alt text](https://cdn.discordapp.com/attachments/1026955720253505607/1186025931349966848/image.png?ex=6591bfb9&is=657f4ab9&hm=ff5db2ff26972c07673ab28f0ef591f6c9e655742ddf675244e319f90063a475&)
 
## <em>🐧OPCJA PIĄTA🐧</em>
 
Ostatnia opcja, najzywczajniej wychodzi ze skryptu, czyli kończy pętlę utworzoną przez komende "while true; do" 
 
![alt text](https://cdn.discordapp.com/attachments/1026955720253505607/1186026580087144569/image.png?ex=6591c054&is=657f4b54&hm=c2f2543e8bb52d50f9364d2d69fe761c585b63843628d0d9bcd28780e8a7f0d7&)
 
## 🐧AUTOR🐧
 
* **Gabriel Jędrzejczyk** - **2TIM**  - [jedrekkumpel](https://github.com/jedrekkumpel/)
