# Addressing concepts

## Naming

Orbit is divided into several domains:

- Low: bare metal.
- Medium: virtual machines and containers.
- Lagrange: critical infrastructure services that live at predefined addresses 
  and can't migrate freely, such as PXE server.
- Stationary: internal non-critical services.
- Transfer: public access.

## Networking

### Addresses

/24 subnetworks are used for each logical domain:

- 192.168.0.0/24: Networking infrastructure, such as routers and switches.
- 192.168.1.0/24: DHCP pool. Consumer appliances also have to live somewhere, in
the end.
- 192.168.2.0/24: Metal machines.
- 192.168.3.0/24: Virtual machines.
- 192.168.4.0/24: Standalone containers.
- 192.168.12.0/24: Critical infrastructure services.
- 192.168.15.0/24: Virtual services.
