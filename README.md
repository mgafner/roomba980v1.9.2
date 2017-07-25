# roomba980v1.9.2
bash script for remote controlling the iRobot Roomba 980 with firmware version 1.9.2

i wrote this after i bought this iRobot and when my firmware not was updated. I took the infos from this thread: https://community.smartthings.com/t/roomba-980-wifi-connectivity-reverse-engineering/44860/93 . Thanks to Rene Mas ;-)

Some helpful commands to start for Linux Users:

# first step, get password:
curl --insecure -X POST -H "Content-Type: application/json" -d '{"do":"get","args":["passwd"],"id":1}' https://192.168.1.100/umi

# answer should look like:
{"ok":{"passwd":"ouMNGS7Lxk1UR7kP"},"id":1}

# encode the authentication header:
echo -n user:ouMNGS7Lxk1UR7kP | base64
dXNlcjpvdU1OR1M3THhrMVVSN2tQ

# get the BLID:
curl --insecure -X POST -H "Content-Type: application/json" -H "Authorization: Basic dXNlcjpvdU1OR1M3THhrMVVSN2tQ" -d '{"do":"get","args":["sys"],"id":2}' https://192.168.1.100/umi

# answer should look like this:
{"ok":{"umi":2,"pid":2,"blid"[135,49,152,16,37,58,59,110],"sw":"v1.2.9","cfg":0,"boot":3456,"main":7890,"wifi":517,"nav":"01.08.04","ui":2346,"audio":32,"bat":"lith"},"id":2}

# command to start the irobot:
auth_header=$(echo -n user:$PASSWORD | base64)
curl --insecure -X POST -H "Content-Type: application/json" -H "Authorization: Basic $auth_header" -d '{"do":"set","args":["cmd" {"op":"start"}],"id":3}' https://$HOST/umi
