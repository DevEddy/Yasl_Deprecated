#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["Server/Yasl.Server.csproj", "Server/"]
COPY ["Client/Yasl.Client.csproj", "Client/"]
COPY ["Shared/Yasl.Shared.csproj", "Shared/"]
RUN dotnet restore "Server/Yasl.Server.csproj"
COPY . .
WORKDIR "/src/Server"
RUN dotnet build "Yasl.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Yasl.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Yasl.Server.dll"]