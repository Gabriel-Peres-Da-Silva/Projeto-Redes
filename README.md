# Subredes
$$
\begin{align*}
\hline
&\text{Primeira subrede: 2 bits - 2 (4) endereços}\\
&\tt{192.168.10.0/30}\quad\text{máscara: \tt{255.255.255.252}}\\
&\tt{192.168.10.3/30}\\
\hline
&\text{Segunda subrede: 2 bits - 2 (4) endereços}\\
&\tt{192.168.10.4/30}\quad\text{máscara: \tt{255.255.255.252}}\\
&\tt{192.168.10.7/30}\\
\hline
&\text{Terceira subrede: 2 bits - 2 (4) endereços}\\
&\tt{192.168.10.8/30}\quad\text{máscara: \tt{255.255.255.252}}\\
&\tt{192.168.10.11/30}\\
\hline
&\text{Quarta subrede: 2 bits - 2 (4) endereços}\\
&\tt{192.168.10.12/30}\quad\text{máscara: \tt{255.255.255.252}}\\
&\tt{192.168.10.15/30}\\
\hline
&\text{Quinta subrede: 2 bits - 2 (4) endereços}\\
&\tt{192.168.10.16/30}\quad\text{máscara: \tt{255.255.255.252}}\\
&\tt{192.168.10.19/30}\\
\hline
&\text{Sexta subrede: 2 bits - 2 (4) endereços}\\
&\tt{192.168.10.20/30}\quad\text{máscara: \tt{255.255.255.252}}\\
&\tt{192.168.10.23/30}\\
\hline
&\text{Sétima subrede (IPv6): 2 bits - 2 (4) endereços}\\
&\tt{2001:cafe:db:1::1/64}\\
&\tt{2001:cafe:db:1::2/64}\\
\end{align*}
$$

# Tabela Relacional
| IOS | Roteador | Interface | Endereço |
|:--: | :--: | :--: | :--: |
| Cisco | R1 | f0/0 | 192.168.10.1/30 |
| Mikrotik | R6 | e0 | 192.168.10.2/30  |
|||||
| Cisco | R1 | e1/2 | 192.168.10.5/30 |
| Cisco | R4 | e1/2 | 192.168.10.6/30 |
|||||
| Cisco | R1 | e1/1 | 192.168.10.9/30 | 
| Cisco | R3 | e1/1 | 192.168.10.10/30 |
|||||
| Cisco | R3 | e1/3 | 192.168.10.13/30 |
| Cisco | R4 | e1/3 | 192.168.10.14/30 |
|||||
| Cisco | R3 | e1/2 | 192.168.10.17/30 | 
| Cisco | R2 | e1/2 | 192.168.10.18/30 |
|||||
| Cisco | R2 | f0/0 | 192.168.10.21/30 |
| Mikrotik | R5 | e0 | 192.168.10.22/30 | 
|||||
| Cisco | R1 | e1/0 | 2001:cafe:db:1::1/64 |
| Cisco | R2 | e1/0 | 2001:cafe:db:1::2/64 |
|||||
| (Túnel) | R1 | -> | 192.168.10.25/30 |
| (Túnel) | R1 | <- | 192.168.10.26/30 |

## Tabela da VLAN
| Dispositivo | Switch | Porta | Endereço | Rede | VLAN |
| :--: | :--: | :--: | :--: | :--: | :--: |
| Gateway |  |  | 192.168.21.5 | 192.168.21.0/29 | VLAN |
| PC1 | Taylor-Switch | f0/0 | 192.168.21.2/29 | 192.168.21.0/29 | VLAN |
| PC2 | Taylor-Switch | f0/1 | 192.168.21.3/29 | 192.168.21.0/29 | VLAN |
| PC3 | Taylor-Switch | f1/0 | 192.168.21.4/29 | 192.168.21.0/29 | - |
| R4 | Taylor-Switch | f1/1 | 192.168.21.5/29 | 192.168.21.0/29 | - |

# Comandos
## Roteador R1
```bash
configure terminal

interface f0/0
ip address 192.168.10.1 255.255.255.252
no shutdown
exit

interface e1/2
ip address 192.168.10.5 255.255.255.252
no shutdown
exit

interface e1/1
ip address 192.168.10.9 255.255.255.252
no shutdown
exit

ipv6 unicast-routing
interface e1/0
ipv6 enable
ipv6 address 2001:cafe:db:1::1/64
no shutdown
exit

interface tunnel 1
ip address 192.168.10.25 255.255.255.252
tunnel mode ipv6
tunnel source e1/0
tunnel destination 2001:cafe:db:1::2

end
wr
```

## Roteador R2
```bash
configure terminal

interface e1/2
ip address 192.168.10.18 255.255.255.252
no shutdown
exit

interface f0/0
ip address 192.168.10.21 255.255.255.252
no shutdown
exit

ipv6 unicast-routing
interface e1/0
ipv6 enable
ipv6 address 2001:cafe:db:1::2/64
no shutdown
exit

interface tunnel 1
ip address 192.168.10.26 255.255.255.252
tunnel mode ipv6
tunnel source e1/0
tunnel destination 2001:cafe:db:1::1

end
wr
```

## Roteador R3
```bash
configure terminal

interface e1/1
ip address 192.168.10.10 255.255.255.252
no shutdown
exit

interface e1/3
ip address 192.168.10.13 255.255.255.252
no shutdown
exit

interface e1/2
ip address 192.168.10.17 255.255.255.252
no shutdown
exit

end
wr
```

## Roteador R4
```bash
configure terminal

interface e1/2
ip address 192.168.10.6 255.255.255.252
no shutdown
exit

interface e1/3
ip address 192.168.10.14 255.255.255.252
no shutdown
exit

interface f0/0
ip address 192.168.21.5 255.255.255.248
no shutdown

end
wr
```

## Roteador R5
```bash
admin

ip address add address=192.168.10.22/30 interface=ether1
```

## Roteador R6
```bash
admin

ip address add address=192.168.10.2/30 interface=ether1
```

## PC 1
```bash
ip 192.168.21.2/29 192.168.21.5
show ip
wr
```

## PC 2
```bash
ip 192.168.21.3/29 192.168.21.5
show ip
wr
```

## PC 3
```bash
ip 192.168.21.4/29 192.168.21.5
show ip
wr
```

## Taylor-Switch
```bash
vlan database
vlan 10
exit

configure terminal
int fa1/1
switchport mode accesss
switchport access vlan 10

exit

int fa1/2
switchport mode accesss
switchport access vlan 10

end
wr
```


# Configuração OSPF
## Roteador R1
```bash
configure terminal

router ospf 1
network 192.168.10.8 0.0.0.3 area 0
network 198.168.10.0 0.0.0.3 area 1
network 192.168.10.4 0.0.0.3 area 0
network 192.168.10.24 0.0.0.3 area 1 # tunel
exit

ipv6 router ospf 1
interface e1/0
ipv6 ospf 1 area 1

end
wr
```

## Roteador R2
```bash
configure terminal

router ospf 1
network 192.168.10.16 0.0.0.3 area 0
network 192.168.10.20 0.0.0.3 area 1
network 192.168.10.24 0.0.0.3 area 1 # tunel
exit

ipv6 router ospf 1
interface e1/0
ipv6 ospf 1 area 1

end
wr
```

## Roteador R3
```bash
configure terminal

router ospf 1
network 192.168.10.8 0.0.0.3 area 0
network 192.168.10.16 0.0.0.3 area 0
network 192.168.10.12 0.0.0.3 area 2

end
wr
```

## Roteador R4
```bash
configure terminal

router ospf 1
network 192.168.10.12 0.0.0.3 area 2
network 192.168.21.0 0.0.0.7 area 2
network 192.168.10.4 0.0.0.3 area 0

end
wr
```

## Roteador R5
```bash
admin

routing ospf area add name=area1 area-id=0.0.0.1
routing ospf network add network=192.168.10.0/30 area=area1
```

## Roteador R6
```bash
admin

routing ospf area add name=area1 area-id=0.0.0.1
routing ospf network add  network=192.168.10.20/30 area=area1
```


# Configuração SSH
## Roteador R3
```bash
configure terminal

hostname R3
ip domain-name DOMINOS
crypto key generate rsa
768 # tamanho (chave)
ip ssh version 2
line vty 0 4
transport input ssh
login local
exit

username ademiro password 1234

end
wr
```
