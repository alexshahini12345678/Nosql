# Minimal Api with MongoDB

## Beschreibung

Das mit ASP.NET 8 realisierte [Minimal WebApi](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/overview?view=aspnetcore-8.0) unterstützt CRUD-Operationen für eine Film-Collection. Für die Persistenz der Daten wird eine MongoDb angebunden.

Die möglichst einfach gehaltenen Anwendung soll das Wissen aus den Modulen [165 NoSQL-Datenbanken einsetzen](https://www.modulbaukasten.ch/module/165/1/de-DE) und [347 Dienst mit Container anwenden](https://www.modulbaukasten.ch/module/347/1/de-DE) aus dem [Modulbaukasten](https://www.modulbaukasten.ch/) von [ICT-Berufsbildung](https://www.ict-berufsbildung.ch/) in einem gemeinsamen Praxisprojekt vertiefen.

> **Hinweis**: Es handelt sich hier um ein Schulungsprojekt, das nicht für den produktiven Einsatz geeignet ist. Wichtige Themen wie Sicherheit (Authentication, Authorization, HTTPS, etc.), Asynchronität, Exception-Handling, Logging, etc. wurden bewusst weggelassen.

Folgende CRUD-Operationen werden unterstützt:

**Insert**

Fügt einen Film hinzu.
Die Angabe einer *id* ist zwingend und sie darf nicht bereits vorhanden sein.

```
POST /api/movies HTTP/1.1
Host: localhost:5001
Content-Type: application/json
Content-Length: 224

{
    "id": "1",
    "title": "The Imitation Game",
    "year": "2014",
    "summary": "Das wahre Rätsel war der Mann, der den Code knackte",
    "actors": ["Benedict Cumberbatch", "Keira Knightley"]
}
```
**Get**

Gibt alle vorhandenen Filme zurück.

```
GET /api/movies HTTP/1.1
Host: localhost:5001
```

**GetById**

Gibt den Film mit der im Pfad übergebenen *id* zurück.
Exisitert kein Film mit der übergebenen *id*, wird Status *404 Not Found* zurückgegeben.

```
GET /api/movies/1 HTTP/1.1
Host: localhost:5001
```

**Delete**

Löscht den Film mit der im Pfad übergebenen *id*
Exisitert kein Film mit der übergebenen *id*, wird Status *404 Not Found* zurückgegeben.

```
DELETE /api/movies/2 HTTP/1.1
Host: localhost:5001
```

**Update**

Aktualisiert den Film mit der im PFad übergebenen *id* mit den übergebenen Daten.

```
PUT /api/movies/1 HTTP/1.1
Host: localhost:5001
Content-Type: application/json
Content-Length: 188

{
    "title": "The Imitation Game 2",
    "year": "2022",
    "summary": "Das wahre Rätsel war der Mann, der den Code knackte",
    "actors": ["Benedict Cumberbatch", "Keira Knightley"]
}
```

## Getting started

Laden Sie das Repo auf Ihren lokalen Rechner

```
git clone https://gitlab.com/gbssg/min-api-with-mongo.git
cd min-api-with-mongo
```

### Konsole

Damit das Projekt auf Ihrem lokalen Rechner ausführbar ist, müssen folgende Voraussetzungen erfüllt sein:

**.NET 8**

Damit das Projekt kompiliert und ausgeführt werden kann, muss der [.NET 8 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) auf Ihrem Rechner installiert sein.
Der SDK ist für Windows, Linux und macOS verfügbar.

**MongoDB**

Stellen Sie sicher, dass Sie Zugriff auf eine MongoDB haben. Sie können dazu einen MongoDB-Container mit folgendem Befehl starten. 

```
docker run --name mongodb -d -p 27017:27017 -v data:/data/db -e MONGO_INITDB_ROOT_USERNAME=gbs -e MONGO_INITDB_ROOT_PASSWORD=geheim mongo
```

Passen Sie bei Bedarf den ConnectionString in [appsettings.json](./WebApi/appsettings.json) an.
Im [MongoDB Connection Guide](https://www.mongodb.com/docs/drivers/go/current/fundamentals/connection/) ist beschrieben, wie ein MongoDB ConnectionString aufgebaut ist.

Starten Sie das WebApi in einer Konsole wie folgt:

```
dotnet run --project ./WebApi/WebApi.csproj
```

### Docker Compose

Damit das Projekt auf Ihrem lokalen Rechner ausführbar ist, muss die [Docker Engine](https://docs.docker.com/engine/install/) auf Ihrem Rechner gestartet sein.

Starten Sie die Anwendung (Web API und MongoDB-Container) in einer Konsole wie folgt:

```
docker compose up 
```
## Beispiel-Requests

Für den Test des WebApis eignet sich die API-Plattform [Postman](https://www.postman.com/)
Importieren Sie in Ihren Workspace die Collection MinimalApiWithMongoDb von File [PostmanCollection.json](./PostmanCollection.json).

Folgende Anwendungsfälle lassen sich mit einer leeren Datenbank durchspielen:
- InsertMovie-Imitation Game
- InsertMovie-Top Gun
- GetAllMovies
- GetMovieById-Top Gun
- DeleteMovieById-Top Gun
- Update Movie-Imitation Game

> **Wichtig**: Wenn Sie in Postman eigene Request erfassen, achten Sie darauf, dass nebst der URL auch die Http-Methode (GET, POST, DEL, PUT) korrekt gewählt ist. Bei POST-Requests ist der Body mit Option *raw* und Format *JSON* zu erfassen.

## Arbeitsaufträge

Das Übungsprojekt kann mit 4 Arbeitsaufträgen von Grund auf erstellt werden.
Die Arbeitsaufträge sind wie folgt gegliedert.

- Arbeitsauftrag 1: Grundgerüst eines Minimal API (mit Dockerfile und docker-compose.yml)
- Arbeitsauftrag 2: Einbindung MongoDB
- Arbeitsauftrag 3: Endpunkte mit CRUD-Operationen
- Arbeitsauftrag 4: MovieService mit CRUD-Operationen

Die Arbeitsaufträge sind in Verzeichnis [_Arbeisauftraege](./_Arbeitsauftraege/) als MD-Files abgelegt. 

Mit folgender Anweisung werden alle vier PDF-Aufträge aus den MD-Files erstellt:
```
./_Arbeitsauftraege/buildPdf.sh
```

## Quellen

- [Building a Web API with ASP.NET Core and MongoDB](https://www.youtube.com/watch?v=VSsAsA6_-GE)
- [Tutorial: Create a minimal API with ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/tutorials/min-web-api?view=aspnetcore-8.0&tabs=visual-studio)

  --------------------------------------------------------------------------
# Minimal API mit MongoDB: Schritt-für-Schritt Anleitung

## Voraussetzungen
- VM LP-22.04 (Ubuntu 22.04)
- Visual Studio Code (VS Code)
- Git (nur lokal, keine Remote-Pushes)
- Docker & Docker Compose (optional, nicht zwingend erforderlich)
- Internetzugang für Installationstools

---

## Teil 1: Grundgerüst eines Minimal API

### 1.1 Ziele
- Erstellen des Grundgerüsts einer .NET 8 Minimal API  
- Anlegen eines `Dockerfile` und einer `docker-compose.yml`, um die Anwendung zu bauen und auszuführen (optional)

### 1.2 Umgebung
- Übung auf VM LP-22.04  
- IDE: Visual Studio Code

---

### Aufgabe 1: Installation .NET 8 (ca. 10 Minuten)

1. Öffne ein Terminal auf der VM.  
2. Prüfe, ob .NET 8 bereits installiert ist:  
    vier Leerzeichen`dotnet --list-sdks`  
   Wenn .NET 8 nicht aufgeführt ist, installiere es:  
    vier Leerzeichen`sudo apt-get update`  
    vier Leerzeichen`sudo apt-get install -y dotnet-sdk-8.0`  
   Nach der Installation nochmals prüfen:  
    vier Leerzeichen`dotnet --list-sdks`  
   Du solltest nun `8.0.xxx` in der Ausgabe sehen.

---

### Aufgabe 2: Lokales Git-Repository erstellen (ca. 10 Minuten)

1. Erstelle einen neuen Ordner für dein Projekt, z. B.:  
    vier Leerzeichen`mkdir ~/Documents/min-api-with-mongo`  
    vier Leerzeichen`cd ~/Documents/min-api-with-mongo`  
2. Initialisiere ein lokales Git-Repository:  
    vier Leerzeichen`git init`  
3. Lege eine Datei `README.md` an:  
    vier Leerzeichen`touch README.md`  
4. (Optional) Git-Status prüfen:  
    vier Leerzeichen`git status`  
5. Füge `README.md` zur Versionskontrolle hinzu und committe:  
    vier Leerzeichen`git add README.md`  
    vier Leerzeichen`git commit -m "README.md angelegt"`

---

### Aufgabe 3: Grundgerüst erstellen (ca. 10 Minuten)

1. Navigiere ins Projektverzeichnis:  
    vier Leerzeichen`cd ~/Documents/min-api-with-mongo`  
2. Erstelle das .NET-Projekt mit dem Web-Template:  
    vier Leerzeichen`dotnet new web --name WebApi --framework net8.0`  
   Es entsteht der Ordner `WebApi` mit einem Minimal-API-Gerüst.  
3. Erstelle im Projektverzeichnis eine `.gitignore`:  
    vier Leerzeichen`dotnet new gitignore`  
4. Öffne VS Code im Projektordner:  
    vier Leerzeichen`code .`  
5. In VS Code öffne ein Terminal (Terminal → New Terminal) und wechsle in den Ordner `WebApi`:  
    vier Leerzeichen`cd WebApi`  
6. Starte die Anwendung lokal:  
    vier Leerzeichen`dotnet run`  
   In der Konsole wird nun eine URL ausgegeben (z. B. `https://localhost:5001` oder `http://localhost:5000`).  
7. Öffne diese im Browser. Es sollte “Hello World!” (Standardausgabe) erscheinen.  
8. Beende die Anwendung mit **Ctrl + C**.  
9. Passe die Datei `Properties/launchSettings.json` an, damit die App standardmäßig über `http://localhost:5001` läuft:  
   - Öffne `Properties/launchSettings.json`.  
   - Lösche die Sektion `iisSettings`.  
   - Lösche die Profile `IIS Express` und das HTTPS-Profil.  
   - Belasse nur das HTTP-Profil, z. B.:

     ```jsonc
     {
       "profiles": {
         "WebApi": {
           "commandName": "Project",
           "dotnetRunMessages": true,
           "launchBrowser": true,
           "applicationUrl": "http://localhost:5001",
           "environmentVariables": {
             "ASPNETCORE_ENVIRONMENT": "Development"
           }
         }
       }
     }
     ```
   - Speichere und starte die App erneut:  
     vier Leerzeichen`dotnet run`  
   - Die Anwendung sollte nun unter `http://localhost:5001` erreichbar sein.  
10. (Optional) Führe einen lokalen Commit aus:  
    vier Leerzeichen`cd ..`  
    vier Leerzeichen`git add .`  
    vier Leerzeichen`git commit -m "Grundgerüst .NET 8 Minimal API erstellt"`

---

### Aufgabe 4: Dockerfile erstellen (ca. 20 Minuten)

> **Hinweis:** Containerisierung ist optional. Falls du Docker nicht nutzen möchtest, überspringe diesen Schritt.

1. Wechsle ins Verzeichnis `WebApi` (falls nicht bereits dort):  
    vier Leerzeichen`cd ~/Documents/min-api-with-mongo/WebApi`  
2. Lege eine Datei `Dockerfile` an:  
    vier Leerzeichen`touch Dockerfile`  
3. Öffne `Dockerfile` und füge folgenden Inhalt ein (Multistage-Build):

    ```dockerfile
    ############################
    # Build-Stage
    ############################
    FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
    WORKDIR /app

    # Projektdateien kopieren und Abhängigkeiten wiederherstellen
    COPY *.csproj ./
    RUN dotnet restore

    # Restliche Quelldateien kopieren und veröffentlichen
    COPY . ./
    RUN dotnet publish -c Release -o /app/out

    ############################
    # Runtime-Stage
    ############################
    FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
    WORKDIR /app
    COPY --from=build /app/out ./

    # Exponiere Port 5001
    EXPOSE 5001

    ENTRYPOINT ["dotnet", "WebApi.dll"]
    ```

4. Speichere die Datei.  
5. Baue und teste das Docker-Image (aus dem `WebApi`-Ordner):

   ```bash
   docker build -t min-api-with-mongo:latest .
   docker run -d --name test-webapi -p 5001:5001 min-api-with-mongo:latest
---
### Aufgabe 6: Lokale Commits
Führe nach jedem abgeschlossenen Schritt einen lokalen Commit durch, um bei Bedarf zurücksetzen zu können:  
    vier Leerzeilengit add .  
    vier Leerzeilengit commit -m "<kurze Beschreibung des Schritts>"

---

## Teil 2: Einbindung von MongoDB

> **Hinweis:** Dieser Abschnitt baut auf dem abgeschlossenen Teil 1 auf. Wenn du keine Containerisierung verwendest, starte MongoDB manuell auf deiner VM.

### 2.1 Ziele
- MongoDB in Docker-Container starten (oder lokal installieren)  
- MongoDB-Treiber in .NET-Anwendung integrieren  
- Orchestrierung von API und MongoDB (optional) über Docker Compose

### 2.2 Umgebung
- VM LP-22.04  
- VS Code

---

### Aufgabe 1: MongoDB-Container starten (ca. 10 Minuten)
1. Erstelle ein Docker-Volume für persistente Daten:  
    vier Leerzeichendocker volume create mongodb_data  
2. Starte einen MongoDB-Container im Hintergrund mit benanntem Volume:  
    vier Leerzeilendocker run -d \  
    vier Leerzeilen  --name mongodb \  
    vier Leerzeilen  -v mongodb_data:/data/db \  
    vier Leerzeilen  -p 27017:27017 \  
    vier Leerzeilen  mongo:latest  
   - `-d`: Ausführung im Hintergrund  
   - `-v mongodb_data:/data/db`: Persistenz via Named Volume  
   - `-p 27017:27017`: 1:1 Port-Mapping  
3. Prüfe, ob der Container läuft:  
    vier Leerzeilendocker ps  
   Du solltest einen laufenden `mongo:latest`-Container sehen.  
4. (Optional) Öffne eine Shell im Container, um zu prüfen:  
    vier Leerzeilendocker exec -it mongodb bash  
    vier Leerzeilenmongosh  
    vier Leerzeilenshow dbs  
    vier Leerzeilenexit  
    vier Leerzeilenexit

---

### Aufgabe 2: VS Code-Extension für C# installieren (ca. 5 Minuten)
1. Öffne VS Code.  
2. Gehe zu **Extensions** (⇧⌘X bzw. ⌃⇧X).  
3. Suche nach **C# (von Microsoft)** und installiere die Extension.  
4. Schließe und öffne ggf. das Projekt neu, damit IntelliSense aktiv wird.

---

### Aufgabe 3: MongoDB-Treiber einbinden & `/check`-Endpoint implementieren (ca. 15 Minuten)
1. Wechsle ins Verzeichnis `WebApi`:  
    vier Leerzeichencd ~/Documents/min-api-with-mongo/WebApi  
2. Installiere das NuGet-Package `MongoDB.Driver`:  
    vier Leerzeilendotnet add package MongoDB.Driver  
3. Öffne `Program.cs` in VS Code.  
4. Füge nach `var app = builder.Build();` (bzw. direkt nach `var builder = WebApplication.CreateBuilder(args);`) einen neuen Endpoint `/check` hinzu:  

    ```csharp
    app.MapGet("/check", () =>
    {
        try
        {
            // Connection-String fix kodiert (später konfigurierbar machen)
            var connectionString = "mongodb://localhost:27017";
            var client = new MongoDB.Driver.MongoClient(connectionString);
            var databases = client.ListDatabaseNames().ToList();
            return Results.Ok($"Vorhandene Datenbanken: {string.Join(", ", databases)}");
        }
        catch (Exception ex)
        {
            return Results.Problem($"Fehler beim Zugriff auf MongoDB: {ex.Message}");
        }
    });
    ```

5. Speichere `Program.cs`.  
6. Starte die Anwendung lokal:  
    vier Leerzeilendotnet run  
7. Rufe im Browser oder per `curl` folgenden Aufruf auf:  
    vier Leerzeilenhttp://localhost:5001/check  
   Erwartete Ausgabe: Liste der vorhandenen Datenbanken (z. B. `admin, config, local`).  
   Bei Fehler: entsprechende Fehlermeldung.  
8. Beende die Anwendung mit **Ctrl + C**.  
9. (Optional) Commit:

    ```bash
    cd ..
    git add .
    git commit -m "MongoDB.Driver hinzugefügt; /check-Endpoint implementiert"
    ```

---

### Aufgabe 4: Konfiguration des Connection-Strings (Options Pattern) (ca. 15 Minuten)
1. Erstelle im Ordner `WebApi` eine neue Datei `DatabaseSettings.cs`:

    ```csharp
    public class DatabaseSettings
    {
        public string ConnectionString { get; set; } = "";
    }
    ```

2. Öffne `appsettings.json` und ergänze den Abschnitt `DatabaseSettings`:

    ```jsonc
    {
      "AllowedHosts": "*",
      "DatabaseSettings": {
        "ConnectionString": "mongodb://localhost:27017"
      }
    }
    ```

3. Passe `Program.cs` an:

    ```csharp
    var builder = WebApplication.CreateBuilder(args);

    // Konfiguration des DatabaseSettings-Abschnitts
    var dbSettingsSection = builder.Configuration.GetSection("DatabaseSettings");
    builder.Services.Configure<DatabaseSettings>(dbSettingsSection);

    var app = builder.Build();

    app.MapGet("/check", (Microsoft.Extensions.Options.IOptions<DatabaseSettings> options) =>
    {
        try
        {
            var mongoDbConnectionString = options.Value.ConnectionString;
            var client = new MongoDB.Driver.MongoClient(mongoDbConnectionString);
            var databases = client.ListDatabaseNames().ToList();
            return Results.Ok($"Vorhandene Datenbanken: {string.Join(", ", databases)}");
        }
        catch (Exception ex)
        {
            return Results.Problem($"Fehler beim Zugriff auf MongoDB: {ex.Message}");
        }
    });

    app.Run();
    ```

4. Speichere `Program.cs`.  
5. Anwendung erneut starten:  
    vier Leerzehlendotnet run  
6. Browser: `http://localhost:5001/check` → Verbindung sollte nun den Connection-String aus `appsettings.json` nutzen.  
7. Beende die Anwendung mit **Ctrl + C**.  
8. (Optional) Commit:

    ```bash
    cd ..
    git add DatabaseSettings.cs appsettings.json Program.cs
    git commit -m "ConnectionString in appsettings.json konfigurierbar gemacht"
    ```

### Aufgabe 5: `docker-compose` erweitern (ca. 15 Minuten)
> **Hinweis:** Nur, falls du Docker Compose verwendest.

1. Öffne im Projekt-Root (`min-api-with-mongo`) die Datei `docker-compose.yml`.  
2. Erweitere sie, sodass neben `webapi` auch ein `mongodb`-Service gestartet wird und das API erst startet, wenn MongoDB verfügbar ist. Beispiel:  

    


    version: "3.8"
    


    


    services:
      mongodb:
        image: mongo:latest
        container_name: mongodb
        volumes:
          - mongodb_data:/data/db
        ports:
          - "27017:27017"
        restart: unless-stopped

      webapi:
        build:
          context: ./WebApi
          dockerfile: Dockerfile
        ports:
          - "5001:5001"
        depends_on:
          - mongodb
        environment:
          # Überschreibt DatabaseSettings:ConnectionString via Umgebungsvariable
          DatabaseSettings__ConnectionString: "mongodb://mongodb:27017"
        restart: unless-stopped

    volumes:
      mongodb_data:

   **Erläuterung:**  
   - `depends_on: [mongodb]` sorgt dafür, dass Docker Compose versucht, zuerst den `mongodb`-Container hochzufahren.  
   - Über `environment` wird die Umgebungsvariable `DatabaseSettings__ConnectionString` gesetzt, damit der Connection-String innerhalb des Docker-Netzwerks auf den Service `mongodb` zeigt.  

3. Speichere die Datei.  
4. Starte beide Container:  
    

    docker compose up --build -d

5. Warte kurz, bis beide Dienste laufen. Prüfe mit:  
    

    docker ps

6. Rufe im Browser `http://localhost:5001/check` auf.  
   Nun sollte der `/check`-Endpoint über das Docker-Netzwerk Kontakt zu MongoDB haben und die Datenbanken auflisten (z. B. `admin, config, local`).  
7. Stoppe die Dienste:  
    

    docker compose down

8. (Optional) Führe einen lokalen Commit aus:  
    

    git add docker-compose.yml
    git commit -m "docker-compose: MongoDB-Service hinzugefügt und ConnectionString angepasst"
---

## Teil 3: REST-Endpoints für CRUD-Operationen

> **Hinweis:** Die Datenbank und Collection sind aktuell noch leer. Die CRUD-Operationen implementieren wir schrittweise.

### 3.1 Ziele
- Unterschied zwischen GET, POST, PUT und DELETE-Requests verstehen  
- Erstellen von CRUD-Endpoints  
- Testen der Endpoints mit einem REST-Client

---

### Aufgabe 1: Klasse `Movie` erstellen
1. Erstelle im Ordner `WebApi` eine neue Datei `Movie.cs`:
    
    
    
    cd ~/Documents/min-api-with-mongo/WebApi
    touch Movie.cs
2. Öffne `Movie.cs` und füge folgenden Code ein:
    
    
    
    using MongoDB.Bson.Serialization.Attributes;
    
    public class Movie
    {
        [BsonId]
        public string Id { get; set; } = "";
        public string Title { get; set; } = "";
        public int Year { get; set; }
        public string Summary { get; set; } = "";
        public string[] Actors { get; set; } = Array.Empty<string>();
    }
3. Speichere `Movie.cs`.  
4. (Optional) Führe einen lokalen Commit aus:
    
    
    
    cd ..
    git add WebApi/Movie.cs
    git commit -m "Movie-Klasse erstellt"

---

### Aufgabe 2: REST-Endpoints in `Program.cs` vorbereiten
1. Öffne `WebApi/Program.cs`.  
2. Füge in der Nähe der bestehenden `/check`-Route (oder an geeigneter Stelle) die Definitionen der CRUD-Endpoints hinzu. Beispiel:
    
    
    
    // Insert Movie
    // Wenn das übergebene Objekt erfolgreich eingefügt wurde, Statuscode 200 OK zurückgeben.
    // Bei Fehler 409 Conflict zurückgeben.
    app.MapPost("/api/movies", (Movie movie) =>
    {
        throw new NotImplementedException();
    });

    // Get all Movies
    // Alle Filme mit Statuscode 200 OK zurückgeben.
    app.MapGet("/api/movies", () =>
    {
        throw new NotImplementedException();
    });

    // Get Movie by id
    // Gewünschtes Movie-Objekt (Statuscode 200) oder 404 Not Found zurückgeben.
    app.MapGet("/api/movies/{id}", (string id) =>
    {
        throw new NotImplementedException();
    });

    // Update Movie
    // Aktualisiertes Movie-Objekt zurückgeben oder 404 Not Found bei ungültiger id.
    app.MapPut("/api/movies/{id}", (string id, Movie movie) =>
    {
        throw new NotImplementedException();
    });

    // Delete Movie
    // Bei erfolgreicher Löschung 200 OK zurückgeben, sonst 404 Not Found.
    app.MapDelete("/api/movies/{id}", (string id) =>
    {
        throw new NotImplementedException();
    });
3. Speichere `Program.cs`.  
4. (Optional) Führe einen lokalen Commit aus:
    
    
    
    cd ..
    git add WebApi/Program.cs
    git commit -m "REST-Endpoints in Program.cs vorbereitet"

### Aufgabe 3: Beispiel-Code für GET `/api/movies/{id}` ohne Datenbankanbindung
1. Ersetze in `Program.cs` den Platzhalter-Stub für GET `/api/movies/{id}` durch folgendes Beispiel:  
    
    
    app.MapGet("/api/movies/{id}", (string id) =>  
    {  
        if (id == "1")  
        {  
            var myMovie = new Movie()  
            {  
                Id = "1",  
                Title = "Ein Quantum Trost",  
                Year = 2008,  
                Summary = "James Bond auf den Spuren von Vesper Lynd...",  
                Actors = new[] { "Daniel Craig", "Olga Kurylenko" }  
            };  
            return Results.Ok(myMovie);  
        }  
        else  
        {  
            return Results.NotFound();  
        }  
    });  
2. Speichere `Program.cs`.  
3. Starte die Anwendung:  
    
    
    cd WebApi  
    dotnet run  
4. Teste den Endpoint im Browser oder per `curl`:  
    
    
    http://localhost:5001/api/movies/1  
   Es sollte das JSON-Objekt des Films “Ein Quantum Trost” erscheinen.  
   Für GET `/api/movies/2` bekommst du 404 Not Found.  
5. Beende die Anwendung mit **Ctrl+C**.  
6. (Optional) Führe einen lokalen Commit aus:  
    
    
    cd ..  
    git add WebApi/Program.cs  
    git commit -m "Beispiel-Implementation für GET /api/movies/{id}"

---

### Aufgabe 4: Testen mit REST-Client
1. Installiere in VS Code die Extension **REST Client** (falls noch nicht geschehen).  
2. Lege im Projektverzeichnis `WebApi` eine Datei `testing.http` an:  
    
    
    cd ~/Documents/min-api-with-mongo/WebApi  
    touch testing.http  
3. Öffne `testing.http` und füge folgende Zeilen ein:  
    
    
    ### Get Movie by Id (Beispiel ohne DB)  
    GET http://localhost:5001/api/movies/1  
    Accept: application/json  
    
    ### Get Movie by Id (404-Fall)  
    GET http://localhost:5001/api/movies/2  
    Accept: application/json  
4. Klicke in VS Code auf “Send Request”, um die Anfragen auszuführen und die Antworten zu prüfen.  
5. (Optional) Führe einen lokalen Commit aus:  
    
    
    cd ..  
    git add WebApi/testing.http  
    git commit -m "Beispiel-Requests für GET /api/movies/{id}"

---

## Teil 4: MovieService mit CRUD-Operationen (Dependency Injection)

> **Hinweis:** In diesem Abschnitt wird die eigentliche Datenbankintegration über den MovieService vorgenommen.

### 4.1 Ziele
- Auslagern aller DB-Zugriffe in einen `MovieService`  
- Nutzung von Dependency Injection (DI) für den Service  
- Integration des `MovieService` in die vorhandenen Endpoints

---

### Aufgabe 1: Interface `IMovieService` erstellen
1. Erstelle im Ordner `WebApi` eine Datei `IMovieService.cs`:  
    
    
    cd ~/Documents/min-api-with-mongo/WebApi  
    touch IMovieService.cs  
2. Öffne `IMovieService.cs` und füge folgendes Gerüst ein:  
    
    
    using System.Collections.Generic;  
    
    public interface IMovieService  
    {  
        IEnumerable<Movie> Get();              // Alle Filme abrufen  
        Movie Get(string id);                   // Film nach Id abrufen  
        void Create(Movie movie);               // Film erstellen  
        void Update(string id, Movie movie);    // Film aktualisieren  
        void Delete(string id);                 // Film löschen  
    }  
3. Speichere `IMovieService.cs`.  
4. (Optional) Führe einen lokalen Commit aus:  
    
    
    cd ..  
    git add WebApi/IMovieService.cs  
    git commit -m "IMovieService Interface erstellt"

---

### Aufgabe 2: Klasse `MongoMovieService` implementieren
1. Erstelle im Ordner `WebApi` eine Datei `MongoMovieService.cs`:  
    
    
    cd ~/Documents/min-api-with-mongo/WebApi  
    touch MongoMovieService.cs  
2. Öffne `MongoMovieService.cs` und füge folgendes Gerüst ein:  
    
    
    using System.Collections.Generic;  
    using Microsoft.Extensions.Options;  
    using MongoDB.Driver;  
    
    public class MongoMovieService : IMovieService  
    {  
        private readonly IMongoCollection<Movie> _movieCollection;  
        private const string mongoDbDatabaseName = "gbs";  
        private const string mongoDbCollectionName = "movies";  
    
        // Constructor: Settings per DI  
        public MongoMovieService(IOptions<DatabaseSettings> options)  
        {  
            var mongoDbConnectionString = options.Value.ConnectionString;  
            var mongoClient = new MongoClient(mongoDbConnectionString);  
            var database = mongoClient.GetDatabase(mongoDbDatabaseName);  
            _movieCollection = database.GetCollection<Movie>(mongoDbCollectionName);  
        }  
    
        public IEnumerable<Movie> Get()  
        {  
            return _movieCollection.Find(movie => true).ToList();  
        }  
    
        public Movie Get(string id)  
        {  
            return _movieCollection.Find(movie => movie.Id == id).FirstOrDefault();  
        }  
    
        public void Create(Movie movie)  
        {  
            _movieCollection.InsertOne(movie);  
        }  
    
        public void Update(string id, Movie updatedMovie)  
        {  
            _movieCollection.ReplaceOne(movie => movie.Id == id, updatedMovie);  
        }  
    
        public void Delete(string id)  
        {  
            _movieCollection.DeleteOne(movie => movie.Id == id);  
        }  
    }  
3. Speichere `MongoMovieService.cs`.  
4. (Optional) Führe einen lokalen Commit aus:  
    
    
    cd ..  
    git add WebApi/MongoMovieService.cs  
    git commit -m "MongoMovieService implementiert"
### Aufgabe 3: `MovieService` im DI-Container registrieren
1. Öffne `WebApi/Program.cs`.  
2. Füge direkt nach `var builder = WebApplication.CreateBuilder(args);` folgende Zeile ein, um `IMovieService` zu registrieren:  
    
    
    
    builder.Services.AddSingleton<IMovieService, MongoMovieService>();
3. Ergänze in `Program.cs` auch die Swagger-Registrierung (falls gewünscht) und die CRUD-Endpoints. Ein mögliches Gesamtgerüst:  
    
    
    
    var builder = WebApplication.CreateBuilder(args);
    
    
    // OpenAPI/Swagger (optional)
    builder.Services.AddEndpointsApiExplorer();
    builder.Services.AddSwaggerGen();
    
    
    // DatabaseSettings-Konfiguration
    var dbSettingsSection = builder.Configuration.GetSection("DatabaseSettings");
    builder.Services.Configure<DatabaseSettings>(dbSettingsSection);
    
    
    // MovieService registrieren
    builder.Services.AddSingleton<IMovieService, MongoMovieService>();
    
    
    var app = builder.Build();
    
    
    // Swagger Middleware aktivieren (nur in Development)
    if (app.Environment.IsDevelopment())
    {
        app.UseSwagger();
        app.UseSwaggerUI(c =>
        {
            c.SwaggerEndpoint("/swagger/v1/swagger.json", "Movie API V1");
            c.RoutePrefix = string.Empty; // Swagger UI unter Root (http://localhost:5001)
        });
    }
    
    
    // /check-Endpoint
    app.MapGet("/check", (Microsoft.Extensions.Options.IOptions<DatabaseSettings> options) =>
    {
        try
        {
            var mongoDbConnectionString = options.Value.ConnectionString;
            var client = new MongoDB.Driver.MongoClient(mongoDbConnectionString);
            var databases = client.ListDatabaseNames().ToList();
            return Results.Ok($"Vorhandene Datenbanken: {string.Join(", ", databases)}");
        }
        catch (Exception ex)
        {
            return Results.Problem($"Fehler beim Zugriff auf MongoDB: {ex.Message}");
        }
    });
    
    
    // CRUD-Endpoints mit MovieService
    app.MapPost("/api/movies", (IMovieService movieService, Movie movie) =>
    {
        try
        {
            movieService.Create(movie);
            return Results.Ok(movie);
        }
        catch (MongoWriteException)
        {
            return Results.Conflict($"Film mit Id {movie.Id} existiert bereits.");
        }
    });
    
    
    app.MapGet("/api/movies", (IMovieService movieService) =>
    {
        var movies = movieService.Get();
        return Results.Ok(movies);
    });
    
    
    app.MapGet("/api/movies/{id}", (IMovieService movieService, string id) =>
    {
        var movie = movieService.Get(id);
        return movie != null ? Results.Ok(movie) : Results.NotFound();
    });
    
    
    app.MapPut("/api/movies/{id}", (IMovieService movieService, string id, Movie movie) =>
    {
        var existing = movieService.Get(id);
        if (existing == null)
            return Results.NotFound();
    
        movieService.Update(id, movie);
        return Results.Ok(movie);
    });
    
    
    app.MapDelete("/api/movies/{id}", (IMovieService movieService, string id) =>
    {
        var existing = movieService.Get(id);
        if (existing == null)
            return Results.NotFound();
    
        movieService.Delete(id);
        return Results.Ok($"Film mit Id {id} gelöscht.");
    });
    
    
    app.Run();
4. Speichere `WebApi/Program.cs`.  
5. (Optional) Führe einen lokalen Commit aus:  
    
    
    
    cd ..
    git add WebApi/Program.cs
    git commit -m "MovieService im DI-Container registriert und CRUD-Endpoints implementiert"

---

### Aufgabe 4: Testen der CRUD-Endpoints mit REST-Client
1. Öffne VS Code und stelle sicher, dass die Extension **REST Client** installiert ist.  
2. In der Datei `WebApi/testing.http` fügst du folgende Requests hinzu (nach den bisherigen Einträgen):  
    
    
    
    ### POST: Neuen Film erstellen
    POST http://localhost:5001/api/movies
    Content-Type: application/json
    
    {
      "Id": "1",
      "Title": "Inception",
      "Year": 2010,
      "Summary": "Ein Dieb, der Träume stiehlt...",
      "Actors": ["Leonardo DiCaprio", "Joseph Gordon-Levitt"]
    }
    
    
    ### GET: Alle Filme abrufen
    GET http://localhost:5001/api/movies
    Accept: application/json
    
    
    ### GET: Film nach Id abrufen
    GET http://localhost:5001/api/movies/1
    Accept: application/json
    
    
    ### PUT: Film aktualisieren
    PUT http://localhost:5001/api/movies/1
    Content-Type: application/json
    
    {
      "Id": "1",
      "Title": "Inception (Remastered)",
      "Year": 2010,
      "Summary": "Ein Meisterwerk von Christopher Nolan...",
      "Actors": ["Leonardo DiCaprio", "Joseph Gordon-Levitt"]
    }
    
    
    ### DELETE: Film löschen
    DELETE http://localhost:5001/api/movies/1
3. Wechsle ins Verzeichnis `WebApi` und starte die Anwendung:  
    
    
    
    cd ~/Documents/min-api-with-mongo/WebApi
    dotnet run
4. Führe nacheinander alle Requests in `testing.http` aus:  
    - **POST** → Neuer Film wird angelegt (Status 200 OK).  
    - **GET** `/api/movies` → Liste aller Filme (sollte nun 1 Eintrag enthalten).  
    - **GET** `/api/movies/1` → Details des Films (Status 200).  
    - **PUT** `/api/movies/1` → Film wird aktualisiert (Status 200).  
    - **DELETE** `/api/movies/1` → Film wird gelöscht (Status 200).  
    - **GET** `/api/movies/1` nochmals → 404 Not Found.  
5. Beende die Anwendung mit **Ctrl+C**.  
6. (Optional) Führe einen lokalen Commit aus:  
    
    
    
    cd ..
    git add WebApi/testing.http
    git commit -m "CRUD-Tests in testing.http ergänzt"
## Teil 5: MovieService mit Swagger (Zusatzaufgabe)

### 5.1 Ziele
- Hinzufügen einer OpenAPI/Swagger-Dokumentation für die WebAPI

---

### Aufgabe 1: OpenAPI/Swagger-Dokumentation einbinden (ca. 20 Minuten)
1. Wechsle ins Verzeichnis `WebApi`:  
    
    
    
    cd ~/Documents/min-api-with-mongo/WebApi
2. Installiere die beiden NuGet-Packages:  
    
    
    
    dotnet add package Microsoft.AspNetCore.OpenApi
    dotnet add package Swashbuckle.AspNetCore
3. Öffne `Program.cs` und ergänze in der builder-Konfiguration:  
    
    
    
    var builder = WebApplication.CreateBuilder(args);
    
    
    // OpenAPI/Swagger
    builder.Services.AddEndpointsApiExplorer();
    builder.Services.AddSwaggerGen();
    
    
    // DatabaseSettings-Konfiguration
    var dbSettingsSection = builder.Configuration.GetSection("DatabaseSettings");
    builder.Services.Configure<DatabaseSettings>(dbSettingsSection);
    
    
    // MovieService registrieren
    builder.Services.AddSingleton<IMovieService, MongoMovieService>();
    
    
    var app = builder.Build();
    
    
    // Swagger-Middleware aktivieren (nur in Development)
    if (app.Environment.IsDevelopment())
    {
        app.UseSwagger();
        app.UseSwaggerUI(c =>
        {
            c.SwaggerEndpoint("/swagger/v1/swagger.json", "Movie API V1");
            c.RoutePrefix = string.Empty; // Swagger UI unter Root (http://localhost:5001)
        });
    }
    
    
    // /check-Endpoint
    app.MapGet("/check", (Microsoft.Extensions.Options.IOptions<DatabaseSettings> options) =>
    {
        try
        {
            var mongoDbConnectionString = options.Value.ConnectionString;
            var client = new MongoDB.Driver.MongoClient(mongoDbConnectionString);
            var databases = client.ListDatabaseNames().ToList();
            return Results.Ok($"Vorhandene Datenbanken: {string.Join(", ", databases)}");
        }
        catch (Exception ex)
        {
            return Results.Problem($"Fehler beim Zugriff auf MongoDB: {ex.Message}");
        }
    });
    
    
    // CRUD-Endpoints (wie bereits definiert)
    app.MapPost("/api/movies", (IMovieService movieService, Movie movie) =>
    {
        try
        {
            movieService.Create(movie);
            return Results.Ok(movie);
        }
        catch (MongoWriteException)
        {
            return Results.Conflict($"Film mit Id {movie.Id} existiert bereits.");
        }
    });
    
    
    app.MapGet("/api/movies", (IMovieService movieService) =>
    {
        var movies = movieService.Get();
        return Results.Ok(movies);
    });
    
    
    app.MapGet("/api/movies/{id}", (IMovieService movieService, string id) =>
    {
        var movie = movieService.Get(id);
        return movie != null ? Results.Ok(movie) : Results.NotFound();
    });
    
    
    app.MapPut("/api/movies/{id}", (IMovieService movieService, string id, Movie movie) =>
    {
        var existing = movieService.Get(id);
        if (existing == null)
            return Results.NotFound();
    
        movieService.Update(id, movie);
        return Results.Ok(movie);
    });
    
    
    app.MapDelete("/api/movies/{id}", (IMovieService movieService, string id) =>
    {
        var existing = movieService.Get(id);
        if (existing == null)
            return Results.NotFound();
    
        movieService.Delete(id);
        return Results.Ok($"Film mit Id {id} gelöscht.");
    });
    
    
    app.Run();
4. Speichere `WebApi/Program.cs`.  
5. (Optional) Führe einen lokalen Commit aus:  
    
    
    
    cd ..
    git add WebApi/Program.cs
    git commit -m "Swagger/OpenAPI hinzugefügt"
