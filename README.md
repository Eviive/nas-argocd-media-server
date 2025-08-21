# NAS ArgoCD Media Server

## Overview

This repository contains the configuration for my NAS's Media Server.  
It uses ArgoCD's [App-of-Apps pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/) to deploy all applications.

## Applications

| Name        | Provider | Version |
|-------------|----------|---------|
| Jellyfin    | Docker   | 10.10.7 |
| Sonarr      | Docker   | 4.0.15  |
| Radarr      | Docker   | 5.26.2  |
| Prowlarr    | Docker   | 1.37.0  |
| Bazarr      | Docker   | 1.5.2   |
| Scraparr    | Docker   | 2.2.4   |
| qBittorrent | Docker   | 5.1.2   |
