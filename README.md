# 🦷 W pełni Autonomiczna Recepcjonistka AI (Voicebot) dla Gabinetu Stomatologicznego

[![AI: Retell & OpenAI](https://img.shields.io/badge/AI-Retell%20%7C%20GPT--4o-black.svg)](#)
[![Automatyzacja: n8n](https://img.shields.io/badge/Automatyzacja-n8n-orange.svg)](#)
[![Komunikacja: Twilio](https://img.shields.io/badge/Komunikacja-Twilio-red.svg)](#)
[![Baza Danych: Google Calendar](https://img.shields.io/badge/Baza-Google%20Calendar-blue.svg)](#)

Zaawansowany system konwersacyjny oparty na sztucznej inteligencji, pełniący rolę wirtualnej recepcjonistki ("Asia"). Bot potrafi prowadzić naturalne, płynne rozmowy telefoniczne z pacjentami w języku polskim, w czasie rzeczywistym zarządzając kalendarzem wizyt oraz komunikacją SMS.

> **Ważne:** Ten projekt jest moim autorskim rozwiązaniem komercyjnym. Repozytorium służy jako Case Study architektury i logiki systemu. Kod źródłowy / Webhooki pozostają prywatne.

---

## 🎧 Posłuchaj jak działa "Asia" (Demo)

*Kliknij poniżej, aby rozwinąć nagrania lub zrzuty ekranu z działania systemu.*

<details>
  <summary><strong>▶️ Posłuchaj nagrania z rozmowy (Umówienie wizyty)</strong></summary>
  <br>
  link
</details>

<details>
  <summary><strong>▶️ Zobacz zrzut ekranu z automatyzacji w n8n</strong></summary>
  <br>
  <img width="1134" height="584" alt="Zrzut ekranu 2026-02-25 o 22 37 09" src="https://github.com/user-attachments/assets/b418f012-cb4f-40c9-818c-5067ee61eaed" />
</details>

---

## 🌟 Główne Funkcjonalności

System obsługuje 100% standardowych operacji telefonicznych na linii front-desk:

1. **Rezerwacja nowych wizyt:** Analiza czasu rzeczywistego (odrzucanie dat z przeszłości) i sprawdzanie wolnych luk w kalendarzu.
2. **Zmiana terminu (Reschedule):** Bezpieczne wyszukiwanie obecnej wizyty pacjenta po numerze telefonu, anulowanie starej i rezerwacja nowej.
3. **Anulowanie wizyty:** Mechanizm weryfikacji dwuetapowej zapobiegający przypadkowemu usunięciu wizyty innego pacjenta (zabezpieczenie logiką IF w n8n).
4. **Automatyczne powiadomienia SMS:** Po każdej udanej akcji (rezerwacja, zmiana, anulacja), system wysyła spersonalizowanego SMS-a z potwierdzeniem przez API Twilio.
5. **Obsługa zapytań ogólnych (Baza Wiedzy):** Udzielanie informacji o cenniku usług (np. leczenie kanałowe, higienizacja) w płynnej konwersacji.

---

## ⚙️ Architektura Systemu i Tech Stack

Projekt wymagał precyzyjnej orkiestracji kilku potężnych narzędzi:

* **Mózg i Głos (Retell AI + GPT-4o):** Odpowiada za rozpoznawanie mowy (STT), generowanie naturalnego, empatycznego głosu z zachowaniem polskiej składni (TTS) oraz utrzymywanie kontekstu rozmowy (LLM). Wymagało to zaawansowanego *Prompt Engineeringu*.
* **System Nerwowy (n8n):** Odbiera Webhooki z Retell AI (Tool Calls) i działa jako główny router. Za pomocą węzła `Switch` kieruje zapytania na 4 różne ścieżki (`check_availability`, `book_appointment`, `reschedule_appointment`, `cancel_appointment`).
* **Baza Danych (Google Calendar API):** Służy jako jedyne źródło prawdy (Single Source of Truth) o dostępności gabinetu. Logika n8n wylicza i filtruje zajęte bloki czasowe.
* **Infrastruktura Komunikacyjna (Twilio):** Obsługuje numery SIP/GSM dla połączeń przychodzących oraz odpowiada za natychmiastową wysyłkę SMS-ów transakcyjnych po zakończeniu operacji w kalendarzu.

## 🛡️ Zabezpieczenia i Edge Cases (Rozwiązane problemy)

* **Bezpieczeństwo danych (Failsafe Deletion):** Wdrożono specjalne filtry na ścieżce usuwania wydarzeń. System weryfikuje zawartość (`Summary`) wydarzenia z podanym numerem telefonu pacjenta. Eliminuje to ryzyko usunięcia wizyty innej osoby w przypadku zapytań ogólnych (tzw. "puste query").
* **Ochrona przed Halucynacjami:** Model AI otrzymał ścisłe ramy czasowe z dynamicznie wstrzykiwaną obecną datą i strefą czasową (`Europe/Warsaw`). Zapobiega to proponowaniu wizyt w przeszłości lub poza godzinami pracy.
* **Formatowanie danych (Data Normalization):** Numery dyktowane przez pacjentów są transformowane i normalizowane przed przesłaniem do n8n, aby zagwarantować spójne wyszukiwanie w kalendarzu.

---
**Zaprojektowane i wdrożone przez: _Paweł Zajczyk_ 📧 pawelzajczyk8@gmail.com
