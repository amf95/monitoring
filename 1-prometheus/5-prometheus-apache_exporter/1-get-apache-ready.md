
> Usually its enabled by default.

```bash
a2enmod status
```

```bash
vim /etc/apache2/mods-enabled/status.conf
```

```bash
<Location "/server-status">
    SetHandler server-status
    Require local
</Location>
ExtendedStatus On
```

```bash
systemctl restart apache2
```