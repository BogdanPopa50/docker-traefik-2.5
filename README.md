# traefik-2.5
Configuration demo: docker  + traefik-2.5( proxy to containers and LAN )


1. Create a network for traefik:
$ docker network create traefik_network


2. In file traefik.yml put the correct e-mail address:

certificatesResolvers:
  le:
    acme:
      email: E-MAIL@E-MAIL.com # THIS NEED TO BE REPLACED
      storage: /etc/traefik/acme/acme.json
      tlsChallenge: true
      httpChallenge:
        # used during the challenge
        entryPoint: web

3. Add labels on docker containers


4. Configure static configurations for LAN hosts in routes.yml

## STATIC CONFIGURATION

http:
  routers:
    nas:
      service: service_nas
      rule: Host(`nas.example.com`)
      tls:
        certresolver: le
      entryPoints:
        - websecure
    nas_:
      service: service_nas
      rule: "Host(`nas.example.com`)"
      entryPoints:
        - web


    dvr:
      service: service_dvr
      rule: Host(`dvr.example.com`)
      tls:
        certresolver: le
      entryPoints:
        - websecure
    dvr_:
      service: service_dvr
      rule: "Host(`dvr.example.com`)"
      entryPoints:
        - web

  services:
    service_nas:
      loadBalancer:
        servers:
          - url: "http://192.168.1.14:80"
 
    service_dvr:
      loadBalancer:
        servers:
          - url: "http://192.168.1.15:80"


