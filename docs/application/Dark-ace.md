# Create Server <small>with Application API</small>
Create a new server.

## Usage
``` php
<?php
	$pterodactyl->createServer(array $data);
?>
```

## Parameters

| Parameter | Description | Rules |
| - | - | - |
| data | The data to create server | |
 
### data
| Parameter | Description | Rules |
| - | - | - |
| external_id |  External id | sometimes&#124;nullable&#124;string&#124;between:1,191&#124;unique:servers |
| name |  Name | required&#124;string&#124;min:1&#124;max:255 |
| user | Server owner's user id | required&#124;integer&#124;exists:users,id |
| description |  Description | sometimes&#124;string |
| egg |  Egg id | required&#124;exists:eggs,id |
| pack |  Pack id | sometimes&#124;nullable&#124;numeric&#124;min:0 |
| docker_image |  Docker image | sometimes&#124;string&#124;max:255 |
| startup |  Startup command | required&#124;string |
| limits |  Limits | required&#124;array |
| limits.memory |  Memory limit | required&#124;numeric&#124;min:0 |
| limits.swap |  Swap limit | required&#124;numeric&#124;min:-1 |
| limits.disk |  Disk limit | required&#124;numeric&#124;min:0 |
| limits.io |  IO limit | required&#124;numeric&#124;between:10,1000 |
| limits.cpu |  CPU limit | required&#124;numeric&#124;min:0 |
| feature_limits |  Feature limits | required&#124;array |
| feature_limits.databases |  Database limit | present&#124;nullable&#124;integer&#124;min:0 |
| feature_limits.allocations |  Allocation limit | present&#124;nullable&#124;integer&#124;min:0 |
| environment |  Environment variables of the egg | present&#124;array |
| allocation |  Allocation ids | sometimes&#124;array |
| allocation.default |  Default allocation id | sometimes&#124;bail&#124;unique:servers&#124;exists:allocations,id |
| allocation.additional |  Additional allocation ids | sometimes&#124;array |
| allocation.additional.* |  Additional allocation id | exists:allocations,id |
| deploy |  Deploy information | sometimes&#124;required&#124;array |
| deploy.locations |  Deploy locations | array |
| deploy.locations.* |  Deploy location | integer&#124;min:1 |
| deploy.dedicated_ip |  Use dedicated ip or not | required_with:deploy,boolean |
| deploy.port_range |  Port ranges | array |
| deploy.port_range.* |  Port range | string |
| start_on_completion | Start server after finished installation or not | sometimes&#124;boolean |
| skip_scripts | Skip egg scripts or not | sometimes&#124;boolean |
| oom_disabled | Disable oom killer or not | sometimes&#124;boolean |


## Returns

Returns a `server instance`.

``` json
{
	"id": 14,
	"externalId": 11,
	"uuid": "76c59598-22df-4490-92bc-f6fb4a80e0c7",
	"identifier": "76c59598",
	"node": 1,
	"name": "server1",
	"description": "",
	"suspended": false,
	"pack": null,
	"createdAt": "2019-01-23T04:38:09+00:00",
	"updatedAt": "2019-08-05T09:36:13+00:00",
	"limits": {
		"memory": 128,
		"swap": 0,
		"disk": 256,
		"io": 500,
		"cpu": 0
	},
	"allocations": [{
		"id": 55,
		"nodeId": null,
		"ip": "123.123.123.123",
		"ipAlias": null,
		"port": 50053,
		"serverId": null,
		"createdAt": null,
		"updatedAt": null,
		"object": "allocation",
		"alias": "node-1.pterodactyl.panel",
		"assigned": true
	}],
	"object": "server",
	"featureLimits": {
		"databases": 0,
		"allocations": 0
	},
	"user": 1,
	"allocation": 55,
	"nest": 8,
	"egg": 20,
	"container": {
		"startup_command": ".\/Jcmp-Server",
		"image": "hcgcloud\/pterodactyl-jc2mp:latest",
		"installed": false,
		"environment": {
			"SERVER_AUTOUPDATE": "0",
			"STARTUP": ".\/Jcmp-Server",
			"P_SERVER_LOCATION": "test",
			"P_SERVER_UUID": "76c59598-22df-4490-92bc-f6fb4a80e0c7"
		}
	},
	"relationships": {
		"allocations": {
			"object": "list",
			"data": [{
				"object": "allocation",
				"attributes": {
					"id": 55,
					"ip": "123.123.123.123",
					"alias": "node-1.pterodactyl.panel",
					"port": 50053,
					"assigned": true
				}
			}]
		}
	}
}
```

## Example

``` php
<?php
	$nest_id = 1;
	$egg_id = 5;
	$location_id = 1;
	$egg = $pterodactyl->egg($nest_id, $egg_id); //get docker_image and startup directly from egg
	$server = $pterodactyl->createServer([
		"external_id" => '11',
		"name" => $name,
		"user" => 1,
		"egg" => $egg_id,
		"docker_image" => $egg->dockerImage,
		"skip_scripts" => false,
		"environment" => [
			"SERVER_AUTOUPDATE" => '1'
		],
		"limits" => [
			"memory" => 256,
			"swap" => 0,
			"disk" => 1024,
			"io" => 500,
			"cpu" => 100
		],
		"feature_limits" => [
			"databases" => 1,
			"allocations" => 2
		],
		"startup" => $egg->startup,
		"description" => "",
		"deploy" => [
			"locations" => [$location_id],
			"dedicated_ip" => false,
			"port_range" => []
		],
		"start_on_completion" => true
	]);
	print_r($server);
?>
```
