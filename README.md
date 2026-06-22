# Readarr

[![Build Status](https://github.com/Juansero29/Readarr/workflows/Readarr%20CI%20%26%20Docker%20Build/badge.svg)](https://github.com/Juansero29/Readarr/actions/workflows/readarr-ci.yml)

### Readarr is currently in active development as a personal fork

Readarr is an ebook and audiobook collection manager for Usenet and BitTorrent users. It can monitor multiple RSS feeds for new books from your favorite authors and will grab, sort, and rename them.
Note that only one type of a given book is supported. If you want both an audiobook and ebook of a given book you will need multiple instances.

## About This Fork

This project is a personal fork of [Faustvii/Readarr](https://github.com/Faustvii/Readarr), which continues development after the original [Readarr/Readarr](https://github.com/Readarr/Readarr) project was retired by the Servarr team. This fork is maintained for personal use and development purposes only. It is not affiliated with, endorsed by, or related to the Servarr team.

### qBittorrent 5.2+ compatibility

qBittorrent **5.2.0 and later** changed the Web API login endpoint (`POST /api/v2/auth/login`) to return **HTTP 204 No Content** with an empty body on successful authentication, instead of HTTP 200 with body `"Ok."`. Unpatched Readarr clients treat the empty response as a login failure even when credentials are correct.

**Symptom:** System health shows *"Unable to communicate with qBittorrent. Failed to authenticate with qBittorrent."* while Radarr/Sonarr on the same qBittorrent instance work fine.

**Fix in this fork:** The qBittorrent download client accepts empty/whitespace login responses as success (ported from [Radarr PR #11258](https://github.com/Radarr/Radarr/pull/11258)). Wrong passwords still fail via non-empty `"Fails."` responses or HTTP 401/403.

## Docker

Pre-built images are published to GitHub Container Registry:

```bash
docker pull ghcr.io/juansero29/readarr:latest
```

Example run (single instance):

```bash
docker run -d \
  --name=readarr \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/Madrid \
  -p 8787:8787 \
  -v /path/to/config:/config \
  ghcr.io/juansero29/readarr:latest
```

Use separate containers (different `/config` volumes) for ebooks vs audiobooks if you manage both.

To rebuild the image manually, run the **Docker Build Manual** workflow from the [Actions tab](https://github.com/Juansero29/Readarr/actions).

## Major Features Include

* Can watch for better quality of the ebooks and audiobooks you have and do an automatic upgrade. *e.g. from PDF to AZW3*
* Automatically detects new books
* Can scan your existing library and download any missing books
* Automatic failed download handling will try another release if one fails
* Manual search so you can pick any release or to see why a release was not downloaded automatically
* Advanced customization for profiles, such that Readarr will always download the copy you want
* Fully configurable book renaming
* SABnzbd, NZBGet, QBittorrent, Deluge, rTorrent, Transmission, uTorrent, and other download clients are supported and integrated
* Full integration with Calibre (add to library, conversion) (Requires Calibre Content Server)
* And a beautiful UI

## License

* [GNU GPL v3](http://www.gnu.org/licenses/gpl.html)
