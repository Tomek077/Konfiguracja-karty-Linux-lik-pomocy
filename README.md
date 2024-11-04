---

# **Lekcja: Konfiguracja adresu IP w Ubuntu i Windows 10 za pomocą GNS3**

---

## **Cel lekcji:**

Nauczysz się, jak skonfigurować adresy IP na dwóch maszynach wirtualnych połączonych bezpośrednio w programie **GNS3**:

- **Linux Server** z systemem Ubuntu.
- **Windows 10 Workstation**.

Dowiesz się również, jak umożliwić odpowiedzi na polecenie **ping** w systemie Windows 10.

---

## **Wprowadzenie:**

- **GNS3** to program umożliwiający symulację sieci komputerowych.
- Będziemy pracować na dwóch maszynach wirtualnych połączonych bezpośrednio.
- Ustawimy statyczne adresy IP na obu maszynach, aby mogły się komunikować.

---

## **Przygotowanie:**

- **Zainstaluj** program **GNS3** na swoim komputerze.
- **Utwórz** w GNS3 dwa urządzenia:
  - **Linux Server** z Ubuntu (wersja terminalowa).
  - **Windows 10 Workstation**.
- **Połącz** je bezpośrednio ze sobą w środowisku GNS3.

---

## **Konfiguracja adresów IP:**

### **Krok 1: Ustawienie adresu IP na Windows 10**

1. **Uruchom** maszynę wirtualną **Windows 10** w GNS3.

2. **Otwórz** **Ustawienia sieci**:

   - Kliknij na ikonę **Sieć** w prawym dolnym rogu ekranu.
   - Wybierz **Ustawienia sieci i Internet**.

3. **Przejdź** do **Centrum sieci i udostępniania**:

   - W sekcji **Stan sieci** kliknij **Centrum sieci i udostępniania**.

4. **Zmień ustawienia karty sieciowej**:

   - Kliknij na **Zmień ustawienia karty sieciowej** po lewej stronie.

5. **Otwórz właściwości interfejsu sieciowego**:

   - Znajdź **Ethernet** (lub odpowiedni interfejs).
   - **Kliknij prawym przyciskiem** myszy i wybierz **Właściwości**.

6. **Konfiguruj protokół TCP/IPv4**:

   - Zaznacz **Protokół internetowy w wersji 4 (TCP/IPv4)**.
   - Kliknij **Właściwości**.

7. **Ustaw statyczny adres IP**:

   - Wybierz opcję **Użyj następującego adresu IP**.
   - **Adres IP**: **192.168.0.51**
   - **Maska podsieci**: **255.255.255.0**
   - **Brama domyślna**: **192.168.0.1** (lub pozostaw puste, jeśli nie używasz bramy).
   - **Preferowany serwer DNS**: **8.8.8.8** (lub pozostaw puste).

8. **Zapisz ustawienia**:

   - Kliknij **OK** w obu oknach, aby zapisać zmiany.

---

### **Krok 2: Umożliwienie odpowiedzi na polecenie ping w Windows 10**

1. **Otwórz Zaporę Windows Defender**:

   - Kliknij **Start** i wpisz **Zapora Windows Defender z zabezpieczeniami zaawansowanymi**.
   - Uruchom tę aplikację.

2. **Włącz regułę odpowiadania na ping**:

   - W lewym panelu wybierz **Reguły przychodzące**.
   - Znajdź regułę **Udostępnianie plików i drukarek (Echo - ruch przychodzący ICMPv4)**.
     - **Uwaga**: Możesz użyć filtra lub posortować listę, aby łatwiej znaleźć regułę.
   - **Kliknij prawym przyciskiem** myszy na regule i wybierz **Włącz**.

3. **Zamknij** okno zapory.

---

### **Krok 3: Ustawienie adresu IP na Linux Server**

1. **Uruchom** maszynę wirtualną **Linux Server** w GNS3.

2. **Otwórz terminal**:

   - Jeśli jesteś w trybie tekstowym, już jesteś w terminalu.
   - Jeśli nie, naciśnij **Ctrl + Alt + T**.

3. **Instalacja mc (jeśli nie jest zainstalowany)**:

   ```bash
   sudo apt-get update
   sudo apt-get install mc
   ```

   - Wpisz swoje hasło, jeśli zostaniesz o to poproszony.

4. **Uruchomienie mc**:

   ```bash
   mc
   ```

5. **Przejdź do katalogu /etc/netplan**:

   - W **lewy panel** mc powinien być widoczny katalog.
   - Jeśli nie jesteś w katalogu głównym **/**, naciśnij kilka razy **Backspace**, aby tam wrócić.
   - Przejdź do **etc** -> **netplan**.

6. **Sprawdzenie istniejących plików konfiguracyjnych**:

   - Jeśli są pliki z rozszerzeniem **.yaml**, zmień ich nazwy, dodając na końcu **.bak**:
     - **Zaznacz** plik.
     - Naciśnij **F6** (zmień nazwę).
     - Dodaj **.bak** na końcu nazwy.
     - Naciśnij **Enter**.

7. **Skopiowanie przykładowego pliku konfiguracyjnego**:

   - W **prawym panelu** przejdź do **/usr/share/doc/netplan/examples**.
   - Znajdź plik **static.yaml**.
   - **Zaznacz** plik i naciśnij **F5** (kopiuj).
   - Skopiuj plik do **/etc/netplan/** w **lewym panelu**.
   - **Nie zmieniaj nazwy** pliku podczas kopiowania.

---

### **Krok 4: Znalezienie nazwy interfejsu sieciowego**

1. **Wyjdź** z mc, naciskając **F10**.

2. **W terminalu** wpisz:

   ```bash
   ip addr show
   ```

3. **Zapisz nazwę interfejsu**:

   - Poszukaj interfejsu z adresem **inet** (np. **inet 192.168.0.X/24**).
   - Nazwa interfejsu to ciąg znaków przed dwukropkiem, np.:

     ```
     2: enp0s3: <...>
     ```

   - W tym przykładzie nazwa interfejsu to **enp0s3**.
   - **Zapisz** tę nazwę.

---

### **Krok 5: Edycja pliku konfiguracyjnego**

1. **Uruchom ponownie mc**:

   ```bash
   mc
   ```

2. **Przejdź do /etc/netplan** w **lewym panelu**.

3. **Zaznacz** plik **static.yaml** i naciśnij **F4** (edytuj).

4. **Dostosuj plik konfiguracyjny**:

   - **Zmodyfikuj nazwę interfejsu** w sekcji **ethernets**:

     ```yaml
     network:
       version: 2
       renderer: networkd
       ethernets:
         enp0s3:
     ```

     - Zastąp **enp0s3** swoją nazwą interfejsu.

   - **Ustaw statyczny adres IP**:

     ```yaml
     addresses: [192.168.0.50/24]
     ```

   - **Wyłącz DHCP**, jeśli jest włączony:

     ```yaml
     dhcp4: no
     ```

   - **Ustaw bramę domyślną** (jeśli potrzebna):

     ```yaml
     gateway4: 192.168.0.1
     ```

   - **Ustaw serwery DNS**:

     ```yaml
     nameservers:
       addresses: [8.8.8.8, 8.8.4.4]
     ```

5. **Upewnij się**, że zachowujesz poprawny format YAML:

   - **Nie używaj** tabulatorów, tylko spacje.
   - Zachowuj **wcięcia** (najczęściej 2 spacje).

6. **Zapisz zmiany**:

   - Naciśnij **F2** (zapisz).
   - Naciśnij **F10** (wyjdź z edytora).

---

### **Krok 6: Zastosowanie nowej konfiguracji**

1. **Wyjdź** z mc, naciskając **F10**.

2. **W terminalu** wpisz:

   ```bash
   sudo netplan apply
   ```

3. **Sprawdź, czy nie ma błędów**:

   - Jeśli pojawią się komunikaty o błędach, wróć do edycji pliku i popraw je.

---

### **Krok 7: Sprawdzenie ustawień sieci**

1. **W terminalu** wpisz:

   ```bash
   ip addr show
   ```

2. **Upewnij się**, że adres IP został ustawiony na **192.168.0.50/24**.

---

## **Testowanie połączenia**

### **Krok 8: Sprawdzenie komunikacji między maszynami**

#### **Z Windows 10:**

1. **Otwórz Wiersz polecenia**:

   - Kliknij **Start**, wpisz **cmd**, i uruchom **Wiersz polecenia**.

2. **Wykonaj polecenie ping**:

   ```bash
   ping 192.168.0.50
   ```

3. **Sprawdź odpowiedzi**:

   - Jeśli konfiguracja jest poprawna, powinieneś otrzymać odpowiedzi od **192.168.0.50**.

#### **Z Linux Server:**

1. **W terminalu** wpisz:

   ```bash
   ping 192.168.0.51
   ```

2. **Sprawdź odpowiedzi**:

   - Powinieneś otrzymać odpowiedzi od **192.168.0.51**.

---

## **Podsumowanie**

Gratulacje! Udało Ci się:

- Skonfigurować statyczne adresy IP na obu maszynach.
- Umożliwić odpowiedzi na polecenie **ping** w Windows 10.
- Sprawdzić komunikację między maszynami w środowisku GNS3.

---

## **Porady dla Ciebie:**

- **Czytaj powoli** każdą instrukcję.
- **Sprawdzaj dwa razy** wprowadzone dane.
- **Notuj** ważne informacje.
- **Poproś o pomoc**, jeśli czegoś nie rozumiesz.
- **Nie spiesz się** – dokładność jest ważniejsza niż tempo.

---

## **Przydatne informacje:**

- **Adresy IP** muszą być w tej samej podsieci, aby maszyny mogły się komunikować bezpośrednio.
  - **Linux Server**: **192.168.0.50**
  - **Windows 10**: **192.168.0.51**
- **Maska podsieci**: **255.255.255.0** (co odpowiada **/24**)
- **Brama domyślna**: Jeśli nie masz routera w sieci, możesz ją pominąć.
- **Serwery DNS**: Ustaw, jeśli planujesz dostęp do internetu.

---

## **Ćwiczenie dodatkowe:**

- **Zmodyfikuj adresy IP** na inne w tej samej podsieci (np. **192.168.0.100** i **192.168.0.101**) i sprawdź połączenie.
- **Spróbuj zablokować i odblokować** odpowiedzi na **ping** w Windows 10, aby zobaczyć różnicę.

---

# **Dziękuję za uwagę i powodzenia!**

---
