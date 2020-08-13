---
title: pact_mock_server_ffi
custom_edit_url: https://github.com/pact-foundation/pact-reference/edit/master/rust/pact_mock_server_ffi/CHANGELOG.md
---
<!-- This file has been synced from the pact-foundation/pact-reference repository. Please do not edit it directly. The URL of the source file can be found in the custom_edit_url value above -->

## 0.0.7 - Updated XML Matching and Date/Time parsing

* 45fc1a0 - fix: cleanup warnings and fixed test (Ronald Holshausen, Fri Jun 12 10:51:44 2020 +1000)
* 4996c0f - feat: make body processing functions public so other language impl can use them (Ronald Holshausen, Thu Jun 4 16:02:55 2020 +1000)
* f71c57a - bump version to 0.0.7 (Ronald Holshausen, Wed May 27 10:47:48 2020 +1000)

## 0.0.6 - Refactor

* 1104d65 - bump version to 0.0.6 (Ronald Holshausen, Sun May 24 12:05:35 2020 +1000)

## 0.0.5 - matching multipart form posts

* dac517b - feat: implemented FFI support for matching multipart form posts (Ronald Holshausen, Sun May 24 11:17:58 2020 +1000)
* 78854a8 - bump version to 0.0.5 (Ronald Holshausen, Fri May 15 16:38:58 2020 +1000)

## 0.0.4 - Bugfix Release

* c93e364 - fix: correct the backing array list for query parameters from FFI call (Ronald Holshausen, Wed May 13 11:52:55 2020 +1000)
* 9c84713 - fix: correct the backing arary list for headers from FFI call (Ronald Holshausen, Tue May 12 16:45:38 2020 +1000)
* d5c4f96 - bump version to 0.0.4 (Ronald Holshausen, Tue May 12 12:56:30 2020 +1000)
* 8f01bc6 - bump version to 0.0.4 (Ronald Holshausen, Tue May 12 12:54:56 2020 +1000)

## 0.0.3 - matching of binary payloads + fixes handling provider state parameters

* 3a12b6f - fix: incorrectly handling provider state parameters from FFI call (Ronald Holshausen, Fri May 8 16:31:45 2020 +1000)
* 708db47 - feat: implement matching of binary payloads (application/octet-stream) (Ronald Holshausen, Fri May 8 15:52:03 2020 +1000)
* 136c61b - feat: update FFI to support provider states with parameters (Ronald Holshausen, Wed May 6 15:50:10 2020 +1000)
* 95899fe - bump version to 0.0.3 (Ronald Holshausen, Tue May 5 17:14:49 2020 +1000)

## 0.0.2 - Bugfix Release

* 75c965e - fix: correct issue with headers/query with multiple values (Ronald Holshausen, Tue May 5 12:53:28 2020 +1000)
* 2eba288 - fix: update conan test packages to use updated API (Ronald Holshausen, Tue May 5 12:52:28 2020 +1000)
* 2679653 - fix: for failing integration test (Ronald Holshausen, Fri May 1 16:16:30 2020 +1000)
* 9b1c192 - fix: use a single result enum #66 (Ronald Holshausen, Fri May 1 15:42:27 2020 +1000)
* da885a3 - feat: add support for TLS with the mock server #65 (Ronald Holshausen, Thu Apr 30 16:41:30 2020 +1000)
* a45d0c3 - fix: FFI mismatch json should have the actual values as UTF-8 string not bytes #64 (Ronald Holshausen, Thu Apr 30 11:16:25 2020 +1000)
* b30fd2d - feat: add support functions for regular expressions for foreign DSLs (Ronald Holshausen, Tue Apr 28 17:33:48 2020 +1000)
* 4287f0e - fix: correct the windows lib name in conan package (Ronald Holshausen, Mon Apr 27 14:18:01 2020 +1000)
* c1015d5 - fix: correct the windows lib name in conan package (Ronald Holshausen, Mon Apr 27 14:02:44 2020 +1000)
* a0d701e - fix: Macos on conan package (Ronald Holshausen, Fri Apr 24 15:23:53 2020 +1000)
* bb1e35e - fix: Windows URL on conan package (Ronald Holshausen, Fri Apr 24 15:00:24 2020 +1000)
* a13c0fc - fix: Add OSX to the conan package (Ronald Holshausen, Fri Apr 24 14:46:49 2020 +1000)

## 0.0.1 - Changes to support C++ DSL

* 5f8d0a0 - feat: handle bodies with embedded matchers and generators (Ronald Holshausen, Thu Apr 23 12:25:05 2020 +1000)
* 0613180 - feat: add FFI function to create mock server from Pact handle (Ronald Holshausen, Wed Apr 22 17:01:34 2020 +1000)
* fc17d30 - feat: implemented FFI methods for query parameters, headers and bodies (Ronald Holshausen, Wed Apr 22 13:03:52 2020 +1000)
* 9ec8817 - feat: added interaction handle (Ronald Holshausen, Fri Apr 17 18:28:11 2020 +1000)
* d357ad2 - feat: create conan package for linking with C++ projects (Ronald Holshausen, Thu Apr 16 18:00:24 2020 +1000)
* aa80ff1 - pact_mock_server_ffi: Remove extern crate from lib.rs (Audun Halland, Sun Nov 17 23:11:46 2019 +0100)
* 713cd6a - Explicit edition 2018 in Cargo.toml files (Audun Halland, Sat Nov 16 23:55:37 2019 +0100)
* f86cb19 - Merge branch 'master' into feature/create-mock-server-several-times (Ronald Holshausen, Sun Oct 13 14:13:53 2019 +1100)
* d4298f1 - Allow to recreate mock-server (Jérémy Demeule, Sun Oct 6 20:47:15 2019 +0200)
* 7425e6f - Fix pack_mock_server C api and usage (Jérémy Demeule, Sun Oct 6 20:11:57 2019 +0200)
* 2488ab9 - Merge branch 'master' of https://github.com/pact-foundation/pact-reference (milleniumbug, Wed Sep 18 11:32:03 2019 +0200)
* 80404ab - bump version to 0.0.1 (Ronald Holshausen, Sat Sep 7 12:43:08 2019 +1000)

## 0.0.0 - First Release
