# linuxfirewall
taak linux 

#zone transfer werkt niet via iptables --> kijk bijgevoegd doc.

#protect ping flooding.

iptables -t filter -A INPUT -p icmp --icmp-type echo-request -m limit --limit 10/minutes -j ACCEPT
iptables -t filter -A INPUT -p icmp -j DROP
iptables -t filter -A OUTPUT -p icmp --icmp-type echo-reply -j ACCEPT

#laat enkel security updates door.

iptables -A OUTPUT -d security.debian.org --dport 80 -j ACCEPT
