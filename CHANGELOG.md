<!-- note marker start -->
**NOTE**: This repo contains only the documentation for the private BoltsOps Pro repo code.
Original file: https://github.com/boltopspro/vpc/blob/master/CHANGELOG.md
The docs are publish so they are available for interested customers.
For access to the source code, you must be a paying BoltOps Pro subscriber.
If are interested, you can contact us at contact@boltops.com or https://www.boltops.com

<!-- note marker end -->

# Change Log

All notable changes to this project will be documented in this file.
This project *loosely tries* to adhere to [Semantic Versioning](http://semver.org/).

## [1.1.3]
- Add individual port rules

## [1.1.2]
- Improve @secure_db_ports_on_public_network_acl check, default to false

## [1.1.1]
- Fix @secure_db_ports_on_public_network_acl check

## [1.1.0]
- #4 secure db ports on public network acl
- Fix FlowLogRole iam permission
- FlowLogGroup TrafficType ALL - note it wont pass CIS, as that currently looks for REJECT

## [1.0.5]
- FlowLogGroup TrafficType ALL

## [1.0.4]
- FlowLogGroup TrafficType ACCEPT

## [1.0.3]
- add lono_type to gemspec

## [1.0.2]
- update vpc name: development-vpc, production-vpc, etc

## [1.0.1]
- delete .meta/config.yml

## [1.0.0]
- #3 lono v6 upgrade: auto_camelize false

## [0.4.0]
- #2 PublicDbSubnetGroup and PrivateDbSubnetGroup outputs
- only display outputs for created resources
- vpc with only public subnets: dont create NATs

## [0.3.0]
- docs: add one account docs
- add useful VPC outputs

## [0.2.0]
- use Camelize naming

## [0.1.0]
- Initial release
