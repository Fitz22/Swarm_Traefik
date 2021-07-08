# Swarm_Traefik
Make a swarm cluster with Traefik


1 _ Créer Network Overlay pour Swarm \
  $ sudo docker network create --driver=overlay traefik-public \
2 _ Exporter le NodeID poru contraindre des containers \
  $ export NODE_ID=$(sudo docker info -f '{{.Swarm.NodeID}}') \
  $ sudo docker node update --label-add traefik-public.traefik-public-certificates=true $NODE_ID \
4 _ Créer password Hashed pour Traefik API Auth \
  $ sudo apt-get install apache2-utils -y \
  $ sudo echo $(htpasswd -nbB <USER> "<PASS>") | sed -e s/\\$/\\$\\$/g \
  \
  Exemple dans le traefik.yaml \
    version: '3' \
        services: \
           myservice: \ 
              image: myimage \
              labels: \
                - "traefik.frontend.auth.basic.users=<USER-PASSWORD-OUTPUT>" \
  \
  5 _ Deploy traefik \
    $ sudo docker stack deploy -c traefik.yml traefik  \
  6 _ Deploy portainer \
    $ sudo docker stack deploy -c portainer.yml portainer \
  7 & 8 _ Check \
    $ sudo docker stack ps traefik \
    $ sudo docker stack ps portainer \
