# NoSQL Setup mit .NET 8 und MongoDB (Docker)

## Voraussetzungen

### Prüfen, ob .NET 8 installiert ist:

```bash
dotnet --list-sdks
```

### Falls .NET 8 nicht installiert ist, folgende Befehle ausführen:

```bash
sudo apt-get update
sudo apt-get install -y dotnet-sdk-8.0
```

---

## Erstellen eines .NET WebAPI-Projekts

```bash
dotnet new web --name WebApi --framework net8.0
```

---

## Dockerfile (im Verzeichnis `WebApi`) – Multi-Stage Build

```dockerfile
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
```

---

## Docker Compose Setup (`docker-compose.yml`)

```yaml
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

volumes:
  mongoData:
```

---

## Anwendung starten

Starte die Anwendung mit Docker Compose:

```bash
docker compose up
```

Die WebAPI ist danach unter [http://localhost:5001](http://localhost:5001) erreichbar.
