# LAB_OSPF_1

<aside>
💡

Pre-requis : ≥ Packet Tracer 8.2

</aside>

> Commandes Utiles (en plus)
> 

```
### Rappel

# Passer en mode admin
Routeur>enable

# Passer en mode configuration
Routeur# conf t

# Voir la configuration du routeur
Routeur#show run

# Quitter un mode 
Router#exit

# Voir les logs de debug OSPF
Router#debug ip ospf adj

# Relancer le processus OSPF (changement ed conf par exemple )
Router#clear ip ospf process

# Voir les voisin OSPF
Router#show ip ospf neighbor

# Voir la table de routage
Router#show ip route
```

# OSPF Configuraion

router 1

```
Router(config)#router ospf 1
Router(config-router)#network 10.10.10.0 0.0.0.3 area 0
Router(config-router)#network 100.100.100.0 0.0.0.7 area 0
Router(config-router)#network 31.31.31.0 0.0.0.7 area 0
Router(config-router)#passive-interface g0/0
```

router 0

```
Router(config)#router ospf 1
Router(config-router)#network 100.100.100.0 0.0.0.7 area 0
Router(config-router)#network 30.30.30.0 0.0.0.7 area 0
Router(config-router)#network 40.40.40.0 0.0.0.7 area 0
Router(config-router)#passive-interface g0/0
```

router 3

```
Router(config)#router ospf 1
Router(config-router)#network 30.30.30.0 0.0.0.7 area 0
Router(config-router)#network 31.31.31.0 0.0.0.7 area 0
Router(config-router)#network 53.53.53.0 0.0.0.3 area 0
Router(config-router)#passive-interface g0/0
```

router 2

```
# Route par defaut vers router 1
Router(config)#ip route 0.0.0.0 0.0.0.0 10.10.10.1
```

router 5

```
# Route par defaut vers router 3
Router(config)#ip route 0.0.0.0 0.0.0.0 53.53.53.3
```

router 4

```

# Route par defaut vers router 0
Router(config)#ip route 0.0.0.0 0.0.0.0 40.40.40.1
```

# NAT ( PAT ) Configuration

router 4

```
Router(config)#access-list 1 permit 192.168.1.0 0.0.0.255 
Router(config)#ip nat inside source list 1 interface GigabitEthernet0/0
Router(config)#int g0/0
Router(config-if)#ip nat outside
Router(config-if)#exit
Router(config)#int g0/1
Router(config-if)#ip nat inside
```

router 5

```
Router(config)#access-list 1 permit 192.168.1.0 0.0.0.255 
Router(config)#ip nat inside source list 1 interface GigabitEthernet0/1
Router(config)#int g0/1
Router(config-if)#ip nat outside
Router(config-if)#exit
Router(config)#int g0/0
Router(config-if)#ip nat inside
```

router 2

```
Router(config)#access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)#ip nat inside source list 1 interface GigabitEthernet0/0
Router(config)#int g0/1.1
Router(config-subif)#ip nat inside
Router(config-subif)#exit
Router(config)#int g0/1.2
Router(config-subif)#ip nat inside
Router(config-subif)#exit
Router(config)#int g0/0
Router(config-if)#ip nat outside
```

## Bonus

<aside>
💡

Dans notre cas ici nous n’avons pas de routes static ou par defaut a redistribuer aux autres routeurs faisant tourner le processus OSPF mais si ca avait été le cas voici les commandes

</aside>

```

# Redistribuer les routes statics
Router(config)#router ospf <ospf_process_id>
Router(config-router)#redistribute static subnets

# Redistribuer les routes par défaut
Router(config)#router ospf <ospf_process_id>
Router(config-router)#default-information originate
```
