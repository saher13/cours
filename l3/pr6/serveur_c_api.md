# API C - Création d'un serveur UDP

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
if ((sock = socket(PF_INET, SOCK_DGRAM, 0)) == -1) {
  /* traitement de l'erreur */
}
```

## Attachement de la socket au serveur
```c
int bind(int s, struct sockaddr *addr, socklen_t addrlen);
```
* `int s` : socket à connecter
* `struct sockaddr *addr` : adresse de l'hôte
* `int addrlen` : taille de l'adresse de l'hôte

La fonction `bind()` utilise une `struct sockaddr`
```
struct sockaddr {
  unsigned char sa_len;    /* longueur de l'adresse */
  unsigned char sa_family; /* famille de protocole */
  char sa_data[14];        /* adresse complète */
}
```

Pour les sockets IPv4, on castera une `struct sockadrr_in`
```c
struct sockaddr_in {
  short            sin_family; /* famille de protocol */
  unsigned short   sin_port;   /* numero de port */
  struct in_addr   sin_addr;
  char             sin_zero[8];

};
```
```c
struct in_addr {
  unsigned long s_addr;          // load with inet_pton()
};
```

On définit une `struct sockaddr_in` pour représenter l'adresse de la socket à joindre.

```c
struct sockaddr_in addr;
addr.sin_family = AF_INET;
addr.sin_port = htons(PORT);
addr.sin_addr.s_addr = htonl(INADDR_ANY);
```
* `addr.sin_family`
  * `AF_INET`
* `addr.sin_port`
  * `htons(PORT)` conversion du port de l'endianess locale à l'endianess réseau.
* `INADDR_ANY` attache la socket à toutes les interfaces locale. (details ?)

```c
if(bind(sock, (struct sockaddr*)&addr, sizeof(addr)) == -1) {
  /* traitement de l'erreur */
}
```
En TCP, on doit attendre la connexion de clients avec 
```c
int sockclient;
int l;
struct sockaddr_in addrclient; 
if( (sockclient = accept(sock, (struct sockaddr *) &addrclient, &l) ) == -1){
  /* traitement de l'erreur */
}
```
`accept` est bloquant, il va attendre jusqu'à ce qu'un client se connecte. Ensuite on écrit/lit comme avec un pipe sur `sockclient`.

En UDP, Pour recevoir un message on utilise `recvfrom` avec un buffer.
```c
buff = char[256];
int nrec;

if ((nrec = recvfrom(sock, buff, 256, 0, (struct sockaddr *)(&addr), &addr_len)) == -1) {
  /* traitement de l'erreur */
}
```

De même, pour recevoir un message on passe aussi par un buffer.  
NB : Il faut connaitre l'adresse à laquelle envoyer le message car on n'est pas en mode connecté. Lorsqu'on répond à une demande, on utilise l'adresse qui a été rempli par `recvfrom`

```c
buff = char[256];
/* remplissage de buff*/
if (sendto(sock, buff, 256, 0, (struct sockaddr *)(&addr), sizeof(addr)) == -1) {
  /* traitement de l'erreur */
}
```

## Remarque

(Ne pas oublier de fermer la socket (et libérer mémoire allouée)).

Contrairement au client l'appel `bind()` à bind est obligatoire.
