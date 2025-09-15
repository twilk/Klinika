# Product Requirements Document (PRD) – System e-Zgód dla Kliniki Stomatologicznej

## 1. Cel systemu
System eliminuje papierowe zgody i umożliwia ich składanie w formie elektronicznej na tablecie, z automatyczną archiwizacją, możliwością wysyłki podpisanego dokumentu do pacjenta e-mailem i utrzymaniem pełnej historii leczenia.  
Cel: skrócenie czasu obsługi pacjenta, eliminacja kosztów archiwizacji papierowej, pełna zgodność z RODO i ustawą o dokumentacji medycznej.

## 2. Zakres funkcjonalny

### 2.1 MVP (must-have)
- Podpis elektroniczny odręczny (SES) na tablecie.  
- Generowanie dokumentów w PDF/A-1b z podpisem i metadanymi.  
- Archiwizacja dokumentów w repozytorium pacjenta.  
- Wysyłka podpisanej zgody na e-mail (Postmark/Resend lub outbox).  
- Historia leczenia pacjenta (zabiegi + zgody).  
- Eksport pacjenta do ZIP (z indeksem JSON/CSV).  
- AuditLog wszystkich działań.  
- Retencja dokumentów zgodnie z prawem.  
- Obsługa trybu offline (kolejkowanie zgód).  
- Dashboard administracyjny.

### 2.2 Should-have (V2)
- Portal pacjenta z dostępem do historii.  
- Role i uprawnienia (RBAC).  
- Raportowanie i analityka.  

### 2.3 Could-have (V3+)
- Integracja z podpisem kwalifikowanym i TSA.  
- Integracje z P1, NFZ, systemami gabinetowymi.  
- Zaawansowane raporty finansowe i medyczne.  

### 2.4 Won’t-have (świadome odłożenie)
- Pełny system rejestracji wizyt.  
- Funkcje CRM/marketingowe.

## 3. Personas i User Journeys

- **Pacjent Anna, 35 lat** – chce szybko podpisać zgodę bez druków. Journey: recepcja → tablet → podpis → PDF na e-mailu.  
- **Lekarz Michał, 42 lata** – potrzebuje łatwo przypisać zabieg do zgody. Journey: wybór pacjenta → wybór szablonu → podpis → zapis zabiegu → historia leczenia.  
- **Recepcja Marta, 28 lat** – zakłada pacjentów i wspiera lekarzy. Journey: rejestracja nowego pacjenta → przypisanie do wizyty → inicjacja zgody.  
- **Admin Krzysztof, 50 lat** – odpowiada za audyty i retencję. Journey: logi działań → eksport pacjentów → raporty SLA.

## 4. Makiety opisowe

- **Widok lekarza (tablet):** lista pacjentów, profil pacjenta, formularz zgody z podpisem.  
- **Widok pacjenta (tablet):** pełnoekranowy dokument + canvas do podpisu.  
- **Widok administracji (desktop):** dashboard (statystyki zgód), lista pacjentów, eksport ZIP, audyt.  

## 5. Procesy biznesowe
1. Podpis zgody → generacja PDF/A → archiwizacja → wysyłka e-mail → log w audycie.  
2. Dodanie zabiegu → przypisanie do pacjenta → widok w historii leczenia.  
3. Eksport pacjenta → ZIP z dokumentami i indeksem JSON/CSV.  
4. Retencja → automatyczne oznaczenie dokumentów do archiwizacji/usunięcia.  
5. Audyt → każda akcja zapisywana z ID użytkownika i timestampem.  
6. Tryb offline → podpisy kolejkowane i synchronizowane.

## 6. Mapa ryzyk i mitigacja
- **Ryzyko prawne:** zmiana przepisów → regularny przegląd prawny (raz/rok).  
- **Ryzyko techniczne:** awaria tabletu → wsparcie offline i backup urządzeń.  
- **Ryzyko organizacyjne:** brak akceptacji personelu → szkolenia i proste UI.  
- **Ryzyko bezpieczeństwa:** wyciek danych → szyfrowanie, audyt, testy penetracyjne.

## 7. Plan rollout/pilotażu
1. **Etap 1:** test lokalny w jednym gabinecie (2 tygodnie).  
2. **Etap 2:** wdrożenie w całej klinice (po sukcesie pilota).  
3. **Etap 3:** integracje z systemami zewnętrznymi.  

## 8. Integracje (planowane)
- **P1** – system państwowy ochrony zdrowia.  
- **NFZ** – raportowanie świadczeń.  
- **Systemy gabinetowe** (np. Estomed, DentalPro).  
- **E-mail** – Postmark, Resend.  
- **ePUAP/TSA** – kwalifikowane podpisy i znaczniki czasu.

## 9. Wymagania niefunkcjonalne
- **Bezpieczeństwo:** TLS 1.3, AES-256, MFA, polityka haseł.  
- **Wydajność:** PDF < 3 s, eksport 100 plików < 10 s.  
- **Dostępność:** aplikacja PWA, WCAG 2.1 AA.  
- **Niezawodność:** SLA 99,9%, RPO 15 min, RTO 2h.  
- **Zgodność:** RODO, ustawa o dokumentacji medycznej, ISO 27001.  

## 10. Metryki monitoringu
- Liczba podpisanych zgód/dzień.  
- Średni czas generacji PDF.  
- Liczba błędów e-maili.  
- Dostępność systemu (uptime).  
- Procent zgód elektronicznych vs papierowych.  

## 11. Scenariusze awaryjne
- **Tablet rozładowany:** pacjent podpisuje na innym urządzeniu, zgoda oznaczona jako „manualna”.  
- **Brak internetu:** zgoda zapisana offline, synchronizacja później.  
- **Błędny e-mail pacjenta:** dokument dostępny przez administrację i eksport ZIP.  

## 12. API high-level
- `POST /patients` – rejestracja pacjenta.  
- `GET /patients/:id` – pobranie profilu.  
- `POST /consents` – utworzenie zgody.  
- `GET /consents/:id` – pobranie PDF.  
- `POST /treatments` – dodanie zabiegu.  
- `POST /export` – eksport pacjenta do ZIP.  

## 13. Testy akceptacyjne (user stories)
- **Jako pacjent** podpisuję zgodę RODO → dostaję PDF na e-mail.  
- **Jako lekarz** dodaję zabieg → widzę go w historii pacjenta.  
- **Jako admin** eksportuję pacjenta → otrzymuję ZIP z dokumentami i indeksem.  
- **Jako recepcjonistka** zakładam pacjenta → pojawia się w systemie i można podpisać zgodę.

## 14. Compliance dodatkowe
- **ISO 27001** – zarządzanie bezpieczeństwem informacji.  
- **HL7/FHIR** – potencjalna integracja z systemami medycznymi.  
- **WCAG 2.1 AA** – dostępność interfejsu.

## 15. Plan maintenance & support
- **Backupy:** codziennie, retencja 30 dni, test odtwarzania raz w miesiącu.  
- **Monitoring:** Prometheus + alerty e-mail.  
- **Aktualizacje:** kwartalne przeglądy bezpieczeństwa, aktualizacje npm.  
- **Support:** admin kliniki jako 1st line, vendor jako 2nd line.  

## 16. Budżet i ograniczenia
- **Budżet:** ograniczony – preferowane rozwiązania low-maintenance (Supabase, Docker).  
- **Zasoby:** zespół 3–4 osób (dev, devops, analityk, PM).  
- **Czas:** MVP w 4–6 tygodni.  

## 17. Słownik pojęć
- **SES:** Simple Electronic Signature – podpis odręczny na tablecie.  
- **PDF/A:** standard archiwizacji dokumentów elektronicznych.  
- **AuditLog:** nieusuwalny dziennik zdarzeń systemu.  
- **RBAC:** Role-Based Access Control – zarządzanie uprawnieniami.  
- **TSA:** Time Stamping Authority – urząd znacznika czasu.

## 18. Appendix – przykłady zgód
- **Zgoda RODO** – przetwarzanie danych osobowych.  
- **Zgoda na zabieg chirurgiczny** – opis zabiegu, ryzyka, alternatywy.  
- **Zgoda RTG** – informacja o promieniowaniu i ochronie.

## 19. Kryteria akceptacji MVP
- Pacjent może podpisać dowolną zgodę i otrzymać PDF/A e-mailem.  
- Dokument zapisuje się w repozytorium i jest widoczny w historii pacjenta.  
- Eksport ZIP zawiera wszystkie dokumenty i indeks.  
- AuditLog rejestruje każdą operację.  
- Retencja oznacza dokumenty po okresie przechowywania.  
- Tryb offline działa poprawnie.

---

**Status:** Dokumentacja PRD kompletna i rozszerzona, gotowa do wdrożenia oraz do wykorzystania jako punkt odniesienia dla zespołu i klienta.  
