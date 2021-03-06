# loxone-influx

This script listens for events on Loxone's websocket (WS) API and adds event data to Influx based on the config.

The script is based on the script at: https://github.com/raintonr/loxone-stats-influx

## Usage

- Update `default.json` in the `config` folder with:
  - Loxone IP address
  - Loxone username
  - Influx IP address
  - Influx database
  - Your environment's UUID's
- Create `local.json` in the `config` folder with:
  - Loxone password
- run using `node loxone-ws-influx.js` or create a service from it using `service.js`.

The file `local.json` is in `.gitignore` so it won't be added to source control.

UUID's can be easily obtained by opening your `*.Loxone` file and searching for the building block name you are interested in. 

>NOTE: The Loxone WS API will only emit change updates for items that are configured as "Use" in the "User interface" section of the block's Loxone config.
> To avoid clutter in the Loxone native mobile app, I am using a dedicated user for this script, so items that I need in Influx but don't want in the mobile app are assigned in Loxone Config to this user only.

## Configuration

The configuration items under the `uuids` node need to have the following format:

```json
"uuids" : {
    "uuid-of-loxone-item": {
        "measurement" : "influx measurement name",
        "tags": {
            "any": "tag",
            "tag2": "value2"
        }
    }
}
```

### Creating ACR task

```
az acr task create \
     --registry andrasg \
     --name loxone-influx \
     --image loxone-influx:latest \
     --context https://github.com/andrasg/loxone-influx.git \
     --file dockerfile \
     --git-access-token <PAT> \
     --platform linux/arm/v7
```
