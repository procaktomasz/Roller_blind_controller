# Roller Blind Controller

## Opis projektu
Projekt `Roller Blind Controller` zawiera konfigurację ESPHome dla sterownika rolety z silnikiem krokowym sterowanym przez płytkę ESP8266 (Wemos D1 mini) i układ ULN2003. Konfiguracja pozwala na integrację z Home Assistant oraz zdalny dostęp przez API, OTA i wbudowany serwer WWW.

## Struktura repozytorium
- `roller_blind_controller.yaml`: główna definicja urządzenia wraz z logiką sterowania, kalibracją i konfiguracją połączenia.
- `secrets.yaml`: dane logowania do sieci Wi-Fi, OTA oraz serwera WWW (plik nie jest wersjonowany, należy utworzyć go lokalnie).

## Kluczowe funkcje
- Sterowanie roletą jako encja `cover` w Home Assistant z pełną informacją o pozycji.
- Silnik krokowy 28BYJ-48 sterowany poprzez ULN2003 z regulowaną prędkością, przyspieszeniem i hamowaniem.
- Zapamiętywanie pozycji w pamięci flash oraz automatyczne (konfigurowalne) usypianie napędu po zakończeniu ruchu.
- Natychmiastowe raportowanie do HA stanu `opening` / `closing` / `idle`, umożliwiające automatyzacje reagujące na start ruchu rolety.
- Wbudowany serwer WWW z autoryzacją, protokół API i aktualizacje OTA.
- Tryb kalibracji z przyciskami ustawiającymi pozycję otwarcia i zamknięcia oraz z wyborem kierunku obrotu.

## Parametry konfiguracji
- `cover_name`, `esphome_name`, `id_cover` – identyfikatory wykorzystywane w Home Assistant i ESPHome.
- `step_total`, `max_speed`, `accel`, `decel` – podstawowe parametry ruchu silnika.
- `relax_delay` – czas (np. `5s`) po którym sterownik silnika przechodzi w tryb uśpienia po zakończeniu ruchu.

## Wymagania sprzętowe
- Moduł ESP8266 (np. Wemos D1 mini).
- Sterownik ULN2003.
- Silnik krokowy 28BYJ-48 (lub zgodny) sprzęgnięty z roletą.
- Zasilanie 5 V zapewniające odpowiedni zapas prądowy dla silnika.
- Połączenie z siecią Wi-Fi 2,4 GHz.

## Instrukcja kalibracji rolety
1. W interfejsie ESPHome lub Home Assistant włącz przełącznik `Kalibracja`. Tryb kalibracji odblokowuje zmianę kierunku oraz zapisywanie nowych granic.
2. Jeśli chcesz rozpocząć od ustawień domyślnych, naciśnij przycisk `Kalibracja - reset kalibracji`. Silnik ustawi pozycję początkową i zresetuje liczbę kroków.
3. Wybierz kierunek pracy silnika w selektorze `Kalibracja - kierunek silnika`. Zmiana jest możliwa tylko w trybie kalibracji.
4. Ustaw fizycznie roletę w pozycji całkowicie otwartej. Następnie naciśnij `Kalibracja - ustaw jako otwarta`, aby zapisać pozycję 0 kroków.
5. Opuść roletę do pozycji całkowicie zamkniętej (już z użyciem sterowania w HA lub interfejsu WWW). Kliknij `Kalibracja - ustaw jako zamknięta`, aby zapisać aktualną liczbę kroków jako zakres pełnego zamknięcia.
6. Opcjonalnie włącz przełącznik `Kalibracja - auto usypianie`, aby po ruchu automatycznie uśpić sterownik silnika (zapobiega nagrzewaniu silnika i oszczędza energię). Włączenie powoduje utrzymywanie rolety w stałym docisku, więc uniemożliwia ręczny ruch rolety.
7. Wyłącz przełącznik `Kalibracja`, aby powrócić do normalnego trybu pracy. Sterownik zapamięta nowe ustawienia i wykorzysta je przy każdym kolejnym ruchu. Odczekaj około minutę, aby zapewnić zapis danych w pamięci urządzenia. Po tym czasie ustawienia są trwałe nawet po zaniku zasilania.

## Następne kroki
- Uzupełnij plik `secrets.yaml` lokalnymi danymi (SSID, hasła, klucze OTA) w ESPHome.
- Skonfiguruj integrację ESPHome w Home Assistant i dodaj urządzenie zgodnie z nazwą `esphome_name` z pliku `roller_blind_controller.yaml` (zmień ją, jeśli potrzebujesz innego identyfikatora).
- Przetestuj otwieranie, zamykanie oraz zatrzymywanie rolety, aby potwierdzić poprawność kalibracji.
- Dostosuj `relax_delay`, jeśli potrzebujesz innego czasu podtrzymania zasilania po ruchu.
- W razie potrzeby ustaw prędkość silnika która będzie właściwa dla twojego projektu.

## Obudowa
- Obudowa wykorzystana w projekcie bazuje na modelu udostępnionym na Thingiverse: [ESP8266 / ULN2003 Stepper Case](https://www.thingiverse.com/thing:4246155).

## Licencja
- Projekt jest dostępny na licencji MIT, pełna treść znajduje się w pliku `LICENSE`.
---

# Roller Blind Controller (English Version)

## Project Overview
The `Roller Blind Controller` repository contains an ESPHome configuration for a roller blind controller based on a stepper motor driven by an ESP8266 board (Wemos D1 mini) and a ULN2003 driver. The setup integrates with Home Assistant and exposes remote access through the ESPHome API, OTA updates, and the built-in web server.

## Repository Structure
- `roller_blind_controller.yaml`: main device definition, automation logic, calibration helpers, and connectivity settings.
- `secrets.yaml`: credentials for Wi-Fi, OTA, and the web server (not versioned; create locally).

## Key Features
- Home Assistant `cover` entity with full position reporting.
- 28BYJ-48 stepper motor driven via ULN2003 with configurable speed, acceleration, and deceleration profiles.
- Position stored in flash memory with automatic (configurable) motor sleep once motion completes.
- Immediate `opening` / `closing` / `idle` state updates, so Home Assistant automations can react as soon as motion begins.
- Built-in password-protected web server, API access, and OTA updates.
- Calibration mode with buttons for defining open/closed endpoints and a direction selector.

## Configuration Parameters
- `cover_name`, `esphome_name`, `id_cover` – identifiers used across ESPHome and Home Assistant.
- `step_total`, `max_speed`, `accel`, `decel` – core motion parameters for the stepper motor.
- `relax_delay` – delay (e.g., `5s`) before the driver is put to sleep after the blind stops moving.

## Hardware Requirements
- ESP8266 module (e.g., Wemos D1 mini).
- ULN2003 driver board.
- 28BYJ-48 stepper motor (or compatible) mechanically linked to the blind.
- Stable 5 V power supply with sufficient current headroom for the motor.
- 2.4 GHz Wi-Fi network.

## Blind Calibration Guide
1. In the ESPHome or Home Assistant UI, enable the `Kalibracja` (Calibration) switch. The calibration mode unlocks direction changes and endpoint updates.
2. To start from defaults, press the `Kalibracja - reset kalibracji` button. The motor moves to the reference position and resets the step counter.
3. Select the motor direction via `Kalibracja - kierunek silnika`. Direction changes are only allowed while calibration mode is active.
4. Move the blind to the fully open position. Press `Kalibracja - ustaw jako otwarta` to store the zero-step position.
5. Close the blind completely (using HA or the web interface). Press `Kalibracja - ustaw jako zamknieta` to save the current step count as the closed limit.
6. Optionally enable `Kalibracja - auto usypianie` to put the driver to sleep after each movement. This prevents motor heating and reduces power draw but keeps the blind held firmly, making manual adjustment impossible.
7. Disable the `Kalibracja` switch to return to normal operation. Wait roughly one minute to ensure the new limits are written to flash; they then persist across power outages.

## Next Steps
- Populate the `secrets.yaml` file with your local credentials (SSID, passwords, OTA keys).
- Register the device in Home Assistant using the `esphome_name` defined in `roller_blind_controller.yaml` (change it if you prefer a different identifier).
- Test opening, closing, and stopping the blind to confirm the calibration works as expected.
- Tune `relax_delay` if you need the driver to stay energized longer (or release sooner) after movement.
- Adjust the motor speed as needed to match your project's requirements.

## Enclosure
- The physical case used in this project is based on the model published on Thingiverse: [ESP8266 / ULN2003 Stepper Case](https://www.thingiverse.com/thing:4246155).

## License
- This project is released under the MIT License; see the `LICENSE` file for details.

_This English translation was generated automatically._
