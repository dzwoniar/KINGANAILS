# Kinga Nails — strona salonu z kreatorem rezerwacji

Prototyp strony salonu stylizacji paznokci z cennikiem pakietów, dodatkami i wieloetapowym wyborem wizyty. Projekt pokazuje pełną ścieżkę w przeglądarce: usługa → dzień → godzina → dane kontaktowe → podsumowanie i wiadomość WhatsApp.

**Status:** demo frontendowe. Wybrane terminy są blokowane lokalnie w tej przeglądarce dla bieżącego adresu strony, a wysłanie wiadomości wymaga osobnego działania użytkownika w WhatsApp.

## Funkcje

- Cennik renderowany z tablic `TIERS` i `EXTRAS`.
- Kreator wyboru usługi, dnia oraz godzinowego slotu.
- Generowanie do 14 dni pracy w horyzoncie kolejnych 28 dni, zgodnie z konfiguracją dni tygodnia; zajętość poszczególnych godzin jest sprawdzana w osobnym kroku.
- Lokalne blokowanie wybranych terminów przez `localStorage`.
- Budowanie odnośnika WhatsApp z przygotowaną treścią zgłoszenia.
- Rozwijane FAQ i responsywny układ.

## Technologia i struktura

[index.html](index.html) zawiera HTML, CSS oraz JavaScript bez frameworka. Funkcja `renderBooking()` aktualizuje kreator na podstawie stanu w pamięci. Dodatkową zależnością są Google Fonts: Playfair Display i DM Sans. Nie ma backendu, bazy danych, npm ani procesu budowania.

```text
.
├── index.html   # widok, konfiguracja, dane usług i logika rezerwacji
└── README.md
```

## Uruchomienie lokalne

Wymagane: Git, Python 3 i współczesna przeglądarka z włączonym JavaScript oraz `localStorage`.

```sh
git clone https://github.com/dzwoniar/KINGANAILS.git
cd KINGANAILS
python -m http.server 8000
```

Otwórz [http://localhost:8000](http://localhost:8000). Na Windows alternatywą jest `py -3 -m http.server 8000`. Zatrzymanie serwera: `Ctrl+C`. Internet jest potrzebny do Google Fonts i otwarcia WhatsApp, ale sama logika kreatora działa w przeglądarce.

## Konfiguracja i przepływ danych

Na początku skryptu znajdują się `PHONE` (obecnie numer zastępczy), `AVAIL_DAYS`, `START_H` i `END_H`. Cennik i FAQ można zmieniać w `TIERS`, `EXTRAS` oraz `FAQS`.

`confirmBooking()` zapisuje imię, telefon, usługę i termin pod kluczem `kinga_booked` w `localStorage`, po czym pokazuje podsumowanie. `waURL()` tworzy adres `wa.me` z zakodowaną wiadomością. Kod sam nie wysyła tej wiadomości. Dane demonstracyjne można usunąć przez skasowanie klucza `kinga_booked` w narzędziach deweloperskich przeglądarki.

## Ograniczenia

- Brak wspólnego kalendarza i synchronizacji: dwa urządzenia mogą wybrać ten sam termin. Ekran „Rezerwacja wysłana” nie oznacza potwierdzenia przez salon.
- Sloty mają odstęp jednej godziny; deklarowany czas usługi nie blokuje kolejnych slotów. Nie ma panelu zarządzania ani mechanizmu anulowania wizyt.
- Walidacja danych kontaktowych sprawdza tylko, czy pola nie są puste. Dane trafiają do pamięci przeglądarki i do treści linku WhatsApp.
- Pola użytkownika są wstawiane do szablonów HTML; przed rozbudową o rzeczywistą obsługę klientów należy zastąpić tę operację bezpiecznym renderowaniem tekstu i dodać walidację po stronie serwera.

Repozytorium nie zawiera automatycznych testów. Podczas lokalnego przeglądu można sprawdzić przechodzenie między krokami, odtwarzanie lokalnych zajętych terminów oraz podsumowanie bez wysyłania wiadomości.
