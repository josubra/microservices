FROM microsoft/dotnet:2.1-sdk AS base
WORKDIR /app
EXPOSE 80

RUN apt-get update \
    && apt-get -y install gnupg \
    && curl -sL https://deb.nodesource.com/setup_8.x | bash \
    && apt-get -y install nodejs \
    && apt-get -y install bzip2 \
    && apt-get -y install libfontconfig

RUN npm i npm@latest -g
RUN npm install -g @angular/cli

FROM base AS build
WORKDIR /src
COPY src/WebApps/WebSPA/WebSPA.csproj src/WebApps/WebSPA/
RUN dotnet restore src/WebApps/WebSPA/WebSPA.csproj
COPY . .
WORKDIR /src/src/WebApps/WebSPA
RUN dotnet build WebSPA.csproj -c Release -o /app

FROM build AS publish
RUN dotnet publish WebSPA.csproj -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "WebSPA.dll"]
