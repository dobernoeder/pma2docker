# Changelog
Documentation of changes

## [1.5.1] - 2025-12-30
### Added
- NEW: Check for LogMod under /usr/local/lib

### Changed
- BUGFIX: When no suffix was set, single images and some multi arch images had a trailing '-'


## [1.5.0] - 2025-12-30
### Added
- NEW: Support for image suffix, for example with date or build hashes

### Changed
- CHANGED: Enhanced Verbose- and Info-Logging

### Removed
- REMOVED: Option "--log-mode", replaced by "--log-disable-label"


## [1.4.0] - 2021-09-09
### Added
- Added message output library for better formated pma2docker output
- Added different output modes: plain, simple and modern


## [1.3.1] - 2021-09-01
###Changed
- Fixed: Problem with call for creation of multiarch image led to single arch image with first architecture in list


## [1.3.0] - 2021-09-01
### Added
- Added Parameter to read only single images from repository
- Added experimental mode to read single images from *.tar.gz and push it to repository as multi-arch manifest
- Added function to combine several local single images and push them to your target repostory including multi-arch manifest
- Added cleanup option to remove created manifests and downloaded images from your local disk

### Changed
- Huge code cleanup
- Better overview in usage dialog
- Better handling for modes which only need a Source or Target repository but not booth


## [1.2.0] - 2021-08-25
### Added
- Added Pull-Split feature to download multiarch image and split with suffix only
- Added function to choose only specific architectures from source images to push to target image
- Added function to extract image for specific architectures or all architectures to an single image file

### Changed
- Stability improvments
- Better user feedback when aborting process 


## [1.1.0] - 2021-08-24
- Added validity check for source and target repository
- Added usage information dialog
- Added handling for parameters
- Added parameter to upload single architectures only
- Support login parameter to automatically login to source and target repository

## Changed
- Improved handling for parameter recognition
- Updated CHANGELOG.md
- Updated README.md


## [1.0.0] - 2021-08-20
Released first version
### Added
- Added CHANGELOG.md, README.md and LICENSE
- Added handling for image specification without tags
- Added PULL-RENAME-PUSH-Behaviour to be compliant to most naming conventions on docker hub
- Added process to create manifests for new target repository


## [Original Project]
The original project can be found at: [github.com](https://github.com/dobernoeder/pma2docker)
[UPCOMING] https://github.com/dobernoeder/pma2docker/compare/1.5.0...master
[1.3.0] https://github.com/dobernoeder/pma2docker/compare/1.4.0...1.5.0
[1.3.0] https://github.com/dobernoeder/pma2docker/compare/1.3.0...1.4.0
[1.3.0] https://github.com/dobernoeder/pma2docker/compare/1.2.0...1.3.0
[1.2.0] https://github.com/dobernoeder/pma2docker/compare/1.1.0...1.2.0
[1.1.0] https://github.com/dobernoeder/pma2docker/compare/1.0.0...1.1.0
[1.0.0] https://github.com/dobernoeder/pma2docker/commits/1.0.0
