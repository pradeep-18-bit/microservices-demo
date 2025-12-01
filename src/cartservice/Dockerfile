# Build stage
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS builder
ARG TARGETARCH
WORKDIR /app

# Copy entire cartservice folder
COPY . .

# Restore
RUN dotnet restore src/cartservice/src/cartservice.csproj -a $TARGETARCH

# Publish
RUN dotnet publish src/cartservice/src/cartservice.csproj \
    -p:PublishSingleFile=true \
    -a $TARGETARCH \
    --self-contained true \
    -p:PublishTrimmed=true \
    -p:TrimMode=full \
    -c release \
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
