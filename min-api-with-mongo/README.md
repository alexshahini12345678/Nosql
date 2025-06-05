--------------------------------------------------------------------------------------------------------
Ablauf 1
Prüfen Sie mit folgendem Command, ob .NET 8 bereits installiert ist.

dotnet --list-sdks

Falls .NET 8 nicht aufgeführt ist, installieren Sie es wie folgt:

dotnet --list-sdks
sudo apt-get update
sudo apt-get install -y dotnet-sdk-8.0
dotnet --list-sdks


--------------------------------------------------------------------------------------------------------
Ablauf 2 
mkdir min-api-with-mongo
cd min-api-with-mongo
--------------------------------------------------------------------------------------------------------
Ablauf 3

# .NET Projekt im Unterordner WebApi erstellen
dotnet new web --name WebApi --framework net8.0

# In das Projektverzeichnis wechseln
cd WebApi

# Anwendung starten
dotnet run

# Anwendung mit [STRG] + [C] beenden
--------------------------------------------------------------------------------------------------------
Ablauf 4

- Nur http-Profil behalten

| launchSettings.json | 
                                                                                    
{
  "profiles": {
    "http": {
      "commandName": "Project",
      "launchBrowser": true,
      "applicationUrl": "http://localhost:{deinPort}",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "dotnetRunMessages": true
    }
  }
}
--------------------------------------------------------------------------------------------------------
Im WebApi Ordner

# 1. Build compile image

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build-env
WORKDIR /build
COPY . .
RUN dotnet restore
RUN dotnet publish -c Release -o out

# 2. Build runtime image

FROM mcr.microsoft.com/dotnet/aspnet:8.0
LABEL description="Minimal API with MongoDB"
LABEL organisation="GBS St. Gallen"
LABEL author="Martin Früh"
WORKDIR /app
COPY --from=build-env /build/out .
ENV ASPNETCORE_URLS=http://*:5001
EXPOSE 5001
ENTRYPOINT ["dotnet", "WebApi.dll"]

--------------------------------------------------------------------------------------------------------

Ablauf 5
| docker-compose.yml im Ordner "min-api-with-mongo"|
services:
  webapi:
    build: WebApi
    restart: always
    depends_on:
      - mongodb
    environment:
      DatabaseSettings__ConnectionString: "mongodb://gbs:geheim@mongodb:27017"
    ports:
      - 5001:5001
  mongodb:
    image: mongo
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: gbs
      MONGO_INITDB_ROOT_PASSWORD: geheim
    volumes:
      - mongoData:/data/db
	ports:
	- "27017:27017"
volumes:
  mongoData:


Anwendung mit docker starten:

docker compose up --build -d
--------------------------------------------------------------------------------------------------------

Ablauf 7

MongoDB.Driver & Endpunkte

# Im Projektverzeichnis (z. B. WebApi)
dotnet add package MongoDB.Driver

- Program.cs
- DatabaseSettings.cs
- appsettings.json
- Movie.cs (WebApi/Movie.cs)
- testing.http





dotnet new web --name MongoMovieApi --framework net8.0
cd MongoMovieApi
dotnet add package MongoDB.Driver




----------
docker logs min-api-with-mongo-mongodb-1
----------
mongosh "mongodb://gbs:geheim@localhost:27017"
