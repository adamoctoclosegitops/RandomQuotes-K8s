FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG TARGETARCH
ARG RUNTIME

WORKDIR /src
COPY ["RandomQuotes.Web/RandomQuotes.Web.csproj", "RandomQuotes.Web/"]
RUN dotnet restore "RandomQuotes.Web/RandomQuotes.Web.csproj"

COPY . .
WORKDIR "/src/RandomQuotes.Web"

# Use RUNTIME passed from buildx args (linux-x64, linux-arm64, etc.)
RUN dotnet publish "RandomQuotes.Web.csproj" -c Release \
    --runtime $RUNTIME \
    --self-contained false \
    -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .

ENV RANDOM_SECRET_PHRASE=blah
ENV ASPNETCORE_URLS="http://+:8080"
ENV ASPNETCORE_ENVIRONMENT="Production"
ENTRYPOINT ["dotnet", "RandomQuotes.Web.dll"]
