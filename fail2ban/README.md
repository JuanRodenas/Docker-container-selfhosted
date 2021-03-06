# About

#

<p align="center">
    <a href="https://www.fail2ban.org">
        <img src="https://github.com/JuanRodenas/Docker-container-selfhosted/blob/main/fail2ban/fail2ban.png" alt="fail2ban">
    </a>
    <br>
</p>
<!-- markdownlint-enable MD033 -->

#

Fail2ban scans log files and bans IPs that show the malicious signs.

* [Github](https://github.com/fail2ban/fail2ban)
* [Documentation](http://www.fail2ban.org/wiki/index.php/Fail2Ban)
* [Docker Image](https://github.com/crazy-max/docker-fail2ban)

We are going to be using a prebuilt docker image by crazymax to facilitate the process.
There are a lot of fail2ban configurations availabe on the internet, and you can even create your own, this guide provides the following features :

- SSH jail configuration
- Vaultwarden jail configuration

We are gonna use `/var/log` directory to parse log files.

## jails

The jail.d directory is provided with three examples.

- sshd.conf : provides ssh protection, will ban any IP trying to bruteforce your ssh credentials, a must-have if you are still on port 22.

- bitwarden-auth.conf : provides your vaultwarden password manager from bruteforce. 

- bitwarden-admin.conf : provides your vaultwarden admin dashboard from bruteforce. 

You can find the vaultwarden filters in the filter.d directory. For example, in `filter.d/vaultwarden-auth` you can find :

```ini
[INCLUDES]
before = common.conf

[Definition]
failregex = ^.*Username or password is incorrect\. Try again\. IP: <HOST>\. Username:.*$
ignoreregex =
```

This filter is using regex and will match any log that fails to connect to your vaultwarden vault. The log file that is parsed is `/var/log/syslog`, redirection is provided with the docker-compose available in the [vaultwarden guide](../vaultwarden).

# Usage

## Configuration

Replace the environment variables in `.env` with your own, then run :

```bash
sudo docker-compose up -d
```

The jails and filters configured are going to be active, you can now create as many as you want to protect your services.

## Fail2ban

[Fail2ban commands](http://www.fail2ban.org/wiki/index.php/Commands) can be used through the container. 
Here is an example if you want to unban an IP manually:

```
sudo docker exec -t fail2ban fail2ban-client set <JAIL> unbanip <IP>
```

The same example with [bitwarden](../vaultwarden), in the case you failed to enter the right password 3 times:
```
sudo docker exec -t fail2ban fail2ban-client set bitwarden-auth unbanip <IP>
```

For a more general example, you can use any fail2ban command as follows:
```
sudo docker exec -t fail2ban fail2ban-client <COMMAND>
```

# Update

The image is automatically updated with [watchtower](../watchtower) thanks to the following label :

```yaml
  # Watchtower Update
  - "com.centurylinklabs.watchtower.enable=true"
```

# Security

Be careful not to get ban as the iptable action is pretty aggressive, you will not be able to SSH with the banned IP.