#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["Ordering/Ordering.API/Ordering.API.csproj", "Ordering/Ordering.API/"]
COPY ["Ordering/Ordering.Application/Ordering.Application.csproj", "Ordering/Ordering.Application/"]
COPY ["Ordering/Ordering.Domain/Ordering.Domain.csproj", "Ordering/Ordering.Domain/"]
COPY ["Ordering/Ordering.Infraustructure/Ordering.Infraustructure.csproj", "Ordering/Ordering.Infraustructure/"]
RUN dotnet restore "Ordering/Ordering.API/Ordering.API.csproj"
COPY . .
WORKDIR "/src/Ordering/Ordering.API"
RUN dotnet build "Ordering.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Ordering.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Ordering.API.dll"]