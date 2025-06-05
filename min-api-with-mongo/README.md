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
Minimal API mit MongoDB – Schritt-für-Schritt Anleitung
======================================================

Voraussetzungen
---------------
VM LP-22.04 · VS Code · Git (lokal) · Docker & Compose (optional) · Internet

--------------------------------------------------------
Teil 1 – Grundgerüst (.NET 8 Minimal API)
--------------------------------------------------------

1. .NET 8 installieren  
   dotnet --list-sdks # prüfen  
   sudo apt update && sudo apt install -y dotnet-sdk-8.0  

2. Projekt & Git  
   mkdir ~/Documents/min-api-with-mongo && cd $_  
   git init && touch README.md  
   git add README.md && git commit -m "README.md angelegt"

3. API-Gerüst  
   dotnet new web --name WebApi -f net8.0  
   dotnet new gitignore  
   code . → Terminal: cd WebApi → dotnet run (→ http://localhost:5001)  

4. launchSettings.json (HTTP 5001) anpassen und neu starten.  

--------------------------------------------------------
# Datei: WebApi/Dockerfile  (optional Container)
--------------------------------------------------------
############################
# Build-Stage
############################
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app
COPY *.csproj ./
RUN dotnet restore
COPY . ./
RUN dotnet publish -c Release -o /app/out

############################
# Runtime-Stage
############################
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app
COPY --from=build /app/out ./
EXPOSE 5001
ENTRYPOINT ["dotnet","WebApi.dll"]

# Build/Run Beispiel  
# docker build -t min-api-with-mongo .  
# docker run -d -p 5001:5001 min-api-with-mongo

--------------------------------------------------------
Teil 2 – MongoDB integrieren
--------------------------------------------------------

1. Container  
   docker volume create mongodb_data  
   docker run -d --name mongodb -v mongodb_data:/data/db -p 27017:27017 mongo

2. VS Code C#-Extension installieren.

3. MongoDB.Driver + /check-Endpoint  
   cd WebApi && dotnet add package MongoDB.Driver  

--------------------------------------------------------
# Datei: WebApi/Program.cs   (Snippet / check Endpoint)
--------------------------------------------------------
app.MapGet("/check", () =>
{
    try
    {
        var client = new MongoDB.Driver.MongoClient("mongodb://localhost:27017");
        var dbs    = client.ListDatabaseNames().ToList();
        return Results.Ok($"DBs: {string.Join(", ", dbs)}");
    }
    catch (Exception ex) { return Results.Problem($"Mongo Error: {ex.Message}"); }
});
--------------------------------------------------------
# Datei: WebApi/DatabaseSettings.cs
--------------------------------------------------------
public class DatabaseSettings
{
    public string ConnectionString { get; set; } = "";
}
--------------------------------------------------------
# Auszug: appsettings.json
--------------------------------------------------------
{
  "AllowedHosts": "*",
  "DatabaseSettings": { "ConnectionString": "mongodb://localhost:27017" }
}
--------------------------------------------------------

docker-compose (optional)
--------------------------------------------------------
# Datei: docker-compose.yml
--------------------------------------------------------
version: "3.8"

services:
  mongodb:
    image: mongo:latest
    container_name: mongodb
    volumes: [ "mongodb_data:/data/db" ]
    ports: [ "27017:27017" ]
    restart: unless-stopped

  webapi:
    build:
      context: ./WebApi
      dockerfile: Dockerfile
    ports: [ "5001:5001" ]
    depends_on: [ mongodb ]
    environment:
      DatabaseSettings__ConnectionString: "mongodb://mongodb:27017"
    restart: unless-stopped

volumes:
  mongodb_data:
--------------------------------------------------------

Teil 3 – CRUD-Endpoints
--------------------------------------------------------

# Datei: WebApi/Movie.cs
--------------------------------------------------------
using MongoDB.Bson.Serialization.Attributes;

public class Movie
{
    [BsonId] public string Id { get; set; } = "";
    public string Title   { get; set; } = "";
    public int    Year    { get; set; }
    public string Summary { get; set; } = "";
    public string[] Actors { get; set; } = System.Array.Empty<string>();
}
--------------------------------------------------------

Stub-Endpoints in Program.cs  
POST /api/movies · GET /api/movies · GET /api/movies/{id} · PUT · DELETE

Beispiel-GET ohne DB ( id=="1" ) – siehe Program.cs.

--------------------------------------------------------
Teil 4 – MovieService & Dependency Injection
--------------------------------------------------------

# Datei: WebApi/IMovieService.cs
--------------------------------------------------------
using System.Collections.Generic;
public interface IMovieService
{
    IEnumerable<Movie> Get();
    Movie Get(string id);
    void Create(Movie movie);
    void Update(string id, Movie movie);
    void Delete(string id);
}
--------------------------------------------------------
# Datei: WebApi/MongoMovieService.cs
--------------------------------------------------------
using System.Collections.Generic;
using Microsoft.Extensions.Options;
using MongoDB.Driver;

public class MongoMovieService : IMovieService
{
    private readonly IMongoCollection<Movie> _col;
    public MongoMovieService(IOptions<DatabaseSettings> opt)
    {
        var cli = new MongoClient(opt.Value.ConnectionString);
        _col = cli.GetDatabase("gbs").GetCollection<Movie>("movies");
    }
    public IEnumerable<Movie> Get()            => _col.Find(_=>true).ToList();
    public Movie Get(string id)                => _col.Find(m=>m.Id==id).FirstOrDefault();
    public void Create(Movie m)                => _col.InsertOne(m);
    public void Update(string id, Movie m)     => _col.ReplaceOne(x=>x.Id==id, m);
    public void Delete(string id)              => _col.DeleteOne(x=>x.Id==id);
}
--------------------------------------------------------
Program.cs Registrierung  
builder.Services.AddSingleton<IMovieService,MongoMovieService>();  
+ Swagger-Setup (AddEndpointsApiExplorer / AddSwaggerGen).

CRUD-Routes rufen MovieService an.

--------------------------------------------------------
# Datei: WebApi/testing.http   (REST-Client Beispiele)
--------------------------------------------------------
### POST
POST http://localhost:5001/api/movies
Content-Type: application/json

{ "Id":"1","Title":"Inception","Year":2010,
  "Summary":"Traum-Dieb",
  "Actors":["Leonardo DiCaprio"] }

### GET all
GET http://localhost:5001/api/movies

### GET 1
GET http://localhost:5001/api/movies/1

### PUT
PUT http://localhost:5001/api/movies/1
Content-Type: application/json

{ "Id":"1","Title":"Inception (Remastered)","Year":2010,
  "Summary":"Nolan","Actors":["Leonardo DiCaprio"] }

### DELETE
DELETE http://localhost:5001/api/movies/1
--------------------------------------------------------

