# DependencyHelperPOC
## Installation
1. Download rc.common shell script.
2. SCP it overwritting the default on your device(command: "scp rc.common root@192.168.1.1:/etc").

## Usage
The new rc.common adds two commands meant to use when making your.

__procd_depends_on_service 'fullservicepatch'__

example: 
```procd_depends_on_service /etc/init.d/network```

In this case *network* service will be attempted to start and if it fails. The script will stop with exit 1.

__procd_wants_service 'fullservicepatch'__

example: 
```procd_wants_service /etc/init.d/ulogd```

In this case *ulogd* service will be attempted to start and if it fails the script will not call exit 1.

## Other info

This was made for procd as it has no means of managing dependencies on other services like some other service supervision systems like *systemd*.

The concept is pretty simple and it should work on most systems.

## Testing

Added a new service *krasher* to start up procedure of procd before *network* starts.
![image](https://user-images.githubusercontent.com/88534947/131078575-8570c8bb-c023-49be-9c78-044b3d49e98f.png)

*krasher* has dependency on *projectiot* in this case:
```procd_depends_on_service /etc/init.d/projectiot```

*projectiot* needs *network* service to work so it has a dependency on it.
```procd_depends_on_service /etc/init.d/network```

__Result__:

*projectiot* starts early in the service startup order along with *krasher* and it starts *network* service along with it.

## Warning

Right now there are no checks for services including each other and causing an *infinite loop* so you have to be careful with how you use this.


## Disclaimer

I'm not responisble if your device installation is damaged or bootloops are caused using this.

Use it at your own risk.
