# Changelog
## 4.99.24 - 2023-05-03

### Added
- `hook/build.sh` image tag `latest` for docker image

### Changed
- upgrade tcl version to version 8.6.13
- downgrade tcllib to version 1.20
- upgrade thread to version 2.8.8
- install `rl_json` using download source from github
- `hook/push.sh` push docker image with tag `latest`
- github pipeline will use ``runs-on: ubuntu-22.04`
