# Build stage
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS builder
ARG TARGETARCH
WORKDIR /app

# Copy everything
COPY . .

# Restore dependencies
RUN dotnet restore src/cartservice.csproj -a $TARGETARCH

# Publish the app
RUN dotnet publish src/cartservice.csproj \
    --no-restore \
    -p:PublishSingleFile=true \
    --self-contained true \
    -p:PublishTrimmed=true \
    -p:TrimMode=full \
    -c Release \
    -o /cartservice \
    -a $TARGETARCH

# Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=builder /cartservice .

EXPOSE 7070
ENV DOTNET_EnableDiagnostics=0 \
    ASPNETCORE_HTTP_PORTS=7070 \
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=true

USER 1000
ENTRYPOINT ["/app/cartservice"]
