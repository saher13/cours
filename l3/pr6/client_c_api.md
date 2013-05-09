# API C - Création d'un client

## Récuperation de l'adresse de l'hôte à atteindre

```c
struct hostent {
  char *h_name;
    /* nom officiel */
  char **h_aliases;
    /* tableau des surnoms */
  int *h_addrtype;
    /* type de l'adresse */
  int h_length;
    /* longueur de l'adresse */
  char **h_addr_list;
    /* tableau d'adresses finissant par NULL */
};
```

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

## Connection au serveur
```c
int connect(int s, struct sockaddr *addr, int addrlen);
```
* `int s` : socket à connecter
* `struct sockaddr *addr` : adresse de l'hôte à joindre
* `int addrlen` : taille de l'adresse de l'hôte

La fonction `connect()` utilise une `struct sockaddr`
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
memcpy(&(addr.sin_addr.s_addr), hent->haddr_list[0], hent->h_length);
```
* `addr.sin_family`
  * `AF_INET`
* `addr.sin_port`
  * `htons(PORT)` conversion du port de l'endianess locale à l'endianess réseau.
* `hent->haddr_list[0]` copie d'une adresse (ici la première adresse connue) de l'hôte que l'on veut atteindre
* 

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

## Remarque

Un client ne sera pas obligé d'utiliser `bind()`. En effet, quand on ne fait pas cet appel, c'est le système qui se chargera de choisir l'adresse et le port à écouter. Quand on se connecte à une socket du serveur, alors le serveur connaitra cette addresse et ce port.
