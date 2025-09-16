# EsphomeCandyMachine
An Esp8266 that runs a candy dispenser with an API through home assistant

# API Documentation
I've set up some API calls that can send or recieve certain actions with the Candy Machine through my instance of Home Assistant. The API is accessed through the HA_URL and a persistent authentication key. All requests must include the key as an authorization token.

##  Give Candy
To activate the candy dispenser post a request to:
```{{HA_URL}}/api/services/esphome/nodemcu_beta_dispense_candy```

The json data should include ``from_str`` with a string value for the sender that will be displayed on the Candy Machine Display.

Example:
``` 
curl \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"from_str": "Sender's Name"}' \
  {{HA_URL}}/api/services/esphome/nodemcu_beta_dispense_candy
  ```

## Send Message
To send a message to the display screen of the candy machine post a request to:
```{{HA_URL}}/api/services/esphome/nodemcu_beta_get_message```

Json data should include ``from_str`` and ``mess_str``
Example:
``` 
curl \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"from_str": "Sender's Name",
       "mess_str": "Message here"}' \
  {{HA_URL}}/api/services/esphome/nodemcu_beta_get_message
  ```
## Play RTTL
You can create RTTTL strings at [Nokia Composer](https://eddmann.com/nokia-composer-web/) or [RTTTL Composer](https://rtttl.skully.tech/). To send an RTTTL string that will play on the Candy Machine speaker use:
```{{HA_URL}}/api/services/esphome/nodemcu_beta_rtttl_play```

Json data should include ``song_str`` 
Example:
``` 
curl \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"song_str": "Beep: d=4,o=5,b=200:8,c,8,d,8,e"}' \
  {{HA_URL}}/api/services/esphome/nodemcu_beta_rtttl_play
  ```

## Check Status
There are two "sensors" available on the Candy Machine. The **locked** status and the **time remaining** status. 

To check the **locked** status make a get request to:
``{{HA_URL}}/api/states/lock.nodemcu_beta_candy_machine_lock``

No other data needed.
Example:
```
curl \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  http://localhost:8123/api/states/lock.nodemcu_beta_candy_machine_lock
  ```
  To check the **time remaining** status make a get request to:
``{{HA_URL}}/api/states/sensor.nodemcu_beta_candy_lock_minutes_left``

No other data needed.
Example:
```
curl \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  http://localhost:8123/api/states/sensor.nodemcu_beta_candy_lock_minutes_left
  ```
  Each request will return a json object. The relevant response data is the ``state`` attribute.