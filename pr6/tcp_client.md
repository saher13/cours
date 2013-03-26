# API C - Création d'un client

## Récuperation de l'adresse de l'hote à atteindre

```c
struct hostent *gethostbyname(char *hostname)
```

```c
struct hostent *hent;
if(!(hent = gethostbyname(hostname))) {
  /* traitement de l'erreur */
}
```

## Création d'une socket

```c
int socket(int domain, int type, int protocol)
```

* domain :
  * `PF_INET` : IPv4
  * `pF_INET6` : IPv6
  * ...
* type :
  * `SOCK_STREAM` : la socket créée est comparable à un pipe. Doit être connectée pour être utilisable. (voir `connect()`)
  * `SOCK_DGRAM` : 
* protocol : ...

```c
socket sock;
if ((sock = socket(PF_INET, SOCK_STREAM, 0)) == -1) {
  /* traitement de l'erreur */
}
```

* création d'une `sockaddr_in`
* definition de `addr.sin_family` avec `AF_INET`
* définition de `addr.sin_port` avec `htons(PORT)` pour convertir la représentation locale en représentation sur le réseau.
* copie de l'adresse (d'une adresse) de l'hôte que l'on veut atteindre
```c
struct sockaddr_in addr;
addr.sin_family = AF_INET;
addr.sin_port = htons(PORT);
memcpy(&(addr.sin_addr.s_addr), hent->haddr_list[0], hent->h_length);
```

On utilise cette sockaddr_in pour connecter notre socket
```c
if(connect(sock, (struct sockaddr*)&addr, sizeof(addr)) == -1) {
  /* traitement de l'erreur */
}
```

Pour recevoir un message on utilise `recv` avec un buffer.
```c
buff = char[256];
int nrec;
while ((nrec = recv(sock, buff, 256, 0)) > 0) {
  /* traitement de buff */
}
if (nrec == - 1) {
  /* traitement de l'erreur */
}
```

De même, pour recevoir un message on passe aussi par un buffer.
```c
buff = char[256];
/* remplissage de buff */
if(send(sock, buff, 256, 0) == -1) {
  /* traitement de l'erreur */
}
```

(Ne pas oublier de fermer la socket (et libérer mémoire allouée)).

```
!!! PARLER DE BIND ? !!
```