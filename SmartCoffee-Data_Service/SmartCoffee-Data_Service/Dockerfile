#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["SmartCoffee-Data_Service/SmartCoffee-Data_Service.csproj", "SmartCoffee-Data_Service/"]
COPY ["SmartCoffee-Data_Service.Service/SmartCoffee-Data_Service.Service.csproj", "SmartCoffee-Data_Service.Service/"]
COPY ["SmartCoffee-Data_Service.Domain/SmartCoffee-Data_Service.Domain.csproj", "SmartCoffee-Data_Service.Domain/"]
COPY ["SmartCoffee-Data_Service.Persistence/SmartCoffee-Data_Service.Persistence.csproj", "SmartCoffee-Data_Service.Persistence/"]
RUN dotnet restore "SmartCoffee-Data_Service/SmartCoffee-Data_Service.csproj"
COPY . .
WORKDIR "/src/SmartCoffee-Data_Service"
RUN dotnet build "SmartCoffee-Data_Service.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SmartCoffee-Data_Service.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "SmartCoffee-Data_Service.dll"]
