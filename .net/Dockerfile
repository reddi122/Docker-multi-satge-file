# ====== Stage 1: Build ======
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy project files
COPY *.sln ./
COPY <YourProjectFolder>/*.csproj ./<YourProjectFolder>/
RUN dotnet restore

# Copy the rest of the code
COPY . .
WORKDIR /src/<YourProjectFolder>

# Build the project (Release configuration)
RUN dotnet publish -c Release -o /app/publish

# ====== Stage 2: Runtime ======
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app

# Copy the published app from build stage
COPY --from=build /app/publish .

# Expose port (default for ASP.NET Core)
EXPOSE 80

# Start the app
ENTRYPOINT ["dotnet", "<YourProjectName>.dll"]
