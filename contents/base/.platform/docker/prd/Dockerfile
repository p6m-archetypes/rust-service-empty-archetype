# prd/Dockerfile — the artifact CI publishes and the platform deploys
#
# Layout-agnostic on purpose: this overlay retrofits an EXISTING application, so it copies the whole
# repo and runs the application's OWN build command rather than assuming a module name, a crate, a
# src/ layout or an output path. `build_command` and `runtime_artifact` are answers; their defaults
# match what this language's p6m service archetype produces, so a greenfield-shaped repo needs
# neither.

FROM rust:latest AS builder
WORKDIR /app
RUN apt-get update && apt-get install -y protobuf-compiler && rm -rf /var/lib/apt/lists/*
COPY . .
RUN {{ build_command }}

FROM debian:bookworm-slim
WORKDIR /app
RUN apt-get update && apt-get install -y ca-certificates && rm -rf /var/lib/apt/lists/*
RUN useradd --system --no-create-home --shell /sbin/nologin appuser
COPY --from=builder /app/target/release/{{ runtime_artifact }} /usr/local/bin/{{ runtime_artifact }}
USER appuser
EXPOSE {{ service_port }}
EXPOSE {{ management_port }}
ENTRYPOINT ["{{ runtime_artifact }}"]
