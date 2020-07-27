# Changelog

Items starting with `DEPRECATE` are important deprecation notices.

## 2.4.0 (2020-07-27)

+ Add job to produce node_exporter metrics regularly
+ Add needrestart

## 2.3.0 (2020-03-17)

+ Update APT::NeverAutoRemove configuration
+ Add repositories purge
+ Add unneeded packages list

+ Use python3 by default in fact and packages
+ Factorize function call in facts script
+ Fix command output encoding in facts script
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
