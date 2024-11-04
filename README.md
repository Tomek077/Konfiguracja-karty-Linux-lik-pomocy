---

# **Lekcja: Konfiguracja adresu IP w Ubuntu za pomocą mc**

---

## **Cel lekcji:**

Nauczysz się, jak skonfigurować adres IP w systemie Ubuntu, używając menedżera plików **mc** (Midnight Commander) w terminalu. Pokażemy Ci krok po kroku, jak skopiować przykładowy plik konfiguracyjny, dostosować go do swoich potrzeb oraz jak uniknąć potencjalnych błędów.

---

## **Wprowadzenie:**

- **mc** to prosty w obsłudze menedżer plików działający w terminalu.
- Będziemy pracować z plikami konfiguracyjnymi sieci, aby ustawić statyczny adres IP.
- Ważne jest dokładne wykonywanie kroków i czytanie instrukcji ze zrozumieniem.

---

## **Przygotowanie:**

- **Upewnij się**, że masz dostęp do konta z uprawnieniami administratora (sudo).
- **Przygotuj** notatnik i długopis do zapisywania ważnych informacji.

---

## **Krok po kroku:**

### **Krok 1: Uruchomienie terminala**

- Znajdź na pulpicie ikonę **Terminal**.
- Kliknij na nią, aby otworzyć okno terminala.
- **Lub** naciśnij jednocześnie klawisze **Ctrl + Alt + T**.

---

### **Krok 2: Instalacja mc (jeśli nie jest zainstalowany)**

- W terminalu wpisz komendę:

  ```bash
  sudo apt-get install mc
  ```

- Naciśnij **Enter**.
- Jeśli pojawi się prośba o hasło, wpisz swoje hasło i naciśnij **Enter**.
- Poczekaj, aż instalacja się zakończy.

---

### **Krok 3: Uruchomienie mc**

- W terminalu wpisz:

  ```bash
  mc
  ```

- Naciśnij **Enter**.
- Otworzy się program **mc** z dwoma panelami.

---

### **Krok 4: Przejście do katalogu /etc/netplan**

- **Lewy panel**:

  - Upewnij się, że jesteś w katalogu głównym **/**.
  - Jeśli nie, wybierz katalog **".."** (dwie kropki) kilka razy, aż dojdziesz do **/**.

- Wybierz katalog **etc**:

  - Za pomocą strzałek na klawiaturze przejdź do **etc**.
  - Naciśnij **Enter**.

- Przejdź do katalogu **netplan**:

  - Znajdź **netplan**.
  - Naciśnij **Enter**.

---

### **Krok 5: Sprawdzenie istniejących plików konfiguracyjnych**

- W katalogu **/etc/netplan** sprawdź, czy są pliki z rozszerzeniem **.yaml**.

  - Przykład: **01-netcfg.yaml**

- **Jeśli taki plik istnieje**, zrób z nim kopię zapasową:

  - **Zaznacz plik** (użyj strzałek).
  - Naciśnij **F6** (przenieś/zmień nazwę).
  - Dodaj na końcu nazwy pliku **.bak**.

    - Przykład:

      ```
      01-netcfg.yaml.bak
      ```

  - Naciśnij **Enter**, aby potwierdzić.

---

### **Krok 6: Przejście do katalogu z przykładami**

- **Prawy panel**:

  - Naciśnij **Tab**, aby przełączyć się do prawego panelu.
  - Upewnij się, że jesteś w katalogu głównym **/**.
  - Przejdź do katalogu **usr**:

    - Wybierz **usr** i naciśnij **Enter**.

  - Następnie do **share**:

    - Wybierz **share** i naciśnij **Enter**.

  - Następnie do **doc**:

    - Wybierz **doc** i naciśnij **Enter**.

  - Następnie do **netplan**:

    - Wybierz **netplan** i naciśnij **Enter**.

  - Na koniec do **examples**:

    - Wybierz **examples** i naciśnij **Enter**.

---

### **Krok 7: Skopiowanie przykładowego pliku konfiguracyjnego**

- W prawym panelu znajdź plik **static.yaml**.
- **Zaznacz plik** i naciśnij **F5** (kopiuj).
- Pojawi się okno kopiowania.

  - Upewnij się, że:

    - **Źródło**: /usr/share/doc/netplan/examples/static.yaml
    - **Cel**: /etc/netplan/ (lewy panel)

- **Zmień nazwę pliku na**:

  ```
  01-netcfg.yaml
  ```

- Naciśnij **Enter**, aby potwierdzić kopiowanie.

---

### **Krok 8: Edycja nowego pliku konfiguracyjnego**

- Wróć do **lewego panelu** (naciśnij **Tab**).
- Znajdź plik **01-netcfg.yaml**.
- **Zaznacz plik** i naciśnij **F4** (edytuj).

---

### **Krok 9: Dostosowanie pliku do swoich potrzeb**

- W edytorze zobaczysz zawartość pliku.

- **Znajdź i zmień następujące linie:**

  **a) Ustawienie adresu IP:**

  ```yaml
  addresses: [192.168.1.100/24]
  ```

  - Zmień **192.168.1.100/24** na swój adres IP.

    - Przykład:

      ```yaml
      addresses: [192.168.0.50/24]
      ```

  **b) Ustawienie bramy sieciowej (gateway):**

  ```yaml
  gateway4: 192.168.1.1
  ```

  - Zmień **192.168.1.1** na adres bramy w Twojej sieci.

    - Przykład:

      ```yaml
      gateway4: 192.168.0.1
      ```

  **c) Ustawienie serwerów DNS:**

  ```yaml
  nameservers:
    addresses: [8.8.8.8, 8.8.4.4]
  ```

  - Możesz pozostawić domyślne serwery lub wpisać inne.

    - Przykład:

      ```yaml
      nameservers:
        addresses: [1.1.1.1, 8.8.8.8]
      ```

- **Upewnij się**, że zachowujesz poprawny format pliku YAML.

  - **Ważne:** Zwróć uwagę na wcięcia i spacje. Nie używaj tabulatorów.

---

### **Krok 10: Zapisanie zmian**

- Po wprowadzeniu zmian naciśnij **F2** (zapisz).
- Naciśnij **F10** (wyjdź z edytora).

---

### **Krok 11: Zastosowanie nowej konfiguracji**

- Wyjdź z programu **mc**, naciskając **F10**.
- W terminalu wpisz:

  ```bash
  sudo netplan apply
  ```

- Naciśnij **Enter**.

- **Jeśli pojawią się błędy**, sprawdź plik konfiguracyjny pod kątem:

  - Poprawności wcięć.
  - Literówek w nazwach parametrów.

---

### **Krok 12: Sprawdzenie ustawień sieci**

- W terminalu wpisz:

  ```bash
  ip addr show
  ```

- Naciśnij **Enter**.
- Znajdź interfejs sieciowy (np. **enp0s3**, **eth0**).
- Sprawdź, czy adres IP jest taki, jaki ustawiłeś.

---

## **Podsumowanie**

Gratulacje! Właśnie skonfigurowałeś statyczny adres IP w systemie Ubuntu za pomocą **mc**.

---

## **Porady dla Ciebie:**

- **Czytaj powoli** każde polecenie i instrukcję.
- **Sprawdzaj dwa razy** wprowadzone dane, zwłaszcza adresy IP.
- **Notuj** ważne komendy i informacje.
- **Jeśli coś jest niejasne**, poproś o pomoc nauczyciela lub kolegę.
- **Nie spiesz się**. Ważne jest zrozumienie każdego kroku.

---

## **Przydatne komendy:**

- **Instalacja mc:**

  ```bash
  sudo apt-get install mc
  ```

- **Uruchomienie mc:**

  ```bash
  mc
  ```

- **Zastosowanie konfiguracji sieci:**

  ```bash
  sudo netplan apply
  ```

- **Sprawdzenie adresów IP:**

  ```bash
  ip addr show
  ```

---

## **Dodatkowe informacje:**

- **Format plików YAML:**

  - Używaj **spacji**, nie tabulatorów.
  - Zachowuj odpowiednie **wcięcia** (najczęściej 2 spacje).
  - Błędy w formatowaniu mogą spowodować problemy z konfiguracją.

- **Przykład poprawnego pliku 01-netcfg.yaml:**

  ```yaml
  network:
    version: 2
    renderer: networkd
    ethernets:
      enp0s3:
        addresses: [192.168.0.50/24]
        gateway4: 192.168.0.1
        nameservers:
          addresses: [1.1.1.1, 8.8.8.8]
  ```

  - **Uwaga:** Nazwa interfejsu (**enp0s3**) może być inna na Twoim komputerze. Sprawdź ją poleceniem:

    ```bash
    ip addr show
    ```

---

## **Ćwiczenie dodatkowe:**

- Spróbuj zmienić adres IP na inny i ponownie zastosować konfigurację.
- Sprawdź połączenie z 192.168.0.51

  ```bash
  ping -c 4 192.168.0.51
  ```

---

## **Pamiętaj:**

- **Praktyka czyni mistrza**. Im więcej ćwiczysz, tym lepiej zrozumiesz temat.
- **Nie bój się popełniać błędów**. Ważne jest, aby je poprawiać i wyciągać z nich wnioski.
- **Bądź cierpliwy** i wyrozumiały dla siebie.

---

# **Dziękuję za uwagę i powodzenia!**

---
