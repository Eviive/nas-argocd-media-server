# NAS ArgoCD Media Server

## Overview

This repository contains the configuration for my NAS's Media Server.  
It uses ArgoCD's [App-of-Apps pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/) to deploy all applications.

## Applications

| Name        | Provider |
|-------------|----------|
| Jellyfin    | Docker   |
| Sonarr      | Docker   |
| Radarr      | Docker   |
| Prowlarr    | Docker   |
| Bazarr      | Docker   |
| Scraparr    | Docker   |
| qBittorrent | Docker   |
