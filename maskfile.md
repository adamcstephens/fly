# fly a kite

## bootstrap (host)

**OPTIONS**

* version
  * flags: -v
  * type: string
  * desc: k3s version:

```bash
VERSION=${version:-v1.21.0+k3s1}
echo ":: Bootstrapping $host"
k3sup install --k3s-version $VERSION --user $USER --k3s-extra-args '--disable traefik' --host $host
```

## sync

```bash
helmfile sync --wait
```

## gen

### gen cookiesecret

thanks: https://gist.github.com/Pradeek/1053886

```python
import base64
import uuid
secret = base64.b64encode(uuid.uuid4().bytes + uuid.uuid4().bytes)
print(secret.decode('utf-8'))
```

### gen signingsecret

```bash
openssl rand -hex 16
```
