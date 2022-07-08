#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.
ARG VERSION=6.0-alpine

FROM mcr.microsoft.com/dotnet/aspnet:$VERSION AS base
WORKDIR /app


FROM mcr.microsoft.com/dotnet/sdk:VERSION AS build
WORKDIR /src
COPY ["mvcdotnetdocker/mvcdotnetdocker.csproj", "mvcdotnetdocker/"]
RUN dotnet restore "mvcdotnetdocker/mvcdotnetdocker.csproj"
COPY . .
WORKDIR "/src/mvcdotnetdocker"
RUN dotnet build "mvcdotnetdocker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "mvcdotnetdocker.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

CMD ASPNETCORE_URLS=http://*:$PORT dotnet mvcdotnetdocker.dll
