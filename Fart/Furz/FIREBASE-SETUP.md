# Furz-Counter → geteilter Live-Zähler mit Firebase

Die Seite liegt weiter auf **GitHub Pages**. Firebase liefert nur den
gemeinsamen Zählerstand in Echtzeit. Kostenlos, kein eigener Server.

> **Status: ✅ eingerichtet** (Projekt `furz-o-mat`, Standort `europe-west1`,
> Regeln veröffentlicht, Config in `index.html` eingetragen und end-to-end
> getestet). Die folgende Anleitung dient nur noch zur Dokumentation bzw. für
> den Fall, dass die Einrichtung mal wiederholt werden muss.

## Einmalige Einrichtung (ca. 5 Min)

1. **Firebase-Projekt anlegen**
   - Öffne <https://console.firebase.google.com/> und logge dich mit deinem
     Google-Konto ein.
   - „Projekt hinzufügen" → Name z. B. `furz-o-mat` → Google Analytics kannst
     du **deaktivieren** → „Projekt erstellen".

2. **Realtime Database erstellen**
   - Linkes Menü → **Build → Realtime Database** → „Datenbank erstellen".
   - Standort: **europe-west1** (Belgien) wählen.
   - Startmodus: „Im **gesperrten** Modus starten" (Regeln setzen wir gleich).

3. **Sicherheitsregeln setzen**
   - In der Realtime Database oben auf den Reiter **„Regeln"**.
   - Ersetze den Inhalt durch Folgendes und klicke **„Veröffentlichen"**:

   ```json
   {
     "rules": {
       "fartCount": {
         ".read": true,
         ".write": "newData.isNumber() && (newData.val() === 0 || (!data.exists() && newData.val() === 1) || (data.exists() && newData.val() === data.val() + 1))"
       }
     }
   }
   ```

   Das erlaubt jedem das Lesen und **nur** ein Hochzählen um 1 oder ein
   Zurücksetzen auf 0 – so kann niemand den Zähler auf Fantasiewerte setzen.

4. **Web-App-Config holen**
   - Zahnrad oben links → **Projekteinstellungen** → Reiter „Allgemein".
   - Unter „Meine Apps" auf das **Web-Symbol `</>`** klicken, Spitzname z. B.
     `furz-web` → „App registrieren".
   - Firebase zeigt dir ein `firebaseConfig`-Objekt. Kopiere die Werte.

5. **Config in `index.html` eintragen**
   - Öffne `index.html`, oben im `<script>` steht der Block `var firebaseConfig = {…}`.
   - Ersetze die `DEIN_...`-Platzhalter durch deine echten Werte.
   - Wichtig: Die `databaseURL` muss exakt die aus deiner Datenbank sein
     (steht oben in der Realtime Database, z. B.
     `https://furz-o-mat-default-rtdb.europe-west1.firebasedatabase.app`).

6. **Hochladen / committen**
   - `index.html` ins GitHub-Repo pushen. GitHub Pages aktualisiert die Seite.

Fertig. Öffne die Seite auf zwei Geräten – drückst du auf dem einen, zählt es
auf dem anderen sofort mit.

## Ist das sicher? Sind die Keys geheim?
Der `apiKey` in der Web-Config ist **kein Geheimnis** – er identifiziert nur
dein Projekt und darf öffentlich im HTML stehen. Der Schutz kommt aus den
**Datenbank-Regeln** oben (nur +1 oder 0). Das ist für einen Spaß-Counter genau
richtig.

## Kosten
Der Spark-Plan (kostenlos) reicht für so einen Counter locker – viele tausend
Klicks pro Tag liegen weit unter den Freigrenzen.
