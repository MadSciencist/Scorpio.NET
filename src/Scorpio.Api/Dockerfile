FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /src
COPY ["src/Scorpio.Api/Scorpio.Api.csproj", "src/Scorpio.Api/"]
COPY ["src/Scorpio.Messaging/Scorpio.Messaging.Abstractions.csproj", "src/Scorpio.Messaging/"]
COPY ["src/Scorpio.Messaging.RabbitMQ/Scorpio.Messaging.RabbitMQ.csproj", "src/Scorpio.Messaging.RabbitMQ/"]
RUN dotnet restore "src/Scorpio.Api/Scorpio.Api.csproj"
COPY . .
WORKDIR "/src/src/Scorpio.Api"
RUN dotnet build "Scorpio.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Scorpio.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Scorpio.Api.dll"]