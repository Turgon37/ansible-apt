# Changelog

Items starting with `DEPRECATE` are important deprecation notices.

## 2.3.0 (2020-03-17)

+ Use python3 by default in fact and packages
+ Update molecule configuration for CI

## 2.2.0 (2019-09-26)

+ Add UnattendedUpgrade whitelist option
+ Split UnattendedUpgrade lists into separated files
+ Add cache backup options

## 2.1.0 (2019-07-02)

+ Rename apt__configurations_whitelist into apt__configurations_purge_whitelist
+ Split apt__configurations_purge_whitelist into multiple vars

## 2.0.0 (2019-07-01)

+ remove template in favor of settings content from inventory
* rename apt__conf_purge into apt__configurations_purge
+ add listchanges configuration

## 1.0.0 (2018-10-13)

First release
