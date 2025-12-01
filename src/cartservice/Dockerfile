# Build stage
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS builder
ARG TARGETARCH

WORKDIR /app

# Copy everything including protos and csproj
COPY . .

# Restore
RUN dotnet restore src/cartservice/cartservice.csproj -a $TARGETARCH

# Publish
RUN dotnet publish src/cartservice/cartservice.csproj \
    -p:PublishSingleFile=true \
    --self-contained true \
    -p:PublishTrimmed=true \
    -p:TrimMode=full \
    -a $TARGETARCH \
    -c Release \
    -o /cartservice

# Runtime stage
FROM mcr.microsoft.com/dotnet/runtime-deps:8.0

WORKDIR /app
COPY --from=builder /cartservice .

EXPOSE 7070
ENV DOTNET_EnableDiagnostics=0 \
    ASPNETCORE_HTTP_PORTS=7070

USER 1000
ENTRYPOINT ["/app/cartservice"]
