# Maquina ROCKY  
## VirtualBox
### **Trabajo**
>Tengo dos interfaces de red:
>
* **enp0s3** -> Host-only  
    ip: 192.168.10.5  
    mask: 255.255.255.0  
    gateway: 192.168.10.1  
    configuracion:

    ```sh
    sudo nmcli c m enp0s3 ipv4.addresses 192.168.10.5/24 \
    ipv4.gateway 192.168.10.1 \
    ipv4.method manual && sudo nmcli c up enp0s3
    ```
* **enp0s8** -> NAT  
    ip: dhcp  
    mask: dhcp  
    gateway: dhcp  
    configuracion:

    ```sh
	sudo nmcli c m eno0s8 ipv4.method auto && sudo nmcli c up enp0s8
    ```  

### **Casa**

## VMWare
### **Trabajo**
>Tengo dos interfaces de red:
>
* **ens224** -> Host-only  
    ip: 192.168.15.5  
    mask: 255.255.255.0  
    gateway: 192.168.15.1  
    configuracion:

    ```sh
    sudo nmcli c m ens224 ipv4.addresses 192.168.15.5/24 \
    ipv4.gateway 192.168.15.1 \
    ipv4.method manual && sudo nmcli c up ens224
    ```
* **ens160** -> NAT  
    ip: dhcp  
    mask: dhcp
    gateway: dhcp  
    configuracion:

    ```sh
	sudo nmcli c m ens160 ipv4.method auto && sudo nmcli c up ens160
    ```

### **Casa**
