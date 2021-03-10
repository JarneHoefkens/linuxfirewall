# linuxfirewall
taak linux hardening

#zone transfer werkt niet via iptables volgens mij. 

#Zonetransfer heeft te maken met de DNS server. In het bestand /etc/bind/named.conf.local kan je een aantal “aanpassingen” meegeven die invloed hebben op de werking van DNS.

#/etc/bind/named.conf.local

zone "firewall-zonetransfer" {

        notify no;
        
        allow-update {none;};
        
        type master;
        
        file "/etc/bind/firewall-zonetransfer";
        
        allow-transfer {none;};
        
};



#protect ping flooding.

iptables -t filter -A INPUT -p icmp --icmp-type echo-request -m limit --limit 5/minutes -j ACCEPT

iptables -t filter -A INPUT -p icmp -j DROP

iptables -t filter -A OUTPUT -p icmp --icmp-type echo-reply -j ACCEPT



#laat enkel security updates door.
.
.
iptables -A OUTPUT -d security.debian.org --dport 80 -j ACCEPT

sudo iptables -A OUTPUT -m state --state RELATED,ESTABLISHED -j ACCEPT

sudo iptables -A OUTPUT -o lo -j ACCEPT

sudo iptables -A OUTPUT -j DROP
