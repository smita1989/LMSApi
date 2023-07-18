#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["LMSApi/LMSApi.csproj", "LMSApi/"]
RUN dotnet restore "LMSApi/LMSApi.csproj"
COPY . .
WORKDIR "/src/LMSApi"
RUN dotnet build "LMSApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "LMSApi.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "LMSApi.dll"]