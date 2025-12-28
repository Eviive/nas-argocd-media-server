# NAS ArgoCD Media Server

## Overview

This repository contains the configuration for my NAS's Media Server.  
It uses ArgoCD's [App-of-Apps pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/) to deploy all applications.

## Dependencies

| Name         | Provider |
|--------------|----------|
| Bazarr       | Docker   |
| Jellyfin     | Docker   |
| Jellyseerr   | Docker   |
| Prowlarr     | Docker   |
| CoreDNS      | Docker   |
| Gluetun      | Docker   |
| FlareSolverr | Docker   |
| qBittorrent  | Docker   |
| Radarr       | Docker   |
| Scraparr     | Docker   |
| Sonarr       | Docker   |
