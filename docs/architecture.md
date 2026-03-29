# Architecture

## Overview

This homelab is built around a single Ubuntu server that hosts self-managed services through Docker.

## Flow

Client → Tailscale → Server → Docker → Services

## Components

- Ubuntu Server 24.04
- Docker Engine
- Tailscale VPN
- Portainer
- Glances

## Design Notes

- Single-node design for simplicity
- Private-only access model
- Service management handled through Docker
- Monitoring kept lightweight