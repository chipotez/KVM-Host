**Preparando Host.**

1.- Actualizar S.O.
  
    [root@gold72 ~]# yum update -y
      Complementos cargados:product-id, search-disabled-repos, subscription-manager
      rhel-7-server-extras-rpms                                                    | 3.2 kB  00:00:00
      rhel-7-server-optional-rpms                                                  | 3.3 kB  00:00:00
      rhel-7-server-rh-common-rpms                                                 | 3.8 kB  00:00:00
      rhel-7-server-rpms                                                           | 3.5 kB  00:00:00
      rhel-7-server-supplementary-rpms                
      
    Instalado:
      kernel.x86_64 0:3.10.0-327.28.3.el7 

    Actualizado:
      systemd-sysv.x86_64 0:219-19.el7_2.12                    teamd.x86_64 0:1.17-6.el7_2
        (......)
    ¡Listo!
  
  2.- Instalaremos el software necesario para proporcionar un entorno  que permita alojar equipos virtualizados:
  
      [root@gold72 ~]# yum install qemu-kvm qemu-img libvirt -y
      Complementos cargados:product-id, search-disabled-repos, subscription-manager
      Resolviendo dependencias
      --> Ejecutando prueba de transacción
      (....)
      
      Instalado:
        libvirt.x86_64 0:1.2.17-13.el7_2.5  qemu-img.x86_64 10:1.5.3-105.el7_2.7  

      Dependencia(s) instalada(s):
      (....)
      
      ¡Listo!
  
  3.- Instalaremo utilidades adicionales necesarias para configurar libvirt e instalar máquinas virtuales:
  
      [root@gold72 /]# yum install virt-install libvirt-python python-virthost libvirt-client
    Complementos cargados:product-id, search-disabled-repos, subscription-manager
    El paquete libvirt-client-1.2.17-13.el7_2.5.x86_64 ya se encuentra instalado con su versión más reciente
    Resolviendo dependencias
    --> Ejecutando prueba de transacción
    
    Dependencias resueltas
    
    ==========================================================================================================
     Package                        Arquitectura      Versión                Repositorio               Tamaño
    ==========================================================================================================
    Instalando:
     libvirt-python                       x86_64      1.2.17-2.el7           rhel-7-server-rpms         309 k
     virt-install                         noarch      1.2.1-8.el7            rhel-7-server-rpms          85 k
    Instalando para las dependencias:
     libosinfo                            x86_64      0.2.12-3.el7           rhel-7-server-rpms         256 k
     python-ipaddr                        noarch      2.1.9-5.el7            rhel-7-server-rpms          36 k
     virt-manager-common                  noarch      1.2.1-8.el7            rhel-7-server-rpms         1.0 M
    
    Resumen de la transacción
    =======================================================================================================
    Instalar  2 Paquetes (+3 Paquetes dependientes)
    
    Instalado:
      libvirt-python.x86_64 0:1.2.17-2.el7                                virt-install.noarch 0:1.2.1-8.el7
    
    Dependencia(s) instalada(s):
      libosinfo.x86_64 0:0.2.12-3.el7                  virt-manager-common.noarch 0:1.2.1-8.el7
    
    ¡Listo!

  
  4.- Por defecto, este demonio se marca el inicio automático en cada arranque. Compruebe con el siguiente comando:

    [root@gold72 /]# systemctl status libvirtd
      ● libvirtd.service - Virtualization daemon
        Loaded: loaded (/usr/lib/systemd/system/libvirtd.service; enabled; vendor preset: enabled)
         Active: inactive (dead)
           Docs: man:libvirtd(8)
                 http://libvirt.org
                 
    [root@gold72 /]# reboot 
    Connection to 192.168.122.61 closed by remote host.

    [root@gold72 ~]# systemctl status libvirtd
    ● libvirtd.service - Virtualization daemon
       Loaded: loaded (/usr/lib/systemd/system/libvirtd.service; enabled; vendor preset: enabled)
       Active: active (running) since dom 2016-09-11 22:40:35 CDT; 9s ago
         Docs: man:libvirtd(8)
               http://libvirt.org
     Main PID: 1291 (libvirtd)
       CGroup: /system.slice/libvirtd.service
          ├─1291 /usr/sbin/libvirtd
          ├─1478 /sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/libexec/libvirt_...
          └─1479 /sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/libexec/libvirt_...
    
    sep 11 22:40:35 gold72.empiem.com systemd[1]: Starting Virtualization daemon...
    sep 11 22:40:35 gold72.empiem.com systemd[1]: Started Virtualization daemon.
    sep 11 22:40:35 gold72.empiem.com dnsmasq[1478]: started, version 2.66 cachesize 150
    sep 11 22:40:35 gold72.empiem.com dnsmasq[1478]: compile time options: IPv6 GNU-getopt DBus no-i18n IDN DHCP DHCPv6 no-Lua TFT...t auth
    sep 11 22:40:35 gold72.empiem.com dnsmasq-dhcp[1478]: DHCP, IP range 192.168.124.2 -- 192.168.124.254, lease time 1h
    sep 11 22:40:35 gold72.empiem.com dnsmasq[1478]: reading /etc/resolv.conf
    sep 11 22:40:35 gold72.empiem.com dnsmasq[1478]: using nameserver 192.168.122.1#53
    sep 11 22:40:35 gold72.empiem.com dnsmasq[1478]: read /etc/hosts - 2 addresses
    sep 11 22:40:35 gold72.empiem.com dnsmasq[1478]: read /var/lib/libvirt/dnsmasq/default.addnhosts - 0 addresses
    sep 11 22:40:35 gold72.empiem.com dnsmasq-dhcp[1478]: read /var/lib/libvirt/dnsmasq/default.hostsfile
    Hint: Some lines were ellipsized, use -l to show in full.


5.- Si por alguna razón esto funciona, lo configuraremos para el inicio automático ejecutando el siguiente comando:

    [root@gold72 /]# systemctl enable libvirtd
    
6.- Para detener/iniciar/reiniciar este demonio, esto es lo que necesita ejecutar:

    [root@gold72 /]# systemctl stop libvirtd
    [root@gold72 /]# systemctl start libvirtd
    [root@gold72 /]# systemctl restart libvirtd