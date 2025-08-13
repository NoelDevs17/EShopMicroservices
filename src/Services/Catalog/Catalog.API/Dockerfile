# Etapa 1: build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copia solo el .csproj
COPY Services/Catalog/Catalog.API/Catalog.API.csproj ./Catalog.API/
RUN dotnet restore ./Catalog.API/Catalog.API.csproj

# Copia todo el contenido del proyecto
COPY Services/Catalog/Catalog.API/. ./Catalog.API/

# Cambiar directorio y compilar
WORKDIR /src/Catalog.API
RUN dotnet build -c Release -o /app/build

# Etapa 2: publicación
FROM build AS publish
RUN dotnet publish -c Release -o /app/publish

# Etapa 3: runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=publish /app/publish .

EXPOSE 80
ENTRYPOINT ["dotnet", "Catalog.API.dll"]
