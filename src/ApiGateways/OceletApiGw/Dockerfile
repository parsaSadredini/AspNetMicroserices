#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["ApiGateways/OceletApiGw/OceletApiGw.csproj", "ApiGateways/OceletApiGw/"]
RUN dotnet restore "ApiGateways/OceletApiGw/OceletApiGw.csproj"
COPY . .
WORKDIR "/src/ApiGateways/OceletApiGw"
RUN dotnet build "OceletApiGw.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "OceletApiGw.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "OceletApiGw.dll"]