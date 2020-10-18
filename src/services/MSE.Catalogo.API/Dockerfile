#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/services/MSE.Catalogo.API/MSE.Catalogo.API.csproj", "src/services/MSE.Catalogo.API/"]
RUN dotnet restore "src/services/MSE.Catalogo.API/MSE.Catalogo.API.csproj"
COPY . .
WORKDIR "/src/src/services/MSE.Catalogo.API"
RUN dotnet build "MSE.Catalogo.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MSE.Catalogo.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MSE.Catalogo.API.dll"]