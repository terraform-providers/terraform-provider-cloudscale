---
layout: "cloudscale"
page_title: "cloudscale.ch: cloudscale_subnet"
sidebar_current: "docs-cloudscale-resource-subnet"
description: |-
  Provides a cloudscale.ch Subnet resource. This can be used to create, modify, and delete subnets.
---

# cloudscale\_subnet

Provides a cloudscale.ch Subnet resource. This can be used to create, modify, and delete subnets.

## Example Usage

```hcl
# Create a new private network
resource "cloudscale_network" "privnet" {
  name                    = "privnet"
  zone_slug               = "lpg1"
  mtu                     = "9000"
  auto_create_ipv4_subnet = "false"
}

# Create a new subnet
resource "cloudscale_subnet" "privnet-subnet" {
  cidr         	  = "10.11.12.0/24"
  network_uuid 	  = cloudscale_network.privnet.id
  gateway_address = "10.11.12.10"
  dns_servers     = ["1.2.3.4", "5.6.7.8", "9.10.11.12"]
}

# Create a server with fixed IP address
resource "cloudscale_server" "fixed" {
  name            = "fix"
  zone_slug       = "lpg1"
  flavor_slug     = "flex-2"
  image_slug      = "debian-9"
  interfaces      {
    type          = "public"
  }
  interfaces      {
    type          = "private"
    addresses {
      subnet_uuid = "cloudscale_subnet.privnet-subnet.id"     
      address     = "10.11.12.13"
    }
  }
  volume_size_gb  = 10
  ssh_keys        = ["ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFEepRNW5hDct4AdJ8oYsb4lNP5E9XY5fnz3ZvgNCEv7m48+bhUjJXUPuamWix3zigp2lgJHC6SChI/okJ41GUY=", "ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFEepRNW5hDct4AdJ8oYsb4lNP5E9XY5fnz3ZvgNCEv7m48+bhUjJXUPuamWix3zigp2lgJHC6SChI/okJ41GUY="]
}
```

## Argument Reference

The following arguments are supported when creating/changing Subnets:

* `cidr` - (Required) The address range in CIDR notation. Must be at least /24.
* `network_uuid` - (Required) The network of the subnet.
* `gateway_address` - (Optional) The gateway address of the subnet.
* `dns_servers` - (Optional) A list of DNS resolver IP addresses, that act as DNS servers.


## Attributes Reference

In addition to the arguments listed above, the following computed attributes are exported:

* `href` - The cloudscale.ch API URL of the current subnet.
