# Use ASP.NET runtime base image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080

# Use SDK image to build the app
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy the project file and restore dependencies
COPY ["RandomQuotes.Web/RandomQuotes.Web.csproj", "RandomQuotes.Web/"]
RUN dotnet restore "RandomQuotes.Web/RandomQuotes.Web.csproj"

# Copy the rest of the code and build
COPY . .
WORKDIR "/src/RandomQuotes.Web"
RUN dotnet publish "RandomQuotes.Web.csproj" -c Release -o /app/publish --no-self-contained

# Final stage: runtime image with published app
FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .

# Environment configuration
ENV RANDOM_SECRET_PHRASE=blah
ENV ASPNETCORE_URLS="http://+:8080"
ENV ASPNETCORE_ENVIRONMENT="Production"

ENTRYPOINT ["dotnet", "RandomQuotes.Web.dll"]
